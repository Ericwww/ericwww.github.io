---
title: js中的循环与遍历
date: 2019-04-15T00:00:00+08:00
comments: true
toc: true
tags:
 - 前端
---

前一个相对定位和绝对定位的坑还没动笔，然后昨天自己整理了一下js里的循环

## do-while语句

后测试循环语句，只有在循环体中的代码执行之后，才会测试出口条件，即无论如何，循环体中的代码都会执行一次。

```js
do{
    statement
}while(expression);
```

## while语句
前测试循环语句，在循环体内的代码初执行之前，就会对出口条件求值

```js
while(expression) statement
```

## for语句
前测试循环语句，但它具有在执行循环前初始化变量和定义循环后要执行的代码的能力。

#### 语法

```js
for(initializationl;expression;post-loop-expression) statement
```

#### 示例

```js
var count=10;
for(var i=0;i<count;i++){
    console.log(i);
}
```

可以用while改写以上代码

```js
var count=10;
var i=0;
while(i<count){
    console.log(i);
    i++;
}
```

while循环做不到的，for循环一样做不到。for循环只是把与循环有关的代码集中在了一个位置。

for语句的初始化表达式、控制表达式、循环后表达式都是可选的。若全省略便会创建一个无限循环。

## for-in语句
它是一种精准的迭代语句，可以用来枚举对象的属性。

```js
for(property in expression) statement
```

它会一直持续到所有属性都被枚举一遍，若要迭代的对象的变量值为null或undefined，for-in会抛出错误。

在ES5中更正了这一行为，对这种情况不再抛出错误，而只是不执行循环体。

## for-of语句
在可迭代对象上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值，执行语句。

### for-in与for-of
for-in语句以原始插入顺序迭代对象的可枚举属性，for-of语句遍历可迭代对象定义要迭代的数据。

使用for-in会遍历数组所有可枚举属性，包括原型。而for-of适用于遍历数组对象、字符串、map、set等拥有迭代器对象的集合。

## forEach方法
该方法属于引用类型Array的一个迭代方法

前段时间在大行星2.0开发的时候用到过这个方法，所以这里提一下

其余迭代方法可参考MDN

语法：

```js
arr.forEach(callback [, thisArg]);
```

> * callback:为数组中每个元素执行的函数，该函数接收三个参数
>   * currentValue:数组中正在处理的当前元素
>   * index:数组中正在处理的当前元素的索引
>   * array:`forEach()`正在操作的数组
> * thisArg:可选参数，当执行回调函数时，用作this的值