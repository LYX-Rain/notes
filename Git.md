# Git使用笔记

## 1、安装

在Windows上安装Git
从[官网](https://git-for-windows.github.io)下载，然后按默认选项安装即可。
安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功

## 2、基本配置

安装完成后，还需要最后一步设置，在命令行输入：

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

## 3、创建版本库

版本库又名仓库(repository)，可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
通过git init命令把一个目录变成Git可以管理的仓库：

```
$ git init
```

可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。
如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。

## 4、使用

### 第一步，用命令git add告诉Git，把文件添加到仓库：

```
$ git add readme.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

### 第二步，用命令git commit告诉Git，把文件提交到仓库：

```
$ git commit -m "wrote a readme file"
```

git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

## git常用命令：

### 创建

```bash
git init                    #初始化git仓库
git status                  #查看git仓库状态
git diff <filename>         #查看文件修改过的地方
git add <filename>          #添加文件到暂存区
git commit -m "blablabl"    #提交文件到版本库
git commit --amend          #对上一次的提交做修改
```

### 远程库操作

```bash
git remote -v                                               #查看远程仓库
git remote add origin git@server-name:path/repo-name.git    #关联远程库(github已创建)
git push -u origin master                                   #关联后第一次推送
git push origin master                                      #平常推送
git pull [remoteName] [localBranchName]                     #拉取远程仓库
git clone git@server-name:path/repo-name.git                #克隆仓库
```

### 版本回退和撤销修改

```bash
git log                         #查看历史版本
git log --pretty=oneline        #查看简要历史版本(可以查看更多版本) git reset --hard HEAD^ 版本回退(上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100)
git reset --hard [commit id]    #版本回退第二种命令， 用commit 的id 来指定版本回退
git reflog                      #查看命令历史, 可查看每次的命令操作 id 来进行历史操作的任意跳转(用到上面的命令)
git checkout -- <filename>      #撤销文件在工作区的修改
```

### 标签

```bash
git tag                                 #查看当前所有标签
git tag -n                              #查看所有标签详情
git tag -a <tagname> -m "blablabla..."  #添加新标签
git tag -d <tagname>                    #删除标签
git push origin :refs/tags/<tagname>    #删除远程指定标签
git push origin --tags                  #推送所有标签到远程库
```

### 分支

```bash
git branch                              #查看当前所有分支
git branch <branchname>                 #创建分支
git branch -d <branchname>              #删除分支
git checkout <branchname>               #切换分支
git checkout -b <branchname>            #创建分支并切换到新创建的分支
git merge <branchname>合并<branchname>  #分支到当前分支
git log --graph                         #查看分支的合并情况
```