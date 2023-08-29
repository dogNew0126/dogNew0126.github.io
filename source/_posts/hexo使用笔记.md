---
title: hexo使用笔记
date: 2022-05-10 23:13:26
tags:
    - hexo
categories: 
    - hexo
description: 关于hexo的命令
index_img: https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E6%99%AF%E8%89%B2%2F3D7CBB22BB349FF59B2412A97D14355A.png
---
# hexo笔记
## 写作命令
1. 新建分页: `hexo new page 名称`
2. 新建文章: `hexo new 名称` 或 `hexo n 名称`
3. 新建草稿: `hexo new draft 名称` 或 `hexo n draft 名称`
4. 草稿生成文章: `hexo publish 名称` 或 `hexo p 名称`
5. 草稿生成分页: `hexo publish page 名称` 或 `hexo p 名称`  

## 操作命令
1. 清除已生成文件: `hexo clean`
2. 运行本地服务器（预览）: `hexo s`
3. 运行本地服务器（预览草稿）: `hexo s --drafts`
4. 生成静态文件: `hexo g`
5. 部署到服务器: `hexo d`
6. 生成并部署文件: `hexo g -d` 或 `hexo d -g`  

## 常见事项
1. 多标签
```
tags: 
- 标签1
- 标签2
```
或`tags:[标签1，标签2]`
2. 设置阅读全文
+ 方法1：在文章中使用`<!--more-->`手动进行截断,Hexo提供的方式（推荐）
+ 方法2：在文章的front-matter中添加description,并提供文章摘录
+ 方法3：自动形成摘要,在主题配置文件中修改：
```
auto_excerpt:
    enable:true
    length:150
```
默认截取的长度为150字符，可以根据需要自行设定
>建议使用`<!--more-->`(及第一种方式)，除了可以精确控制需要显示的展露内容以外，这种方式也可以让Hexo中的插件更好的识别
