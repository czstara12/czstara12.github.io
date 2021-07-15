---
title: 树莓派4B usb-otg 连接电脑
date: 2021-07-15 15:14:05
categories:
- Linux
tags:
- RaspberryPi
---

# 树莓派4 usb-otg

本文记录了将树莓派通过自带的usb type-c口连接到电脑 从而控制树莓派的方法 适用于树莓派zero 树莓派4B

同时附带树莓派热点连接电脑方法

<!-- more -->

本文介绍的方法相对简单 也不是最优方案 只支持windows和linux的连接 对macos的连接支持可见[此](https://www.hardill.me.uk/wordpress/2019/11/02/pi4-usb-c-gadget/)

打开树莓派USB OTG模式。修改`/boot/config.txt`文件，在最后一行加入：

```bash
dtoverlay=dwc2
```

载入内核模块，启动USB网卡功能。修改`/boot/cmdline.txt`文件，在第一行末尾添加空格并加入：

```bash
modules-load=dwc2,g_ether
```

修改ip 在/etc/dhcpcd.conf末尾添加

```
interface usb0
static ip_address=192.168.39.39/24
# static routers=192.168.137.1          # 禁用路由设定，否则会导致Wifi路由不能用
static domain_name_servers=8.8.8.8
```

连接电脑还需要安装相应的网卡驱动

驱动地址

https://github.com/LowLevelOfLogic/RaspberryPi/blob/master/docs/refers/mod-duo-rndis.zip

## 配置IP问题

两种方式

1. 添加dhcp服务 让电脑可以自动获取合适的ip

   使用dnsmasq

2. 修改网卡配置为静态ip

   控制面板 网络连接 属性 ip

   树莓派静态ip暂时设置为 192.168.39.39

### 配置DHCP

```
sudo apt-get update
sudo apt-get install dnsmasq
sudo touch /etc/dnsmasq.d/usb0.conf
```

```
interface=usb0
dhcp-option=3
dhcp-range=192.168.39.100,192.168.39.255,255.255.255.0,12h
```

附一个树莓派热点开启方法

## 树莓派热点

通过create_ap组件

```
sudo apt-get install hostapd
git clone https://github.com/oblique/create_ap
cd create_ap
sudo make install
```

开热点前要把默认开启的usb网络关闭(如果配置了的话)

```
systemctl stop dnsmasq.service
create_ap -n wlan0 --no-virt -c 7 wifissid
```

也可以参考[树莓派官方教程](https://www.raspberrypi.org/documentation/configuration/wireless/access-point-routed.md)