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

### 通配符
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