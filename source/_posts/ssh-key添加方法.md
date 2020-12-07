---
title: ssh-key添加方法
date: 2020-11-24 16:52:43
categories:
- Linux
tags:
- SSH
---

本文记录SSH秘钥生成与linux免密登陆的操作步骤

<!-- more -->

# SSH-key生成,转换与配置

## SSH登陆密钥对添加

将公钥文件写入~/.ssh/authorized_keys中即可

## SSH秘钥生成

```shell
ssh-keygen
```

然后一路回车

会在~/.ssh/目录下生成名为id_rsa的私钥和id_rsa.pub的公钥

通常来说 生成的秘钥格式如下 可以直接改名为authorized_keys并修改权限600使用

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRWCd3ZV7MWRJhpM84AtjhnJQS18XJlDmGDNBPxvyefbCTZDsCNJ7bsC7R8gHbRE8PM0ndnRcMiPTwrw0ZMH1WoMwkOcLJZqXRvJfhfgkjnJ7aXBMsjjCjlia/2eIoB9sUVuXmi5fP4t2jZo7fHKkDJ0jaOIPdqBjBrWLWePFtXrJ1SH6XPhUO3uUhXhmne/bjqsIWZl5/dYPM7oksdOpc//d5GizfPbrj8loSF3O3tgmVxuzEF14rZ4kSEhaiNlD5sQXIe5+is9iFmAFMtchtEp3rT3n7hbuMD9xDS3Svk5HGvtctZY1jRwzkrgHeHD4b3pNgqM2DftebpF pi@raspberrypi
```

特点 只有一行 没有换行符

生成的私钥格式如下

```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAQEA0Vgnd2VezFkSYaTPOALY4ZyUEtfFyZQ5hgzQT8b8nn2wk2Q7AjSe
27Au0fIB20RPDzNJ3Z0XDIj08K8NGTB9VqDMJDnCyWal9QnkbyX4X4JI5ye2lwTLI4wo5Y
mv9niKAfbFFbl5ouXz+Ldo2aO3xypAydI2jiD3agYwa1i1njxbV6ydUh+lz4VDt7lIV4Zp
3v246rCFmZef3WDzO6JLHTqXP/3eRos3z264/JaEhdyAbTt7YJlcbsxBdeK2eJEhIWojZQ
+bEFyHuforPYhZgBTLXIbRKd6095+4W7jA/cQ0t0r5ORxr7XLWWNY0cM5K4B3hw+G96TYK
jNg37Xm6RQAAA8idsYglnbGIJQAAAAdzc2gtcnNhAAABAQDRWCd3ZV7MWRJhpM84AtjhnJ
QS18XJlDmGDNBPxvyefbCTZDsCNJ7bsC7R8gHbRE8PM0ndnRcMiPTwrw0ZMH1WoMwkOcLJ
ZqX1CeRvJfhfgkjnJ7aXBMsjjCjlia/2eIoB9sUVuXmi5fP4t2jZo7fHKkDJ0jaOIPdqBj
BrWLWePFtXrJ1SH6XPhUO3uUhXhmne/bjqsIWZl5/dYPM7oksdOpc//d5GizfPbrj8loSF
3IBtO3tgmVxuzEF14rZ4kSEhaiNlD5sQXIe5+is9iFmAFMtchtEp3rT3n7hbuMD9xDS3Sv
k5HGvtctZY1jRwzkrgHeHD4b3pNgqM2DftebpFAAAAAwEAAQAAAQB+6mbi574VPVr7f6Nx
XaiG/xp2YgIzN324W0RfWVAF9kV61iVALQ6yOZnpBkNB36Pen0WE6ZvzqYR19mqGfvM99b
ZNsAb7exPZ/ulSyT5PCPmRym3UGL/fCTYyEstvLZzdm/HYPd4UeDz06JzUdYERafhlYuBY
Qnw89wubyOgyyWL12+Z1hBDl6QFJ+C0p7rild1dmjy4lDT/lBfn6OGykhFrVLEh7qCjRt4
H62t7s3U23OkSdMgsbPboJH6tyWLGHaafvxEzv14s9cvwVdzJYAZhkZ5ph5RLrY+bEwolV
R4RyszUi1wQISkUNLkzjFQDwDLo5LgU1vD3qXWuJJE9xAAAAgQCTKiE1XuEGWiYooL2War
1qh5swNgNnSIV9TU+7h0Dd61sdm0sjmRxTuXPPrjCGhk3SYWI3ZyV1+fFtXIZ23JWoxst7
FqIRRSfEEDIrRrAmYsXKFFYIt9fAifuOm19rCSET27GeiAfvlK+q7HXCbHQ84bsuN1Tmxl
r5liocXh+NNgAAAIEA/RyvSeDgRgDKSrbiKhHrFjjC+mZTqD9PtW6glKRkdyQrh+Hri/8F
QmVKcJrPxmlkC2zSNOXhaUyv1Ab7Dg8FrcSjjgce4SZlzB51rq1nIXplHzdGq//HASVdP3
u5exdDKkmE3HFhVtWDZBOnJxWtAU/xPXXFXSrqPuYHyTj3SYsAAACBANO7oNn+DZ4PxLGy
Z5Rmd7fTlubWyvydCTsCVuoFCx1eDja2ZnVh1UIThIUWTvcscrgIbTg0a1n1PEY28QJ5jH
Th3v6P1a+nzV+k6QQSpbEOS3CfnohT+ghnfbWrIQWSr5M3RFKAxxLu3Vhl5Lx479RXnk95
lnk0BCztOruEV2VvAAAADnBpQHJhc3BiZXJyeXBpAQIDBA==
-----END OPENSSH PRIVATE KEY-----
```

一般来说 私钥的内容是不能公开的 不过本示例的私钥对应的公钥没有放在任何一个系统上 所以无所谓啦

## 秘钥格式转换

如果你用其他软件生成秘钥那么格式就有可能是这样的

### 公钥

```
---- BEGIN SSH2 PUBLIC KEY ----
Comment: "rsa-key-20200904"
AAAAB3NzaC1yc2EAAAABJQAAAQEAqXpPr867/R4hIUxQQHWIZJRYFNR9YibHYM96
oYPaJ+rlAhcUq3GJbqFEsUvAdNQ+V9Jlg7NHVlBgue8Jxt2xH02xJc3+g9Xpdnii
1IUqkqNHkkNfglpSjiC42JDqHmjaGFo/7ESHqOt9+YbaVo969/W/gW+sp87lUMO/
E9ExK7YT3fH/qGublN9faHsew+hu3wEF/z1S67LotGGBp4//sLzTK6rLWoQYQpk3
zA9xWtl/nJrOSmhKRdSUPIk32/NRj/2mzqyPTIsdTjffdlrCBqhCPWlTpAe12JFy
AyytlB1R+cppQnb0wihmmyx+s7U0NVEA0N05wQmyFsn1O+608w==
---- END SSH2 PUBLIC KEY ----
```

特点 多行

使用命令

```
ssh-keygen -e -f .ssh/id_rsa.pub > id_rsa_new.pub
```

来将他转换为单行的秘钥

### 私钥

一般来说 putty可以导入多种格式的私钥 并对其进行转换

一般常见的有两种秘钥 OPENSSH和RSA格式

OPENSSH是新格式 如果想要将其转换为RSA格式

使用puttygen先将私钥转为.ppk格式，再转换回来即可

上面所生成的格式为OPENSSH格式

RSA格式如下

```
-----BEGIN RSA PRIVATE KEY-----
MIIEoQIBAAKCAQEAqXpPr867/R4hIUxQQHWIZJRYFNR9YibHYM96oYPaJ+rlAhcU
q3GJbqFEsUvAdNQ+V9Jlg7NHVlBgue8Jxt2xH02xJc3+g9Xpdnii1IUqkqNHkkNf
glpSjiC42JDqHmjaGFo/7ESHqOt9+YbaVo969/W/gW+sp87lUMO/E9ExK7YT3fH/
qGublN9faHsew+hu3wEF/z1S67LotGGBp4//sLzTK6rLWoQYQpk3zA9xWtl/nJrO
SmhKRdSUPIk32/NRj/2mzqyPTIsdTjffdlrCBqhCPWlTpAe12JFyAyytlB1R+cpp
Qnb0wihmmyx+s7U0NVEA0N05wQmyFsn1O+608wIBJQKCAQBAIHEtw9htnLKW+kfh
CeCUwoIxZSiGANXRmp0ahOrC/u7eMj8sHR8+neKj8yY6Cx6CGEIWKCjUjR2uIxh0
wpYL2DUw5ijznQw6qi/GCN+tGy/Welt9bkjAKA6XIhNlc8gk4+DV8GqunktXfyIu
lyecF8vryWPsd8xjv7AVVhKa6lUasUaOAjPVbb8dVIw+uPgQ30ujUkYpprYirjtr
9oE3ju69/WpFqPZya7YKID47sF9L2BQurFduaSN0UwZYk6wBgrwvKbuNHXOgatvg
fcXk2154tYZlra1kiAIwXcQYDXlrVD/qC8kQu4X+LO/eh4tzyIcbAkrXIFpdBWSc
6c0dAoGBANRKMY7ClimubwtRrWoKrqAQWtmBaiRZzhQI9bMT+ah2vgsMOAs9Savn
sB7GsBa1aiQQNW2pr7/z3PyBSwTgUNiETYM9aVrlv0zZ3pISDNCKoptXEDy/qRDy
XoQpKrvQK4vCgvZG+c5X7GyMC919MIwpY1gCyXvDZSZK+zr10dFfAoGBAMxffjZu
hnsyYJHbktRcmhqiDuSXVSv/PoWY6HqtHSZLu+NyeZT3aZfjXrfqnG0Hr4RD83Ru
g1ob4boH0H2j0w3jH8SCVOlNRx7erJqTAYsM5nZeMDOzE6PKOTE1nGuEYHMJVoDs
XgC+AzyBqd4hQwCjEoziH/gyzwqxZQvL/CDtAoGAW80OguxcnGcbQyo9Ju/cz5he
oz7hyoezD5Ur+mmAt41LQwwz6S9Cc9nPnpsbtsr9d2D0gnLkN1SylrRzd7ryhyR0
i8eHgUDBbVdLfW+W1rh9qvU3dDc0Wlr4cICBLp81bN590kg0rEGyWHPpdIkpwBHl
xTGjPHArvYg1Se3ChA0CgYBHzoZKedw5H4m2tO5mSgyhkuKjV8P6s6BYc/6npJuJ
/u784wgflTLwLUrK/2dkk/9l0q9773pCQSXLvY5xVTxQ/MX474WeFDuVOXrqM6aY
o6rrUYaOtIpcJHT1nTb1WAY2QY0YimY3nLUHa9PmQwm0HZ674L8f2oAYg4ReFzzo
+QKBgQDAT4E9j0C6BtQ8WOqL077qnK36yR6OCaw/o3wCN1hRh6uYvEQt/OON60pc
XwfEIMky3TSTsmo8mPw/N1TdFhBMepub/OJ0JG8VK73p6HnWUfxgQ4rnkvSuCes7
/0LkvdFmMtYGu6RpdiA3hhSSmFCX66dlywhhhTncYXseMw2tDQ==
-----END RSA PRIVATE KEY-----
```

## 使用puttygen创建和转换秘钥

![image-20200904192859880](http://d0.ananas.chaoxing.com/download/f6fea8dd7d7901ce9ec41f277dad7e2b?fn=image-20200904192859880)

点击Generate生成秘钥对

点击Save public key保存公钥

格式为多行格式 需要转换为单行格式 也可以直接从上面的大框中复制 为单行格式 直接复制到文本文件即可

点击Save private key保存私钥

生成的格式为.ppk格式 可以读入到puttygen中进一步转换

#### 读取后转换

OpenSSH key为老版本RSA格式

OpenSSH key(force new file format)为新版本OPENSSH格式

![image-20200904193409065](http://d0.ananas.chaoxing.com/download/28449c7cab3519b39c96567589dc9ed8?fn=image-20200904193409065)

