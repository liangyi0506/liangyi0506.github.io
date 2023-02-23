# github使用教程

## 一. 什么是GitHub

> ​	github是一个基于git的代码托管平台，付费用户可以建私人仓库，我们一般的免费用户只能使用公共仓库，也就是代码要公开。

详细的参考文档，可以查阅：[易百教程的文档](https://www.yiibai.com/git/)

## 二. 注册GitHub账户，以及创建仓库

1. #### 注册GitHub账户

   登录GitHub的官网地址：(https://github.com/)

2. #### 创建远程仓库

   Create a New Repository，填好名称后Create，之后会出现一些仓库的配置信息

3. #### 本地GitHub安装

   下载 git的Windows版本 [Git for Windows](https://gitforwindows.org/)

   之后，安装即可

4. #### 本地仓库创建

   ##### （1）首先在本地创建ssh key

   ```bash
   ssh-keygen -t rsa -C "your_email@youremail.com"
   ```

   ​	your_email@youremail.com 是你在注册GitHub时所用的邮箱，之后会要求确认路径和输入

   密码，如果我们按照默认选项的话，会生成一个id_rsa.pub

   ##### （2）复制id_rsa.pub中的内容到GitHub中 SSH Keys中

   ​	github上，进入 Account Settings（账户配置），左边选择SSH Keys，Add SSH Key,title随便

   填，粘贴在你电脑上生成的key。

   ​	{{<figure src="/images/GitHub-sshkeys.png" title="git三部分">}}

   ​	验证是否成功：

   ```bash
   ssh -T git@github.com
   ```

   ​	如果是第一次的会提示是否continue，输入yes就会看到：You've successfully authenticated, 

   but GitHub does not provide shell access 。这就表示已成功连上github。

   ##### （3）设置username和email

   ```bash
   git config --global user.name "your name"
   git config --global user.email "your_email@youremail.com"
   git config --global --list # 查看是否设置成功
   ```

   ##### （4）本地设置仓库

   > ​	在本地设置仓库有两种办法：
   >
   > ​			一种是将已有的仓库克隆下来；
   >
   > ​			一种是将现有的本地仓库链接并上传

   **克隆已有的仓库**：

   ​	如果是克隆本地的仓库

   ```bash
   git clone /path/to/repository 
   ```

   ​	如果是远程服务器的仓库

   ```bash
   git clone username@host:/path/to/repository
   ```

   **上传现有的本地仓库**：

   ​	如果现在的文件夹还不是git，需要在要上传的文件下执行：

   ```bash
   git init
   ```

   ​	然后，链接到某个远程服务器

   ```bash
   git remote add origin <server>
   ```
	

   

## 三. GitHub工作流

> ​	git的本地维护由三部分组成：第一个是你的**工作目录**——实际持有文件；第二个是**缓存区(Index)**——临时保存改动；最后是**HEAD**——指向最后一次提交的结果

{{<figure src="/images/GitHub-trees.png" title="git三部分">}}
1. #### 将更改保存至缓存区

   ```bash
   git add file_name # 提交单个文件的修改
   ```

   或者

   ```bash
   git add . # 提交所有文件的修改
   ```

2. #### 提交修改到HEAD

   ```bash
   git commit -m "代码提交信息"
   ```

   **注意**：此时，改动已经提交到本地的HEAD，但是还没有同步到远程仓库

3. #### 推送改动到远程仓库

   如果此时还没有克隆现有仓库，需要链接某个远程服务器：

   ```bash
   git remote add origin <server> # 例子：git remote add origin git@github.com:liangyi0506/liangyi0506.github.io.git
   ```

   现在，可以将修改推送到远程仓库了：

   ```bash
   git push -u origin main # git push -u <指定远程主机名> <本地分支名>:<远程分支名>
   ```

   可以把main换成任何你想要推送的任何分支

   **注意**：如果不修改 push 后面的参数的话，以后不需要重复添加这些参数

   **参数**：

   ​	-u ：指定默认主机，这样后面就可以不加任何参数使用`git push`
   # 


