---
title: tmux 简明指南
date: 2024-09-29 11:24:16
categories:
- Linux
tags:
- terminal
- tmux
---

# tmux 简明指南

tmux（Terminal Multiplexer）是一款强大的终端复用工具，允许用户在一个终端中运行多个会话，方便地在不同任务之间切换，大大提升了工作效率。本文将简要介绍 tmux 的使用，包括如何开启鼠标控制、常用的配置、常用的命令，以及如何通过脚本快速自定义创建多个窗口和分割。

<!-- more -->

## 1. 开启鼠标控制

默认情况下，tmux 不支持鼠标操作。通过开启鼠标支持，可以使用鼠标在不同的窗格和窗口之间切换，调整窗格大小等。

在 tmux 配置文件 `~/.tmux.conf` 中添加以下内容：

```bash
# 开启鼠标支持
set -g mouse on
```

保存配置文件后，重新加载 tmux 配置：

```bash
# 在 tmux 会话中，按下以下快捷键
<Prefix> :source-file ~/.tmux.conf
```

其中，`<Prefix>` 默认是 `Ctrl+b`，即先按 `Ctrl+b`，然后松开，再按 `:` 进入命令模式，输入上述命令即可。

## 2. 常用的配置

以下是一些常用的 tmux 配置，可根据个人喜好进行调整：

```bash
# 修改前缀键为 Ctrl+a
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# 设置状态栏样式
set -g status-bg colour235
set -g status-fg colour136
set -g status-interval 5

# 窗口列表样式
setw -g window-status-format "#[fg=green]#[bg=black] #I:#W "
setw -g window-status-current-format "#[fg=black]#[bg=yellow] #I:#W "

# 允许窗口重命名
set-option -g allow-rename on

# 使用 vi 模式
setw -g mode-keys vi

# 开启鼠标支持
set -g mouse on

# 复制模式下使用系统剪贴板（需要 xclip 或 pbcopy）
bind-key -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "xclip -sel clip -i"
```

## 3. 常用的命令

以下是一些 tmux 中常用的命令和快捷键：

### 会话管理

- **创建新会话**

  ```bash
  tmux new -s session_name
  ```

- **列出现有会话**

  ```bash
  tmux ls
  ```

- **附加到会话**

  ```bash
  tmux attach -t session_name
  ```

- **分离会话**

  ```bash
  <Prefix> d
  ```

- **杀死会话**

  ```bash
  tmux kill-session -t session_name
  ```

### 窗口管理

- **新建窗口**

  ```bash
  <Prefix> c
  ```

- **切换窗口**

  ```bash
  <Prefix> n  # 下一个窗口
  <Prefix> p  # 上一个窗口
  <Prefix> l  # 上一次访问的窗口
  ```

- **重命名窗口**

  ```bash
  <Prefix> ,
  ```

- **关闭窗口**

  ```bash
  <Prefix> &
  ```

### 窗格管理

- **分割窗格**

  ```bash
  <Prefix> "  # 水平分割
  <Prefix> %  # 垂直分割
  ```

- **切换窗格**

  ```bash
  <Prefix> o           # 下一窗格
  <Prefix> 方向键       # 移动到指定方向的窗格
  ```

- **调整窗格大小**

  ```bash
  <Prefix> Ctrl+方向键
  ```

- **关闭窗格**

  ```bash
  <Prefix> x
  ```

### 复制粘贴（复制模式）

- **进入复制模式**

  ```bash
  <Prefix> [
  ```

- **退出复制模式**

  ```bash
  q
  ```

- **滚动**

  ```bash
  使用方向键或 PgUp、PgDn
  ```

- **复制文本**

  ```bash
  在复制模式下，按空格开始选择，移动光标选择文本，按 y 复制
  ```

## 4. 通过脚本快速自定义创建多个窗口和分割

tmux 支持通过脚本自动化地创建会话、窗口和窗格。这对于需要重复设置相同开发环境的场景非常有用。

### 示例：创建一个自定义会话

以下是一个示例脚本，可以快速创建一个包含多个窗口和分割的 tmux 会话。

#### 脚本内容（保存为 `start_tmux.sh`）：

```bash
#!/bin/bash

SESSION_NAME="my_session"

# 如果会话不存在，则创建
if ! tmux has-session -t $SESSION_NAME 2>/dev/null; then
  # 创建新会话，起始窗口为 editor
  tmux new-session -s $SESSION_NAME -n editor -d

  # 在第一个窗口中分割窗格
  tmux split-window -h -t $SESSION_NAME:1
  tmux send-keys -t $SESSION_NAME:1.1 'vim' C-m
  tmux send-keys -t $SESSION_NAME:1.2 'htop' C-m

  # 创建第二个窗口
  tmux new-window -t $SESSION_NAME:2 -n server
  tmux send-keys -t $SESSION_NAME:2 'cd /path/to/project && ./run_server.sh' C-m

  # 创建第三个窗口
  tmux new-window -t $SESSION_NAME:3 -n logs
  tmux split-window -v -t $SESSION_NAME:3
  tmux send-keys -t $SESSION_NAME:3.1 'tail -f /var/log/syslog' C-m
  tmux send-keys -t $SESSION_NAME:3.2 'tail -f /var/log/nginx/access.log' C-m

  # 返回到第一个窗口
  tmux select-window -t $SESSION_NAME:1
fi

# 附加到会话
tmux attach -t $SESSION_NAME
```

#### 脚本说明：

- **会话名称**：`SESSION_NAME="my_session"` 定义了会话的名称。
- **检查会话是否存在**：`tmux has-session` 用于检查会话是否已存在，避免重复创建。
- **创建会话和窗口**：`tmux new-session` 和 `tmux new-window` 用于创建新的会话和窗口。
- **分割窗格**：`tmux split-window` 用于分割当前窗口的窗格，`-h` 表示水平分割，`-v` 表示垂直分割。
- **发送命令**：`tmux send-keys` 用于在指定的窗格中执行命令，`C-m` 表示回车。
- **选择窗口**：`tmux select-window` 用于切换到指定的窗口。

#### 使用方法：

1. **保存脚本**：将上述脚本内容保存为 `start_tmux.sh`。
2. **赋予执行权限**：

   ```bash
   chmod +x start_tmux.sh
   ```

3. **运行脚本**：

   ```bash
   ./start_tmux.sh
   ```

### 脚本模板

以下是一个通用的 tmux 脚本模板，可根据需要进行修改：

```bash
#!/bin/bash

SESSION_NAME="your_session_name"

if ! tmux has-session -t $SESSION_NAME 2>/dev/null; then
  # 创建会话并命名第一个窗口
  tmux new-session -s $SESSION_NAME -n window1 -d

  # 在窗口1中分割窗格
  tmux split-window -h -t $SESSION_NAME:1
  tmux split-window -v -t $SESSION_NAME:1.1
  tmux select-layout -t $SESSION_NAME:1 tiled

  # 在各个窗格中执行命令
  tmux send-keys -t $SESSION_NAME:1.1 'command_for_pane1' C-m
  tmux send-keys -t $SESSION_NAME:1.2 'command_for_pane2' C-m
  tmux send-keys -t $SESSION_NAME:1.3 'command_for_pane3' C-m

  # 创建其他窗口并执行命令
  tmux new-window -t $SESSION_NAME:2 -n window2
  tmux send-keys -t $SESSION_NAME:2 'command_for_window2' C-m

  tmux new-window -t $SESSION_NAME:3 -n window3
  tmux send-keys -t $SESSION_NAME:3 'command_for_window3' C-m

  # 返回到窗口1
  tmux select-window -t $SESSION_NAME:1
fi

tmux attach -t $SESSION_NAME
```

#### 模板说明：

- **修改会话名称**：将 `SESSION_NAME="your_session_name"` 中的 `your_session_name` 替换为你想要的会话名称。
- **自定义窗口和窗格**：根据需要添加或删除 `tmux new-window`、`tmux split-window` 和 `tmux send-keys` 等命令。
- **命令执行**：在 `tmux send-keys` 中替换 `command_for_pane` 和 `command_for_window` 为你需要执行的实际命令。

#### 常用 tmux 脚本命令：

- **创建新会话**：

  ```bash
  tmux new-session -s session_name -n window_name -d
  ```

- **创建新窗口**：

  ```bash
  tmux new-window -t session_name:window_number -n window_name
  ```

- **分割窗格**：

  ```bash
  tmux split-window -h/-v -t session_name:window_number.pane_number
  ```

- **选择布局**：

  ```bash
  tmux select-layout -t session_name:window_number layout_name
  ```

  常用布局有 `even-horizontal`、`even-vertical`、`main-horizontal`、`main-vertical`、`tiled`。

- **选择窗口或窗格**：

  ```bash
  tmux select-window -t session_name:window_number
  tmux select-pane -t session_name:window_number.pane_number
  ```

- **发送命令**：

  ```bash
  tmux send-keys -t session_name:window_number.pane_number 'your_command' C-m
  ```

通过以上模板和示例，您可以轻松地通过脚本快速定制 tmux 的工作环境，实现高效的工作流程。
