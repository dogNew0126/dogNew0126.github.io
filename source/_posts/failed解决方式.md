---
title: 解决hexo报错spwan failed
date: 2022-05-26 10:43:29
tags:
  - hexo
  - 解决报错
categories:
  - hexo
description: Hexo部署出现错误Error:Spawn failed解决方式
index_img: https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E6%99%AF%E8%89%B2%2F6%20-%20OlW3SBF.jpg
---

# Hexo部署出现错误Error:Spawn failed解决方式

{% note success %}

Hexo部署出现以下错误Error:Spawn failed解决方式

{% endnote %}

```
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (C:\Users\myosotis\Desktop\Hexo_blog\node_modules\hexo-util\lib\spawn.js:51:21)
      at ChildProcess.emit (events.js:315:20)
      at ChildProcess.cp.emit (C:\Users\myosotis\Desktop\Hexo_blog\node_modules\cross-spawn\lib\enoent.js:34:29)
      at Process.ChildProcess._handle.onexit (internal/child_process.js:277:12) {
    code: 128
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```

从网上找了几个方法：

## 方法一

```
##进入站点根目录

##删除git提交内容文件夹
rm -rf .deploy_git/

##执行
git config --global core.autocrlf false

##最后
hexo clean && hexo g && hexo d
```

没能成功，仍然报错。

## 方法二

{% note success %}

修改_config.yml文件，将配置地址http方式切换成ssh方式

{% endnote %}

```
##进入站点根目录

##删除git提交内容文件夹
vim _config.yml

##修改
deploy:

type: git

repository: https://github.com/Uninfo/blog.github.io.git 
-> git@github.com:Uninfo/blog.github.io.git

branch: master

##最后
hexo clean && hexo g && hexo d
```

不习惯用vim，直接用编辑器打开`_config.yml`文件，将配置地址的http方式换成ssh方式。

解决，成功部署。

## 方法三

{% note success %}

强制上传，没试过，网上说不建议

{% endnote %}

```
##进入站点根目录

##进入depoly文件夹
cd .deploy_git/

##强制推送
git push -f
```

