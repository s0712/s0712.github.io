---
title: BFC概念理解+布局规则+形成方法+用处
date: 2018-04-02 15:21:34
tags: CSS布局
categories: CSS
---

想要理解BFC与IFC,首先要先理解另外两个概念: <font color=red>Box</font>和<font color=red>FC</font>(即 formatting context)

### Box
--

CSS布局的基本单位

一个页面是有很多个 Box 组成的,元素的类型和 display 属性决定了这个 Box 的类型,不同类型的Box,会参与不同的 Formatting Context.

<font color=red>Block level</font> 的box会参与形成BFC, 比如<font color=red>display</font>的值为<font color=red>block , list-item ,table </font>的元素.

<font color=red>Inline level</font>的box会参与形成IFC, 比如<font color=red>display</font>的值为<font color=red>inline,inline-table,inline-block</font>的元素.

### FC (Formatting Context)
--

它是W3C CSS2.1 规范中的一个概念,定义的事页面中的一块渲染区域,并有一套渲染规则,它决定了其子元素将如何定位,以及其他元素的关系,和相互作用.

常用的<font color=red> Formatting Context</font>有:<font color=red> Block Formatting Context </font>(BFC | 块级格式化上下文) 和 <font color=red>Inline Formatting Context</font> (IFC | 行内格式化上下文).

下面就来介绍下 IFC 和 BFC的布局规则.

--

### IFC布局规则

--

> 在行内格式化上下文中,框(boxes)一个接一个的水平排列, 起点是包含块的顶部,水平方向上的 margin ,border 和 padding 在框之间得到保留, 框在垂直方向上可以以不同的方式对齐: 它们的顶部或是底部对齐,或根据其中文字的基线对齐. 包含那些框的长方形区域,会形成一行,叫做行框.
> 

--

### BFC布局规则

--

根据W3C整理成中文
> 1. 内部的Box会在垂直方向,一个接一个的放置.
> 2. Box垂直方向的距离由margin 决定,属于同一个BFC的两个相邻的Box 的margin 会发生重叠
> 3. 每个元素的左外边缘(margin-left),与包含块的左边( contain box left ) 相接触 (对于从左往右的格式化,否则相反) 即使存在浮动也是如此.除非这个元素自己形成了一个新的BFC.
> 4. BFC的区域不会与float box 重叠
> 5. BFC就是页面上的一个隔离的独立容器,容器里面的子元素不会影响到外面的元素.反之也如此.
> 6. 计算BFC的高度时,浮动元素也参与计算.
> 


## 怎样形成一个BFC?

>- 根元素
>- float 属性不为 none
>- position 为absolute 或fixed
>- display 为inline-block table-cell,table-caption, flex或inline-flex
>- overflow 不为visible

那么相信到这儿,你肯定会有个疑问: 为什么display:inline-block; 的元素是 inline level 的元素,参与形成IFC, 却能创建BFC?

后来个人理解,觉得答案可能是这样的,:inline-block 的元素的内部是一个BFC, 但是它本身可以和其他inline 元素一起形成IFC.


## BFC用处

#### 1.清除浮动
相信大家都有遇到过由于子元素都是浮动的,受浮动影响,父元素的高度 "塌陷了".将下面的代码拷贝 即可显示塌陷

```
CSS
   .container {
        border:1px solid blue;
    }
    .aside {
        width: 80px;
        height: 50px;
        float: left;
        background: pink;
    }
    .main {
        width: 100px;
        height: 70px;
        background: yellow;
        float: left;
    }
    
HTML
<div class="container">
        <div class="aside">aside</div>
        <div class="main">main</div>
    </div>

```
塌陷效果如网页显示,解决办法是:为 .containe加上 overflow:hidden; 使其形成BFC, 根据BFC规则第六条, 计算高度时,就会计算float的元素的高度,达到清除浮动影响的效果.


#### 2.布局: 自适应两栏布局

有一种布局问题,浮动的元素会遮挡其他兄弟元素,将下面代码拷贝,即可实现 "浮动元素遮挡"

```
CSS
 .container {
    	width: 300px;
        border:1px solid blue;
        
    }
    .aside {
        width: 80px;
        height: 50px;
        float: left;
        background: pink;
    }
    .main {
        width: 100px;
        height: 70px;
        background: yellow;
    }
 HTML
 <div class="container">
        <div class="aside">aside</div>
        <div class="main">main</div>
   </div>

```

在网页上即可看到车祸现场效果,解决办法: 为 .main 加上 overflow:hidden; 触发main 元素的BFC, 根据规则的第四条,第五条,BFC 的区域是独立的,不会与页面的其他元素相互影响, 且不会与float 元素重叠, 因此就可以形成两列的自适应布局.

#### 3.防止垂直margin合并

应该在布局的时候都有遇到过这样的问题,两个相邻的元素的在垂直方向上的margin值被合并了,如下面代码实现显示,本来 hehe,haha 之间的垂直外边距和应该是200px,但现在只显示了100px,那么为什么呢?

```
CSS
p{
		color:#f55;
		background: #fcc;
		width: 200px;
		line-height: 100px;
		text-align: center;
		margin:100px;
		border:1px solid red;
	}
HTML
<p>haha</p>
<p>hehe</p>

```

那么上面的问题是如何产生的呢?因为他们的外边距相遇发生了合并~

怎么解决? 为其中的一个元素的外面包裹一层父元素, 并为这个父元素设置overflow:hidden; 使其形成BFC, 因为BFC内部是一个独立的容器, 所以不会与外部相互影响, 可以防止margin 合并.代码如下

```
HTMl
<p>haha</p>
<div class="wrap"><p>hehe</p></div>
CSS
.wrap{overflow:hidden}
```
为什么加了overflow:hidden就可以把边框加上? 
因为生成了BFC, 生成BFC后,会让内部元素参与计算,一旦父元素生成了BFC规则, 那么其所有子元素都会参与计算

以上就是BFC布局的解释及场景.👏