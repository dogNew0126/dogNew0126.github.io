---
title: 在VScode中用js写webgl显示代码提示
date: 2022-07-11 10:44:45
tags:
  - WebGL
categories:
  - WebGL
index_img: https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E5%8A%A8%E7%89%A9%2F%E8%80%81%E8%99%8E%2F324fe9d469659ee390fc23f3c9c78576496783876.jpg%401554w.webp
description: 在VSCode中用js写WebGL显示代码提示
---

# 在VScode中用js写webgl显示代码提示

用VSCode写WebGL时发现缺少提示信息，没有代码补全。在网上看到一篇博客说是使用document.getElementById获取的对象vscode不知道是什么类型，提示为any类型。

如果在document.getElementById的上一行加入

`/** @type {HTMLCanvasElement} */`

这样再看canvas的类型就显示出来了，也有代码补全功能。

另外如果在函数中想使用传入的参数，给这个参数加入代码提示，则在对应的函数前加上：

`/** @param {!WebGLRenderingContext} gl */`

这样在函数中使用gl的时候也会有正确的代码提示。
