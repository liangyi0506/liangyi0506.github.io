# Windows使用Navicat连接 WSL2中的MySQL服务




本文中使用的

MySQL版本是MySQL8.0

WSL发行版是Ubuntu-20.04

Windows版本是Windows11 22H2

---

#### 一. 在WSL-Ubuntu20.04中安装MySQL

1.更新Ubuntu包

```bash
sudo apt update
```

2.安装MySQL（默认是Mysql8.0）

```bash
sudo apt install mysql
```

3.启动MySQL服务器

```bash
sudo /etc/init.d/mysql start
```

​	或

```bash
sudo service mysql start
```

4.启动安全脚本提示符

```bash
sudo mysql_secure_installation
```

这一步很可能在设置密码的时候会发生下面的错误提示：

> Failed! Error: SET PASSWORD has no significance for user ‘root‘@‘localhost‘ as the authe

如果发生，可以采用下面的方式解决错误：

（1）先进入MySQL中

```bash
sudo mysql
```

（2）更改密码

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'mynewpassword';
```

（3）再次启动安全脚本提示符

```sql
sudo mysql_secure_installation
```

（4）输入刚才键入的密码，然后都选no即可

#### 二. 在Ubuntu中修改MySQL的配置

```bash
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

​	将下面两句进行注释

> ​	#bind-address = 127.0.0.1
>
> ​	#mysqlx-bind-address = 127.0.0.1

​	修改后文件如下：

```bash
[mysqld]
#
# * Basic Settings
#
user            = mysql
pid-file        = /var/run/mysqld/mysqld.pid
socket  = /var/run/mysqld/mysqld.sock
port            = 3306
datadir = /var/lib/mysql
 
 
# If MySQL is running as a replication slave, this should be
# changed. Ref https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_tmpdir
tmpdir          = /tmp
#
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
# bind-address           = 127.0.0.1
# mysqlx-bind-address    = 127.0.0.1
#
# * Fine Tuning
#
key_buffer_size         = 16M
max_allowed_packet      = 64M
thread_stack            = 256K
 
thread_cache_size       = -1
 
# This replaces the startup script and checks MyISAM tables if needed
# the first time they are touched
myisam-recover-options  = BACKUP
 
# max_connections        = 151
 
# table_open_cache       = 4000
```

#### 三. 登录、赋权、添加可以远程访问的用户

​	**登录**

```bash
# 登录 root账户
mysql -u root -p
```

​	**root用户没有SYSTEM_USER权限，进行赋权**

```sql
grant system_user on *.* to 'root';
```

​	**mysql8.0以后需要使用如下命令开启远程服务**

```sql
create user 'user_name'@'%' identified by 'your_password';
grant ALL on *.* to 'user_name'@'%';
alter user 'user_name'@'%' identified with mysql_native_password by 'your_password';
```

​	**刷新权限**

```sql
flush privileges;
```

​	**重启MySQL服务**

```bash
service mysql restart
```

#### 四. 查看WSL2发行版的IP

```bash
ifconfig
```

{{<figure src = "/images/network-ip.png" title = "network-ip">}}

#### 四. 使用navicat登录

​	和本地登录一样，只是要改IP为WSL的IP
{{<figure src = "/images/navicat-wsl-connect.png" title = "navicat-wsl-connect" >}}

​	点击测试连接，显示连接成功即可。



参考
---

[在windows下远程调用、连接（navicat）wsl2中 mysql 8.* 的详细步骤](https://blog.csdn.net/weixin_42580217/article/details/122583888)

[Win10连接WSL2-Ubuntu中的MySQL_Simon loves games-CSDN博客_连接wsl的mysql](https://blog.csdn.net/simongame/article/details/107800586)

[mysql you need (at least one of) the SYSTEM_USER privilege(s) for this operation - 赵先鑫 - 博客园](https://www.cnblogs.com/zhaoxianxin/p/13434416.html)

[Mysql 8.0.13 开启远程访问权限（ERROR 1064 (42000): You have an error in your SQL syntax; check the manual th）_GentleCP的博客-CSDN博客](https://blog.csdn.net/GentleCP/article/details/87936263)

