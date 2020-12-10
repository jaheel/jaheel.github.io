---
title: hexo-建站总结
tags:
  - hexo
  - bash

categories: hexo
comments: false
---
# 搭站

## 1 基础

OS : win 10 x64 教育版

Cmd: git bash

Architecture: Hexo

## 2 准备工具

### 2.1 git

下载网址： http://git-scm.com/download/win

### 2.2 Node.js

下载网址：https://nodejs.org/zh-cn/download/

包含：node、npm

<!-- more -->

## 3 安装hexo

```bash
# 安装
npm install hexo-cli -g

# 查看版本
hexo -v

```

## 4 配置站点

step 1: 建立站点

```bash
# 建站
hexo init blog
# 进入blog文件夹
cd blog
# npm配置
npm install
```

step 2: 配置站点信息

​	打开**_config.yml**文件，修改配置信息

```python
# Site 
title: #填站点名称
subtitle: # 小标题
description:
author:
language: # en(英语)
timezone: # 时区

# URL
url:https://jaheel.github.io/blog
root:/jaheel.github.io/ #仓库名

```

## 5 安装主题

选择了**Next**主题，克隆到themes/next文件夹中

```bash
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

修改**_config.yml**:

```python
theme: next
```

## 6 生成与部署

### 6.1 生成

```bash
hexo g # 自动生成
hexo s # 本地测试
```

### 6.2 部署

**_config.yml**文件末尾添加：

```python
deploy:
	type: git
	repo: git@github.com:jaheel/jaheel.github.io.git #SSH方式，避免HTTPS方式多次输入用户名、密码
	branch: master
```

部署到git:

```bash
# 未安装git部署依赖包，需输入以下命令下载
npm install hexo-deployer-git --save
# 部署
hexo d
```



部署成功后即可输入 **xxxx.github.io** 网址进行联网预览

## 7 绑定域名

域名代理厂商：阿里云万网（其他代理商同理）

step 1: 

​		购买想要的域名后，在**域名管理控制台**找到个性化域名，添加解析

```
记录类型：CNAME
主机记录：www
记录值： xxxx.github.io
解析线路：default

记录类型：A
主机记录：@
记录值：xxxx.github.io对应的IPv4地址
解析线路：default
```



step 2:

​		回到github仓库，进入setting，设置Custom domain，输入域名，点击Save保存

step 3:

​		进入本地blog/source目录下，创建记事本，输入域名，保存，命名为CNAME所有文件（无后缀）

```bash
hexo clean
hexo g
hexo d
```

打开浏览器在地址栏输入域名即可访问

#  高级配置

## 1 标签

```bash
hexo new page tags
```

进入新增的tags文件夹，用VSCode打开，修改头部

```
title: xxx
type: "tags"
comments: false # 最好禁了
```



然后就在各个加入的文件中加入头部就行，例如：

```
title: 标签测试文章
tags:
  - Testing
  - Another Tag
---
```

## 2 分类

```bash
hexo new page categories
```

其余操作同标签

# 配置主题

官网：https://theme-next.iissnan.com/theme-settings.html#fonts-customization

# sitemap配置

教程网址：http://lindaxiao-hust.github.io/2016/04/06/hexo-next-sitemap/