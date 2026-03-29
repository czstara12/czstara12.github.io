---
title: EOS更新日志
date: 2024-03-14 13:58:38
categories:
tags:
---

记录EOS搭建时的操作

<!-- more -->

[TOC]

# 修改记录

## TODO:版本号

1. 创建venv
2. 安装jupyterlab, opencv-python, serial
3. 打开vnc, serial
4. 设置wifi, hostname, username, ssh
5. 自启动jupyter lab
6. 设置jupyter lab远程访问和密码
7. 调整桌面分辨率1920x1080
8. iic状态显示

### todo list

- [ ] USB RNDIS
- [ ] 添加本文件到boot分区目录
- [ ] 打开了串口命令行 关闭它

## Eos1.0 2022年07月31日12:48:45

1. 安装，配置novnc 自启动 webvnc面板为zflypi:6080
2. 添加zsh ohmyzsh byubo软件 并设为默认
3. 小记 基于debain系统的局限性 分支处ubuntu版本 由ubuntu版本支持ros

## Eos0.9 2022年5月18日12:21:01

1. 安装uhubctl加入/boot/boot.sh控制文件 
2. gpio22开关操作加入/boot/boot.py 在开机后重启t265电源
3. 修复/etc/hosts中指向自己的localhost项目 应为zflypi
4. 添加实验样例
5. 添加T265航点使用的一些库
6. 设置默认wifi设置 ssid:zflywifi psk:zflywifi
7. 整理根目录文件
8. 将VNC验证更改为标准VNC验证 以允许第三方vnc客户端连接

## Eos0.8

此版本基于raspberry pi os 64bit官方系统全新制作 linux内部版本debain11 

更新系统2022年5月5日14:06:53

下载并安装[Miniforge3-4.11.0](https://github.com/conda-forge/miniforge/releases/tag/4.11.0-2)版本

基础配置 VNC 串口 拓展内存卡 设置显示分辨率

jupyter-lab 并配置开机自启动为服务启动 远程访问 显示小部件ipywidgets

还原硬件串口AMA0

安装realsense T265驱动

添加综合功能测试模块

添加usb_otg虚拟网卡windows上免驱(默认ip192.168.39.39)

添加readme文件

修复pyrealsense(已测试)

~~按键切换create_ap热点~~

修复realsense设备启动问题 加入t265补丁(开关GPIO cm4版本使用 4b版本尚未添加此特性) 加入t265例程

添加演示实验

~~加入快速扫描连接wifi的jupyter lab脚本~~

CM4切换外置天线

添加开机启动守护程序服务auto-task.service 运行自定义脚本

添加python二维码条形码识别定位库pyzbar及例程

开启uart3

开启miniuart bt迷你串口到蓝牙

设置新wifi连接配置

关闭cm4 usb-otg

修改hostname为zflypi

安装serial库

安装mamba

## 问题

~~树莓派空载发热问题~~系统原因 无法软件修复 建议添加风扇

~~英特尔t265启动问题~~英特尔已终止T265项目 暂时可能无解决方案