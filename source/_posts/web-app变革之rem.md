---
title: web_app变革之rem
date: 2017-08-30 09:23:29
tags: [css3, javascript]
categories: css
---

## rem 是什么？

rem(font size of root element) 是指相对于元素的字体大小单位。 简单的说它就是一个相对单位; 看到rem我们一定会想起em这个单位， em(font size of the element) 是指相对于父元素的字体单位(大小),他们之间其实很相似，只不过计算规则是依赖元素是一个依赖父元素计算。

## 为什么web_app要使用rem？

这里我特别强调web app，web page就不能使用rem吗，其实也当然可以，不过出于兼容性的考虑在web app下使用更加能突显这个单位的价值和能力，接下来我们来看看目前一些企业的web app是怎么做屏幕适配的。

### 1.实现强大的屏幕适配布局：

我们现在在切页面布局的使用常用的单位是px，这是一个绝对单位，web app的屏幕适配有很多中做法，例如：流式布局、限死宽度，还有就是通过响应式来做，但是这些方案都不是最佳的解决方法。

流式布局的解决方案有不少弊端，它虽然可以让各种屏幕都适配，但是显示的效果极其的不好，因为只有几个尺寸的手机能够完美的显示出视觉设计师和交互最想要的效果。

在页面布局的时候都是通过百分比来定义宽度，但是高度大都是用px来固定住，所以在大屏幕的手机下显示效果会变成有些页面元素宽度被拉的很长，但是高度还是和原来一样，实际显示非常的不协调，这就是流式布局的最致命的缺点，往往只有几个尺寸的手机下看到的效果是令人满意的，其实很多视觉设计师应该无法接受这种效果，因为他们的设计图在大屏幕手机下看到的效果相当于是被横向拉长来一样。

流式布局并不是最理想的实现方式，通过大量的百分比布局，会经常出现许多兼容性的问题，还有就是对设计有很多的限制，因为他们在设计之初就需要考虑流式布局对元素造成的影响，只能设计横向拉伸的元素布局，设计的时候存在很多局限性。

### 2.固定宽度做法

 还有一种是固定页面宽度的做法，早期有些网站把页面设置成320的宽度，超出部分留白，这样做视觉，前端都挺开心，视觉在也不用被流式布局限制自己的设计灵感了，前端也不用在搞坑爹的流式布局。但是这种解决方案也是存在一些问题，例如在大屏幕手机下两边是留白的，还有一个就是大屏幕手机下看起来页面会特别小，操作的按钮也很小，手机淘宝首页起初是这么做的，但近期改版了，采用了rem。

 ### 3.响应式布局

 响应式这种方式在国内很少有大型企业的复杂性的网站在移动端用这种方法去做，主要原因是工作大，维护性难，所以一般都是中小型的门户或者博客类站点会采用响应式的方法从web page到web app直接一步到位，因为这样反而可以节约成本，不用再专门为自己的网站做一个web app的版本。

 ### 4设置viewport进行缩放

 天猫的web app的首页就是采用这种方式去做的，以320宽度为基准，进行缩放，最大缩放为320*1.3 = 416，基本缩放到416都就可以兼容iphone6 plus的屏幕了，这个方法简单粗暴，又高效。说实话我觉得他和用接下去我们要讲的rem都非常高效，不过有部分同学使用过程中反应缩放会导致有些页面元素会糊的情况。

 ## rem能等比例适配所有屏幕

 ## js代码实现

``` javascript 	
(function(doc, win) {
var docEl = doc.documentElement,
isIOS = navigator.userAgent.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/),
dpr = isIOS ? Math.min(win.devicePixelRatio, 3) : 1,
dpr = window.top === window.self ? dpr : 1, //被iframe引用时，禁止缩放
resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize';
docEl.dataset.dpr = dpr;
var recalc = function() {
var width = docEl.clientWidth;
if (width / dpr > 1080) {
width = 1080 * dpr;
}
docEl.dataset.width = width
docEl.dataset.percent = 100 * (width / 1080);
docEl.style.fontSize = 100 * (width / 1080) + 'px';
};
recalc()
if (!doc.addEventListener) return;
win.addEventListener(resizeEvt, recalc, false);
})(document, window);

```