---
layout: post
title: 'Apache Reverse Proxy'
tags:
  - apache
  - proxy
category: common
---
反向代理也是一种代理服务器。 它根据客户端的请求， 从后端的服务器上获取资源， 然后将这些资源返回给客户端， 这个后端服务器可以是一个也可以是多个。如果是多个那就是负载均衡。
下面主要介绍单个的后端服务器的配置

<!--more-->

首先， 下载[Apache](http://www.apachehaus.com/cgi-bin/download.plx?dli=kpnVsRGMBNTT6F0aO5GZIpkVOpkVFVVcSRkSzJ1Z), 并解压到某个文件夹， 文件夹的路径可以自定义。自定义的文件夹在启动的过程中会出现很多文件找不到的情况， 这种情况下， 只需要根据提示一步一步修改就行了。

然后及时配置代理了
