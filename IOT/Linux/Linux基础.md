## 文件系统
```
.         代表此层目录
..        代表上一层目录
-         代表前一个工作目录
~         代表“目前使用者身份”所在的主文件夹
~account  代表 account 这个使用者的主文件夹（account是个帐号名称）
```
- cd：变换目录
- pwd：显示目前的目录
- mkdir：创建一个新的目录
- rmdir：删除一个空的目录
- cd （change directory, 变换目录）

## 用户管理
### 添加账户和给予用户权限
```bash
adduser username

# 打开/etc/sudoers文件
sudo visudo
# 找到root，在下一行添加
username ALL=(ALL) ALL 

```

## Makefile
* 准备工作：[[WSL安装配置]]
* 安装make：`sudo apt install make`
打开`wsl`，新建`main.c`，`message.h`，`message.c`
```c
// main.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "message.h"

int main(){

  

    message();

    return 0;

}
// message.h
void message();

// message.c
#include <stdio.h>
#include "message.h"
void message(){

    printf("hello world\n");

}

```
编译生成
```bash
gcc main.c message.c -o hello
./hello
```
### Makefile的规则
```bash
target:dependencies
	recipe
```

target：可以是一个 object file（目标文件），也可以是一个可执行文件，还可以是一个标签（label）。
prerequisites：生成该 target 所依赖的文件和/或 target。
recipe：该 target 要执行的命令（任意的 shell 命令）。

这是一个文件的依赖关系，也就是说，target 这一个或多个的目标文件依赖于 dependencies 中的文 件，其生成规则定义在 recipe 中。

使用Makefile生成可执行文件
新建makefile文件，在里面写入
```bash
hello:main.o message.o
	gcc main.o message.o -o hello

main.o:main.c
	gcc -c main.c -o main.o

message.o:message.c
	gcc -c message.c -o message.o
```
保存退出，输入`make`命令
```bash
make
# 生成可执行文件hello
./hello
```
### make如何工作
在默认的方式下只输入 make 命令。
1. make 会在当前目录下找名字叫“Makefile”或“makefile”的文件。 
2. 如果找到，它会找文件中的第一个目标文件（target），并把这个文件作为最终的目标文件。 
3. 如果 target 文件不存在，或是 edit 所依赖的后面的 .o 文件的文件修改时间要比 edit 这个文件新， 那么，他就会执行后面所定义的命令来生成 target 这个文件。 
4. 如果 target 所依赖的 .o 文件也不存在，那么 make 会在当前文件中找目标为 .o 文件的依赖性，如 果找到则再根据那一个规则生成 .o 文件。
5. 如果 C 文件和头文件存在的，于是 make 会生成 .o 文件，然后再用 .o 文件生成 make 的终极任务，也就是可执行文件 target 了。

make 会一层又一层地去找文件的依赖关系，直到最终编译出第一个目 标文件。在找寻的过程中，如果出现错误，比如最后被依赖的文件找不到，那么make就会直接退出，并报错，而对于所定义的命令的错误，或是编译不成功，make 根本不理。make 只管文件的依赖性，即，如果在找了依赖关系之后，冒号后面的文件还是不在，就不工作。
在编程中，如果这个工程已被编译过了，当修改了其中一个源文件，比如 `main.c` ，那 么根据依赖性，目标 main.o 会被重编译（也就是在这个依性关系后面所定义的命令），于 是 `main.o` 的文件也是最新的啦，于是 `main.o` 的文件修改时间要比 `target` 要新，所以 `target` 也会被重新 链接了（详见 `target` 目标文件后定义的命令）。 而如果改变了 `message.h` ，那么，`main.o` 、`message.o` 都会被重编译，并且，`target` 会被重链接。
### 使用变量
变量在声明时需要给予初值，而在使用时，需要给在变量名前加上 `$` 符号，并用小括号 `()` 把变量给包括起来。
假如需要编译两个可执行文件，`hello`和`world`
```bash
world hello: main.o message.o
	gcc main.o message.o -o world

main.o:main.c
	gcc -c main.c -o main.o

message.o:message.c
	gcc -c message.c -o message.o
```
为了简化名称的重复使用，可以用变量存储，先定义变量，使用时用$加()
```bash
targets = world hello
sources = main.c message.c
objects = main.o message.o

$(targets):$(objects)
	gcc $(objects) -o $(targets)

main.o:main.c
	gcc -c main.c -o main.o

message.o:message.c
	gcc -c message.c -o message.o

```

#### 自动化变量
\$@ 表示目标文件，\$<表示第一个依赖
```bash
targets = world hello
sources = main.c message.c
objects = main.o message.o

$(targets):$(objects)
	gcc $(objects) -o $(targets)

main.o:main.c
	gcc -c $< -o $@

message.o:message.c
	gcc -c $< -o $@
```

### 伪目标
“伪目标”并不是一个文件，只是一个标签，由于“伪目标” 不是文件，所以 make 无法生成它的依赖关系和决定它是否要执行。只有通过显式地指明这个“目 标”才能让其生效。当然，“伪目标”的取名不能和文件名重名，不然其就失去了“伪目标”的意义了。当然，为了避免和文件重名的这种情况，我们可以使用一个特殊的标记“.PHONY”来显式地指明 一个目标是“伪目标”，向 make 说明，不管是否有这个文件，这个目标就是“伪目标”。
clean，可以用作清除编译的中间文件
all，用作生成多个目标，又不产生自身
```bash
.PHONY:clean all

targets = world hello
sources = main.c message.c
objects = main.o message.o

all:targets
	echo "all down"

$(targets):$(objects)
	gcc $(objects) -o $(targets)
	
main.o:main.c
	gcc -c $< -o $@

message.o:message.c
	gcc -c $< -o $@

clean:
	rm -f *.o $(targets)
```

### 符号

#### = 与 :=

* =
- 简单的赋值运算符
- 用于将右边的值分配给左边的变量
- 如果在后面的语句中重新定义了该变量，则将使用新的值

```bash
HOST_ARCH   = aarch64
TARGET_ARCH = $(HOST_ARCH)

# 更改了变量 a
HOST_ARCH   = amd64

debug:
	@echo $(TARGET_ARCH)
```

* :=
* -立即赋值运算符
- 用于在定义变量时立即求值
- 该值在定义后不再更改
- 即使在后面的语句中重新定义了该变量
```bash
HOST_ARCH   := aarch64
TARGET_ARCH := $(HOST_ARCH)

# 更改了变量 a
HOST_ARCH := amd64

debug:
	@echo $(TARGET_ARCH)
```

#### ?=
- 默认赋值运算符
- 如果该变量已经定义，则不进行任何操作
- 如果该变量尚未定义，则求值并分配
```bash
HOST_ARCH  = aarch64
HOST_ARCH ?= amd64

debug:
    @echo $(HOST_ARCH)
```

#### 累加 +=
```bash
CXXFLAGS := -m64 -fPIC -g -O0 -std=c++11 -w -fopenmp

CXXFLAGS += $(include_paths)
```

#### \\
- 续行符
```bash
LDLIBS := cudart opencv_core \
          gomp nvinfer protobuf cudnn pthread \
          cublas nvcaffe_parser nvinfer_plugin 
```

#### \* 与 %
- `*`: 通配符表示匹配任意字符串，可以用在目录名或文件名中
- `%`: 通配符表示匹配任意字符串，并将匹配到的字符串作为变量使用
`main.o`与`message.o`的生成规则一样，名称不同，则可以使用通配符进行简化
```bash
.PHONY:clean all

targets = world hello
sources = main.c message.c
objects = main.o message.o

all:$(targets)
    @echo "all down"  

$(targets):$(sources)
    gcc $(sources) -o $@

%.o:%.c
    gcc -c $< -o $@

clean:
    rm -f *.o $(targets)
```

### 函数
函数调用，很像变量的使用，也是以 “$” 来标识的，其语法如下：
```bash
$(fn, arguments) or ${fn, arguments}
```
- fn: 函数名
- arguments: 函数参数，参数间以逗号 `,` 分隔，而函数名和参数之间以“空格”分隔
#### shell
```bash
$(shell <command> <arguments>)
```
- 名称：shell 命令函数 —— shell
- 功能：调用 shell 命令 command
- 返回：函数返回 shell 命令 command 的执行结果
```bash
# shell 指令，src 文件夹下找到 .cpp 文件
cpp_srcs := $(shell find src -name "*.cpp") 
# shell 指令, 获取计算机架构
HOST_ARCH := $(shell uname -m)
```

#### subst
```bash
$(subst <from>,<to>,<text>)
```
- 名称：字符串替换函数——subst
- 返回：函数返回被替换过后的字符串
- 功能：把字串 `<text>` 中的` <from> `字符串替换成` <to>  `
```bash

cpp_srcs := $(shell find src -name "*.cpp")
cpp_objs := $(subst src/,objs/,$(cpp_objs)) 

```


#### patsubst
```bash
$(patsubst <pattern>,<replacement>,<text>)
```

- 名称：模式字符串替换函数 —— patsubst
- 功能：通配符 `%`，表示任意长度的字串，从 text 中取出 patttern， 替换成 replacement
- 返回：函数返回被替换过后的字符串

```bash
cpp_srcs := $(shell find src -name "*.cpp") #shell指令，src文件夹下找到.cpp文件
cpp_objs := $(patsubst %.cpp,%.o,$(cpp_srcs)) #cpp_srcs变量下cpp文件替换成 .o文件
```

#### foreach
```bash
$(foreach <var>,<list>,<text>)
```
- 名称：循环函数——foreach。
- 功能：把字串`<list>`中的元素逐一取出来，执行`<text>`包含的表达式
- 返回：`<text>`所返回的每个字符串所组成的整个字符串（以空格分隔）
```basg'
library_paths := /datav/shared/100_du/03.08/lean/protobuf-3.11.4/lib \
                 /usr/local/cuda-10.1/lib64

library_paths := $(foreach item,$(library_paths),-L$(item))
```

#### dir
```bash
$(dir <names...>)
```
- 名称：取目录函数——dir。
- 功能：从文件名序列中取出目录部分。目录部分是指最后一个反斜杠（“/”）之前 的部分。如果没有反斜杠，那么返回“./”。
- 返回：返回文件名序列的目录部分。
```bash
$(dir src/foo.c hacks)    # 返回值是“src/ ./”。
```

### 编译过程
#### 预处理
```bash
cpp_srcs := $(shell find src -name *.cpp)
pp_files := $(patsubst src/%.cpp,src/%.i,$(cpp_srcs))

src/%.i : src/%.cpp
	@g++ -E $^ -o $@

preprocess : $(pp_files)

clean :
	@rm -f src/*.i

debug :
	@echo $(pp_files)

.PHONY : debug preprocess clean
```

#### 编译成汇编语言
```bash
cpp_srcs := $(shell find src -name *.cpp)
as_files := $(patsubst src/%.cpp,src/%.s,$(cpp_srcs))

src/%.s : src/%.cpp
	@g++ -S $^ -o $@

assemble : $(as_files)

clean :
	@rm -f src/*.s

debug :
	@echo $(as_files)

.PHONY : debug assemble clean
```

#### 编译成目标文件
```bash
cpp_srcs := $(shell find src -name *.cpp)
cpp_objs := $(patsubst src/%.cpp,objs/%.o,$(cpp_srcs))

objs/%.o : src/%.cpp
	@mkdir -p $(dir $@)
	@g++ -c $^ -o $@

objects : $(cpp_objs)

clean :
	@rm -rf objs src/*.o

debug :
	@echo $(as_files)

.PHONY : debug objects clean
```

#### 编译选项
- `-m64`: 指定编译为 64 位应用程序
- `-std=`: 指定编译标准，例如：-std=c++11、-std=c++14
- `-g`: 包含调试信息
- `-w`: 不显示警告
- `-O`: 优化等级，通常使用：-O3
- `-I`: 加在头文件路径前
- `fPIC`: (Position-Independent Code), 产生的没有绝对地址，全部使用相对地址，代码可以被加载到内存的任意位置，且可以正确的执行。这正是共享库所要求的，共享库被加载时，在内存的位置不是固定的

- `-l`: 加在库名前面
- `-L`: 加在库路径前面
- `-Wl,<选项>`: 将逗号分隔的 <选项> 传递给链接器
- `-rpath=`: "运行" 的时候，去找的目录。运行的时候，要找 .so 文件，会从这个选项里指定的地方去找

编译带头文件的程序
```bash
cpp_srcs := $(shell find src -name *.cpp)
cpp_objs := $(patsubst src/%.cpp,objs/%.o,$(cpp_srcs))

# 你的头文件所在文件夹路径（建议绝对路径）
include_paths := 
I_flag        := $(include_paths:%=-I%)


objs/%.o : src/%.cpp
	@mkdir -p $(dir $@)
	@g++ -c $^ -o $@ $(I_flag)

workspace/exec : $(cpp_objs)
	@mkdir -p $(dir $@)
	@g++ $^ -o $@ 

run : workspace/exec
	@./$<

debug :
	@echo $(I_flag)

clean :
	@rm -rf objs

.PHONY : debug run
```
