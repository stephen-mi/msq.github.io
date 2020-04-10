---
layout:     post
title:      快速理解js中的call,apply的作用
subtitle:   js中call,apply用法
date:       2019-01-16
author:     MSQ
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - javascript
---


今天被人问到js中的call，apply的区别和用途，解释了一番后，想到之前在逼乎上看到一位小伙伴生动形象的解释，本身不难理解，看下MDN就知道了，但是不常用，遇到了，还要脑回路回转下。或者时间长了，还是要确定下去看下文档，为了方便记忆：

  猫吃鱼，狗吃肉，奥特曼打小怪兽。
  
  有天狗想吃鱼了
  
  猫.吃鱼.call(狗，鱼)
  
  狗就吃到鱼了
  
  猫成精了，想打怪兽
  
  奥特曼.打小怪兽.call(猫，小怪兽)
  
  或者 马云.赚钱.call(我)



还有一位杨志大佬解释的更清楚

我们要先明白存在call和apply的原因，才能记得牢一点：

在javascript OOP中，我们经常会这样定义：
```
function cat(){
}
cat.prototype={
    food:"fish",
    say: function(){
        alert("I love "+this.food);
    }
}
var blackCat = new cat;
blackCat.say();
```
但是如果我们有一个对象 whiteDog = {food:"bone"}, 我们不想对它重新定义say方法，

那么我们可以通过call或apply用blackCat的say方法：blackCat.say.call(whiteDog);

所以，可以看出call和apply是为了动态改变this而出现的，当一个object没有某个方法，但是其他的有，我们可以借助call或apply用其它对象的方法来操作。

用的比较多的，通过document.getElementsByTagName选择的dom 节点是一种类似array的array。

它不能应用Array下的push,pop等方法。我们可以通过：
```
var domNodes = Array.prototype.slice.call(document.getElementsByTagName("*"));
```
这样domNodes就可以应用Array下的所有方法了。

其他的就不提了，讲多了反而迷惑。

call 和apply的用法都是一样的，知识传参不同，apply是数组，call是参数

就这样记住了。