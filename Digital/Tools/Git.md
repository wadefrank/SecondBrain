# Git 常用语法

* --move，分支移动
* --force，强制执行
* -M(--move --force)
* -u(--set-upstream)，建立本地分支与远程分支的跟踪关系

# Git常用动作

## 撤回

只撤回提交但保留修改

```shell
# 撤回最近一次 commit，保留修改内容在工作区
git reset --soft HEAD~1

# 撤回最近N次 commit(例如撤回 2 次)
git reset --soft HEAD~2
```

撤回提交并取消暂存修改（回到未 git add的状态）

```shell
# 等同于 git reset --mixed HEAD~1
git reset HEAD~1
```
​
# GitHub

## 使用命令行创建一个新仓库

```shell
# 1.创建一个README.md
echo "# SecondBrain" >> README.md

# 2.在项目目录初始化一个新的Git仓库
git init

# 3. 添加文件并做第一次提交
git add README.md
git commit -m "first commit"

# 4.关键步骤：将当前分支（master）强制重命名为main
git branch -M main

# 5.添加远程仓库地址
git remote add origin git@github.com:wadefrank/SecondBrain.git

# 6.将本地的 main 分支推送到远程的 origin 仓库，并设置上游跟踪
git push -u origin main
```