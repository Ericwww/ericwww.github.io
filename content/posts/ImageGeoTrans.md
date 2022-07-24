---
title: 图像的几何变换(1)
date: 2018-03-11T22:38:00+08:00
comments: true
tags:
 - OpenCV
---

本篇主要介绍仿射变换

<!-- more -->

仿射变换类型有：平移、缩放、旋转

仿射变换矩阵：

$$
A=\left\\{\begin{matrix}a\_{11} & a\_{12} & a\_{13} \\\\ a\_{21} & a\_{22} & a\_{23} \\\\ 0 & 0 & 1\end{matrix}\right\\}
$$

### 平移

平移是最简单的仿射变换，假设将空间坐标$(x,y)$，先沿x轴正方向平移200，再沿y轴正方向平移300，平移后的坐标为$(\\widetilde x,\\widetilde y)=(x+200,y+300)$

将假设一般化，假设任意空间坐标$(x,y)$先沿x轴平移$t\_{x}$，再沿y轴平移$t\_{y}$，则最后得到的坐标为$(\\widetilde x,\\widetilde y)=(x+t\_{x},y+t\_{y})$。

用矩阵形式来表示该平移变换过程如下：

$$
\left\\{\begin{matrix}\\widetilde x \\\\ \\widetilde y \\\\1\end{matrix}\right\\}=\left\\{\begin{matrix}1 & 0 & t_{x} \\\\0 & 1 & t_{y} \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}x \\\\y \\\\1\end{matrix}\right\\}
$$

其中，若$t\_{x}>0$，则表示沿x轴正方向移动；若$t\_{x}<0$，则表示沿x轴负方向移动。$t\_{y}$与之类似。

### 放大和缩小

二维空间坐标$(x,y)$以$(0,0)$为中心在水平方向上缩放$s\_{x}$倍，在垂直方向上缩放$s\_{y}$倍，指的是变换后的坐标位置离$(0,0)$的水平距离变为原坐标离位置中心点的水平距离的$s\_{x}$倍，垂直距离变为原坐标离中心点的垂直距离的$s\_{y}$倍。

根据以上定义，$(x,y)$以$(0,0)$为中心缩放变换后的坐标为$(\widetilde x,\widetilde y)=(x\*s\_{x},y\*s\_{y})$，显然，变换后的坐标位置离中心点的水平距离由$|x|$缩放为$|s\_{x}x|$，垂直距离由$|y|$缩放为$|s\_{y}y|$。

用矩阵形式来表示该缩放过程如下：

$$
\left\\{\begin{matrix}\\widetilde x \\\\ \\widetilde y \\\\1\end{matrix}\right\\}=\left\\{\begin{matrix}s\_{x} & 0 & 0 \\\\0 & s\_{y} & 0 \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}x \\\\y \\\\1\end{matrix}\right\\}
$$

若$s\_{x}>1$，则表示在水平方向上放大，就是离中心点的水平距离变大了，反之，则在水平方向上缩小。

若$s\_{y}>1$，则表示在垂直方向上放大，反之，则在垂直方向上缩小。

若$s\_{x}=s\_{y}$，即为等比缩放。

以上介绍的是以原点$(0,0)$为中心的缩放变换，那么$(x,y)$一任意一点$(x\_{0},y\_{0})$为中心在水平方向上缩放$s\_{x}$倍，在垂直方向上缩放$s\_{y}$倍，则缩放后的坐标为$(\\widetilde x,\\widetilde y)=(x\_{0}+s\_{x}(x-x\_{0}),y\_{0}+s\_{y}(y-y\_{0}))$

用矩阵形式可以表示为：

$$
\left\\{\begin{matrix}\\widetilde x \\\\\\widetilde y \\\\1\end{matrix}\right\\}=\left\\{\begin{matrix}1 & 0 & x\_{0} \\\\0 & 1 & y\_{0} \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}s\_{x} & 0 & 0 \\\\0 & s\_{y} & 0 \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}1 & 0 & -x\_{0} \\\\0 & 1 & -y\_{0} \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}x \\\\y \\\\1\end{matrix}\right\\}
$$

### 旋转

除了坐标的平移、缩放，还有一种常用的坐标变换，即旋转。

旋转有顺时针旋转和逆时针旋转。顺时针旋转α后的坐标$(\\widetilde x,\\widetilde y)$，$cosθ=\\frac{x}{p}$，$sinθ=\\frac{y}{p}$，其中p代表$(x,y)$到中心点$(0,0)$的距离，则

$$
cos(θ+α)=cosθcosα-sinθsinα=\\frac{x}{p}cosα-\\frac{y}{p}sinα=\\frac{\\widetilde x}{p}
$$

$$
sin(θ+α)=sinθcosα+cosθsinα=\\frac{y}{p}cosα+\\frac{x}{p}sinα=\\frac{\\widetilde y}{p}
$$

化简以上两个公式，可得$\\widetilde x=xcosα-ysinα$，$\\widetilde y=xsinα+ycosα$

用矩阵形式表示为：

$$
\left\\{\begin{matrix}\\widetilde x \\\\\\widetilde y \\\\1\end{matrix}\right\\}=\left\\{\begin{matrix}cosα & -sinα & 0 \\\\sinα & cosα & 0 \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}x \\\\y \\\\1\end{matrix}\right\\}
$$

相反如果以原点为中心，逆时针旋转α，$cosθ=\\frac{x}{p}$，$sinθ=\\frac{y}{p}$，且

$$
cos(θ-α)=cosθcosα+sinθsinα=\\frac{x}{p}cosα+\\frac{y}{p}sinα=\\frac{\\widetilde x}{p}
$$

$$
sin(θ-α)=sinθcosα-cosθsinα=\\frac{y}{p}cosα-\\frac{x}{p}sinα=\\frac{\\widetilde y}{p}
$$

化简以上两个公式，可得$\\widetilde x=xcosα+ysinα$，$\\widetilde y=-xsinα+ycosα$

用矩阵形式表示为：

$$
\left\\{\begin{matrix}\\widetilde x \\\\\\widetilde y \\\\1\end{matrix}\right\\}=\left\\{\begin{matrix}cosα & sinα & 0 \\\\-sinα & cosα & 0 \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}x \\\\y \\\\1\end{matrix}\right\\}
$$

从以上两个仿射矩阵可以得到，逆时针旋转α和顺时针旋转-α是一样的，所以用程序实现时，只需要实现其中一种就可以了。

以上讨论的旋转变换是以$(0,0)$为中心进行旋转的，如果$(x,y)$绕任意一点$(x\_{0},y\_{0})$逆时针旋转α，则首先将原点移到旋转中心，然后绕原点旋转，最后移回坐标原点，即：

$$
\left\\{\begin{matrix}\\widetilde x \\\\\\widetilde y \\\\1\end{matrix}\right\\}=\left\\{\begin{matrix}1 & 0 & x\_{0} \\\\0 & 1 & y\_{0} \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}cosα & sinα & 0 \\\\-sinα & cosα & 0 \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}1 & 0 & -x\_{0} \\\\0 & 1 & -y\_{0} \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}x \\\\y \\\\1\end{matrix}\right\\}
$$

需要注意，等式右边的计算是从右向左进行的。

### 计算仿射矩阵

#### 方程法

仿射变换有六个未知数，所以只需要三组对应位置坐标，构造出由六个方程组成的方程组即可解六个未知数。

举例：如果$(0,0)$，$(200,0)$，$(0,200)$这三个坐标通过某仿射变换矩阵$A$，分别转换为$(0,0)$，$(100,0)$，$(0,100)$，则可利用这三组对应坐标构造出六个方程，求解出$A$，

OpenCV提供的函数：

```python
cv2.getAffineTransform(src, dst)
```

通过方程法计算参数src到dst的对应仿射变换矩阵。对于该函数的Python API，输入参数src和dst分别代表原坐标和变换后的坐标，且均为3行2列的二维ndarray，每一行代表一个坐标，且数据类型必须为浮点型，否则会报错。

示例：

```python
import cv2
import numpy as np
src = np.array([[0,0],[200,0],[0,200]],np.float32)
dst = np.array([[0,0],[100,0],[0,100]],np.float32)
A = cv2.getAffineTransform(src, dst)
```

打印A的结果为：

```python
array([[ 0.5 , 0. , 0. ],
        [ 0. , 0.5 , 0. ]])
```

#### 矩阵法

对于使用矩阵相乘法计算仿射矩阵，前提是需要知道基本仿射变换步骤，即如果$(x,y)$先缩放再平移，则变换后的矩阵形式为：

$$
\left\\{\begin{matrix}\\widetilde x \\\\\\widetilde y \\\\1\end{matrix}\right\\}=\left\\{\begin{matrix}1 & 0 & t\_{x} \\\\0 & 1 & t\_{y} \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}s\_{x} & 0 & 0 \\\\0 & s\_{y} & 0 \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}x \\\\y \\\\1\end{matrix}\right\\}
$$

显然，以上仿射变换矩阵是由平移矩阵乘以缩放矩阵得到的。需要注意的是，虽然先缩放再平移，但是仿射变换矩阵是平移仿射矩阵乘以缩放仿射矩阵，即等式右边的运算是从右向左进行的。

对于矩阵的乘法，Numpy提供了dot函数来实现。

假设对空间坐标先等比例缩放2倍，然后在水平方向上平移100，在垂直方向上平移200，计算该仿射变换矩阵，代码如下

```python
import numpy as np
s = np.array([[0.5,0,0],[0,0.5,0],[0,0,1]])#缩放矩阵
t = np.array([[1,0,100],[0,1,200],[0,0,1]])#平移矩阵
A = np.dot(t,s)#矩阵相乘

A
array([[ 0.5 , 0. , 100. ],
        [ 0. , 0.5 , 200. ],
        [ 0. , 0. , 1. ]])
```

类似的，如果以$(x\_{0},y\_{0})$为中心进行缩放变化，然后逆时针旋转α，则仿射变换矩阵为：

$$
\left\\{\begin{matrix}1 & 0 & x\_{0} \\\\0 & 1 & y\_{0} \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\left\\{\begin{matrix}cosα & sinα & 0 \\\\-sinα & cosα & 0 \\\\0 & 0 & 1\end{matrix}\right\\}\left\\{\begin{matrix}s\_{x} & 0 & 0 \\\\0 & s\_{y} & 0 \\\\0 & 0 & 1\end{matrix}\right\\}\right\\}\left\\{\begin{matrix}1 & 0 & -x\_{0} \\\\0 & 1 & -y\_{0} \\\\0 & 0 & 1\end{matrix}\right\\}
$$

整理后结果为：

$$
A=\left\\{\begin{matrix}s\_{x}cosα & s\_{y}cosα & (1-s\_{x}cosα)x\_{0}-s\_{y}y\_{0}sinα \\\\-s\_{x}cosα & s\_{y}cosα & (1-s\_{y}cosα)y\_{0}+s\_{x}x\_{0}sinα \\\\0 & 0 & 1\end{matrix}\right\\}
$$

如果还需平移，则只需将结果左乘一个平移仿射矩阵即可。

如果先逆时针旋转α再进行缩放处理，则仿射变换矩阵为：

$$
A=\left\\{\begin{matrix}s\_{x}cosα & s\_{x}cosα & (1-s\_{x}cosα)x\_{0}-s\_{x}y\_{0}sinα \\\\-s\_{y}cosα & s\_{y}cosα & (1-s\_{y}cosα)y\_{0}+s\_{y}x\_{0}sinα \\\\0 & 0 & 1\end{matrix}\right\\}
$$

从得到地两个仿射变换矩阵可以看出，如果是等比例缩放的，即$s\_{x}=s\_{y}$，则两个仿射变换矩阵是相等的。对于这种等比例缩放的仿射变换，OpenCV提供了函数：

```python
cv2.getRotationMatrix2D(center, angle, scale)
```

用于计算仿射变换矩阵，其本质还是通过各个矩阵相乘得到的，其中参数center为变换中心点的坐标，scale是等比例缩放的系数，angle是逆时针旋转的角度。注意，angle是以角度为单位，而不是弧度。

举例：计算以$(40,50)$为中心逆时针旋转30°的仿射变换。Python实现代码如下：

```python
import cv2
A = cv2.getRotationMatrix2D((40,50)), 30, 0.5)
```

返回值是一个2×3的ndarray且数据类型是double，输出值为：

```python
array([[0.4330127,0.25,10.17949192],
        [-0.25,0.4330127,38.34936491]])
```