---
title: eos使用操作教程
date: 2021-11-07 20:29:49
categories:
- raspberrypi
tags:
- raspberrypi
- Linux
- opencv
- 视觉
- intel realsense T265
---

本文是对eos相关特色功能的介绍和使用教程 基于树莓派官方系统修改 有关原版官方系统的特性请参考[官方网站](https://www.raspberrypi.com/documentation/)和[github](https://github.com/raspberrypi/documentation) 

系统账户pi

系统密码raspberry

jupyterlab密码raspberry

<!-- more -->

eos是由我(星辰物语)开发制作由我维护的一个魔改系统.其基本理念是树莓派的官方系统(Raspberry Pi OS)的基础上添加一系列实用功能并重新打包,以减少安装系统后的一系列繁琐配置.所有修改均记录在其内置的开发日志以便查找BUG或者部分还原.目前eos仍处在测试的阶段(v0.5),如果在使用中有什么建议或者意见,欢迎使用邮箱和QQ联系我.

本文第一次写于eos v0.5(2021年11月7日20:56:41)之前经历了v0.1 v0.2 v0.3 v0.4几个版本发展至今

eos主要被我用于视觉所以名字取做eye os另外还可有其他解释eyes of speed,eye of smart等

eos目前只用于raspberry pi 4B和computer module 4上其他树莓派版本未经测试,不保证其有效性

# 相关功能列表

1. usb快速连接
2. 远程jupyter lab访问
3. 预置opencv-python库
4. 预置英特尔双目视觉realsense T265驱动库及python接口

# USB快速连接

使用一根type-c线将树莓派连接至装有windows的计算机上,等待片刻,树莓派可自动虚拟一个网卡连接至电脑,此时可使用固定IP:**192.168.39.39**访问此树莓派以便进行其他操作,如连接ssh VNC等.由于是固定的一个IP,所以最好不要一次性连接两块树莓派同时连接至同一台电脑

# jupyter lab远程访问

使用主流浏览器连接至**树莓派IP+:8888**如**192.168.39.39:8888**可直接访问到jupyter lab的操作界面jupyter lab使用的相关操作可参考

配合上面的USB快速连接可直接使用**192.168.39.39:8888**访问

# 视觉例程

树莓派配备大量视觉例程,有实用型的,也有学习研究型的,进入jupyter lab可直接获取.

# T265驱动

树莓派已经安装好了T265的SDK驱动,无需繁琐且容易失败的安装,相关安装提示可参考本博客的[另一篇文章](https://www.3939831.xyz/2021/07/15/%E8%8B%B1%E7%89%B9%E5%B0%94%E5%AE%9E%E6%84%9F%E6%91%84%E5%83%8F%E5%A4%B4SDK%E5%AE%89%E8%A3%85/#more)

# 更新日志

在系统目录下/boot/readme.md内记录有所有历史版本以及更新日志

# 预计加入功能

1. 开发树莓派盖板 带oled小屏幕 实时显示IP,运行状态和cpu内存资源占用 欢迎及其他提示信息
2. 树莓派盖板 带16:9TFTLCD 显示桌面 连接键鼠直接使用 也可开发显示其他信息
3. jupyter lab内添加扫描连接wifi的脚本
4. 安装ros操作系统
5. 安装mavros支持包

