# Windows使用Navicat连接 WSL2中的MySQL服务




本文中使用的

MySQL版本是MySQL8.0

WSL发行版是Ubuntu-20.04

Windows版本是Windows11 22H2

---

#### 一. 在Ubuntu中修改MySQL的配置

```bash
vim /etc/mysql/mysql.conf.d/mysql.cnf
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

#### 二. 登录、赋权、添加可以远程访问的用户

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

#### 三. 查看WSL2发行版的IP

```bash
ifconfig
```

{{<figure src = "/images/network-ip.png" title = "network-ip">}}

#### 四. 使用navicat登录

​	和本地登录一样，只是要改IP为WSL的IP
{{<figure src = "/images/navicat-wsl-connect.png" title = "navicat-wsl-connect" >}}

​	点击测试连接，显示连接成功即可。

