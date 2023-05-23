# Barycentric coordinate 重心坐标与其推导



#### 重心坐标通常用于得知某个三角形的三个顶点的属性，然后就可以求出三角形你不某一点对应的属性，例如深度缓存时与高洛德着色时，现在以此文介绍如何去推导重心坐标。



1. 直线的重心坐标：

   在定义三角形的重心坐标前，先看一下一条直线上的重心坐标如何定义。

   假设存在两个点 $A,B$ , A点的坐标为（Ax, Ay, Az）,B点的坐标为 (Bx, By, Bz), 在A，B连成的直线 AB上的任意一点$P \ \ (P_{x}, P_{y},P_{z})$ 必然满足：

   ### $(P_{x}, P_{y},P_{z})= (A_{x} + k(B_{x} - A_{x})),(A_{y} + k(B_{y} - A_{y})),(A_{z} + k(B_{z} - A_{z}))$

   常数k也可以通过上式变形改为：

   ### $k = \frac{P_{x} - A_{x}}{B_{x} - A_{x}}=\frac{P_{y} - A_{y}}{B_{y} - A_{y}}=\frac{P_{z} - A_{z}}{B_{z} - A_{z}}$

   可得：

   ### $\overrightarrow{AP}= k\overrightarrow{AB}$

   ### $\overrightarrow{AP}= ((A_{x} + k(B_{x} - A_{x})),(A_{y} + k(B_{y} - A_{y})),(A_{z} + k(B_{z} - A_{z}))) - (A_{x},A_{y},A_{z})$

   

   $\overrightarrow{AP}$ 通过向量的减法，可以理解为 $\overrightarrow{AP}=\overrightarrow{OP} - \overrightarrow{OA}$,同样 $\overrightarrow{AB}=\overrightarrow{OB} - \overrightarrow{OA}$ ,可以得出$\overrightarrow{OP}-\overrightarrow{OA}=k(\overrightarrow{OB} - \overrightarrow{OA})$

   最后得出 $\overrightarrow{OP}=k(\overrightarrow{OB} - \overrightarrow{OA})-\overrightarrow{OA}= (1-k)\overrightarrow{OA}+k\overrightarrow{OB}$

   而 $\overrightarrow{OP}$,$\overrightarrow{OA}$,$\overrightarrow{OB}$分别代表 P，A，B三点的坐标，因此可知：$P = (1-k)A+kB$

   

   几何意义：

   根据以上可得$\overrightarrow{AP}= k\overrightarrow{AB}$ ，即AP的长度 $|\overrightarrow{AP}|$ 为AB长度$|\overrightarrow{AB}|$的 k 倍，而 BP长度$|\overrightarrow{BP}|$又等于 $|\overrightarrow{AB}|- |\overrightarrow{AP}|$,因此  $|\overrightarrow{BP}|$的长度是 $|\overrightarrow{AB}|$的 （1- k）倍，可得：

    $|\overrightarrow{AP}| : |\overrightarrow{BP}| = k:(1-k)$

   

   因为长度是没有正负的，然而实际上k可能为复数，所以需要分析k值的各种情况：

   

   $0 \le k \le 1$

   此时点P在AB之间

    $|\overrightarrow{AP}| : |\overrightarrow{BP}| = k:(1-k)$

   ![1](C:\Users\userData\Desktop\GAMES101\Lecture9\barycentric pictrue\1.png)

   $k \lt 0$

   说明 $\overrightarrow{AP} 、 \overrightarrow{AB}$ 的方向相反，此时P点在AB之外

   $|\overrightarrow{AP}| : |\overrightarrow{BP}| = -k:(1-k)$

   ![2](C:\Users\userData\Desktop\GAMES101\Lecture9\barycentric pictrue\2.png)

​	$k \gt 1$

​	说明 $\overrightarrow{AP} 、 \overrightarrow{AB}$ 的方向相反，此时P点在AB之外,离B点更近

​	$|\overrightarrow{AP}| : |\overrightarrow{BP}| = -k:(k-1)$

![3](C:\Users\userData\Desktop\GAMES101\Lecture9\barycentric pictrue\3.png)





### 直线上的重心坐标

经过上面的推导，可以得知直线AB上的一个点P，可以由一个常数k计算出来，也可以通过P来推出k的值。$P = (1-k)A+kB$

设 j = 1 - k 那么可以得出 P = jA + kB ，j + k = 1

这样就可以使用 j，k 来表示点P，这种表示方式就是重心坐标，因为重心坐标是由某一条直线上的 AB 两点所定义的，因此不同的直线各自会有各自的重心坐标。







### 三角形上的重心坐标

https://blog.csdn.net/wangjiangrong/article/details/115326930