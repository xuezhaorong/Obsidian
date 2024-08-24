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
sudo adduser username
sudo usermod -aG root username
```

* ## `is not in the sudoers file`
```bash
#切换到root用户 
su 
#编辑配置文件 
vim /etc/sudoers 
#增加配置, 在打开的配置文件中，找到root ALL=(ALL) ALL, 在下面添加一行 
#其中xxx是你要加入的用户名称 
xxx ALL=(ALL) ALL
```
另一种方法：
```bash
echo "%wheel ALL=(ALL) ALL" > /etc/sudoers.d/wheel
useradd -m -G wheel -s /bin/bash username
```