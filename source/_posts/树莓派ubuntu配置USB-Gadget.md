---
title: 树莓派ubuntu配置USB Gadget
date: 2022-02-21 09:26:07
categories:
- Raspberry Pi
tags:
- linux
- raspberrypi
---

树莓派安装ubuntu系统时USB Gadget的安装配置

USB Gadget可以让你使用一根usb-c线连接电脑同时让你的网络连接到树莓派

<!-- more -->

# ubuntu配置usb网络

一键配置脚本(适用于ubuntu20.04版本)

```python
#!/bin/python3
def check_string(strs,str):
    for line in strs:
        if str in line:
            return True # The string is found
    return False  # The string does not exist in the file

usercfg_file=open('/boot/firmware/usercfg.txt','r+')
if check_string(usercfg_file.readlines(),'dtoverlay=dwc2') == False:
    usercfg_file.write('\ndtoverlay=dwc2\n')
    print('/boot/firmware/usercfg.txt添加dtoverlay=dwc2')
usercfg_file.close()

cmdline_file=open('/boot/firmware/cmdline.txt','r+')
if check_string(cmdline_file.readlines(),'modules-load=dwc2') == False:
    cmdline_file.write(' modules-load=dwc2')
    print('/boot/firmware/cmdline.txt添加modules-load=dwc2')
cmdline_file.close()

modules_file=open('/etc/modules','r+')
if check_string(modules_file.readlines(),'libcomposite') == False:
    modules_file.write('\nlibcomposite\n')
    print('/etc/modules添加libcomposite')
modules_file.close()

net_file=open('/etc/netplan/12-usb.yaml','w')
net_file.write('''network:
    ethernets:
        usb0:
            dhcp4: false
            optional: true
            addresses: [192.168.39.39/24]
    version: 2
''')
print('创建/etc/netplan/12-usb.yaml')
net_file.close()

import os
os.system('sudo apt install -y dnsmasq')
print('安装dnsmasq')

dnsmasq_conf_file=open('/etc/dnsmasq.d/usb-gadget','w')
dnsmasq_conf_file.write('''dhcp-rapid-commit
dhcp-authoritative
no-ping
interface=usb0
dhcp-range=usb0,192.168.39.2,192.168.39.6,255.255.255.0,1h
domain=usb.lan
dhcp-option=usb0,3
leasefile-ro
port=0
''')
print('创建/etc/dnsmasq.d/usb-gadget')
dnsmasq_conf_file.close()


sh_file=open('/usr/local/sbin/usb-gadget.sh','w')
sh_file.write('''#!/bin/bash
cd /sys/kernel/config/usb_gadget/
mkdir -p pi4
cd pi4
echo 0x1d50 > idVendor # Linux Foundation
echo 0x60c7 > idProduct # Multifunction Composite Gadget
echo 0x0100 > bcdDevice # v1.0.0
echo 0x0200 > bcdUSB # USB2
echo 0xEF > bDeviceClass
echo 0x02 > bDeviceSubClass
echo 0x01 > bDeviceProtocol
mkdir -p strings/0x409
echo "fedcba9876543211" > strings/0x409/serialnumber
echo "test" > strings/0x409/manufacturer
echo "RPi4 USB Gadget" > strings/0x409/product
mkdir -p configs/c.1/strings/0x409
echo "RNDIS network" > configs/c.1/strings/0x409/configuration
echo 250 > configs/c.1/MaxPower
echo "0x80" > configs/c.1/bmAttributes
mkdir -p os_desc
echo "1" > os_desc/use
echo "0xcd" > os_desc/b_vendor_code
echo "MSFT100" > os_desc/qw_sign
# Add functions here
# see gadget configurations below
# End functions
mkdir -p functions/rndis.usb0
echo "02:23:45:67:89:ab" > functions/rndis.usb0/dev_addr
echo "12:23:45:67:89:ab" > functions/rndis.usb0/host_addr
echo "RNDIS" > functions/rndis.usb0/os_desc/interface.rndis/compatible_id
echo "5162001" > functions/rndis.usb0/os_desc/interface.rndis/sub_compatible_id
ln -s configs/c.1 os_desc
ln -s functions/rndis.usb0 configs/c.1
#mkdir -p functions/ecm.usb0
#HOST="00:dc:c8:f7:75:14" # "HostPC"
#SELF="00:dd:dc:eb:6d:a1" # "BadUSB"
#echo $HOST > functions/ecm.usb0/host_addr
#echo $SELF > functions/ecm.usb0/dev_addr
#ln -s functions/ecm.usb0 configs/c.1/
#udevadm settle -t 5 || :
ls /sys/class/udc > UDC
''')
print('创建/usr/local/sbin/usb-gadget.sh')
sh_file.close()
os.system('sudo chmod 750 /usr/local/sbin/usb-gadget.sh')

service_file=open('/lib/systemd/system/usb-gadget.service','w')
service_file.write('''[Unit]
Description=USB gadget initialization
After=network-online.target
Wants=network-online.target
#After=systemd-modules-load.service
Before=dnsmasq.service
[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/local/sbin/usb-gadget.sh
[Install]
WantedBy=sysinit.target
''')
service_file.close()
print('创建/lib/systemd/system/usb-gadget.service')
os.system('sudo systemctl daemon-reload')
os.system('sudo systemctl enable usb-gadget')
print('添加启动服务')
```

ubuntu18.04 修复少量小问题

注 请勿与vmware等有劫持usb功能的软件一同运行 自动程序会加载错误 或者在vmware设置该设备自动连接到windows

```
#!/bin/python3
def check_string(strs,str):
    for line in strs:
        if str in line:
            return True # The string is found
    return False  # The string does not exist in the file

usercfg_file=open('/boot/firmware/usercfg.txt','r+')
if check_string(usercfg_file.readlines(),'dtoverlay=dwc2') == False:
    usercfg_file.write('\ndtoverlay=dwc2\n')
    print('/boot/firmware/usercfg.txt添加dtoverlay=dwc2')
usercfg_file.close()

cmdline_file=open('/boot/firmware/nobtcmd.txt','r+')
if check_string(cmdline_file.readlines(),'modules-load=dwc2') == False:
    cmdline_file.write(' modules-load=dwc2')
    print('/boot/firmware/cmdline.txt添加modules-load=dwc2')
cmdline_file.close()

modules_file=open('/etc/modules','r+')
if check_string(modules_file.readlines(),'libcomposite') == False:
    modules_file.write('\nlibcomposite\n')
    print('/etc/modules添加libcomposite')
modules_file.close()

net_file=open('/etc/netplan/12-usb.yaml','w')
net_file.write('''network:
    ethernets:
        usb0:
            dhcp4: false
            optional: true
            addresses: [192.168.39.39/24]
    version: 2
''')
print('创建/etc/netplan/12-usb.yaml')
net_file.close()

import os
os.system('sudo apt update')
os.system('sudo apt dist-upgrade')
os.system('sudo apt install -y dnsmasq')
print('安装dnsmasq')

dnsmasq_conf_file=open('/etc/dnsmasq.d/usb-gadget','w')
dnsmasq_conf_file.write('''#dhcp-rapid-commit
#dhcp-authoritative
#no-ping
interface=usb0
dhcp-range=usb0,192.168.39.2,192.168.39.6,255.255.255.0,1h
#domain=usb.lan
dhcp-option=usb0,3
leasefile-ro
port=0
''')
print('创建/etc/dnsmasq.d/usb-gadget')
dnsmasq_conf_file.close()


sh_file=open('/usr/local/sbin/usb-gadget.sh','w')
sh_file.write('''#!/bin/bash
cd /sys/kernel/config/usb_gadget/
mkdir -p pi4
cd pi4
echo 0x1d50 > idVendor # Linux Foundation
echo 0x60c7 > idProduct # Multifunction Composite Gadget
echo 0x0100 > bcdDevice # v1.0.0
echo 0x0200 > bcdUSB # USB2
echo 0xEF > bDeviceClass
echo 0x02 > bDeviceSubClass
echo 0x01 > bDeviceProtocol
mkdir -p strings/0x409
echo "fedcba9876543211" > strings/0x409/serialnumber
echo "test" > strings/0x409/manufacturer
echo "RPi4 USB Gadget" > strings/0x409/product
mkdir -p configs/c.1/strings/0x409
echo "RNDIS network" > configs/c.1/strings/0x409/configuration
echo 250 > configs/c.1/MaxPower
echo "0x80" > configs/c.1/bmAttributes
mkdir -p os_desc
echo "1" > os_desc/use
echo "0xcd" > os_desc/b_vendor_code
echo "MSFT100" > os_desc/qw_sign
# Add functions here
# see gadget configurations below
# End functions
mkdir -p functions/rndis.usb0
echo "02:23:45:67:89:ab" > functions/rndis.usb0/dev_addr
echo "12:23:45:67:89:ab" > functions/rndis.usb0/host_addr
echo "RNDIS" > functions/rndis.usb0/os_desc/interface.rndis/compatible_id
echo "5162001" > functions/rndis.usb0/os_desc/interface.rndis/sub_compatible_id
ln -s configs/c.1 os_desc
ln -s functions/rndis.usb0 configs/c.1
#mkdir -p functions/ecm.usb0
#HOST="00:dc:c8:f7:75:14" # "HostPC"
#SELF="00:dd:dc:eb:6d:a1" # "BadUSB"
#echo $HOST > functions/ecm.usb0/host_addr
#echo $SELF > functions/ecm.usb0/dev_addr
#ln -s functions/ecm.usb0 configs/c.1/
#udevadm settle -t 5 || :
ls /sys/class/udc > UDC
netplan apply
systemctl restart dnsmasq.service
''')
print('创建/usr/local/sbin/usb-gadget.sh')
sh_file.close()
os.system('sudo chmod 750 /usr/local/sbin/usb-gadget.sh')

service_file=open('/lib/systemd/system/usb-gadget.service','w')
service_file.write('''[Unit]
Description=USB gadget initialization
After=network-online.target
Wants=network-online.target
#After=systemd-modules-load.service
Before=dnsmasq.service
[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/local/sbin/usb-gadget.sh
[Install]
WantedBy=sysinit.target
''')
service_file.close()
print('创建/lib/systemd/system/usb-gadget.service')
os.system('sudo systemctl daemon-reload')
os.system('sudo systemctl enable usb-gadget')
print('添加启动服务')
```

