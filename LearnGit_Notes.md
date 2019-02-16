# 这个笔记主要记录一些日常使用的Git命令

## 1. 创建版本库

进入要创建版本库的文件夹里面，使用以下命令来初始化版本库，创建一个本地版本库。

    git init

## 2.添加到暂存区同时提交

    git add example.txt
    git commit -m <message>

## 3. 查看当前版本库的状态

    git status

## 4. 版本库里面的文件修改后查看改动的内容

    git diff                 #查看暂存区与工作区里面的区别
    git diff HEAD -- <file>  #查看工作区与版本库之间的区别

## 5.版本回退

    git reset --hard commit_id  #回退到 commit_id 的版本
    git reset --hard HEAD^      #回退到上一个版本
    git reset --hard HEAD^^     #回退到上上一个版本
    git reset --hard HARD~100   #回退到上面一百个版本

## 6.历史命令

    git log --pretty=oneline   #比较好看的形式
    git log --graph            #分支合并图
    git relog                  #历史命令

## 7.撤销修改

    git checkout -- file  #丢弃对工作区的修改
    git reset HEAD <file> #撤销对暂存区的修改

 `git checkout` 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”

## 8.删除文件

    git rm file

## 9. 关联远程版本库

    git remote add origin git@github.com:Name\repository.git
    git push -u origin master   #第一次推送，使用参数u将远程master分支与本地分支进行关联

### 日常推送：

    git push origin master

## 10.克隆版本库

    git clone git@github.com:Name\repository.git #ssh协议克隆代码库

## 11.分支基本操作

    查看分支：git branch

    创建分支：git branch <name>

    切换分支：git checkout <name>

    创建+切换分支：git checkout -b <name>

    合并某分支到当前分支：git merge <name>

    删除分支：git branch -d <name>

## 12.暂存工作现场

    git stash

### 恢复工作现场

    git stash pop
    
### 查看工作现场列表

    git stash list

## 13.分支的推送

    要查看远程库的信息，用 git remote
    显示更加详细的信息加参数 -v

    从本地推送分支，使用 git push origin branch-name
    若推送失败，用git pull抓取远程的新提交（已经关联了，但是别人提交了一个新版本）

    在本地创建和远程分支对应的分支，使用 git checkout -b branch-name origin/branch-name

    建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

## 14.标签管理

    git tag <name>            #给当前分支的最新commit打上标签
    git tag                   #查看标签
    git tag <name> commit_id  #给某一个commit打上标签
    git show <tag_name>       #查看某个标签的信息

    git tag -a <name> -m "description" commit_id  #带说明文字的标签

    git tag -d <tagname>      可以删除一个本地标签
    git push origin <tagname>  可以推送一个本地标签
    git push origin --tags     可以推送全部未推送过的本地标签
    git push origin :refs/tags/<tagname>    可以删除一个远程标签

## &emsp;&emsp;Git Cheat Sheet
![git](https://www.blog.caishenpc.top/git.jpg)