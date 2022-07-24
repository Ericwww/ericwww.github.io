---
title: 行内元素和块元素
date: 2019-04-15T00:00:00+08:00
comments: true
toc: true
tags:
 - 前端
---

这几天在学长学姐的一个公司那边跟着学前端，基本都还是写一个界面为主，在写CSS的时候就碰到了行内元素和块元素这两个知识点，这里做个整理，方便以后回顾。

### 行内元素（inline）
> * 内容决定所占空间的多少，内容多少就占多少空间
> * 不能换行
> * 不能设置宽高（默认宽高是0）
> * margin垂直方向无效（margin-top,margin-bottom）,若设置垂直方向用line-height属性
> * 当多个行内元素排列在一起，他们之间会出现空格

#### 常见行内元素有
`a` `buttun` `span` `input` `label` 等，具体可查阅行内元素

### 块级元素（block）
> * 不管内容多少，块元素都会在浏览器中独占一行，即有多个块元素时，会从上至下排列
> * 可设置宽高，如果不设置宽高那么它的宽度是100%，即占满父级元素一整行
> * 相邻外边距会重叠，比如margin-bottom和margin-top重叠（可用padding或border解决）

#### 常见的块级元素有
`div` `h1-h6` `p` `ul` `li` 等，具体可查阅块级元素

### 行内块元素（inline-block）
> * 行内块状元素综合了行内元素和块状元素的特性，但是各有取舍。因此行内块状元素在日常的使用中，由于其特性，使用的次数也比较多。
> * 不自动换行
> * 能够识别宽高
> * 默认排列方式为从左到右

我们可以通过设置display的属性，来操纵一个元素是哪类元素。

```css
display: inline;
display: block;
display: inline-block;
```
### 相关属性的适用性
text-align：此属性在水平方向上对行内（inline），行内块（inline-block）以及文字都起作用（行内元素或行内块元素设置水平居中用text-align属性）
line-height：此属性可对行内元素设置垂直居中，使line-height和height相等即可
vertical-align：此属性可对行内块设置垂直居中

对块元素的水平和垂直布局，我现在更偏向于使用flex布局（躺）

