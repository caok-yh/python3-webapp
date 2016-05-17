

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

