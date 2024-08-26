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
hello:main.c message.c
	gcc main.c message.c -o hello
```
保存退出，输入`make`命令
```bash
make
# 生成可执行文件hello
./hello
```