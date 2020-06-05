---
layout: article
title: "Git cookbook"
sidebar:
  nav: Cookbook
aside:
  toc: true
tags:
  - cookbook
key: blog-2019-02-19-Git-cookbook
date: 2019-02-10
modify_date: 2019-05-02
---

This cookbook is built for quick and easy search for Git commands
<!--more-->


## Configuration

- SSH reset

``` shell
ssh-keygen -t rsa -C "youremail@example.com"

# -C: comment
# -t: type
```
- configure global/local user and email

``` shell
git config [--global] user.name "Your Name"
git config [--global] user.email "email@example.com"
```

- set ssh-key

``` shell
ssh-keygen -o
```
- add ssh_pub key

``` shell
vim ~/.ssh/authorized_keys
```

- list the content of config file

```
git config --list
```

- set the exectuable of files

```
git config core.filemode false  # local
git config --global core.fileMode false # global
```

- set editor from nano to vim

```
 git config --global core.editor "vim"
```

or append `editor = vim` in `core` of config file

## Elementary

### Three sectors

1. working tree: editing space

2. index file: cache after `add`

3. repository: storage after `commit`

### Basic pipeline

- initialization

```
git init  
```

- add file contents of working tree to index files

```
git add <file>
```

- record changes to the repository

```
git commit -m "<message>"
```

- add files to index files and record to the repo

```
git commit -am "<message>"
```

- check the status of the repository

```
git status
```

- show changes between commits, commit and working tree

```
# compare the working tree with index files
git diff
# compare the index files with HEAD repo (<commit_id> repo)
git diff --cached <commit_id>
# compare the two repos
git diff <commit_id> <commit_id>
```

- discard contents of working tree after editing

```
git checkout -- <file>
```

- discard files of index file after `add`

```
git reset HEAD file
```

- discard new commit before `push`

```
git reset --hard HEAD^
git reset --hard <commit_id>
```

- discard a commit after `push`

1. use `reset`

```
git reset --hard <commit_id>
...
git push --force
```

2. use `rebase` **TODO**

```
git rebase -i HEAD^2 # where 2 refers to the 2 most latest commit
# delete the most latest commit
git push --force
```

3. use `revert`

```
git revert <commit_id> # traceback to the history commit with a new commit
```

- tail the log

```
git log
git log --pretty=oneline --abbrev-commit --graph
```

- tail the command log

```
git reflog
```

- remove 

```
git rm <file>
git commit -m "xxxx"
```

- remove files of index files

**Note**: remove the cache after delete the directory if it has committed, or add .gitignore after commit the directory
```
git rm -r --cached $DIR
```

## Branch

### Basic 

- create branch

```
git branch <new_name>
```

- rename the branch

```
git branch -m [<old_name>] <new_name> # omitted old_name will signify the current branch
```

- switch branch

```
git checkout <branch_name>
```

- switch to the created branch

```
git checkout -b <branch_name>
```

### Merge

- merge branch to the current branch

```
git merge <branch_name>
```


- delete local branch

```
git branch -d <branch_name>
```

- delete branch by force before merge

```
git branch -D <branch_name>
```

- delete remote branch

```
git push origin --delete <branch_name>
```

- check the relation between local branch and remote branch

```
git branch -vv 
```

### Fetch remote branch

- change current branch to specified branch after clone

1. use `clone`

```
git clone -b <remote_branch_name> <repo_link>
```

2. use `checkout` after initialization

[reference link](https://www.jianshu.com/p/856ce249ed78)

```
git init
git remote add origin <repo_link>
git checkout -b <new_name> <origin/remote_name>
```

## Advanced

### Modify the commit

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



# 创建远程仓库


## 添加远程库

- 关联远程仓库：首次在Github上创建repository
```
git remote add origin git@server-name:path/repo-name.git
```

- 删除关联远程仓库

```
git remote rm origin
```

- 首次 push 到远程仓库
```
 git push -u origin master 
```

- 非首次推送到远程仓库
```
git push origin master 
```

- 从远程仓库克隆
```
git clone <gitURL>
```

# Branch

## 创建与合并分支

- 查看分支
```
git branch
```

- 创建分支
```
git branch <branch_name>
```




## 解决冲突

- 禁止使用 fast forward merge 方式：当master 与 branch分别更改无法快速合并时，用普通模式合并，合并后的历史有分支，能看出来曾经做过合并
```
git merge --no-ff -m "xxx"  <branch name>
```
![new_branch](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0)

- git 查看分支情况
```
git log --graph --pretty=oneline --abbrev-commit
```
解决后：

![new_branch](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0)

## Bug分支
- 隐藏现场及回复现场
```
git stash #保存现场
git stash apply  #保存现场
git stash drop  #删除现场
git stash pop #恢复并删除现场
git stash list #查看现场
```

## 多人协作
```
git remote #查看远程库信息
git remote -v #查看远程库信息详细
git push origin master #推送本地 master 分支
git checkout -b dev origin/dev #创建本地 dev 并关联远程 dev 分支
git branch --set-upstream branch-name origin/branch-name #建立本地分支与远程分支得关联
git pull #抓取远程分支
```

- 创建标签
```
git tag <branch_name>  #创建标签
git tag #查看所有标签
git tag v0.9 6224937 #对某一次 commit 打标签
git show <tagname> #查看标签信息
git tag -a v0.1 -m "version 0.1 released" 3628164 #创建有说明的标签
```
- 操作标签
```
git tag -d v0.1 #删除标签
git push origin <tagname> #推送标签到远程
git push origin --tags #推送本地所有未推送到远程的标签
git push origin :refs/tags/<tagname> #删除远程标签
```
- 自定义git
```
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
```
$ ssh-keygen -p
当提升你输入新的密码的时候，按enter就可以啦，继续确认enter就可以
当然上面还有别的办法自己也可以试一下。
```

- git rm与git rm --cached的区别
```
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
```
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