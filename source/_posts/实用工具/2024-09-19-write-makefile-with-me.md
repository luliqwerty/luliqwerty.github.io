---
title: 和我一起写Makefile
date: 2024-09-19
tags: [Makefile]
categories: tools
---

### Makefile执行过程

1. 读取阶段
   1. 读取Makefile文件的所有内容
   2. 根据Makefile的内容再程序中建立起变量
   3. 在程序中构建起显示规则和隐式规则
   4. 建立目标和依赖之间的依赖图
2. 目标更新阶段
   1. 用第一阶段构建起来的数据确定哪个目标需要更新然后执行对应的更新方法
   2. 变量和函数的展开如果发生再第一阶段就称作**立即展开**，否则称为**延迟展开**

### Makefile 的规则

```makefile
target: prerequisites
	recipe
```

#### **target**

- target 可以是一个 **object file （目标文件，eg: main.o）**，也可以是一个**可执行文件（executable file, eg: hello, ./hello execute the target file）**，还可以是一个**标签（label, eg: clean）**。

- 指定最终目标(默认是第一个目标) ：`.DEFAULT_GOAL = your-customized-target(你的自定义总目标)`

- 如果某个目标(target) 跟最终的目标以及过程中的目标文件没有任何依赖关系的话就不会自动执行，必须手动执行  eg:  以下 `Makefile` 文件执行`make`命令时，test.o  和  clean  都不会自动执行，因为他们跟最终的 `target` 无关，必须手动 `make clean`,  `make test.o` 才会执行相应的 `recipe` 。

  ```Makefile
  target: hello.o main.o
  	g++ -o target hello.o main.o
  	
  hello.o: hello.cpp
  	g++ -c hello.cpp
  	
  main.o: main.cpp hello.h
  	g++ -c main.cpp
  	
  test.o: test.cpp
  	g++ -c test.cpp
  	
  clean:
  	rm *.o target
  ```

#### 伪目标：没有对应依赖文件的目标

```Makefile
# 1. 正常可以不用将clean 加入到伪目标中，但是当该文件夹下存在和目标同名的文件时就会冲突，当前路径下有个文件叫做 clean，执行 make clean 就不能正常执行，会提示 clean is up to date
.PHONY: clean

# 2. 当某个目标需要每次都更新时可以将其添加为伪目标
.PHONY: main
# 每次执行 make 命令时 main 的依赖文件不管有没有改动，都会重新执行main下面的recipe
main: main.cpp
	$echo "main.cpp"
	# 命令加不加 $符号的区别：不加则先打印指令再打印指令执行的结果，加就只打印执行结果
```

#### **prerequisites**(依赖)

- 生成改 `target`所依赖的文件（没有依赖项就不写）。
- **依赖类型：** **target: normal prequisites | order-only prequisites**，make 会根据你的依赖文件(normal prequisites) 是否更改过来判断目标文件是否需要重新执行 recipe, 不会检查 order-only prequisites。

#### **recipe**(执行的方法，本质就是shell命令)

- 该 `target`需要执行的命令（可以是任意的 shell 命令）。

```
make 会自动执行 shell 命令
```

### Makefile 中使用变量（相当于C语言宏定义）

#### **引用变量切记要用括号  () {} 都可以**

```makefile
objects = main.o kbd.o command.o dispaly.o \# 反斜杠是连接不同行的内容
	insert.o search.o files.o utils.o
clean:
	rm $(objects) # 删除上面罗列的所有的 .o 文件
# This is a comment!
# \ 相当于换行符

$(info <message>) # info 后面是什么就会输出什么
```

#### **自动得到所有的依赖文件**

```bash
g++ -MM main.cpp
# 这个命令会直接给出 main.cpp 所有的依赖文件
```

#### **多行变量**

```makefile
define shells
$echo hello
$echo main
$echo hahah
endef
```

#### **取消变量定义**

```makefile
undefine shells
```

#### **变量覆盖**

```makefile
# 在makefile objs 的定义是 
objs = main.o test.o
# 变量覆盖会在执行 make 时覆盖原 makefile 中的变量
# 在执行 `make` 命令时，make objs=123456 会使objs的值变为123456，等号的内容不能包含空格；需要有空格的字符串时需要用引号 make objs="main.o hello.o"

# override 会使变量覆盖失效
override objs = main.o test.o
```

#### **系统中的环境变量可以直接使用**

#### **变量替换引用**

```makefile
objs = main.o hello.o test.o
files = $(objs: .o=.cpp) # 将objs字符串中 .o 替换为 .cpp
files = $(objs: %.o=%.cpp) # 同上

# 骚操作
# shell赋值 !=
files != ls *.cpp
objs = $(files: %.cpp = %.o)
```

#### **绑定目标的变量**

```makefile
# 全局的变量 整个 makefile 文件都能访问
global_var=this is a global var

# 这样定义之后只有 first.c 才能访问到 这个变量 target_var
first.c: target_var = this is a target var
first.c:
	$echo target_var = $(target_var)
	$echo global_var = $(global_var)
打印结果：
"target_var = this is a target var"
"global_var = this is a global var"
	
test.c:
	$echo target_var = $(target_var)
	$echo global_var = $(global_var)
打印结果：
"target_var = (此处得到的是一个空串，因为该目标不能访问 target_var)"
"global_var = this is a global var"
```

#### **自动变量**

```makefile
$@ # 本目标的目标名 target
$< # 本条规则第一个依赖名称
$? # 依赖中修改时间晚于目标文件的所有文件名，以空格隔开
$^ # 所有依赖文件名，重复的文件名只列一次，不包含 order-only 依赖
$+ # 和 $^ 类似，$+ 包含重复的文件名
$| # 所有的 order-only 依赖
$* # 目标文件文件名的主干部分，不包括后缀名
$% # 如果目标不是归档文件则为空；否则为对应的成员文件名

# 以下变量分别和上面 $ 后面的符号对应，区别是 D F
# D 代表 directory (当前变量所在的目录，结尾不带/)
# F 代表 file (变量除去目录部分的)
$(@D)
$(@F)
$(<D)
$(<F)
$(?D)
$(?F)
$(^D)
$(^F)
$(+D)
$(+F)
$(|D)
$(|F)
$(*D)
$(*F)
$(%D)
$(%F)
```

#### **二次展开**

#### **多目标与多规则**

- **独立多目标**

  1. 只需要写目标和依赖，不需要写方法

     ```makefile
     block.o input.o scene.o : common.h
     # the same as the follow
     block.o : common.h
     input.o : common.h
     scene.o : common.h
     ```

  2. 生成目标的方法写法一样的，只是依赖与目标不一样时

     [![2024-09-19-110455.png](https://i.postimg.cc/26DsB5Q1/2024-09-19-110455.png)](https://postimg.cc/7CKsFwQ4)

     最后改成这样，不过这样头文件依赖丢了，最后只要有一点改动就会导致整个项目重新编译，**建议不要这样写。**

     ```makefile
     $(objs):
     	g++ -c $(@:%.o=%.cpp)
     ```

- **组合多目标**

  **组合多目标调用一次方法将更新所有目标，独立多目标每个目标的更新需要单独调用一次更新方法**

  ```makefile
  block.o input.o scene.o &: block.cpp input.cpp common.h
  	g++ -c block.cpp
  	g++ -c input.cpp
  	g++ -c scene.cpp
  ```

  所有目标的更新方法都写道其中，每次更新只会调用一次

- **静态模式**

  静态模式就是用 `%` 进行文件匹配来推导出相应的依赖(只能在一定程度上解决文件依赖问题，头文件依赖任然解决不了)

  ```makefile
  $(objs): %.o : %.cpp
  	g++ -c $(@:%.o=%.cpp)
  ```

**特殊目标**

```Makefile
.ONESHELL
.SILENT
.PHONY
$
- (忽略错误继续执行)
	# 如果 hello 文件不存在，rm 会报错不继续执行删除 .o 文件，加了 - 号会继续向后执行
	-rm hello
	-rm *.o
```

### 指定依赖搜索路径

```makefile
# upper case VPATH 大写的 VPATH 是变量
VPATH= src:include # 冒号连接多个文件夹

main.o: %.o : %.cpp
	g++ -c $< -Iinclude # g++ 命令 -I 选项 在 include 文件夹搜索头文件

# lower case vpath 小写的 vpath 是指令
vpath %.h include
vpath %.cpp src
```

### 条件判断

```makefile
# ifdef 判断一个变量是否已经定义
ifdef var
# TO DO LISTS
else
# TO DO LISTS
endif

# ifeq 判断两个字符串是否相等
ifeq($(var1), $(var2))
# TO DO
else
# TO DO
endif

# ifneq 如果不等
```

### 函数

#### 字符串处理函数

```makefile
# 函数调用时相当于使用变量
# $(function_name arguments ...)

files= test.cpp main.cpphello.cpp test.o hello.o

# subst 文本替换 完全匹配就替换
$(subst target, replacement, text)
	--target 需要替换的内容
	--replacement 被替换的内容
	--text 需要处理的内容

# patsubst 模式替换 没有空格分隔的内容不会替换
$(patsubst target, replacement, text)
	--target 需要替换的内容
	--replacement 被替换的内容
	--text 需要处理的内容
newfiles=$(patsubst .cpp, .h, $(files)) # 结果为 "test.h main.cpphello.h test.o hello.o" 中间的 .cpp 不会被替换

# strip 去除字符串头部和尾部的空格，将中间多个空格用一个空格替换，返回去除空格后的文本
$(strip string)
	--string 需要处理的字符串
	
# findstring 字符串查找
$(findtring find, stirng)
	--find 需要查找的字符串
	--string 用来查找的内容
	
# filter 从文本中筛选出符合模式的内容并返回
$(filter pattern..., text)
	--pattern 匹配模式串
	--text 用于筛选的源文本
# eg:
newfiles=$(filter %.cpp, $(files)) # 结果为 "test.cpp main.cpphello.cpp"

# filter-out 过滤被筛选的模式串 返回剩下的内容
$(filter-out pattern..., text)
	--pattern 匹配模式串
	--text 用于筛选的源文本
# eg:
newfiles=$(filter-out %.o, $(files)) # 结果为 "test.cpp main.cpphello.cpp"

# sort 按字典序排序 同时去重
$(sort list)
	--list 需要排序的内容
	
# word 用于返回文本中第 n 个单词
$(word n, text)
	--n 第n 个单词 （n > 0 && n < text.len()）
	--text 待处理文本
	
# wordlist 用于返回一个范围的单词
$(wordlist start, end, text)
	--start 起始位置
	--end 最终位置
	--text 待处理文本

# words 返回单词总数
# $(words text)

# firstword 返回 text 第一个单词
$(firstword text)

# lastword 返回 text 最后一个单词
# $(lastword text)
```

#### 文件名处理函数

```makefile
files=src/main.cpp hello.cpp
# dir 返回文件目录
$(dir files) # 结果为 src
	--files 是需要返回目录的文件名，可以有多个，用空格隔开

# notdir 返回文件名，有目录时会被去掉
$(notdir files) # 结果为 main.cpp hello.cpp

# suffix 用于返回文件后缀名
$(suffix files) # .cpp .cpp

# basename 返回除了后缀的文件名
$(basename files) # src/main hello

# addsuffix 给文件名添加后缀
$(addsuffix suffix, files)
	--suffix 需要添加的后缀

# join 将两个列表连接，多余的部分会原样返回

# wildcard 返回符合通配符的列表
$(wildcard *.cpp) # src/main.cpp hello.cpp

# realpath 返回文件的绝对路径，文件不存在会被忽略
$(realpath files)

# abspath 类似于 realpath 但是当某些文件不存在时仍然会返回当前目录
$(abspath files)
```
TO DO
#### 条件函数

```makefile

```

#### file函数

```makefile

```

#### foreach函数

```makefile

```

#### call函数

```makefile

```

#### value函数

```makefile

```

#### origin函数

```makefile

```

#### flavor函数

```makefile

```

#### eval函数

```makefile

```

#### shell函数

```makefile

```

#### let函数

```makefile

```

#### 信息提示和控制函数

```makefile

```

### 显示规则和隐式规则

**警告：Makefile 的自动推导还是少用，保不齐出什么 bug 你连调试都不知道怎么调试**当项目比较复杂时可能会有以下问题
1. 依赖关系不明确：如果文件之间的依赖关系没有明确指定，make 可能无法正确判断哪些文件需要更新。
2. 文件名约定：如果文件名不符合常见的约定，make 可能无法正确识别。
3. 复杂的构建规则：当项目的构建规则非常复杂时，自动推导可能会出现错误。

#### C语言编译

.c -> .o

```makefile
$(CC) $(CPPFLAGS) $(CFLAGS) -c
```

#### C++编译

.cc  .cpp  .C  -> .o

```makefile
$(CC) $(CPPFLAGS) $(CFLAGS) -c
```

#### 链接

由 .o 文件链接到可执行文件

```makefile
$(CC) $(LDFLAGS) *.o $(LOADLIBES) $(LDLIBS)
```

### 同一项目有多个Makefile文件

#### 包含其他文件

使用include 指令可以读入其他 Makefile 文件的内容，效果就如同在 include 的位置上用对应的文件内容替换一样

```makefile
include mkf1 mkf2
include *.mk

忽略找不到头文件错误
-include *.mk
```

### 嵌套Makefile

```makefile
.PHONY: subsrc subdir clean

subsrc:
	$(MAKE) -C src
	
subdir:
	$(MAKE) -C lib
	
clean:
	$(MAKE) clean -C src
	$(MAKE) clean -C lib
```

可以通过 `export` 指令向子项目的 `Makefile` 传递变量

```makefile
export var # 传递变量 var
export # 传递所有变量
unexport # 取消传递
```

### 总结
Makefile 实质就是一系列的 shell 命令，只要对 shell 命令熟悉就很好写 Makefile

### 后续学习过程，多阅读大型项目的 Makefile 代码

**redis**: https://github.com/redis/redis

**ffmpeg**

**aubio**

**libav**

**OpenH264**

**TinyVM**

**TinyXML2**