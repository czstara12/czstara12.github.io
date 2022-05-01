---
title: 树莓派jupyterlab_64位新环境搭建
date: 2022-02-12 17:05:31
categories:
- Linux
tags:
- Python
- RaspberryPi
- jupyterLab
---

本文介绍了新版树莓派64位web python(jupyterLab)运行环境的搭建方法

以及对应的批量制作时的备份镜像方法

<!-- more -->

# 树莓派环境搭建

下载镜像https://www.raspberrypi.org/downloads/raspberry-pi-os/

注意下载下面的Raspberry Pi OS (64-bit)

![image-20220213095204217](https://raw.githubusercontent.com/czstara12/img_rope/master/img/image-20220213095206016.png)

插入内存卡

格式化sd卡

SDFormatter

![image-20220213095542097](https://raw.githubusercontent.com/czstara12/img_rope/master/img/image-20220213095542097.png)

确定-确定

## 写入镜像

解压缩

使用软件balena-etcher

选择解压出来的文件

![image-20220213095641391](https://raw.githubusercontent.com/czstara12/img_rope/master/img/image-20220213095641391.png)

注意中间的设备是否为要写入的设备

### 设置wifi网络连接

完成后会出现一个名为boot的盘

向其中写入一个名为ssh的空文件

和一个名为wpa_supplicant.conf的配置文件

配置文件内容 源自[此处](https://www.raspberrypi.com/documentation/computers/configuration.html#using-unsecured-networks)

```json
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
	ssid="wifi ssid"#wifi名
	psk="password"#wifi密码
	priority=2#优先级 数字越高 越优先使用
}

network={
	ssid="wifi ssid 2"
    key_mgmt=NONE #无密码设置此处
	priority=1
}
#或者更多
```

要求电脑连接上面配置文件所连接的wifi

![image-20220217132604488](https://raw.githubusercontent.com/czstara12/img_rope/master/img/image-20220217132604488.png)

(注意 如果文件建好后与上图图标不一样或者类型一类显示不一样 请尝试关闭windows的隐藏拓展名功能 然后再把名称改到与图中一致)

然后将内存卡插到树莓派上 并开机

获取树莓派ip(如果可以进入路由器后台 可直接进入路由器后台查看)

然后使用ssh进行连接

(此处使用的工具是MobaXterm也可以使用putty等SSH工具 请读者自行搜索下载)

用户名:pi

默认密码:raspberry

## 调节配置

```
sudo raspi-config
```

拓展内存

![image-20220217133442619](https://raw.githubusercontent.com/czstara12/img_rope/master/img/image-20220217133442619.png)

![image-20220217133458250](https://raw.githubusercontent.com/czstara12/img_rope/master/img/image-20220217133458250.png)

其余谨凭读者喜好设置(打开摄像头接口,打开VNC,打开串口,打开GPIO等)

### 为了后续安装的顺利 此处配置代理服务器

由于一些限制 此处写的比较简略 请读者自行寻找具体方法

```
export all_proxy=http://127.0.0.1:7890
export http_proxy=$all_proxy
export https_proxy=$all_proxy
export ftp_proxy=$all_proxy
export socks_proxy=$all_proxy
```

可能需要临时把主机防火墙关掉 或者使用ssh穿透防火墙

可以用命令`curl cip.cc` 检查终端是否处于代理状态。

类似的还有`curl ifconfig.me`

## 更新软件

```
sudo apt update
sudo apt dist-upgrade
sudo apt install -y vim#安装vim包
```

## 安装miniforge

```
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh"
bash Mambaforge-$(uname)-$(uname -m).sh
```

## 安装jupyterlab

安装

```
conda install jupyterlab
```

开放远程访问

```
jupyter lab --generate-config
sudo vim /home/pi/.jupyter/jupyter_lab_config.py
```

写入以下内容

```python
c.ServerApp.allow_origin = '*' #allow all origins
c.ServerApp.ip = '0.0.0.0' # listen on all IPs
c.ServerApp.root_dir = '/home/pi'
```

设置密码

```
jupyter lab password
[请自行设置想要的密码]
```

以服务的形式添加自启动

添加文件`/lib/systemd/system/jupyterlab.service`

```
[Unit]
Description=jupyterlab Demon

[Service]
ExecStart=/home/pi/miniforge3/bin/jupyter-lab
RestartSec=10s
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=jupyterlablog
User=pi
Group=pi

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl enable jupyterlab.service
sudo reboot
```

## 安装opencv

```
conda install opencv
```

## 安装ipywidgets控件

```shell
conda install matplotlib ipywidgets
```

然后重启

## 配置串口

```
sudo vim /boot/config.txt
```

写入

```
dtoverlay=disable-bt
```

然后重启

```
sudo reboot
```

## 树莓派系统备份

将搭建好树莓派环境的内存卡用读卡器插到电脑上 用Win32DiskImager进行全量备份

然后将全量备份文件传到linux虚拟机上

(必须为英文环境的虚拟机 不可以用经过国产汉化的deepin 会报错 也不可以使用WSL 可以使用WSL2 推荐ubuntu)

使用工具PiShrink进行裁剪

```
wget https://raw.githubusercontent.com/Drewsif/PiShrink/master/pishrink.sh
chmod +x pishrink.sh
./pishrink.sh -s rpi-back.img
```

裁剪后此文件变小 下载回来即可

## 树莓派卡复制

此命令可以将树莓派卡克隆一份

```
sudo dd bs=1M if=/dev/mmcblk0 of=/dev/sda status=progress
```

### tips

树莓派处理图像切记不要在原分辨率(640x480)下处理

至少长宽要缩小4倍

面积缩小16倍(160x120)

## 系统连接

树莓派环境配置完毕后可以在电脑访问jupyter lab编程界面

如果电脑支持mDNS服务 那就可以用浏览器直接访问

```
raspberrypi.local:8888
```

进入到编程界面

或者找到树莓派的具体ip 然后访问

```
192.168.1.xxx.xxx(树莓派的IP):8888
```

进入编程界面

## VNC第三方客户端连接设置

树莓派使用realvnc作为内置vnc服务器 连接必须使用realvnc提供的客户端软件 如果想用第三方的vnc(例如mobaxterm提供的vnc)就必须进行一些修改

参见[此链接](https://www.raspberrypi.com/documentation/computers/remote-access.html#authenticating-to-vnc-server)