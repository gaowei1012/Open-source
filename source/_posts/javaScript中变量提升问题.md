---
title: javaScript中变量提升问题
date: 2017-08-28 12:00:42
tags: [javascript]
categories: 前端面试
---
Scoping in javascript:
首先我们来看两道简单的题目:

``` javascript
var foo = 1;
function bar() {
  if (!foo) {
    var foo = 10;
  }
   consonle.log(foo); //10 
}

bar();
var a = 1;
function () {
  a = 10;
  return;
  function a() {
  }
}

b();
alert(a);//1
```

再来看一个栗子

``` javascript
var x = 1;
console.log(x); // 1
if (true) {
    var x = 2;
    console.log(x); // 2
}
console.log(x); // 2
```

输出结果为 1 , 2, 2 if代码块中的覆盖了全局变量,那是因为javascript是一种函数级作用域(function-level scope）所以if中并没有独立维护一个scope,变量x影响到了全局变量x

C C++ Java 中都是块级作用域,在javascript中我们怎么实现类似于一个这样的效果呢? 答案是 闭包:

``` javascript
function foo() {
    var x = 1;
    if (x) {
        (function () {
            var x = 2;
            // some other code
        }());
    }
    // x is still 1.
}
```

上面代码在if条件块中创建了一个闭包，它是一个立即执行函数，所以相当于我们又创建了一个函数作用域，所以内部的x并不会对外部产生影响
Hoisting in javascript:
在javascript中, 一般变量进入一个作用域可以通过以下几种方式:
语言自定义变量: 所有的作用域中都存在this和arguments这两个默认值
函数形参: 函数的形参存在函数作用域中
函数声明: function foo () {}
变量定义: var foo
其中, 在代码运行前,函数声明和变量定义通常会被解释器移动到起所在作用域的顶部, 如何理解这句话呢?

``` javascript
function foo(){
  bar();
  var x = 1;
}
```

上面这段代码被解释成下面这段代码:

```
function foo() {
  var x ;
  bar();
  x =1;
}
```

x 变量被移动到函数最顶部,然后在bar()后,在对其赋值,再来看下面这两段代码,其实是等价的:

``` javascript
function foo() {
  if (false) {
    var x = 1;
  }
    return ;
    var y = 1;
}

function foo() {
  if (false) {
    var x = 1;
  }
    return ;
    var y = 1;
}
```
所以变量的上升（Hoisting）只是其定义上升，而变量的赋值并不会上升。
我们都知道，创建一个函数的方法有两种，一种是通过函数声明function foo(){}
另一种是通过定义一个变量var foo = function(){} 那这两种在代码执行上有什么区别呢？
看栗子:

``` javascript
function test() {
    foo(); // TypeError "foo is not a function"
    bar(); // "this will run!"
    var foo = function () { // function expression assigned to local variable 'foo'
        alert("this won't run!");
    }
    function bar() { // function declaration, given the name 'bar'
        alert("this will run!");
    }
}
test();
```

在这个例子中，foo()调用的时候报错了，而 bar() 能够正常调用
我们前面说过变量会上升，所以var foo首先会上升到函数体顶部，然而此时的foo为undefined,所以执行报错。而对于函数bar, 函数本身也是一种变量，所以也存在变量上升的现象，但是它是上升了整个函数，所以bar()才能够顺利执行。

再回到一开始我们提出的两个例子，能理解其输出原理了吗？
 
``` javascript
var foo = 1;
function bar() {
    if (!foo) {
        var foo = 10;
    }
    alert(foo);
}
bar();
```

其实就是：

``` javascript
var foo = 1;
function bar() {
    var foo;//提升变量声明
    if (!foo) {
        foo = 10;
    }
    alert(foo);
}
bar();
var a = 1;
function b() {
    a = 10;
    return;
    function a() {}
}
b();
alert(a);
```

其实就是：

``` javascript
var a = 1;
function b() {
    function a() {} //提升函数提,(函数声明也是变量)
    a = 10;
    return;
}
b();
alert(a);
```

ES6中有何区别?
在ES6中存在 let 关键字, 它声明的变量同样存在块级作用域:
而函数本身的作用域,只存在所在的作用域块级内.
上面这段代码在ES5中的输出结果为I am inside!因为f被条件语句中的f上升覆盖了。
在ES6中的输出是I am outside!块级中定义的函数不会影响外部。