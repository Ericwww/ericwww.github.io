---
title: 尝试找出ET和LT的换算关系
date: 2019-01-04T00:00:00+08:00
lastmode: 2019-03-11T00:00:00+08:00
toc: true
tags:
 - Record
---

一到考试周就不想复习，想敲代码，这一次也不例外。

这次想着做一个狒狒4.4新版本的限时采集计算器，每次上线想采集还要再打开wiki看一下真的好烦的【躺

如果能做一个本地计算的就舒服了

之前在研究天气预报的时候，知道了ET和LT之间是满足一定关系的，天气预报也是通过像是一种seed的一个区间来进行播报的

众所周知，艾欧泽亚的限时采集点一般都刷在ET的整点，那么就有了以下思路

输入当前时间戳，通过ET和LT的关系，计算出ET，然后去匹配是在哪个限时采集点的区间内，再返回采集点地图和坐标

然后就再去看了一下獭獭机器人的关于天气预报方面的代码

```python
def calculateForecastTarget(unixSeconds): 
    # Thanks to Rogueadyn's SaintCoinach library for this calculation.
    # lDate is the current local time.
    # Get Eorzea hour for weather start
    bell = unixSeconds / 175

    # Do the magic 'cause for calculations 16:00 is 0, 00:00 is 8 and 08:00 is 16
    increment = int(bell + 8 - (bell % 8)) % 24

    # Take Eorzea days since unix epoch
    totalDays = unixSeconds // 4200
    # totalDays = (totalDays << 32) >>> 0; # Convert to uint

    calcBase = totalDays * 100 + increment

    step1 = (((calcBase << 11)%(0x100000000)) ^ calcBase)
    step2 = (((step1 >> 8)%(0x100000000)) ^ step1)
    
    return step2 % 100

def getEorzeaHour(unixSeconds):
    bell = (unixSeconds / 175) % 24
    return int(bell)

def getWeatherTimeFloor(unixSeconds):
    # Get Eorzea hour for weather start
    bell = (unixSeconds / 175) % 24
    startBell = bell - (bell % 8)
    startUnixSeconds = round(unixSeconds - (175 * (bell - startBell)))
    return startUnixSeconds

```

写了个main，然后输入当前时间看了下输出，getEorzeaHour可以得到ET时间的小时，getWeatherTimeFloor大概是返回当前天气的起始时间戳？calculateForecastTarget输出时报错，但猜测是计算当前时间的天气seed？

这样一来，输入当前时间，再用getEorzeaHour计算出ET时间，判断是否在限时采集的时间区间内。

但在如何返回一个采集点的LT时间区间上发了糊涂，想半天绕不过来，算了半天时间戳还发现差八个小时，后来查了之后才知道时间戳的起始时间是格林威治时间的1970.01.01 00:00，换算成北京时间要+8。

就当给自己留个坑吧，考完试再想想。

数字信号！电磁场！爱我一次！

喔，今天还顺带修了一个博客的bug，之前的tags页面下面有那个评论框，一直不知道咋消掉，以为是主题文件的bug，差点就去提issue了，后来发现是当初创建tags页面的时候忘了关评论，加上一条comments: false就成了。

<del>然后我还发现自己的博客没有备份，什么时候想起来再搞了，应该不会出什么意外吧【口住！</del>

