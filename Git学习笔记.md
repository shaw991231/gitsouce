## Git
>写在前面，由于之前学git的时候没学清楚，平时没怎么用，在某一次博客源码未进行备份的时候丢失了最新版本，因此有了这篇文章，这不是入门教程，只是自己重新回顾的时候记录的文章。
[toc]
### Git简介
Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

### Git与SVN的区别
1、Git 是分布式的，SVN 不是：这是 Git 和其它非分布式的版本控制系统，例如 SVN，CVS 等，最核心的区别。
2、Git 把内容按元数据方式存储，而 SVN 是按文件：所有的资源控制系统都是把文件的元信息隐藏在一个类似 .svn、.cvs 等的文件夹里。
3、Git 分支和 SVN 的分支不同：分支在 SVN 中一点都不特别，其实它就是版本库中的另外一个目录。
4、Git 没有一个全局的版本号，而 SVN 有：目前为止这是跟 SVN 相比 Git 缺少的最大的一个特征。
5、Git 的内容完整性要优于 SVN：Git 的内容存储使用的是 SHA-1 哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。

![img](https://raw.githubusercontent.com/shaw991231/img/master/PICimg/git-process.png)

### Git 工作区、暂存区和版本库

![img](https://raw.githubusercontent.com/shaw991231/img/master/PICimg/1352126739_7909.jpg)

我们先来理解下 Git 工作区、暂存区和版本库概念：
- **工作区**：就是你在电脑里能看到的目录。
- **暂存区**：英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库**：工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。

### 安装过程

#### Windows
直接上Git官网进行下载：https://gitforwindows.org/
若是官网下载太慢，可以使用国内的镜像进行下载：https://npm.taobao.org/mirrors/git-for-windows/

#### Mac
一是已经安装了homebrew，直接使用`brew install git`
二是已经安装了Xcode，则无需下载，Xcode集成了Git
三是上官网下载：http://sourceforge.net/projects/git-osx-installer/

#### Linux

##### Debian/Ubuntu
```bash
$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev

$ apt-get install git

$ git --version
git version 2.27.0
```

##### Centos/RedHat
```bash
$ yum install curl-devel expat-devel gettext-devel \
 openssl-devel zlib-devel

$ yum -y install git-core

$ git --version
git version 2.27.0
```

##### 源码安装
在官网指定相应的系统进行下载：
安装指定系统的依赖包
```bash
$ tar -zxf git-2.27.0.tar.gz
$ cd git-2.27.0
$ make prefix=/usr/local all
$ sudo make prefix=/usr/local install
```

#### Git配置
Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

- `/etc/gitconfig` 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。 
- `~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。
- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

##### 用户信息
配置个人的用户名称和电子邮箱地址：
```bash
$ git config --global user.name "shaw"
$ git config --global user.email shaw@exmple.com
```
##### 查看配置信息
要检查已有的配置信息，可以使用如下命令：
```bash
$ git config --list
user.name=shaw
user.email=shaw@exmple.com
...
```
有时候会看到重复的变量名，那就说明它们来自不同的配置文件（比如 `/etc/gitconfig` 和 `~/.gitconfig`），不过最终 Git 实际采用的是最后一个。
这些配置我们也可以在 `~/.gitconfig` 或 `/etc/gitconfig` 看到，如下所示：
```bash
$ cat ~/.gitconfig
user.name=shaw
user.email=shaw@exmple.com
```
### 工作流程
![](https://raw.githubusercontent.com/shaw991231/img/master/PICimg/git.png)
1. 克隆Git资源作为工作目录
2. 在克隆的资源上修改或添加
3. 如果其他人修改了，可以更新资源
4. 在提交前查看修改
5. 提交修改
6. 在修改完成后，如果发现错误，可以撤回再次修改并提交

### 基本操作

#### 基本命令
##### 用户配置
    ```bash
    $ git config --global user.name "Your Name"
    $ git config --global user.email "email@exmple.com"
    ```
    因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。
##### 创建仓库
    ```bash
    $ git init <directory>
    ```
    创建版本库，可以理解为一个目录，这个目录所有文件都被Git管理，包括修改、删除都能追踪，也可以将历史“还原”。
##### 常用命令
- 添加
        ```bash
        $ git add <file>
        ```
        从工作区添加文件到暂存区
- 提交
        ```bash
        $ git commit -m "message"
        ```
        从暂存区提交文件到版本库
- 状态
        ```bash
        $ git status
        $ git log
        $ git reflog
        ```
        `git status`显示哪些文件已被staged、未被staged以及未追踪(untracked)
        `git log`显示commit历史
        `git reflog`显示本地repo的所有commit日志
- 对比 
        ```bash
        $ git diff <file>
        $ git diff HEAD
        $ git diff --cached
        $ git diff HEAD --<file>
        ```
        `git diff <file>` 比较工作区和暂存区文件的修改
        `git diff HEAD`比较工作区和上一次commit后的修改
        `git diff --cached`比较暂存区和上一次commit后的修改
        `git diff HEAD --<file>`比较工作区和上一次commit后的文件的修改
- 撤销(回退)
        ```bash
        $ git reset HEAD^
        $ git checkout -- <file>
        $ git rest HEAD <file>
        ```
        `git reset HEAD^`版本回退，`HEAD^`是前一个版本，再前一个则`HEAD^^`
        `git checkout -- <file>`工作区修改撤销
        `git rest HEAD <file>`暂存区修改撤销
- 版本库删除与移动
        ```bash
        $ git rm <file>
        $ git mv <file>
        ```
        这个就不说明了。

##### 分支管理
- 创建分支并切换
    ```bash

    ```

- 查看分支
    ```bash
    
    ```

- 切换分支
    ```bash
    
    ```

- 合并某分支到当前分支
    ```bash
    
    ```

- 删除分支
    ```bash
    
    ```

- 处理bug
    ```bash
    
    ```

#### 远程仓库


#### 标签管理


#### 自定义