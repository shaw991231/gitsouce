## Git

>写在前面，由于之前经历丢失源码的惨痛经历，因此有了这篇文章，这不是入门教程，只是学习笔记。入门推荐看廖雪峰老师的教程：https://www.liaoxuefeng.com/wiki/896043488029600

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
    $ git add <file>  #  从工作区添加文件到暂存区
    ```
   
- 提交

    ```bash
    $ git commit -m "message"  # 从暂存区提交文件到版本库
    ```
     
- 状态

    ```bash
    $ git status  # 显示哪些文件已被staged、未被staged以及未追踪(untracked)
    $ git log  # 显示commit历史
    $ git reflog  # 显示本地repo的所有commit日志
    ```

- 对比 
        
    ```bash
    $ git diff <file>  # 比较工作区和暂存区文件的修改
    $ git diff HEAD  # 比较工作区和上一次commit后的修改
    $ git diff --cached  # 比较暂存区和上一次commit后的修改
    $ git diff HEAD --<file>  # 比较工作区和上一次commit后的文件的修改
     ```

- 撤销(回退)
        
    ```bash
    $ git reset HEAD^  # 版本回退
    $ git checkout -- <file>  # 工作区修改撤销
    $ git rest HEAD <file>  # 暂存区修改撤销
    ```
    
    `HEAD^`是前一个版本，再前一个则`HEAD^^`
    
- 版本库删除与移动
        
    ```bash
    $ git rm <file>
    $ git mv <file>
    ```

##### 分支管理

- 创建分支并切换
    
    ```bash
    $ git switch -c <branch>
    $ git checkout -b <branch>
    ```

- 查看分支
    
    ```bash
    $ git branch  # 查看所有分支
    ```

- 切换分支
   
   ```bash
    $ git checkout <branch>
    $ git switch <branch>
    ```

- 合并某分支到当前分支
    
    ```bash
    $ git merge <branch>
    $ git merge --no-ff -m "message" <branch> # `--no-ff `这个参数是禁用Fast forward模式进行合并，会在merge时生成一个新的commit，在分支历史上留下信息，方便以后查看
    ```

    在合并分支有时会遇到合并冲突，可以进行以下步骤：
    1.解决冲突，把git合并失败的文件手动编辑为最后确定的内容。
    1. 再进行提交，合并完成。
    2. `git log --graph`查看分支合并图

- 删除分支
    
    ```bash
    $ git branch -d <branch>
    $ git branch -D <branch>  #将未合并的分支强制删除
    ```

- 处理bug
    
    ```bash
    $ git stash  #将当前工作环境暂时“存储”起来，等修复bug后再恢复
    $ git stash list  #查看“存储”的工作环境
    $ git stash apply  #将工作环境回复，但不删除工作环境
    $ git stash drop  #删除工作环境
    $ git stash pop  #恢复工作环境的同时并删除
    $ git cherry-pick <commit> #将指定的commit应用于其他分支
    ```

#### 远程仓库

##### 查看远程仓库

```bash
$ git remote
$ git remote -v  # 更详细的信息
```

##### 克隆远程仓库

```bash
$ git clone git@server-name:path/repo-name.git
```

##### 添加远程仓库

```bash
$ git remote add origin git@server-name:path/repo-name.git
```

##### 本地建立与远程分支对应的分支

```bash
$ git checkout -b <branch> origin/branch
```

##### 建立本地分支和远程分支的关联

```bash
$ git branch --set-upstream <branch> origin/branch
```

##### 推送分支

```bash
$ git push -u origin master  # 第一次推送，加-u参数，本地与远程的master分支关联起来，origin是习惯命名
$ git push origin <branch>
```

##### 提取远程仓库

```bash
$ git fetch origin/branch
$ git merge origin/branch
```
##### 抓取分支

```bash
$ git pull <remote> <banch>
```
##### 整理提交（commit）历史

```bash
$ git rebase  # 可以将本地未push的分叉提交历史整理成直线
```



#### 标签管理

##### 创建标签

```bash
$ git tag <tag>
$ git tag -a <tag> -m "message" <commit>
```

##### 查看标签

```bash
$ git tag
```

##### 删除标签

```bash
$ git tag -d <tag>
$ git push origin :refs/tags/<tag>  # 删除远程标签：先删除本地再删除远程
```

##### 推送本地标签到远程

```bash
$ git push origin <tag>
$ git push origin --tags  # 一次性推送全部未推送到远程的本地标签
```

#### 自定义

##### 忽略特殊文件

###### 配置.gitignore文件

配置文件的原则为：
1. 忽略系统自动生成的文件、如缩略图等
2. 忽略编译生成的中间文件、执行文件等，如Java编译产生的`.class`文件
3. 忽略你自己带有敏感信息的配置文件，如存放口令的配置文件

###### GitHub准备的各种配置文件

如果生成的无用文件过多，我们不必自己一个一个写需要忽略的文件，在GitHub上有准备好的各种配置文件：https://github.com/github/gitignore

```bash
$ git add -f <file>  # 强制添加被.gitignore忽略的文件
$ git check-ignore -v <file>  #  检查.ignore规则
```

##### 配置别名

```bash
$ git config --global alias.st status  # 将status配置为st，git st 相当于 git status
```

配置文件存放在`./git/config`内

##### 搭建Git服务器

首先要有一台Linux的机器，而且有`sudo`权限的账号。
首先，安装`git`:

```bash
$ sudo apt-get install git
$ sudo adduser git  # 创建git用户，来运行git服务
```
然后收集证书，将需要登录的用户的公钥，就是`id_rsa.pua`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件内，一行一个。
初始化Git仓库，选定一个目录下，如：`/var/demo.git`，在`/var`目录下输入：

```bash
$ sudo git init --bare demo.git
```

此时，这个目录下就是一个空仓库，没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以`.git`结尾。然后，把owner改为`git`：

```bash
$ sudo chown -R git:git demo.git
```
最后，克隆远程仓库：

```bash
$ git clone git@server:/var/demo.git
Cloning into 'demo'...
warning: You appear to have cloned an empty repository.
```

---

参考：
https://www.runoob.com/git/git-tutorial.html
https://www.liaoxuefeng.com/wiki/896043488029600