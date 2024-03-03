---
title: 英特尔实感摄像头SDK安装
date: 2021-07-15 15:08:22
categories:
- 树莓派教程
tags:
- 英特尔实感技术
- 树莓派
- SDK安装
- T265
- Python配置
---


<!-- more -->

# 英特尔实感摄像头T265 SDK在树莓派4B上的编译安装

## 安装

基本上只要执行下列命令就可以了 也有官方的一键安装脚本方法

```sh
sudo apt-get update && sudo apt-get dist-upgrade -y
sudo apt-get install git cmake libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev -y
sudo apt-get install libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev -y
git clone https://github.com/IntelRealSense/librealsense.git
sudo sh ./scripts/setup_udev_rules.sh
mkdir build && cd build
cmake ../ -DCMAKE_BUILD_TYPE=Release -DBUILD_EXAMPLES=true -DFORCE_RSUSB_BACKEND=ON -DBUILD_PYTHON_BINDINGS=bool:true -DPYTHON_EXECUTABLE=$(which python3)
#sudo make uninstall && make clean && make && sudo make install
make && sudo make install
realsense-viewer
```

要点 不要屏蔽TM2固件和深度固件 科学上网

## realsense设备启动问题

另外T265在树莓派上有启动问题

具体表现为插着T265时树莓派上电开机 T265会无法启动

解决方案之一 在树莓派启动后重新插拔一下树莓派

当然 有自动的方法

使用https://github.com/mvp/uhubctl控制电源 重新启动 模拟重新插拔的步骤

注意 树莓派usb电源在内部连通 需要一同将所有接口电源关闭

```
git clone https://github.com/mvp/uhubctl
cd uhubctl
make
sudo make install
#将下面指令添加进/etc/rc.local里
sudo uhubctl -a off -l 2
sleep 1s
sudo uhubctl -a on -l 2
```

官方的Issues贴在这里 也许以后会有更好的办法

https://github.com/IntelRealSense/librealsense/issues/4681

https://github.com/IntelRealSense/librealsense/issues/4113

## pylibrealesnes添加库文件

本文安装是带有python包的 但是不知道为什么官方安装过程没有对python正确配置 因此进行下列修复

```
sudo cp ~/librealsense/build/wrappers/python/pyrealsense2.cpython-37m-arm-linux-gnueabihf.so /usr/lib/python3.7/pyrealsense2.cpython-37m-arm-linux-gnueabihf.so
sudo cp ~/librealsense/build/wrappers/python/pybackend2.cpython-37m-arm-linux-gnueabihf.so /usr/lib/python3.7/pybackend2.cpython-37m-arm-linux-gnueabihf.so
```

