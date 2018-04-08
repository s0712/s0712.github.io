---
title: 面向对象的CSS
date: 2018-04-08 17:32:27
tags: 面向对象 OOCSS
categories: CSS
---

你曾经听过这样一句话:"内容就是王道么?" 作为一个web开发者,因为工作的缘故经常与内容打交道,这可能是很正常不过了,对于我们已经是过渡使用,但是什么才能把游客吸引到一个网站才是真正的王道~

从一个web开发者的观点来看,一些人认为速度才是王道, 久而久之,我也开始认同这个观点,近年来许多有经验的前端工程师提供建议"如何才能通过一些最佳实践的方法提高用户体验度呢?"

不幸的是,众多开发者忽视了CSS的表现(认为他太过简单,是一种机械的动作),而把更多的关注在javascript上或者其他方面.

--

在这篇文章里,我将通过面向对象的css概念的介绍, 让你了解一些经常被忽视的地方是如何在改善用户体验和网页可维护性上面发挥作用的

### OOCSS面向对象的css原则

任何基于面向对象的代码方法,其主要目的都是鼓励代码的复用,OOCSS也是一样,讲究复用,并最终可以更快更高效的书写你的样式,同时方便日后的添加和维护.

根据oocss在github上的描述,oocss是基于 " 两个主要原则 "

#### 1.结构与样式的分离
几乎页面上的每个元素都有不同的视觉特效(也叫皮肤),他们在不同的环境中重复的在使用,思考一下,一个品牌的logo,他的颜色,细微的渐变或是可见的边框,另一方不是视觉特效(结构)也在不断的重复使用.

当把这些不同的特性抽象到一个基于类的模块里,他们变得可复用,可以与用到任何元素上,并且具有相同的效果,让我们来看一个代码的前后对比,你就明白我在说些什么了.

>在运用oocss的原则前,你的css可能是这个样子的:

```css

#button {
  width: 200px;
  height: 50px;
  padding: 10px;
  border: solid 1px #ccc;
  background: linear-gradient(#ccc, #222);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}

#box {
  width: 400px;
  overflow: hidden;
  border: solid 1px #ccc;
  background: linear-gradient(#ccc, #222);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}

#widget {
  width: 500px;
  min-height: 200px;
  overflow: auto;
  border: solid 1px #ccc;
  background: linear-gradient(#ccc, #222);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}


```