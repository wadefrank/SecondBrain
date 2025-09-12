# 常用操作

## 打开终端

**`⌘ Command + 空格`**，输入“终端”，按回车

## 终端进入目录

打开终端，然后输入cd+空格，将文件夹拖到终端，再敲回车即可

## 显示隐藏文件
### 快捷键临时显示
1. 打开访达
2. 进入想要查看隐藏文件的任意文件夹
3. 按下快捷键 ​**​`Command (⌘) + Shift + .`​**​
4. 再次按下 ​**​`Command (⌘) + Shift + .`​**​ 即可重新隐藏它们
### 使用终端（Terminal）命令（永久生效）
如果想在让Mac访达永久显示隐藏文件，打开终端，运行以下命令：
```shell
defaults write com.apple.finder AppleShowAllFiles -bool true

killall Finder
```
如果想恢复隐藏，打开终端，运行以下命令：
```shell
defaults write com.apple.finder AppleShowAllFiles -bool false

killall Finder
```