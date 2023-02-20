# Windows下使用hugo+github pages 创建自己的个人博客


## 一. 为什么要创建自己的个人博客网站	

> ​		我们在进行学习的时候，尤其是在学习新的知识的时候，要想牢固掌握，最好的办法就是将新学习的。
>
> ​		知识向别人讲述，但是这个倾听者不好找，学习写博客同样是一种讲述的办法，同时也是将自己的知识进行收纳整理的一个过程。
>

## 二. 为什么使用hugo+GitHub Pages

> 1. hugo是一款可以方便生成可展示静态网页的工具，方便了不擅长前端编码的同学
>
> 2. 在hugo下可以舒服地使用markdown进行博客文章页面的书写
>
> 3. 我们需要一个展示的平台——一个服务器来存放我们的文章，CSDN这类的国内平台真的是越用越气人，
>
>    而且不支持个性化定制，GithubPages技术已经很成熟，而且参考资料众多

## 三. 如何使用hugo+Github Pages 创建自己的博客

### （一）下载并安装hugo

#### 1. 确认自己的安装环境

​		我这里是windows11，使用的是powershell7作为部署工具，这里可以参考

​		https://gohugo.io/installation/windows/

​		我使用了chocolatey作为下载工具，

​		在powershell7的管理员模式下下安装chocolatey的命令如下

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

#### 2. 下载hugo的命令如下

```powershell
choco install hugo-extended
```



### （二）使用hugo创建自己的站点

#### 1. 使用powershell打开自己想要创建的站点的文件夹路径

```powershell
cd 自己想要创建的站点的文件夹路径
```

#### 2. 创建自己的站点

```powershell
hugo new_site
```

​	这时候你会发现文件夹下多了一个文件夹叫new_site, 这就是我们接下来要编辑的hugo项目

​	接下来进入我们的项目

```powershell
cd new_site
```

### （三）挑选自己喜欢的主题，以LoveIt为例

#### 1. 打开hugo的主题网站，网址如下

​	https://themes.gohugo.io/

#### 2. 我选择了LoveIt作为主题，网址如下

​	https://hugoloveit.com/

​	这个主题有详细的模块，而且提供了丰富的参考文档教你如何使用，并且支持中文

#### 3.下载LoveIt主题

​	在new_site的主目录下，运行

```powershell
git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

​	这样主题就会下载到themes文件夹下面

### （四）生成自己的第一篇文章

#### 1. 使用如下的命令创建自己的第一篇文章

```powershell
hugo new posts/first_post.md
```

​	**注意：**使用编辑器打开就会发下，使用这种方式创建的markdown文件，中draft参数是true，需要把他变成false，或者使用hugo -D / --buildDrafts serve 才能显示

#### 2. 在本地启动站点

```powershell
hugo serve
```

​	登录 

http://localhost:1313

​	就可以看到启动的网站



## （五）将站点部署到GitHub上

#### 1. 在你的GitHub账户中创建一个库，命名为 *github_user_name.github.io*

#### 2. 首先生成我们可以公开展示的文件夹，也就是需要上传到github上的文件夹public，执行

```powershell
hugo --theme=LoveIt --baseUrl="https://liangyi0506.github.io/" 
```

​	这样你会发下在网站的主目录下生成了一个public文件夹，这就是我们用来展示的文件夹，也是需要

上传到GitHub的文件夹

#### 3. 之后，执行基本的git命令将public下文件上传到我们刚刚创建的库中

```powershell
cd public 						
git init						 // 初始化本地仓库
git add .						 // 添加文件到本地
git commit -m "fist commit"		 // 添加文件描述信息
git remote add origin 远程仓库地址 // 链接远程仓库
git pull origin main			 // 把本地仓库的变化链接到远程仓库
git push -u origin main			 // 把本地仓库的文件推送到远程仓库
```

## （六）登录我们的博客地址

​	登录

​	[liangyi0506.github.io](我的博客网址)

​	即可, 这样任何人就都能访问你的网页

