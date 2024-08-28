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
- `-fPIC`: (Position-Independent Code), 产生的没有绝对地址，全部使用相对地址，代码可以被加载到内存的任意位置，且可以正确的执行。这正是共享库所要求的，共享库被加载时，在内存的位置不是固定的

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
编译和连接静态库
```bash
lib_srcs := $(filter-out ./src/main.c,$(shell find ./src -name "*.c"))
lib_objs := $(patsubst ./src/%.c,./objs/%.o,$(lib_srcs))

  
include_paths := ./include
library_paths := ./lib
linking_libs := message

I_options := $(include_paths:%=-I%)
l_options := $(linking_libs:%=-l%)
L_options := $(library_paths:%=-L%)

  
compile_flags := -g -o3 -std=c11 $(I_options)
linking_flags := $(l_options) $(L_options)

  
./objs/%.o:./src/%.c
    @mkdir -p $(dir $@)
    @gcc -c $^ -o $@ $(compile_flags)

./lib/libmessage.a: $(lib_objs)
    @mkdir -p $(dir $@)
    @ar -r $@ $^

static_lib: libxxx.a

.objs/main.o:.src/main.c
    @mkdir -p $(dir $@)
    @gcc -c $^ -o $@ $(compile_flags)  

hello:./objs/main.o
    @mkdir -p $(dir $@)
    @gcc $^ -o $@ $(linking_flags)

run : hello
    @./$<

clean:
    @rm -f $(lib_objs)

debug:
    @echo $(lib_srcs)
    @echo $(lib_objs)

.PHONY:clean all debug run
```

编译动态库并链接
```bash
lib_srcs := $(filter-out ./src/main.c,$(shell find ./src -name "*.c"))
lib_objs := $(patsubst ./src/%.c,./objs/%.o,$(lib_srcs))

include_paths := ./include
library_paths := ./lib
linking_libs := message

I_options := $(include_paths:%=-I%)
l_options := $(linking_libs:%=-l%)
L_options := $(library_paths:%=-L%)
r_options := $(library_paths:%=-Wl,-rpath=%)

compile_flags := -g -o3 -w -fPIC $(I_options)
linking_flags := $(l_options) $(L_options) $(r_options)


./objs/%.o:./src/%.c
    @mkdir -p $(dir $@)
    @gcc -c $^ -o $@ $(compile_flags)

./lib/libmessage.so: $(lib_objs)
    @mkdir -p $(dir $@)
    @gcc -shared $^ -o $@

dynamic: ./lib/libmessage.so

hello:./objs/main.o
    @mkdir $(dir $@)
    @gcc -c $^ -o $@ $(linking_flags)

  
run:./hello
    @./$<

clean:
    @rm -rf ./lib ./objs hello
  
debug:
    @echo $(lib_srcs)
    @echo $(lib_objs)

.PHONY:clean debug dynamic run
```

## Cmake
### Cmake概述
CMake 是一个项目构建工具，并且是跨平台的。关于项目构建我们所熟知的还有Makefile（通过 make 命令进行项目的构建），大多是IDE软件都集成了make，比如：VS 的 nmake、linux 下的 GNU make、Qt 的 qmake等，如果自己动手写 makefile，会发现，makefile 通常依赖于当前的编译平台，而且编写 makefile 的工作量比较大，解决依赖关系时也容易出错。

而 CMake 恰好能解决上述问题， 其允许开发者指定整个工程的编译流程，在根据编译平台，自动生成本地化的Makefile和工程文件，最后用户只需make编译即可，所以可以把CMake看成一款自动生成 Makefile的工具，其编译流程如下图：
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/08/28/10-09-22-3ca40b1bf7c8a76263900ca437aa6026-20240828100922-39d1ea.png)

### Cmake的使用
CMake支持大写、小写、混合大小写的命令。如果在编写CMakeLists.txt文件时使用的工具有对应的命令提示，那么大小写随缘即可，不要太过在意。

#### 注释
CMake 使用 # 进行行注释，可以放在任何位置。
```CMAKE
# 这是一个 CMakeLists.txt 文件
cmake_minimum_required(VERSION 3.0.0)
```

#### 注释块
CMake 使用 #\[\[ ]] 形式进行块注释。
```CMAKE
#[[ 这是一个 CMakeLists.txt 文件。
这是一个 CMakeLists.txt 文件
这是一个 CMakeLists.txt 文件]]
cmake_minimum_required(VERSION 3.0.0)
```

#### 编译可执行文件
源文件头文件都在同一目录下
```CMAKE
cmake_minimum_required(VERSION 3.28)

project(hello)

add_executable(hello main.c message.c)
```
* cmake_minimum_required：指定使用的 cmake 的最低版本，可选，非必须，如果不加可能会有警告
* project：定义工程名称，并可指定工程的版本、工程描述、web主页地址、支持的语言（默认情况支持所有语言），如果不需要这些都是可以忽略的，只需要指定出工程名字即可。
```CMAKE
# PROJECT 指令的语法是：
project(<PROJECT-NAME> [<language-name>...])
project(<PROJECT-NAME>
       [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
       [DESCRIPTION <project-description-string>]
       [HOMEPAGE_URL <url-string>]
       [LANGUAGES <language-name>...])
```
* add_executable：定义工程会生成一个可执行程序
```CMAKE
add_executable(可执行程序名 源文件名称)
```
这里的可执行程序名和project中的项目名没有任何关系

源文件名可以是一个也可以是多个，如有多个可用空格或;间隔

然后执行CMake命令
```bash
# cmake 命令原型
$ cmake CMakeLists.txt文件所在路径
```

在对应的目录下生成了一个makefile文件，此时再执行make命令，就可以对项目进行构建得到所需的可执行程序了。

#### 变量
在cmake里定义变量需要使用`set`
```bash
# SET 指令的语法是：
# [] 中的参数为可选项, 如不需要可以不写
SET(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])
```

VAR：变量名
VALUE：变量值

```bash
set(SRC_LIST main.c message.c)
add_executable(app  ${SRC_LIST})
```


**宏变量**
* 指定使用的C/C++标准
```bash
#增加-std=c++11
set(CMAKE_CXX_STANDARD 11)
#增加-std=c14
set(CMAKE_C_STANDARD 11)
```

* 指定输出的路径
在CMake中指定可执行程序输出的路径，也对应一个宏，叫做`EXECUTABLE_OUTPUT_PATH`，它的值还是通过set命令进行设置:
```bash
set(HOME /home/)
set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin)
```
如果这个路径中的子目录不存在，会自动生成，无需自己手动创建
由于可执行程序是基于 cmake 命令生成的 makefile 文件然后再执行 make 命令得到的，所以如果此处指定可执行程序生成路径的时候使用的是相对路径 ./xxx/xxx，那么这个路径中的 ./ 对应的就是 makefile 文件所在的那个目录。

|            宏             |                   功能                    |
| :----------------------: | :-------------------------------------: |
|    PROJECT_SOURCE_DIR    |        使用cmake命令后紧跟的目录，一般是工程的根目录        |
|    PROJECT_BINARY_DIR    |              执行cmake命令的目录               |
| CMAKE_CURRENT_SOURCE_DIR |        当前处理的CMakeLists.txt所在的路径         |
| CMAKE_CURRENT_BINARY_DIR |               target 编译目录               |
|  EXECUTABLE_OUTPUT_PATH  |           重新定义目标二进制可执行文件的存放位置           |
|   LIBRARY_OUTPUT_PATH    |            重新定义目标链接库文件的存放位置             |
|       PROJECT_NAME       |          返回通过PROJECT指令定义的项目名称           |
|     CMAKE_BINARY_DIR     | 项目实际构建路径，假设在build目录进行的构建，那么得到的就是这个目录的路径 |


#### 搜索文件

源文件
 如果一个项目里边的源文件很多，在编写CMakeLists.txt文件的时候不可能将项目目录的各个文件一一罗列出来，这样太麻烦也不现实。所以，在CMake中为我们提供了搜索文件的命令，可以使用aux_source_directory命令或者file命令。

在 CMake 中使用`aux_source_directory` 命令可以查找某个路径下的所有源文件，命令格式为：
```bash
aux_source_directory(< dir > < variable >)
```
* dir：要搜索的目录
* variable：将从dir目录下搜索到的源文件列表存储到该变量中
```bash
cmake_minimum_required(VERSION 3.0)
project(CALC)
include_directories(${PROJECT_SOURCE_DIR}/include)
# 搜索 src 目录下的源文件
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src SRC_LIST)
add_executable(app  ${SRC_LIST})
```
`CMAKE_CURRENT_SOURCE_DIR` 宏表示当前访问的 CMakeLists.txt 文件所在的路径。

头文件
在编译项目源文件的时候，很多时候都需要将源文件对应的头文件路径指定出来，这样才能保证在编译过程中编译器能够找到这些头文件，并顺利通过编译。在CMake中设置要包含的目录也很简单，通过一个命令就可以搞定了，他就是`include_directories`:

```bash
include_directories(headpath)
```

```bash
cmake_minimum_required(VERSION 3.0)
project(CALC)
set(CMAKE_CXX_STANDARD 11)
set(HOME /home/robin/Linux/calc)
set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin/)
include_directories(${PROJECT_SOURCE_DIR}/include)
file(GLOB SRC_LIST ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
add_executable(app  ${SRC_LIST})
```

file命令
```bash
file(GLOB/GLOB_RECURSE 变量名 要搜索的文件路径和文件类型)
```
* `GLOB`: 将指定目录下搜索到的满足条件的所有文件名生成一个列表，并将其存储到变量中。
* `GLOB_RECURSE`：递归搜索指定目录，将搜索到的满足条件的文件名生成一个列表，并将其存储到变量中。
```bash
file(GLOB MAIN_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
file(GLOB MAIN_HEAD ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)
```
`CMAKE_CURRENT_SOURCE_DIR` 宏表示当前访问的 CMakeLists.txt 文件所在的路径。
**关于要搜索的文件路径和类型可加双引号，也可不加**


#### 生成和链接静态库
**生成静态库**
```bash
add_library(库名称 STATIC 源文件1 [源文件2] ...) 
```
在Linux中，静态库名字分为三部分：lib+库名字+.a，此处只需要指定出库的名字就可以了，另外两部分在生成该文件的时候会自动填充。
```bash
cmake_minimum_required(VERSION 3.28)
project(hello)

include_directories(${PROJECT_SOURCE_DIR}/include)
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC_LIST)
add_library(hello STATIC ${SRC_LIST})
```
**链接静态库**
在cmake中，链接静态库的命令如下：
```bash
link_libraries(<static lib> [<static lib>...])
```
用于设置全局链接库，这些库会链接到之后定义的所有目标上。

* 参数1：指定出要链接的静态库的名字
	可以是全名 libxxx.a
	也可以是掐头（lib）去尾（.a）之后的名字 xxx
* 参数2-N：要链接的其它静态库的名字

如果该静态库不是系统提供的（自己制作或者使用第三方提供的静态库）可能出现静态库找不到的情况，此时可以将静态库的路径也指定出来：
```bash
link_directories(<lib path>)
```

```bash
cmake_minimum_required(VERSION 3.28)
project(world)

include_directories(${PROJECT_SOURCE_DIR}/include)
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC_LIST)

# 包含静态库路径
link_directories(${PROJECT_SOURCE_DIR}/lib)
# 链接静态库
link_libraries(hello)
add_executable(world ${PROJECT_SOURCE_DIR}/src/main.c)
```


#### 生成并链接动态库
**生成动态库**
```bash
add_library(库名称 SHARED 源文件1 [源文件2] ...) 
```
在Linux中，动态库名字分为三部分：lib+库名字+.so，此处只需要指定出库的名字就可以了，另外两部分在生成该文件的时候会自动填充。

```bash
cmake_minimum_required(VERSION 3.28)
project(hello)

include_directories(./include)
aux_source_directory(./src SRC_LIST)
add_library(hello SHARED ${SRC_LIST})
```

**链接动态库**
```bash
target_link_libraries(
    <target> 
    <PRIVATE|PUBLIC|INTERFACE> <item>... 
    [<PRIVATE|PUBLIC|INTERFACE> <item>...]...)
```

用于指定一个目标（如可执行文件或库）在编译时需要链接哪些库。它支持指定库的名称、路径以及链接库的顺序。

* `target`：指定要加载的库的文件的名字

	该文件可能是一个源文件
	该文件可能是一个动态库/静态库文件
	该文件可能是一个可执行文件
	`PRIVATE|PUBLIC|INTERFACE`：动态库的访问权限，默认为`PUBLIC`
		`PUBLIC`：在public后面的库会被Link到前面的target中，并且里面的符号也会被导出，提供给第三方使用。
		`PRIVATE`：在private后面的库仅被link到前面的target中，并且终结掉，第三方不能感知你调了啥库
		`INTERFACE`：在interface后面引入的库不会被链接到前面的target中，只会导出符号。

如果各个动态库之间没有依赖关系，无需做任何设置，三者没有没有区别，一般无需指定，使用默认的 PUBLIC 即可。

动态库的链接具有传递性，如果动态库 A 链接了动态库B、C，动态库D链接了动态库A，此时动态库D相当于也链接了动态库B、C，并可以使用动态库B、C中定义的方法。

动态库的链接和静态库是完全不同的：

静态库会在生成可执行程序的链接阶段被打包到可执行程序中，所以可执行程序启动，静态库就被加载到内存中了。
动态库在生成可执行程序的链接阶段不会被打包到可执行程序中，当可执行程序被启动并且调用了动态库中的函数的时候，动态库才会被加载到内存

因此，在cmake中指定要链接的动态库的时候，应该将命令写到生成了可执行文件之后：
```bash
cmake_minimum_required(VERSION 3.28)
project(world)

include_directories(${PROJECT_SOURCE_DIR}/include)
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC_LIST)

add_executable(world ${PROJECT_SOURCE_DIR}/src/main.c)

target_link_libraries(world ${PROJECT_SOURCE_DIR}/lib/libhello.so)
```




#### 指定输出的路径
```bash
cmake_minimum_required(VERSION 3.28)
project(hello)

include_directories(./include)
aux_source_directory(./src SRC_LIST)

# 设置动态库生成路径
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
add_library(hello SHARED ${SRC_LIST})
```
对于这种方式来说，其实就是通过set命令给`LIBRARY_OUTPUT_PATH`宏设置了一个路径，这个路径就是可执行文件生成的路径。

