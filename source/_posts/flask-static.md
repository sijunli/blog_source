---
title: 如何用flask访问静态文件
toc: false
date: 2016-06-13 01:11:00
tags:
- wiki
- flask
- python
---

在apache中，将静态文件放在指定目录下（默认是__/var/www/html__)，就能在浏览器上直接访问了。而flask框架中稍微有些区别，因为flask下只有工程目录下的__static__文件夹能够用作今天文件的访问，同理，在url中，也需要加上__/static__路径。