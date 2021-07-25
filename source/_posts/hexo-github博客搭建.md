---
title: hexo+github博客搭建
date: 2020-08-04 17:05:10
categories:
- blog
tags:
- hexo
- github
---

正在填坑

<!-- more -->

# hexo+github博客的搭建

正在施工 请过几月再看

## 发布文章

```
hexo new [布局] <文章标题>
```

## 预览

```
hexo server
```

# 搭建基于GitPages的hexo个人博客

## 一 预安装环境

### 1. 安装git

在deb系上

```shell
sudo apt-get install -y git
```

### 2.安装node.js

见https://github.com/nodesource/distributions/blob/master/README.md

### 3.安装hexo

```
sudo npm install -g hexo-cli
hexo -v#查看是否安装成功
```

## 二 开始搭建吧

```
cd ~
mkdir yourBlogName
cd yourBlogName
hexo init
npm install
hexo g
hexo s
```

预览你的博客

http://localhost:4000

### 安装主题

```
git clone https://github.com/theme-next/hexo-theme-next themes/next
cd themes/next
git tag
```

git tag查看relases的版本 并切换到稳定版本

```shell
pi@raspberrypi:~/blog/themes/next $ git tag 
v6.0.0
v6.0.1
v6.0.2
v6.0.3
v6.0.4
v6.0.5
v6.0.6
v6.1.0
v6.2.0
v6.3.0
v6.4.0
v6.4.1
v6.4.2
v6.5.0
v6.6.0
v6.7.0
v7.0.0
v7.0.1
v7.1.0
v7.1.1
v7.1.2
v7.2.0
v7.3.0
v7.4.0
v7.4.1
v7.4.2
v7.5.0
v7.6.0
v7.7.0
v7.7.1
v7.7.2
v7.8.0
```

我这里是v7.8.0

```
git checkout v7.8.0
rm -rf .git
```

并删除.git文件夹

回到主目录

```
cd  ../..
```

编辑_config.yml文件

```
vim _config.yml
```

更改theme项为next

```
theme: next
```

### 验证更改

```
hexo clean
hexo g
hexo s
```

### 更改外观

```
vim themes/next/_config.yml
```

找到Schemes项 将其修改为下面的样子 每一行对应一种外观

```
# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```

## 三 自定义你的博客

### 设置语言

编辑站点配置文件(指根目录下的`_config.yml`,主题目录下的`_config.yml`为主题配置文件,以后都如此称呼)

编辑

```
language: zh-CN
```

配置文件

```yaml
# Hexo 配置
## 文档: https://hexo.io/docs/configuration.html
## 源: https://github.com/hexojs/hexo/

# 设置
title: Hexo #标题
subtitle: '' #副标题
description: '' #描述
keywords: #关键字
author: John Doe #作者
language: zh-CN #语言
timezone: '' #时区

# URL
## 如果您的网站位于子目录中, 将url设置为“ http://example.com/child”，将root设置为“ / child /”
url: http://example.com #链接
root: / #根目录
permalink: :year/:month/:day/:title/ #永久链接格式
permalink_defaults:
pretty_urls:
  trailing_index: true # 设置为false可从永久链接中删除结尾的“ index.html”
  trailing_html: true # 设置为false可从永久链接中删除结尾的“ .html”

# 目录
source_dir: source #源目录
public_dir: public #公开目录
tag_dir: tags #标签目录
archive_dir: archives #归档目录
category_dir: categories #分类目录
code_dir: downloads/code #代码目录
i18n_dir: :lang
skip_render:

# 写作
new_post_name: :title.md # 新帖子的文件名
default_layout: post #默认布局
titlecase: false # 将标题转换为标题栏
external_link:
  enable: true # 在新标签页中打开外部链接
  field: site # 应用于整个网站
  exclude: ''
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  enable: false
  preprocess: true
  line_number: true
  tab_replace: ''

# 主页设置
# path：您的博客索引页面的根路径。 （默认=''）
# per_page：每页显示的帖子。 （0 =禁用分页）
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date

# 分类和标签
default_category: uncategorized
category_map:
tag_map:

# 元数据元素
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo使用Moment.js解析和显示日期
## 您可以按照定义自定义日期格式
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss
## updated_option支持'mtime'，'date'，'empty'
updated_option: 'mtime'

# 分页
## 将per_page设置为0以禁用分页
per_page: 10
pagination_dir: page

# 包含/排除文件
## include：/ exclude：选项仅适用于“ source /”文件夹
include:
exclude:
ignore:

# 扩展
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next

# 部署方式
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: ''

```

按自己需求配置 配置完后即可生成预览

### 主题配置

参考https://wylu.me/posts/e0424f3f/

## 四 发布

![image-20210725170612738](https://raw.githubusercontent.com/czstara12/img_rope/master/img/image-20210725170612738.png)

创建一个和你用户名相同的仓库，后面加github.io

初始化库

```
git init
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
git commit -m 'init'
```

然后安装上面的提示添加远端git库 并推送

```
git remote add origin https://github.com/YUAN321340/YUAN321340.github.io.git
git push -u origin master
```

### 添加Travis CI

https://github.com/marketplace/travis-ci

参见https://hexo.io/zh-cn/docs/github-pages.html

### 添加tokens

### 添加.travis.yml配置文件

```
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - master # 只监控 source 的 branch
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master # hexo 站点源文件所在的 branch
  local_dir: public 
  target_branch: public # 存放生成站点文件的 branch，使用 username.github.io 必须是 master
```

### 设置库的发布源

### 稍等片刻 就可以访问 你的名字.github.io 预览了

发布文章

```
hexo new "My New Post"
```

编写source/_posts/下的对应的.md文件

