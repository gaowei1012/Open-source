---
title: Hexo之next主题设置首页不显示全文(只显示预览)
date: 2017-09-01 21:55:01
tags: [hexo, 美化]
---

- 进入hexo博客项目的themes/next目录
- 用文本编辑器打开_config.yml文件
- 搜索"auto_excerpt",找到如下部分：

``` md 
# Automatically Excerpt. Not recommand.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
enable: false
length: 150

```
- 把enable改为对应的false改为true，然后hexo d -g，再进主页，问题就OK！