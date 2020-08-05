# Git使用

### 查看所有分支

```shell
git branch -a
```

### 查看远程分支

```shell
git branch -r
```

### 关联远程仓库

```shell
git remote add origin git@github.com:flycser/DataX.git
```

备注，关联远程仓库后，可以在本地创建新分支，再推送到远程

```shell
git checkout -b dev
git push --set-upstream origin dev
```

### 删除本地分支

```shell
git branch -d master
```

### 删除远程分支

```shell
git push origin --delete dev
```

### 推送一个空分支到远程分支/新建远程分支(dev)

```shell
git push origin dev
```

### 推送到远程分支

```shell
git push <远程主机名> <本地分支名>:<远程分支名>
git push origin master:master
```



### 修改 .gitignore 文件 立即生效

```shell
git rm -r --cached . # 清除缓存  
git add . # 重新trace file 
git commit -m "update .gitignore" # 提交和注释  
git push origin master # 可选, 同步到remote 
```

### 合并分支

```shell
git merge dev # master合并dev
```

### 切换分支

```shell
git checkout dev # 切换到dev分支
```

### 查看提交历史

```shell
git log
```

### 查看分支结构

```shell
git log --graph --decorate --oneline --simplify-by-decoration --all
```

--decorate 显示每个commit的引用(如:分支、tag等) 

--oneline 一行显示

--simplify-by-decoration 只显示被branch或tag引用的commit

--all 表示显示所有的branch, 可选, 显示分支A, B, C的关系, 则将--all替换为branchA branchB branchC

### pull命令

```shell
git pull origin <remote branch>:<local branch> 

git pull origin <remote branch> // pull到本地当前分支
```

### clone特定远程分支

```shell
git clone -b <branch> <repository>
```



