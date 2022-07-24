---
title: footer绝对置底
date: 2018-01-29T0:25:00+08:00
comments: true
toc: true
tags:
 - 前端
---

今天在写三位一体计算器的时候想将footer绝对置底，以我仅有的CSS知识写了半天无果，便上网搜了一下，找到了知乎专栏的一篇文章解决了我的问题，便来记录一下。

<!-- more -->

### 方法一：footer高度固定+绝对定位

例：

```HTML
<body>
    <header>header</header>
    <main>content</main>
    <footer>footer</footer>
</body>
```

```CSS
html{height:100%;}
body{min-height:100%;margin:0;padding:0;position:relative;}

header{background-color: #ffe4c4;}
main{padding-bottom:100px;background-color: #bdb76b;}/* main的padding-bottom值要等于或大于footer的height值 */
footer{position:absolute;bottom:0;width:100%;height:100px;background-color: #ffc0cb;}
```

但这种方法在你的body里有一个`<h1>`之类的标签时，就会出现侧边滚动条。检查了一下，原因是`<h1>`这类标签有一个margin，如果将margin设置为0px，则不会有这种情况，但有时会影响美观。

### 方法二：footer高度固定+margin负值

例：

```HTML
<body>
    <header>header</header>
    <main>content</main>
    <footer>footer</footer>
</body>
```

```CSS
html,body{height:100%;margin:0;padding:0;}

.container{min-height:100%;}
header{background-color: #ffe4c4;}
main{padding-bottom:100px;background-color: #bdb76b;}/* main的padding-bottom值要等于或大于footer的height值 */
footer{height:100px;margin-top:-100px;background-color: #ffc0cb;}/* margin-top（负值的）高度等于footer的height值 */
```

这种方法亦同上述情况

### 方法三：footer高度任意+js

例：

```HTML
<body>
    <header>header</header>
    <main>main content</main>
    <footer>footer</footer>
</body>
```

```CSS
html,body{margin:0;padding: 0;}
header{background-color: #ffe4c4;}
main{background-color: #bdb76b;}
footer{background-color: #ffc0cb;}

/* 动态为footer添加类fixed-bottom */
.fixed-bottom {position: fixed;bottom: 0;width:100%;}
```

```javascript
$(function(){
    function footerPosition(){
        $("footer").removeClass("fixed-bottom");
        var contentHeight = document.body.scrollHeight,//网页正文全文高度
            winHeight = window.innerHeight;//可视窗口高度，不包括浏览器顶部工具栏
        if(!(contentHeight > winHeight)){
            //当网页正文高度小于可视窗口高度时，为footer添加类fixed-bottom
            $("footer").addClass("fixed-bottom");
        } else {
            $("footer").removeClass("fixed-bottom");
        }
    }
    footerPosition();
    $(window).resize(footerPosition);
});
```

最后我用了第三种方法成功解决了这一个问题

[原文地址](https://zhuanlan.zhihu.com/p/22936824)