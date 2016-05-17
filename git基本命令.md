

[TOC]

## 基本操作

### 创建git库

```markdown
$ git init
```

### 添加文件

```markdown
$ git add readme.txt
```

### 提交仓库

```
$ git commit -m 'wrote a readme file'
```

### 查看状态

查看本地工作区文件变动

```
$ git status
```

查看工作区与暂存区的差异

```
$ git diff
```

查看暂存区与仓库的差异

```
git diff --cached
```

查看工作区域仓库的差异

```
git diff HEAD -- filename
```

### 查看历史记录

```
$ git log
```

增强型查看

```
$ git lg
```

查看命令记录

```
git reflog
```

### 回退

当前版本为`HEAD`,

回退至上一版本

```
git reset --hard HEAD^
```

回退至上10个版本

```
git reset --hard HEAD~10
```

回退至版本号**3628164**

```
git reset --hard 3628164
```

### 撤销修改

* 修改未提交至缓存区(add)时，把工作区撤销回到暂存区的状态

```
git checkout -- filename
```

* 当已经提交到暂存区(add)，还未提交到版本库(commit)时，使用

```
git reset HEAD fielname
```

来把暂存区的修改撤销，此时工作区的修改仍在，使用第一条命令再把工作区撤销回暂存区的状态。

* 如果已经提交到版本库(commit)，可以采用回退至某一版本来撤销，条件是修改还没有提交到版本库。

### 删除文件

确实要删除

```
git rm filename
git commit -m '...'
```

删错了，恢复

```
git checkout -- filename
```

## 分支管理

### 创建分支

```
git checkout -b dev
```

相当于两条命令：

```
git branch dev
git checkout dev
```

列出当前所有分支

```
git branch
```

### 合并分支

```
git checkout master
git merge dev
```

### 删除分支

```
git branch -d name
```

### 不使用fast forward模式

merge时会生成一个新的commit，从分支历史上可以看出分支信息。

```
git merge --no-ff -m 'merge with no-ff' dev
```

### 分支管理策略

![](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

master分支稳定，不在上面干活

干活都在dev分支上，等到稳定版是把dev合并到master

每个人有自己的分支，往dev上合并

### Bug分支

暂存dev分支的修改

```
git stash
```

切换至master分支，并创建临时分支

```
git checkout master
git checkout -b issue-101
```

在临时分支修复bug，提交，切换回master分支，合并，然后删除临时分支

```
git add filename
git commit -m '...'
git checkout master
git merge --no-ff -m 'merge bug fix 101' issue-101
git branch -d issue-101
```

切换回dev分支继续干活，`git stash list`查看缓存列表

```
git checkout dev
git stash list
```

恢复缓存

```
git stash pop
该方法恢复最近的缓存，并删除缓存
```

```
git stash apply stash@{0}
git stash drop stash@{0}
```

### Feature分支

添加新功能时从dev分支创建feature分支

```
git checkout -b feature-vulcan
```

开发完毕，提交，切换至dev，合并然后删除

```
git add vulcan.py
git commit -m 'add feature vulcan'
git checkout dev
git merge --no-ff -m '...' feature-vulcan
git branch -d feature-vulcan
```

如果开发过程中取消新功能，不合并，强制删除新功能：

```
git branch -D feature-vulcan
```

### 多人协作

查看远程仓库`git remote`或`git remote -v`显示更详细信息

#### 推送分支

指定本地分支

```
git push origin master
```

推送dev分支

```
git push origin dev
```

* `master`分支要与远程同步
* `dev`分支需要与远程同步
* `bug`分支修复本地bug，不需要推送到远程
* `feature`分支看是否需要合作来决定推动与否

#### 抓取分支

`git clone`和`git pull`在默认情况下至有`master`分支，要把`dev`分支抓取到本地，使用如下：

```
git checkout -b dev origin/dev
```

然后就可以在本地dev上修改和push到远程了。

如果别人也修改了dev分支，此时push失败，需更新本地dev，使用`get pull`, 但要设置本地dev与远程dev的链接：

```
git branch --set-upstream dev origin/dev
```

多人合作流程：

1. 试图用`git push origin branch-name`推送自己的修改
2. 如果推送失败，则是因为远程分支更新，需要先`git pull`试图合并远程分支
3. 如果合并有冲突，则解决冲突并在本地提交
4. 没有冲突或者解决冲突后，用`git push origin branch-name`推送

## 标签管理

### 创建标签

在当前位置创建标签

```
git tag name
```

`git tag`查看所有标签

`git tag name commit-id`对某个commit打标签

`git show tagname`查看标签信息

创建带说明的标签，用`-a`指定标签名，`-m`指定说明文字

```
git tag -a v0.1 -m 'version 0.1 released' commit-id
```

还可以通过`-s`创建带签名的标签：

```
git tag -s v0.2 -m 'signed version 0.2 released' commit-id
```

标签都只存储在本地，要推送本地标签到远程，使用

```
git push origin v1.0
```

或者一次性推送所有本地标签:

```
git push origin --tags
```

### 删除标签

删除本地标签

```
git tag -d v0.1
```

如果标签已经推送到远程，删除时：

1. 先从本地删除`git tag -d v0.9`
2. 再从远程删除`git push origin :refs/tags/v0.9`

## 忽略特殊文件

在工作区根目录创建`.gitignore`文件，把要忽略的文件名写进去，然后把该文件提交到git。

如果有时候想添加某个文件发现添加不了，原因时被`.gitignore`忽略了，如果确实想添加，可以:

```
git add -f filename
```

或者修改`.gitignore`文件，可以用`git heck-ignore`命令检查违反了哪条规则：

```
git check-ignore -v filename
```

## 配置别名

用`st`表示`status`：

```
git config --global alias.st status
```

常用替换

```
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
```

```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

