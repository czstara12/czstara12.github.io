---
title: 树莓派jupyterlab环境搭建
date: 2020-11-09 11:02:31
categories:
- Linux
tags:
- Python
- RaspberryPi
- jupyterLab
---

# 树莓派环境搭建

本文介绍了树莓派web python(jupyterLab)运行环境的搭建方法

以及对应的批量制作时的备份镜像方法

<!-- more -->

下载镜像https://www.raspberrypi.org/downloads/raspberry-pi-os/

![下载界面(若图像无法加载 可访问原网站czstara12.github.io)](C:\Users\czsta\AppData\Roaming\Typora\typora-user-images\image-20200819163207784.png)

图为带桌面 不含软件

插入内存卡

格式化SDFormatter

![SDFormatter界面(若图像无法加载 可访问原网站czstara12.github.io)](C:\Users\czsta\AppData\Roaming\Typora\typora-user-images\image-20200819163413163.png)

确定-确定

## 写入镜像

解压缩

win32 disk imager

选择解压出来的文件

点write(写入)

![Win32 Disk Imager界面(若图像无法加载 可访问原网站czstara12.github.io)](C:\Users\czsta\AppData\Roaming\Typora\typora-user-images\image-20200819163836570.png)

注意右上方的设备是否为要写入的设备

完成后会出现一个名为boot的盘

向其中写入一个名为ssh的空文件

和一个名为wpa_supplicant.conf的配置文件

配置文件内容

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
	psk="password2"
	priority=1
}
#或者更多
```

要求电脑连接上面配置文件所连接的wifi

![](C:\Users\czsta\AppData\Roaming\Typora\typora-user-images\image-20200819165633434.png)

弹出

将内存卡转移到树莓派上

上电

Advanced IP Scanner

扫描树莓派ip(如果可以进入路由器后台 可直接进入路由器后台查看)

![image-20200819171028114](C:\Users\czsta\AppData\Roaming\Typora\typora-user-images\image-20200819171028114.png)

可以看到

使用ssh进行连接

![image-20200819171146203](C:\Users\czsta\AppData\Roaming\Typora\typora-user-images\image-20200819171146203.png)

用户名

pi

默认密码

raspberry

## 更新软件

更换清华源

```shell
sudo vi /etc/apt/sources.list
```

注释原源 添加新源

```shell
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
```



```shell
sudo vi /etc/apt/sources.list.d/raspi.list
```

```shell
deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui
```

更新

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install -y vim#安装包
```



## 调节配置

```
sudo raspi-config
```

设置boot启动desktop

3-B1-B3

打开摄像头接口

5-B1-是

打开VNC

5-B3-是

打开串口

5-B6-否-是

拓展内存

7-A1

设置分辨率

7-A5

finish

## 安装opencv

```
sudo apt-get install python3-opencv
```



## 安装jupyterlab

服务器在国外 网络不好建议使用代理

```
export http_proxy='http://127.0.0.1:7890'
export https_proxy='https://127.0.0.1:7890'
```

安装

```
sudo apt-get install libffi-dev#安装一个支持包
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple jupyterlab
```

重启

```
sudo reboot
```



开放外网访问

```
jupyter-notebook --generate-config
sudo vim /home/pi/.jupyter/jupyter_notebook_config.py
```

写入

```python
c.NotebookApp.allow_origin = '*' #allow all origins
c.NotebookApp.ip = '0.0.0.0' # listen on all IPs
c.NotebookApp.notebook_dir = '/home/pi'
```

设置密码

```
jupyter-notebook password
```

添加自启动

```shell
sudo vim /etc/rc.local
```

写入

```
su pi -c '/home/pi/.local/bin/jupyter-lab --no-browser' &
```

在exit 0前面



或者以服务的形式自启动

```
# sudo vim /lib/systemd/system/notebook.service
[Unit]
Description=Notebook Demon

[Service]
ExecStart=/home/pi/.local/bin/jupyter-lab
RestartSec=10s
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=jupyternotebooklog
User=pi
Group=pi

[Install]
WantedBy=multi-user.target
```



## 安装nodejs

如果需要 挂代理 很慢

```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```



## 安装小控件

先打开jupyter lab的界面

然后开启拓展

如果需要 挂代理 很慢

```shell
pip3 install matplotlib
pip3 install ipywidgets
jupyter labextension install @jupyter-widgets/jupyterlab-manager@2.0 --no-build
export NODE_OPTIONS=--max-old-space-size=2048
jupyter lab build --dev-build=False --minimize=False
```

然后重启

## 配置串口

```
sudo vim /boot/config.txt
```

写入

```
dtoverlay=pi3-miniuart-bt
```

然后重启

```
sudo reboot
```

## 树莓派系统备份

将搭建好树莓派环境的内存卡用读卡器插到电脑上 用Win32DiskImager进行全量备份

然后将全量备份文件传到linux虚拟机上

(必须为英文环境的虚拟机 不可以用经过国产汉化的deepin 会报错 也不可以使用WSL 推荐ubuntu)

使用工具PiShrink进行裁剪

```
wget https://raw.githubusercontent.com/Drewsif/PiShrink/master/pishrink.sh
chmod +x pishrink.sh
./pishrink.sh rpi-back.img
```

裁剪后此文件变小 下载回来即可

### tip

树莓派处理图像切记不要在原分辨率(640x480)下处理

至少长宽要缩小4倍

面积缩小16倍(160x120)