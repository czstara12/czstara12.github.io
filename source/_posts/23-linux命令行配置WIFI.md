---
title: linux命令行配置WIFI
date: 2023-11-27 13:31:28
categories:
- 技术文章
tags:
- 树莓派
- Jetson Nano
- WIFI配置
- 命令行
- Ubuntu
- NetworkManager
---

树莓派 jetson nano等无显示器配置WIFI

<!-- more -->

## 命令行配置wifi

### raspberrypi os

```sh
sudo wpa_cli -iwlan0
```

### ubuntu 20.04

编辑/etc/netplan/50-cloud-init.yaml

```sh
sudo netplan apply
```

或者

```sh
sudo wpa_cli -iwlan0
```

#### NetworkManager

查看网络设备列表

```
nmcli dev
```

注意，如果列出的设备状态是 unmanaged 的，说明网络设备不受NetworkManager管理，你需要清空 /etc/network/interfaces下的网络设置,然后重启.

开启WiFi

```
nmcli r wifi on
```

扫描附近的 WiFi 热点

```
nmcli dev wifi
```

连接到指定的 WiFi 热点

```
nmcli dev wifi connect "SSID" password "PASSWORD" ifname wlan0
```
