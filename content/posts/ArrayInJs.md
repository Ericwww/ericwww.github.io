---
title: JavaScript中的引用类型Array
date: 2019-05-11T00:00:00+08:00
comments: true
toc: true
tags:
 - 前端
---
js数组的每一项可以保存任何类型的数据。也就是说，可以用数组的第一个位置来保存字符串，第二个位置来保存数组，用第三个位置保存对象。并且，数组的大小是可以动态调整的。

创建数组的基本方式有两种。第一种是使用Array构造函数

```js
var colors=new Array();
```

若预先知道数组要保存的项目的数量，也可给构造函数传递Length的值。

```js
var colors=new Array(20);
```

也可以向Array函数传递数组中应该包含的项

```js
var colors=new Array("red","blue","green");
```

当然，给构造函数传递一个值，也可以创建数组，但情况会比较复杂。

```js
var colors=new Array(3); //创建一个长度为3的空数组
var names=new Array("grey");
```

第二种方法是使用数组字面量表示法，数组字面量由一对包含数组项的方括号去表示，多个数组项之间以逗号隔开。

```js
var colors=new Array["red","blue","green"];
var names=[];
var values=[1,2,]; //Don't do this 这样会创建一个2或3项的数组
var options=[,,,,,]; //Don't do this 这样会创建一个5或6项的数组
```

### 方法
#### 转换方法
数组继承的toLocaleString()、toString()和valueOf()方法，在默认情况下会以逗号分隔的字符串的形式返回数组项。而如果使用join()方法，则可以使用不同的分隔符来构建这个字符串。
join()方法只接收一个参数，即用作分隔符的字符串，然后返回包含所有数组项的字符串。

```js
var colors=["red","green","blue"];
console.log(colors.join(",")); //red,green,blue
console.log(colors.join("||")); //red||green||blue
```

#### 栈方法
数组可以表现得像栈一样。栈是一种LIFO（Last-In-First-Out）的数据结构。栈中项的插入和移除只发生在一个位置——栈的顶部。

push()方法可以接收任意数量的参数，把它们逐个添加到数组的末尾，并返回修改后的数组的长度。
pop()方法则从数组末尾移除最后一项，减少数组Length的值，然后返回移除的项。

```js
var colors=new Array();
var count=colors.push("red","green");
console.log(count); //2
count=colors.push("black");
console.log(count); //2
var item=colors.pop();
console.log(item); //"black"
console.log(colors.length); //2
```

#### 队列方法
栈数据结构的访问规则是LIFO。而队列数据结构的访问规则是FIFO（First-In-First-Out）。队列在列表末端添加项，从列表的前端移除项。
shift()方法能够移除数组中的第一个项并返回该项，同时将数组长度减1。结合push()和shift()，可以像使用队列一样使用数组。

```js
var colors=new Array();
var count=colors.push("red","green");
console.log(count); //2
count=colors.push("black");
console.log(count); //3
var item=colors.shift();
console.log(item); //"red"
console.log(colors.length); //2
```

同时还有一个unshift()方法，它能在数组前端添加任意个项并返回新的数组长度。因此，同时使用unshift()和pop()方法，可以从相反的方向来模拟队列，即在数组的前端添加项，从数组末端移除项。

#### 小结
> push()方法从数组末端添加项，可一次性添加多个，并返回修改后数组的长度
> pop()方法从数组末端移除项，一次只可移除一个并返回移除的项的值
> unshift()方法从数组前端添加项，可一次性添加多个，并返回修改后数组的长度
> shift()方法从数组前端移除项，一次只可移除一个并返回移除的项的值

#### 操作方法
concat()方法可以基于当前数组中的所有项创建一个新数组。具体来说，这个方法会先创建当前数组的一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。在没有给concat()方法传递参数的情况下，它只是复制当前数组并返回副本。如果传给concat()方法的是一个或多个数组，则该方法会将这些数组中的每一项都添加到结果数组中。如果传递的不是数组，这些值就会被简单地添加到结果数组的末尾。

```js
var colors=["red","green","blue"];
var colors2=colors.concat("yellow",["black","brown"]);
console.log(colors); //red,green,blue
console.log(colors2); //red,green,blue,yellow,black,brown
```

slice()方法能够基于当前数组中的一个或多个项创建一个新数组。slice()方法可以接受一个或两个参数，即要返回项的起始和结束位置。
在只有一个参数的情况下，slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项，但不包括结束位置的项。注意！slice()方法不会影响原数组。

```js
var colors=["red","green","blue","yellow","purple"];
var colors2=colors.slice(1);
var colors3=colors.slice(1,4);
console.log(colors); //"red", "green", "blue", "yellow", "purple"
console.log(colors2); //"green", "blue", "yellow", "purple"
console.log(colors3); //"green", "blue", "yellow"
```

splice()方法有多种用法。其主要用途是向数组的中部插入项，但使用这种方法的方式则有如下3种。


> * **删除**：可以删除任意数量的项，只需指定2个参数：要删除的第一项的位置和要删除的项数。例如，splice(0,2)会删除数组中的前两项。
> * **插入**：可以向指定位置插入任意数量的项，只需提供3个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。例如，splice(2,0,”red”,”green”)会从当前数组的位置2开始插入字符串”red”和”green”。
> * **替换**：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice(2,1,”red”,”green”)会删除当前数组位置2的项，然后再从位置2开始插入字符串”red”和”green”。

```js
var colors=["red","green","blue"];
var removed=colors.splice(0,1);
console.log(colors); //"green", "blue"
console.log(removed); //"red"
removed=colors.splice(1,0,"yellow","orange");
console.log(colors); //"green", "yellow", "orange", "blue"
console.log(removed); //空数组
removed=colors.splice(1,1,"red","purple");
console.log(colors); //"green", "red", "purple", "orange", "blue"
console.log(removed); //"yellow"
```

#### 位置方法
有两个位置方法，indexOf()和lastIndexOf()。这两个方法都接收两个参数：要查找的项和表示查找起点位置的索引（可选）。其中indexOf()方法从数组的开头开始向后查找，lastIndexOf()方法则从数组的末尾开始向前查找。
这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回-1。在比较第一个参数与数组中的每一项时，会使用全等操作符。

#### 迭代方法
数组有5个迭代方法。每个方法都接收两个参数：要在每一项上运行的函数和运行该函数的作用域对象——影响this的值（可选）。传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。根据使用的方法不同，这个函数执行后的返回值可能会也可能不会影响方法的返回值。以下是5个迭代方法。

> * every():对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。
> * filter():对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。
> * forEach():对数组中的每一项运行给定函数，这个方法没有返回值。
> * map():对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
> * some():对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true。
> **以上方法都不会修改数组中的包含的值**

对every()来说，传入的函数必须对每一项都返回true，这个方法才返回true；否则，它就返回false。而some()方法则是只要传入的函数对数组中的某一项返回true，就会返回true。

```js
var numbers=[1,2,3,4,5,4,3,2,1];
var everyResult=numbers.every(function(item,index,array){
    return (item>2);
});
console.log(everyResult);
var someResult=numbers.some(function(item,index,array){
    return (item>2);
});
console.log(someResult);
```

filter()函数利用指定的函数确定是否在返回的数组中包含某一项。

```js
var numbers=[1,2,3,4,5,4,3,2,1];
var filterResult=numbers.filter(function(item,index,array){
    return (item>2);
});
console.log(filterResult);
```

map()也返回一个数组，而这个数组的每一项都是在原始数组中的对应项上运行传入函数的结果。

```js
var numbers=[1,2,3,4,5,4,3,2,1];
var mapResult=numbers.map(function(item,index,array){
    return item*2;
});
console.log(mapResult);
```

forEach()方法对数组中的每一项运行传入的函数。这个方法没有返回值，本质上与使用for循环迭代数组一样

```js
var numbers=[1,2,3,4,5,4,3,2,1];
numbers.forEach(function(item,index,array){
    //执行某些操作
});
```

#### 归并方法
reduce()和reduceRight()两个方法都会迭代数组的所有项，然后构建一个最终返回的值。其中reduce()方法从数组的第一项开始，逐个遍历到最后。而reduceRight()则从数组的最后一项开始，向前遍历到第一项。
这两个方法都接收两个参数：一个在每一项上调用的函数和作为归并基础的初始值（可选）。传给reduce()和reduceRight()的函数接收四个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数就是数组的第二项。

```js
var values=[1,2,3,4,5];
var sum=values.reduce(function(prev,cur,index,array){
    return prev+cur;
});
console.log(sum);
```