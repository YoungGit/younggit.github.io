---
title: tmux学习笔记
date: 2016-04-04 14:52:45
categories:
- linux
tags: 
- linux
- shell
- tmux
---
# tmux简单介绍
tmux是一款终端复用工具，我主要在ssh时使用它。类似的有screen，以后有空再学。

当初在windows上可以使用xshell进行ssh连接，非常好用。换到mac上就没有那么好用的工具了，只能使用terminal或者iterm2，克隆会话啥的需要配置，比较麻烦，还是学学tmux比较好。

最常见的使用场景就是使用tmux开左右两个pane，一边vim，一边shell或者看日志。

<!--more-->

# 基本概念
**session**  
会话，一个服务器上可以包含多个session，在终端输入`tmux`就可以打开一个新的session

**window**  
窗口，一个session可以包含多个window

**pane**  
面板，一个window可以包含多个pane（即分屏）

# 入门
最开始只需要掌握以下几个命令

- `<ctrl+b>`默认的prefix-command
- `?`列出所有快捷键，按`q`返回
- `%`左右分屏
- `"`上下分屏
- `方向键`在pane之间转换
- `x`关闭当前pane
- `&`关闭当前window

掌握了这几个命令，基本就能使用tmux了

最重要的就是记住`<ctrl+b>`这个prefix-command，在tmux内执行任何操作前都需要按这个组合键。这个前缀组合按键当然是可以在配置文件里修改的，后面会介绍。

# 进阶
主要介绍一下比较常用的进阶操作，过于冷门的操作就不做介绍了。
## 基本操作
以下操作，在终端中执行时需要加上`tmux`，如新建一个名为“test”的session的命令为`tmux new -s "test"`；在tmux内，先按`ctrl+b`，然后输入`:`就可以敲命令了

```
new              # Create a new session
 -s "Session"    # Create named session
 -n "Window"     # Create named Window
 -c "/dir"       # Start in target directory

attach           # Attach last/available session
 -t "#"          # Attach target session
 -d              # Detach the session from other instances

ls               # List open sessions
 -a              # List all open sessions

lsw              # List windows
 -a              # List all windows
 -s              # List all windows in session

lsp              # List panes
 -a              # List all panes
 -s              # List all panes in session
 -t              # List all panes in target

kill-window      # Kill current window
 -t "#"          # Kill target window
 -a              # Kill all windows
 -a -t "#"       # Kill all windows but the target

kill-session     # Kill current session
 -t "#"          # Kill target session
 -a              # Kill all sessions
 -a -t "#"       # Kill all sessions but the target
 ```

## session操作
在tmux内，先按`ctrl+b`：

|命令|说明|
|---|---|
|s|列出所有session，然后可以切换session|
|$|重命名当前session|
|d|离开当前session，回到终端|


## window操作
在tmux内，先按`ctrl+b`：

|命令|说明|
|---|---|
|c|create window，新建窗口|
|&|关闭当前窗口|
|数字键|切换到指定窗口|
|w|列出所有窗口，然后可以切换窗口|
|,|重命名当前窗口|
|p|previous window，切换到上一窗口|
|n|next window，切换到下一窗口|
|l|前后窗口间互相切换|
|.|修改当前窗口编号，只能改为当前没被占用的编号|
|f|find window，在所有窗口中查找关键词，便于在多个窗口间切换|

## pane操作
在tmux内，先按`ctrl+b`：

|命令|说明|
|---|---|
|"|将当前面板上下分屏|
|%|将当前面板左右分屏|
|x|关闭当前面板|
|方向键|选择对应的面板|
|q|显示当前窗口内所有面板的编号，马上按下数字键可转到指定面板|
|z|tmux1.8加入的功能，将当前面板最大化|
|o|选择当前窗口中下一个面板|
|ctrl+方向键|以1个单元格为单位移动边缘以调整当前面板大小|
|alt+方向键|以5个单元格为单位移动边缘以调整当前面板大小|

## 复制粘贴
1. 按`ctrl+b [`进入复制模式
2. 默认使用方向键进行移动，可以在配置文件中设置`setw -g mode-keys vi`来使用vi模式进行移动
3. 移动到想复制的地方后，按`空格键`之后再移动光标就可以开始选择文本了
4. 选择完成后，按`回车键`完成复制
5. 按`ctrl+b ]`粘贴

## 配置文件
配置文件为~/.tmux.conf，在tmux启动时自动加载设置，如同vimrc。

可配置内容包括通用内容（编码、历史记录、鼠标等）、快捷键绑定、主题和UI。

我只设置了底部状态栏的样式，效果如下图：

![](http://tangosource.com/wp-content/uploads/2015/05/image10.png)

配置内容为：

```
# 颜色
set -g status-bg black
set -g status-fg white

# 位置
set-option -g status-justify centre

# 左侧
set-option -g status-left '#[bg=black,fg=green][#[fg=cyan]#S#[fg=green]]'
set-option -g status-left-length 20

# 中间的window列表
setw -g automatic-rename on
set-window-option -g window-status-format '#[dim]#I:#[default]#W#[fg=grey,dim]'
set-window-option -g window-status-current-format '#[fg=cyan,bold]#I#[fg=blue]:#[fg=cyan]#W#[fg=dim]'

# 右侧
set -g status-right '#[fg=green][#[fg=cyan]%Y-%m-%d#[fg=green]]'
```

# 参考资源
- [A Tmux crash course: tips and tweaks.](http://tangosource.com/blog/a-tmux-crash-course-tips-and-tweaks/)
- [tmux shortcuts & cheatsheet](https://gist.github.com/MohamedAlaa/2961058#file-tmux-cheatsheet-markdown)
- [learn Tmux in Y minites](https://learnxinyminutes.com/docs/tmux/)
