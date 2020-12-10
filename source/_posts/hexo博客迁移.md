---
title: hexo-博客迁移
tags:
  - hexo
  - bash

categories: hexo
comments: false
---

## step 1

将原来电脑已经配置好并生成的Hexo目录拷贝到新电脑。

只需拷贝以下目录:

```
_config.yml
package.json
scaffolds/
source/
themes/
```

放到同一目录下。如：hexo/

## step 2

配置node.js

参考：Hexo-建站

## step 3

安装hexo

```shell
npm install -g hexo
```

## step 4

进入hexo/目录

安装模块:

```shell
npm install
npm install hexo-deployer-git --save
npm install hexo-generator-feed --save
npm install hexo-generator-sitemap --save
```

## step 5

部署

```shell
hexo g
hexo d
```

