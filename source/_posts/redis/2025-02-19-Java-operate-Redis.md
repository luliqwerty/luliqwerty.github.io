---
title: Java 操作 Redis 时遇到 Redis 生成的 key 前有莫名的十六进制
tags: [Redis, 踩坑]
date: 2025-02-19
categories: redis
---
# Java 操作 Redis 时遇到 Redis 生成的 key 前有莫名的十六进制

## Spring Data Redis 的使用

1. 引入依赖

2. Redis 连接配置（前提你得有 Redis 客户端）

    ```yaml
    spring:
      redis:
      	# redis链接地址
        host: 127.0.0.1
        # redis链接端口
        port: 6379
        # redis链接密码
        password:
        # redis连接池
        lettuce:
          pool:
          	# 最大链接数
            max-active: 8
            # 最大建立链接等待时间，-1为无限制
            max-wait: -1
            # 最大空闲数
            max-idle: 8
            # 最小空闲数
            min-idle: 0
          # 关闭超时时间，在关闭客户端链接之前等待任务处理完成的最长时间，在这之后，无论任务是否执行完成，都会被执行器关闭
          shutdown-timeout: 100
        database: 2         # 使用Redis中的第三个分区，默认是0
    
    ```

    

3. 直接使用 RedisTemplate 和 StringRedisTemplate

    配置完以上两个步骤，就可以使用redis了，spring-data-redis默认提供RedisTemplate<Object,Object>和StringRedisTemplate<String,String>工具，其中RedisTemplate<Object,Object>就是键值都是Object类型，而StringRedisTemplate<String,String>就是键值都是String类型。

    使用方法很简单，在需要使用到RedisTemplate的类上通过@Autowired或者@Resource注解就能获取该工具，当然，类上必须有声明由Spring管理的注解，例如@Controller，@Service，@Configuration等。

    ```java
    // 存进redis,key和value都可以是object
    stringRedisTemplate.opsForValue().set("key", "value");
    // 取出 key
    String str = stringRedisTemplate.opsForValue().get("key");
    ```

4. 自定义你的 RedisTemplate（以下就是这次踩坑的原因）

    ```java
    // 我的 RedisTemplate 配置
    @Configuration
    @Slf4j
    public class RedisConfiguration {
        public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory) {
            log.info("开始创建 redis 模板对象");
            RedisTemplate redisTemplate = new RedisTemplate();
            // 设置 redis 的连接工厂对象
            redisTemplate.setConnectionFactory(redisConnectionFactory);
    
            // 设置 redis key 的序列化器
            redisTemplate.setKeySerializer(new StringRedisSerializer());
    
            return redisTemplate;
        }
    }
    ```



## 分析问题

我在自定义 RedisTemplate 没有指定 **泛型** 导致 key 和 value 默认都是 `Object` 类，而 `Object` 被序列化存进 redis 时是没办法直接阅读的。

```java
// 例如
@Autowired
private RedisTemplate redisTemplate;

redisTemplate.opsForValue().set("dish_" + categoryId, list);

// 最后得到的 key 结果是 "\xac\xed\x00\x05t\x00\x07dish_20"
```



解决该问题的代码如下

```java
@Configuration
@Slf4j
public class RedisConfiguration {
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        log.info("开始创建 redis 模板对象");
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        
        // 设置 Redis 的连接工厂对象
        redisTemplate.setConnectionFactory(redisConnectionFactory);

        // 设置 Redis key 的序列化器
        redisTemplate.setKeySerializer(new StringRedisSerializer());

        // 设置 Redis value 的序列化器
        Jackson2JsonRedisSerializer<Object> valueSerializer = new Jackson2JsonRedisSerializer<>(Object.class);
        redisTemplate.setValueSerializer(valueSerializer);

        return redisTemplate;
    }
}

```

注意 Bean 注解可以指定 redisTemplate 的名称

Spring工厂里本身就有redisTemplate，会覆盖原来的redisTemplate

例如上面的代码可以写成

```java
@Bean(name = "stringObjectRedisTemplate")    // key - String，value - Object

@Autowired
private RedisTemplate stringObjectRedisTemplate;
// 使用的就是泛型为 <String, Object> 的 redisTemplate
```



## 问题二再次爆发

或者说根本不算问题，我是标题党（doge

```
ERROR 5392 --- [nio-8080-exec-3] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : 
Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; 
nested exception is org.springframework.data.redis.serializer.SerializationException: 
Could not write JSON: Java 8 date/time type `java.time.LocalDateTime` not supported by default: add Module "com.fasterxml.jackson.datatype:jackson-datatype-jsr310" to enable handling (through reference chain: java.util.ArrayList[0]->com.sky.vo.DishVO["updateTime"]); 
nested exception is com.fasterxml.jackson.databind.exc.InvalidDefinitionException: 
Java 8 date/time type `java.time.LocalDateTime` not supported by default: add Module "com.fasterxml.jackson.datatype:jackson-datatype-jsr310" to enable handling (through reference chain: java.util.ArrayList[0]->com.sky.vo.DishVO["updateTime"])] with root cause
```

这个错误是由于 `LocalDateTime` 类型的序列化问题。错误提示 Java 8 日期/时间类型不支持。这意味着 Jackson 没有正确加载或配置这个模块。我们需要确保 `JavaTimeModule` 被正确注册到 `ObjectMapper` 上。

```java
// 创建 ObjectMapper 并注册 JavaTimeModule
ObjectMapper objectMapper = new ObjectMapper();
objectMapper.registerModule(new JavaTimeModule());  // 注册处理 LocalDateTime 的模块

// 创建 Jackson2JsonRedisSerializer 并设置 ObjectMapper
Jackson2JsonRedisSerializer<Object> jacksonSerializer = new Jackson2JsonRedisSerializer<>(Object.class);
jacksonSerializer.setObjectMapper(objectMapper);

// 设置 RedisTemplate 的 key 和 value 序列化方式
redisTemplate.setKeySerializer(new StringRedisSerializer());
redisTemplate.setValueSerializer(jacksonSerializer);
```



**成功解决 ez**