---
title: 'Git cookbook'
date: 2019-02-10
modified: 2019-05-02
permalink: /posts/2019/02/Git-cookbook/
tags:
  - cookbook
excerpt_separator: <!--more-->
---

This cookbook is built for quick and easy search for Git commands
<!--more-->


# Git

## Git configuration

- SSH reset

```git
ssh-keygen -t rsa -C "youremail@example.com"

# -C: comment
# -t: type
```
- configure global user and email

```git
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
- configure local user and email

```git
git config user.name "Your Name"
git config user.email "email@xx.com"
```

- set ssh-key
```git
ssh-keygen -o
```
- add ssh_pub key
```
vim ~/.ssh/authorized_keys
```

- 查看配置文件内容
```git
git config --list
```

- 文件权限变更不敏感
```git
git config core.filemode false  # 当前版本库
git config --global core.fileMode false # 所有版本库
```

## Elementary

- initialization
```git
git init  
```

- add file contents in working tree
```git
git add <file>
```

- record changes to the repository
```git
git commit -m "xxx"
```

- 提交本地工作目录下所有修改，而不需要先 git add,相当于：git add 和 git commit

```git
git commit -am "message"
```

- modify the commit log

Reference [here](https://blog.csdn.net/sodaslay/article/details/72948722)
```
git commit --amend # to modify the
git push -f origin master # to push by force
```

- modify the commit date
```
git commit --amend --date="$WEEK, $DAY $MONTH $YEAR $HOUR:$MINUTE:$SECOND +0800"
```
where +0800 refers to Beijing time region

- check the status of the repository
```git
git status
```

- show changes between commits, commit and working tree
```git
git diff <file>
```

- 评论区
```git
git diff   #是工作区(work dict)和暂存区(stage)的比较
git diff --cached    #是暂存区(stage)和分支(master)的比较
git diff HEAD  #查看工作区和版本库里面最新版本的区别
```
- 查看提交日志
```
git log
git log --pretty=oneline
```

- 版本回退到上一个版本
```git
git reset --hard HEAD^
git reset --hard 3628164
```

- 查看命令日志
```git
git reflog
```
- 丢弃工作区修改
```git
git checkout -- file
```
- 撤销暂存区修改
```git
git reset HEAD file
```

- 删除暂存区文件
```git
git rm <file>
git commit -m "xxxx"
```

**Note**: remove the cache after delete the directory if it has committed, or add .gitignore after commit the directory
```
git rm -r --cached $DIR
```

- 从暂存区恢复删除文件
```git
git checkout --<file>
```

# 创建远程仓库


## 添加远程库

- 关联远程仓库：首次在Github上创建repository
```git
git remote add origin git@server-name:path/repo-name.git
```

- 删除关联远程仓库

```git
git remote rm origin
```

- 首次 push 到远程仓库
```git
 git push -u origin master 
```

- 非首次推送到远程仓库
```git
git push origin master 
```

- 从远程仓库克隆
```git
git clone <gitURL>
```

# Branch

## 创建与合并分支

- 查看分支
```git
git branch
```

- 创建分支
```git
git branch <name>
```

- 切换分支
```git
git checkout <name>
```

- 创建并切换分支
```git
git checkout -b <name>
```
![new_branch](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)
![new_branch](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)

- 合并某分支到当前分支

```git
git merge <name>
```
![new_branch](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)


- delete local branch

```git
git branch -d <name>
```
![new_branch](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908867187c83ca970bf0f46efa19badad99c40235000/0)

- delete branch by force, that is to delete before merge

```git
git branch -D <name>
```

- delete remote branch

```git
git push origin --delete <name>
```

- check the relation between local branch and remote branch

```git
git branch -vv 
```

- rename the branch

```git
git branch -m <oldName> <newName>
```

- change current branch to specified branch after clone

```git
git checkout -b <newName> <origin/remoteName>
```


## 解决冲突

- 禁止使用 fast forward merge 方式：当master 与 branch分别更改无法快速合并时，用普通模式合并，合并后的历史有分支，能看出来曾经做过合并
```git
git merge --no-ff -m "xxx"  <branch name>
```
![new_branch](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0)

- git 查看分支情况
```git
git log --graph --pretty=oneline --abbrev-commit
```
解决后：

![new_branch](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0)

## Bug分支
- 隐藏现场及回复现场
```git
git stash #保存现场
git stash apply  #保存现场
git stash drop  #删除现场
git stash pop #恢复并删除现场
git stash list #查看现场
```

## 多人协作
```git
git remote #查看远程库信息
git remote -v #查看远程库信息详细
git push origin master #推送本地 master 分支
git checkout -b dev origin/dev #创建本地 dev 并关联远程 dev 分支
git branch --set-upstream branch-name origin/branch-name #建立本地分支与远程分支得关联
git pull #抓取远程分支
```

- 创建标签
```git
git tag <name>  #创建标签
git tag #查看所有标签
git tag v0.9 6224937 #对某一次 commit 打标签
git show <tagname> #查看标签信息
git tag -a v0.1 -m "version 0.1 released" 3628164 #创建有说明的标签
```
- 操作标签
```git
git tag -d v0.1 #删除标签
git push origin <tagname> #推送标签到远程
git push origin --tags #推送本地所有未推送到远程的标签
git push origin :refs/tags/<tagname> #删除远程标签
```
- 自定义git
```git
git config --global color.ui true #配置颜色开启
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
git config --global alias.last 'log -1'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global core.quotepath false # 设置显示中文文件名
```

- 清楚ssh密码
```git
$ ssh-keygen -p
当提升你输入新的密码的时候，按enter就可以啦，继续确认enter就可以
当然上面还有别的办法自己也可以试一下。
```

- git rm与git rm --cached的区别
```git
当我们需要删除暂存区或分支上的文件, 同时工作区也不需要这个文件了, 可以使用
git rm file_path
git commit -m 'delete somefile'
git push
当我们需要删除暂存区或分支上的文件, 但本地又需要使用, 只是不希望这个文件被版本控制, 可以使用
git rm --cached file_path
git commit -m 'delete remote somefile'
git push
```

# git 命令行删除远程分支
```git
git branch -r -d origin/branch-name
git push origin :branch-name
```

# Git默认路径修改
在/etc/profile中最后加入
```
# set Project Path
proj="G:\Github" 
cd $proj
```


# Git 仓库清空

[link](http://www.mamicode.com/info-detail-2158130.html)

# Git 删除历史内容
首先找出git中前五大的文件： 
``` gitbash
git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -g | tail -5
```
第一行的字母其实相当于文件的id,用以下命令可以找出id 对应的文件名
``` gitbash
git rev-list --objects --all | grep 8f10eff91bb6aa2de1f5d096ee2e1687b0eab007
```
删除匹配*.db的所有文件
``` gitbash
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch *.db' --prune-empty --tag-name-filter cat -- --all
```

删除操作
``` gitbash
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch <FILL YOUR FILE NAME OR ID HERE>' --prune-empty --tag-name-filter cat -- --all
```

收回空间
``` gitbash
rm -rf .git/refs/original/ 
git reflog expire --expire=now --all
git gc --aggressive --prune=now
```

强行push
``` gitbash
git push -f origin master
```
[](https://www.cnblogs.com/lout/p/6111739.html)
[](https://blog.csdn.net/u010295496/article/details/72862671)