---
title: IE6经典bug
date: 2018-04-04 10:37:32
tags: BUG
categories: Bug填坑
---

1.IE6怪异解析之padding与border算入宽高.

> 原因: 未加文档声明造成非盒模型解析
>
> 解决方法: 加入文档声明<!DOCTYPE html>

2.IE6在块元素,左右浮动,设定margin时造成margin双倍(双边距)

> 解决方法:display:inline
> 

3.以下三种其实是同一种bug,其实也不算个bug,举个例子:父标签高度20,子标签11,垂直居中,20-11=9,9要分给文字的上面和下面,怎么分?IE6就会与其他的不同,所以,尽量避免.

> a.字体大小为奇数之边框高度少1px
> 
>  解决方法:字体大小设置为偶数或者line-height设为偶数
>
> b.line-height,文本垂直居中差1px,
> 
>  解决办法:padding-top代替line-height居中,或line-height加1减1
> 
> c.与父标签的宽度的奇偶不同的居中造成1px的偏离
> 
> 解决方法:如果父标签是奇数宽度,则子标签也用奇数宽度;如果父标签偶数宽度,则子标签也用偶数宽度
> 
> 

4.内部盒模型超出父级时,父级被撑大
> 解决方法:父标签使用overflow:hidden
> 

5.line-height默认行高bug
> 解决方法:line-height设值
> 

6.行标签之间会有一小段空白
> 解决方法:float或结构并排(可读性差,不建议)
> 

7.标签高度无法小于19px
> 解决方法:overflow:hidden
> 

8.左浮元素margin-bottom失效
> 解决方法:显示设置高度or 父标签设置padding-bottom代替子标签的margin-bottom or 在放个标签让父标签浮动,子标签margin-bottom,即(margin-bottom与float不同时作用于一个标签)
> 

9.img于块元素中,底边多出空白
> 解决方法:父级设置overflow:hidden;或img{display:Block;}或_margin:-5px;
> 

10.li之间会有间距
> 解决方法:float:left;
> 

11.块元素中有文字及有浮动的行元素,行元素换行
> 解决方法:将行元素置于块元素内的文字前
> 

12.position下的left,bottom错位
> 解决办法: 为父级(relative层)设置宽高或*zoom:1;
> 

13.子级中有设置position,则父级overflow失效
> 解决方法:为父级设置position:relative