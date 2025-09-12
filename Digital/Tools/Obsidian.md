# 1 Obsidian简介

obsidian，中文翻译成黑曜石
官网：[https://obsidian.md/](https://obsidian.md/)

## 1.1 支持平台

支持平台：
- 桌面平台：
	- macOS
	- Windows
	- Linuxπ
- 移动平台：
	- iOS
	- Android

## 1.2 特点

1. 支持Markdown
2. 双链笔记先去吃饭
3. 丰富的插件系统
4. 强大的自定义模板
5. 纯文本
6. 本地化资料库
7. 支持同步
8. 支持发布
9. ...

## 1.3 定位

不是什么：
- 不是信息收集工具
- 不是Word
- 不是时间管理工具
是什么：
- 码字工具
- **深度**知识管理工具
	- **内容**为基础
	- **链接**为核心
Obsidian能做什么：
- 写作
- 建立深度知识库
	- 关联知识点
	- 发现新知识点
- 建立索引
- 写清单日记
- ...

# 2 双链

两种链接：
* 入链
* 出链

链接语法：`[[]]`
* 链接到文章，两个方括号中的内容填写为：文章名
* 链接到标题，两个方括号中的内容填写为：文章名#标题名
* 链接到段落，两个方括号中的内容填写为：文章名#^

使用别名：`[[|别名]]`

显示链接内容在当前文章：`![[]]`

# 3 模板

使用模板的意义：
* 结构相同
* 内容不同
使用模板的具体情况，高频重复性笔记，如：
* 读书笔记
* 会议记录


# 4 快捷键

* command+E：阅读模式和编辑模式切换
* command+P：打开命令面板
* command+Shift+C：插入代码块

# 5 GitHub同步

## 5.1 新建GitHub仓库

首先在GitHub上新创建一个空的仓库，然后cd到存放Obsidian笔记的目录，并输入以下指令：
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

## 5.2 添加.gitignore

新建一个.gitignore文件
```shell
vim .gitignore
```

输入：
```
# 忽略 .obsidian 文件夹下的所有内容
.obsidian/

# 忽略 obsidian 垃圾箱中的内容
.trash/

# 忽略 macOS 系统文件
.DS_Store

# 忽略 Windows 系统缩略图缓存
Thumbs.db

# 忽略 Linux 文件管理器生成的缓存
.directory

# 忽略 Obsidian Git 插件的临时文件
.obsidian-git-log.txt

# 忽略 临时或者备份文件
*.swp
```
# 参考资料
## 链接

[[Markdown#常用语法|Markdown常用语法]]

[[Markdown#快捷键|Markdown快捷键]]

## 视频

[Obsidian公开课合集-清单控沙牛](https://space.bilibili.com/443605967/lists/266172?type=season)

[# Obsidian如何使用Github实现同步](https://www.bilibili.com/video/BV1HY5EzCEk5/?spm_id_from=333.1387.homepage.video_card.click&vd_source=45bb2c2960a5fd61cfffe3668d0a3b2b)