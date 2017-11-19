---
layout: post
title: css的权重问题
date: 2017-09-15 21:31:26
tags: 
    深入理解css
categories:
	css
comments: true
---

最近这些日子一直忙着找实习，前两天滴滴面试官问道css权重问题，后来发现跟之前理解的还是有些区别，在这里简单总结一下。

<!-- more --> 

##css样式的优先级

    !important > 行间样式 > id选择器 > class选择器 > 标签选择器 > 通配符（*）

##css的权重问题

    当接触到复杂的选择器时我们就不能简单用优先级来进行判断了，这个时候就需要引入各种选择器的权重值来进行计算。
    * !important:                           无穷大
    * 行间样式:                               1000
    * id选择器:                                100
    * class选择器，属性选择器或伪类:              10
    * 元素和伪元素选择器:                         1

    当使用复杂选择器给元素添加样式时，最后样式的选择是不同样式对应选择器权重值进行加和比较的结果。
    <font color='red'>注意：这里的 无穷大+1 > 无穷大 </font>

    下面我们举个小栗子：
    这里我们写一个div:
    ```javascript
        <div class="div1 div2 div3 div4 div5 div6 div7 div8 div9 div10" id='id'><div>
    ```
        div {
			height: 100px;
			width: 100px;
		}

		#div1{
			background: blue;
		}

		.div1.div2.div3.div4.div5.div6.div7.div8.div9.div10.div11{
			background: yellow;
		}
    
    这里的div有10个class类名，并且对应的样式代码写在id选择器之后。
    div最后是这样的：
    <img src="/imgs/div.png" width="100" height="100"> 
    按照我们之前理解的权重计算规则10个class选择器并列选择同一个div权重为 10 * 10 = 100,同理id选择器权重值也正好是100，类选择器写在后面权重值相同
