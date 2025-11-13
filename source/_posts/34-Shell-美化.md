---
title: Shell 美化
date: 2024-09-29 11:34:24
categories:
- Linux
tags:
- shell
- zsh
- bash
- Powerlevel10k
- Oh My Zsh
---


# 安装 Oh My Zsh 和 Powerlevel10k 主题并设置为默认 Shell 的教程

## 前言

Zsh 是一款功能强大的 Shell，比传统的 Bash 提供了更多的特性。Oh My Zsh 是一个开源的 Zsh 配置管理框架，Powerlevel10k 是一个高性能的 Zsh 主题，能美化你的终端。本教程将指导你如何安装它们并将 Zsh 设置为默认的 Shell。

<!-- more -->

## 步骤一：安装 Zsh

首先，检查是否已安装 Zsh：

```bash
zsh --version
```

如果未安装，按照以下步骤安装。

**在 Ubuntu/Debian 上安装 Zsh：**

```bash
sudo apt update
sudo apt install zsh -y
```

**在 CentOS/Fedora 上安装 Zsh：**

```bash
sudo yum install zsh -y
```

**在 macOS 上安装 Zsh（通常已预装）：**

```bash
brew install zsh
```

## 步骤二：安装 Oh My Zsh

使用以下命令安装 Oh My Zsh：

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

或者，如果你没有 `curl`，可以使用：

```bash
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

## 步骤三：安装 Powerlevel10k 主题

克隆 Powerlevel10k 到 Oh My Zsh 的自定义主题目录：

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

## 步骤四：配置 Zsh 使用 Powerlevel10k

编辑 `.zshrc` 文件：

```bash
nano ~/.zshrc
```

找到以下行并修改：

```bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```

保存并退出编辑器。

## 步骤五：设置 Zsh 为默认 Shell

运行以下命令：

```bash
chsh -s $(which zsh)
```

**注意：**你可能需要注销并重新登录，或者重启终端以使更改生效。

## 步骤六：配置 Powerlevel10k

首次运行 Zsh 时，Powerlevel10k 会启动配置向导。按照提示进行配置。如果未自动启动，可以手动运行：

```bash
p10k configure
```

## 附加步骤：安装字体（可选）

为了正确显示图标和符号，建议安装 MesloLGS NF 字体：

1. 下载字体：[MesloLGS NF 字体下载链接](https://github.com/romkatv/powerlevel10k#manual-font-installation)。
2. 安装字体到系统。
3. 在终端设置中，将字体更改为 MesloLGS NF。

## 结论

通过以上步骤，你已经成功安装并配置了 Oh My Zsh 和 Powerlevel10k 主题，并将 Zsh 设置为默认 Shell。现在，你的终端将变得更加美观和高效。
