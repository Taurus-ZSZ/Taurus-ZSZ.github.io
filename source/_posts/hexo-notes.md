---
title: Hexo 博客学习笔记
date: 2021-10-06 07:31:12
tags:
      - hexo
categories:
      - blog
---

# Hexo 博客学习笔记



## 诡异的部分图片不显示

在写的博客中添加了图片并在hexo中添加了一个图片路径转换的插件

```shell
npm install https://github.com/CodeFalling/hexo-asset-image --save
```

但是在我的markdown写的blog中设置了图片缩放的图片可以正常显示，但是含有中文路径的图片在typroa中不缩放设置在本地浏览中图片不会显示。

> 解决方法：

将*.md文件重命名，将存放图片的相对文件夹重命名 全都重命名成英文的，但是在.md中设置title时可以设置成中文的名字。在重新生成一下就可以了！！！！！

```shell
hexo clean
hexo genertae
hexo server
hexo deploy # 可以简写 hexo d
```



