---
title: maven踩坑
date: 2022-06-13 12:43:58
tags: 
  - bug修复
categories: 
  - bug修复
description: maven下载版本问题
index_img: https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E7%94%B5%E5%BD%B1%2F%E6%B4%9B%E5%A5%87%2F04F647A3CD3768644D826B54B4BF5311.png
---

# maven踩坑

今天专业实训看到之前用的idea自带的maven，仓库保存到了c盘。自己c盘快满了，于是想着自己下一个maven改一下仓库。网上看了下maven版本要下在idea版本发行日期之后发行的版本，没多想直接下了最新版3.8.6。结果pom.xml中的`parent`以及`plugins`疯狂的报错:`Error injecting constructor, java.lang.NoSuchMethodError: org.apache.maven.model`。折磨我了一上午，最后把maven版本换成idea自带的maven版本，错误解决:cry:。
