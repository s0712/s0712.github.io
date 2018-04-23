---
title: CSS日常规范及注意
date: 2018-04-04 14:09:31
tags: CSS规范,弹性盒模型,移动端px与rem
categories: CSS
---

### 基于移动端的PX与REM转换兼容方案

- different size  different DPR

- 目前的设计稿一般是640 750 1125, 一般要先均分成100份.(兼容vh,vw) 750/10=75px  div宽是240px*120px的css书写就改为3.2rem*1.6rem , 配合响应式修改html根的大小.

- 字体不建议使用rem,data-dpr属性动态设置字体大小,屏幕变大放更多的文字,或者屏幕更大放更多的字.

- 神奇的padding/margin-top等比例缩放间距.

---

### ICON-FONT与常用字体排版

- no-image时代,不超过纯色为2的图像

- 不要只写中文字体名,保证西文字体在中文字体前面,Mac->linux->windows

- 切忌不要直接使用设计师PSD 的设计font-family, 关键时刻在去启用font-face

- font-family:sans-serif;系统默认,字体多个单词组成加引号

---

### 常用CSS字体icon网址

http://cssicon.space/#/

---

### CSS代码检测团队项目规范

1. 不要使用多个class选择元素, 如 a.foo.boo, 这在ie6及以下不能正确解析
2. 移除空的css规则,如a{}
3. 正确的使用显示属性,如display:inline 不要和width,height,float,margin,padding同时使用,display:inline-block 不要和float同时使用等.
4. 避免使用过多的浮动,当浮动次数超过十次,会显示警告
5. 避免使用过多的字号,当字号声明超过十次时,会显示警告
6. 避免使用过多web字体,当使用超过五次时,显示警告
7. 避免使用id作为样式选择器
8. 标题元素只定义一次
9. 使用width:100%时要小心
10. 属性值为0时,不要写单位
11. 各浏览器专属的css属性要有规范,例如 .foo{-moz-border-radius:5px;border-radius:5px}
12. 避免使用看起来像正则表达式的css3选择器
13. 遵守盒模型规则

> 这里推荐hint.css来为css做检验
> 


---

### BFC IFC GFC FFC

- BFC就是页面上的一个隔离的独立容器,容器里面的子元素不会影响到外面的元素,反之也如此
- IFC(inline Formatting Contexts)直译为 "内联格式化上下文", IFC的line box(线框) 高度由其包含行内元素中最高的实际高度计算而来(不受到竖直方向的padding/margin影像)
- FFC(Flex Formatting Contexts) 直译为 "自适应格式化上下文", display值为flex或者inline-flex的元素将会生成自适应容器(flex container)
- GFC(GridLayout Formatting Contexts) 直译为 "网格布局格式化上下文",当为一个元素设置diaplay值为grid的时候,此元素将会获得一个独立的渲染区域,我们可以通过在网格容器(grid Container) 上定义网格丁一航(grid definition row) 和网格定义列(grid definition columns)属性各在网格项目(grid item )上定义网格行(grid row)和网格列(grid columns) 为每一个网格项目(grid item ) 定义位置和空间


> 先简单介绍下  之后会每一个会开单独的文章进行细节介绍 白白~