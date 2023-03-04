# Docker&Nginx&Tomcat企业综合实战案例

## 一. 从0开始构建Docker虚拟化平台

### 1.什么是虚拟化

> ​		将物理的资源转变为逻辑上可以管理调用的资源，以打破物理资源结构之间的壁垒，让计算的原件运行在虚拟的集成环境中，而不是直接运行在物理资源上

### 2.虚拟化技术的意义

> 1. 最大化利用硬件资源——可以在不同的环境中多部署项目。
> 2. 可以淘汰老旧服务器资源，对老旧服务器资源进行重组重用。
> 3. 加快对企业服务器资源进行调度管理，加快企业自动化运维

### 3.虚拟化技术的种类

	1. VMare ESXI
	1. XEN
	1. Open-vz
	1. Hyper-v
	1. KVM
	1. Docker

#### 4.Docker虚拟化技术的概念

> Docker 是目前企业中使用最多的虚拟化技术，其特点是：轻量级，安全，稳定，主要是为解决企业轻量级服务器操作系统和应用容器资源诞生的 

#### 5.Docker 要求部署Docker平台的Linux发行版的内核版本至少在3.8以上

#### 6.基于CentOS7.6.x Linux操作系统，通过yum 二进制方式构建Docker虚拟化平台，操作方法和部署指令如下：

（1）安装第三方扩展源

```bash
yum install epel-release -y 
```

（2）安装Docker软件包并检查是否部署成功, 下面两条命令都可以

```bash
yum install docker -y
```

```bash
rpm -qa|grep docker
```

（3）启动Docker服务

```bash
systemctl start docker.service
```

（4）查看Docker引擎服务进程

```bash
ps -ef|grep docker
```

（5）查看Docker版本信息

```bash
docker version
```



## 二.启动一台Nginx WEB服务容器

> 基于Docker虚拟化平台直接启动应用容器，启动一台Nginx WEB服务容器，默认会启动Nginx服务，同时监听80端口，用户通过历览器能够实现页面的访问

操作部署指令和方法如下：

（1）搜索Nginx镜像

```bash
docker search nginx # 搜索nginx镜像， offical一栏 显示 OK 的就是官方版本
```

（2）从Docker仓库中下载Nginx镜像

```bash
docker pull docker.io/nginx
```

（3）查看一下已经下载的Nginx镜像和列表

```bash
docker images	# 查看docker下面所有的镜像
```

```bash
docker images | grep -i nginx # 只查看nginx镜像
```

（4）通过nginx镜像启动nginx镜像

```bash
docker run -itd -p 80:80 nginx:latest
```

	1. run 表示创建一台全新容器
	1. -i 表示interactive交互模式
	1. -t tty代开终端
	1. -d detach后台运行
	1. -p publish发布端口，开启DNAT映射，将宿主机80（第一个）映射到容器80

（5）查看Nginx容器的运行状态和IP

```bash
docker ps # 查看
```

```bash
docker inspect Nginx_CONTAINER_ID |grep -i ipaddr|tail -1
```

​	通过浏览器访问nginx平台 IP地址:端口

{{<figure src = "/images/docker-nginx-success-begin.png" title = "docker-nginx-success-begin">}}

（6）进入nginx镜像中

```bash
docker exec -it Nginx_CONTAINER_ID /bin/bash
```

## 三.开启Tomcat容器和开启Nginx的方法基本一致，只需要将上述命令中的Nginx换成Tomcat即可
