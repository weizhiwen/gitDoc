## linux笔记（Ubuntu）

### 1.ubuntu环境下的切换操作
- 图形桌面->命令行模式：Crrl + Alt + F1/F2/F3/F4/F5/F6
- 命令行模式->图形模式：Ctrl + Alt + F7
- 解除命令行模式锁定光标快捷键：Ctrl + Alt

### 2.修复vi下无插入和命令行模式的提示
- 修改etc/vim/vimrc.tiny 文件  
这个文件默认是只读模式（Read-only），需要用root账户来修改。
	- 设置root的密码  
	`sudo passwd root //修改root密码 `
	系统会提示你输入当前用户的密码，紧接着输入新的root密码，重复输入确认密码，执行完上面的操作root就配置好了
	- 切换root账户  
	`su root` 或者`sudo su`都可以 
	系统会提示输入root账户的密码
	- 打开vimrc.tiny文件
	将`set compatible` 设置成`set nocompatible`，保存退出即可。
	- 退出root账户
	可以输入命令`su 用户名`或者`exit`退出,或者直接按Ctrl + d键
### 3.关机命令
- shutdown -h now 立刻进行关机
- shutdown -r now 现在重新启动计算机
- reboot 现在重新启动计算机  
用户登录：登录时尽量少用root账户登录，因为它是系统管理员，有最大的权限，尽量避免操作失误。可以利用普通用户登录，登录后在用”su -“命令来切换成系统管理员身份  
用户注销：在提示符下输入logout即可

### 3.linux下的目录说明
- 根目录 / linux文件目录是层级的树形结构
	- root 存放root超级管理员的相关文件
	- home 存放普通用户的相关文件或者是FTP站点的目录
	- bin 存放常用命令的目录
	- sbin 要有一定权限才能使用的命令。如系统启动时需要的命令
	- mnt 默认挂载光驱和软驱的目录
	- boot 存放操作系统启动时引导的相关文件
	- etc 存放系统配置和管理的相关文件
	- var 存放经常变化的文件，如socket
	- dev 接口设备文件目录，如had表示硬盘
	- tmp 用来存放暂存盘的目录
	- usr 软件默认安装的目录  
`pwd`命令可以用来显示当前在哪个目录下

### 4.liunx的用户管理
注意：用户的管理是要有root权限的
- 1.添加用户命令 
	`useradd 用户名`
- 2.设置密码
	`passwd 用户名`
- 3.删除用户 
	`userdel 用户名`
- 4.删除用户以及用户的主目录 
	`userdel -r 用户名`