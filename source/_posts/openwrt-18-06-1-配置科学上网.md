---
title: openwrt 18.06.1 配置科学上网
date: 2020-08-04 17:02:43
categories:
- Openwrt
tags:
- 上网
---

openwrt 安装不缀述，网上教程很多，可以刷在路由器上、小主机上、甚至VirtualBox的虚拟机里。本文假定openwrt已经设置好并上网，电脑连接路由器LAN口。

<!-- more -->

# openwrt 18.06.1 配置科学上网

openwrt 安装不缀述，网上教程很多，可以刷在路由器上、小主机上、甚至VirtualBox的虚拟机里。本文假定openwrt已经设置好并上网，电脑连接路由器LAN口。

## 1.安装 shadowsocks-libev 相关的包

```shell
opkg update (多运行几次，我遇到过第一次update后找不到shadowsocks相关的包)
opkg install shadowsocks-libev-config shadowsocks-libev-ss-local shadowsocks-libev-ss-redir shadowsocks-libev-ss-rules shadowsocks-libev-ss-tunnel luci-app-shadowsocks-libev
```



## 2.基础配置

进入openwrt web配置界面，选择 Service->shadowsocks-libev
点击 Remote Servers, 里面已经默认配置一个服务器 sss0，修改地址，端口，密码，加密方式，最重要的，将disabled的勾去掉，点击 save&apply 按钮。
再点击 Local Instances, 点击 ss-local.cfgXXXXX (XXX为随机数字)条目对应的Disabled 按钮，将其变成 Enabled，点击 Save & Apply。配置保存生效以后, ss-local.cfgXXXX 条目的Running 状态由no变为yes。这时，路由器上已经运行一个SOCKS5服务器，端口1080。设置电脑浏览器的代理服务器为路由器地址，端口1080，尝试访问谷歌，如果成功则说明ss客户端在openwrt上工作一切正常。
接着要测试iptables+ss-redir自动转发代理(透明代理)的功能，在Local Instances中，将ss-redir.hi 设为Enabled。再点击 Redir Rules, Disabled勾去掉，点击Destination Settings，dst default 由bypass改为 forward。点击Save&Apply 使配置生效。将电脑浏览器的代理设置取消，访问谷歌，如果成功，则说明无条件透明代理设置成功。所有数据包都由路由器转发到ss服务器了。

## 3.进阶配置

上面最后配置的透明代理将全部流量都转发到远端SS服务器，显然太浪费流量，而且国内的网站去国外转一圈效率也很低。因此我们需要在路由器上识别国内国外流量，区别对待。

1. 首先将 Destination Settings中的dst forward 设为 bypass。

2. 将opkg列表更新由http 改为https, http存在更新不全的情况，可能是GFW搞鬼

   ```shell
   opkg install libustream-mbedtls (如果提示找不到，opkg update 多运行几次)
   sed -i s/http:/https:/g /etc/opkg/distfeeds.conf
   opkg update
   ```

   

3. 安装各种依赖包

   ```shell
   opkg remove dnsmasq
   opkg install dnsmasq-full
   opkg install coreutils-base64 curl ca-certificates ca-bundle
   ```

   

4. 步骤太多太烦，写了一键脚本，执行就好：

```shell
cd /tmp && opkg update && opkg install curl ca-bundle && curl -s -L https://github.com/ysy/ss/raw/master/openwrt_tproxy.tgz -ot.tgz && tar x -z -f t.tgz && cd openwrt_tproxy && ./setup.sh
```

## 4.编译

目前lede代码下载下来配置界面是没有ss包的选项的 需要更改feeds.conf.default文件 将最后一行前面的#去掉

然后

```sh
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
```

