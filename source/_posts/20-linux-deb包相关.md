---
title: linux deb包相关
date: 2023-11-27 13:16:44
categories:
- Linux
tags:
- dpkg
- 软件包管理
- 命令行工具
---

安装,搜索linux软件包

<!-- more -->

# linux deb包相关

```sh
dpkg -l
```

显示安装的包

```sh
dpkg -l | grep name
```

搜索名为name的包

```sh
whereis name
```

查看包相关的路径