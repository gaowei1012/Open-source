---
title: Hexo Next主题美化
date: 2017-08-30 14:33:34
tags: [hexo, 美化]
categories: Hexo
---

## 语言设置

将languages目录下面的zh-Hans.yml修改为zh-CN.yml或者按照文档中的修改根目录配置文件也行

## 修改样式

### 1.网站标题栏背景颜色
当使用Pisces主题时，网站标题栏背景颜色是黑色的，感觉不好看，可以在source/css/_schemes/Pisces/_brand.styl中修改

``` css
.site-meta {
  padding: 20px 0;
  color: white;
  background: $blue-dodger; //修改为自己喜欢的颜色

  +tablet() {
    box-shadow: 0 0 16px rgba(0,0,0,0.5);
  }
  +mobile() {
    box-shadow: 0 0 16px rgba(0,0,0,0.5);
  }
}
```

但是，我们一般不主张这样修改源码的，在next/source/css/_custom目录下面专门提供了custom.styl供我们自定义样式的，因此也可以在custom.styl里面添加：

``` css
// Custom styles.
.site-meta {
  background: $blue; //修改为自己喜欢的颜色
}
```
### 2.修改内容区域的宽度

我们用Next主题是发现在电脑上阅读文章时内容两边留的空白较多，这样在浏览代码块时经常要滚动滚动条才能阅读完整，体验不是很好，下面提供修改内容区域的宽度的方法。 
NexT 对于内容的宽度的设定如下：

	- 700px，当屏幕宽度 < 1600px
	- 900px，当屏幕宽度 >= 1600px
	- 移动设备下，宽度自适应

如果你需要修改内容的宽度，同样需要编辑样式文件。 
在Mist和Muse风格可以用下面的方法： 
编辑主题的 source/css/_variables/custom.styl 文件，新增变量：

``` css
// 修改成你期望的宽度
$content-desktop = 700px

// 当视窗超过 1600px 后的宽度
$content-desktop-large = 900px

```
当怒使用pisces风格时可以用下面的方法：

``` css 
header{ width: 90%; }
.container .main-inner { width: 90%; }
.content-wrap { width: calc(100% - 260px); }
```

### 3.主页显示文章摘要

默认情况下在主页是把文章内容全部显示出来的，如果在根目录的_config.yml中配置分页:

``` md 
per_page: 10
```
那么每页显示10篇文章，这样的花主页就看起来不是很精简了，可以通过下面的方法来配置主页只显示文章摘要： 
在themes/next/的_config.yml中配置：

``` md 
# Automatically Excerpt. Not recommand.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: false 
  length: 150
```

把enable项配置为true就可以了。 
但是我们也可以看到注释中是不推荐这样做的，因为这样会强制把文章前150个字符做为摘要的，会出现描述不完整的情况，而且有时候会把文章的源码显示出来。 
那么我们就用推荐的在文章中用<!-- more -->这种方式来作为文章摘要的方式，可以根据每篇文章的不同情况自己来把控摘要的内容。 
或者是在文章中配置description也是可以的。

``` md 
---
title: Hexo+GitHub搭建个人博客
categories: Hexo
comments: true
keywords: Hexo, Blog, GitHub
tags: [Hexo, Blog, GitHub]
description: 使用Hexo在GitHub上搭建个人博客
date: 2017-01-010 13:00:00
---
```

### 4.阅读次数统计(使用LeanCloud)

参考[文档](http://theme-next.iissnan.com/third-party-services.html)

### 5.加入网站缩略图标

加入后就可以在浏览器的标签栏或者是收藏夹里面现实网站的缩略图标了。 
在themes/next/的_config.yml中配置：

``` md
# Put your favicon.ico into `hexo-site/source/` directory.
favicon: /images/favicon.ico
```
然后把图表放到根目录的source/images/下面.

### 6.添加友情链接页面

和前面的添加关于页面、分类页面类似，不同的是Next主题默认是没有这个页面的，需要我们自己添加一些东西，比如文字翻译，添加对应图标等。 

首先通过下面的命令添加一个页面：

``` md
hexo new pags links
```
 会生成source/links/index.md文件.
 在next主题配置文件中添加：

``` md
 menu:
  home: /
  ......
  about: /about
  links: /links
```

 添加图标：

``` md
 menu_icons:
  enable: true
  ......
  links: users
```

这里如果找到合适的图标呢？可以到[fontawesome.io](http://fontawesome.io/)这个网站来找 ，Next里面用的就是这里面的图标。 
下一步就是要添加翻译了，要不在菜单栏现实的是 menu.links。 
在next/languages/ 下面编辑对应的语言，添加翻译。 
比如在zh-CN.yml中添加：

``` md
menu:
  ...
  links: 友情链接
```

然后可以随意编辑source/links/index.md文件就可以了.



