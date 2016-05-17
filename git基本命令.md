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



