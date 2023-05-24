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

   

   因为 $P = ((1-k)+k)P$ ,那么可以得出：

   $((1-k)A+kB)-((1-k)+k)P = 0$

   化简得到$((1-k)(A-P)+k(B-P)$

   $((1-k)\overrightarrow{PA}+k\overrightarrow{PB}= 0$

   

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

与直线上任意一点满足 P= jA + kB 一样，在三角形ABC所在平面上的任意一点P满足

P = iA + jB + kC 且 i + j + k = 1

若点P在三角形内部或三角形边上，需要满足 $i \ge 0, j \ge 0, k \ge 0$,否则点P在三角形的外面。

由于$P = (i+j+k)P = iP+jP+kP$

可得$iA+jB+kC-iP+jP+kP = 0$ 

即： $i\overrightarrow{PA}+ j\overrightarrow{PB}+ k\overrightarrow{PC} =0$



### 三角形与直线的关系

将三角形ABC的某一个顶点与三角形内任意一点P连线，并连接到该顶点的对边上，交点为D。



那么如上图的情况，点D在AB两点构成的边上，那么可以用直线的重心坐标去表示D点得到$D = xA + yB \ \ x+y=1$， 知道D点坐标后，可以使用CD点去确定点P的坐标了，可得      

$P = wC + zD \\ P=wC + z(xA +yB)= zxA+ zyb+wC  \\  w+z=1$

设 zx = i， zy = j， w = k, 则有 $P =iA+jB + wC$ 成立



由于 $P(P_{x},P_{y},P_{z}) = (iA_{x}+jB_{x}+kC_{x}),(iA_{y}+jB_{y}+kC_{y}),(iA_{z}+jB_{z}+kC_{z})$

可得

$\begin{cases} P_{x} = iA_{x}+jB_{x}+kC_{x}= iA_{x}+jB_{x}+(1-i-j)C_{x} \\ P_{y} = iA_{y}+jB_{y}+kC_{y}= iA_{y}+jB_{y}+(1-i-j)C_{y} \end{cases}$



$\begin{cases}(P_{x}-C_{x}) + j(C_{x}-B_{x}) = i(A_{x}- C_{x}) \\ (P_{y}-C_{y}) + j(C_{y}-B_{y}) = i(A_{y}- C_{y})\end{cases}$

消去 i ：

#### $ \frac {(P_{x}-C_{x}) + j(C_{x}-B_{x})}{(A_{x}- C_{x})} = \frac{(P_{y}-C_{y}) + j(C_{y}-B_{y})}{(A_{y}- C_{y})}$

对j进行求解：

#### $ \frac {j(C_{x}-B_{x})}{(A_{x}- C_{x})} -  \frac{j(C_{y}-B_{y})}{(A_{y}- C_{y})} = -\frac{(P_{x}-C_{x})}{A_{x}-C_{x}} + \frac{ (P_{y}-C_{y})}{(A_{y}- C_{y})}$

去除分母得：

$ j(C_{x}-B_{x})(A_{y}- C_{y}) - j(C_{y}-B_{y})(A_{x}- C_{x}) = -(P_{x}-C_{x})(A_{y}-C_{y}) + (P_{y}-C_{y})(A_{x}- C_{x})$

算出j的值：

#### $j= \frac{-(P_{x}-C_{x})(A_{y}-C_{y}) + (P_{y}-C_{y})(A_{x}- C_{x})}{ -(B_{x} -C_{x})(A_{y}- C_{y}) + (B_{y}-C_{y})(A_{x}- C_{x})}$

同理 i 值也可以用同样的方法求出， k的值可以通过 1 - i - j 得到。



将P点与三角形的三个顶点分别连线 在三角形的内部得到三个新的三角形：













https://blog.csdn.net/wangjiangrong/article/details/115326930

重心坐标（Barycentric coordinates） - 杨超wantnon的文章 - 知乎 https://zhuanlan.zhihu.com/p/58199366

计算机图形学 1：重心坐标系（Barycentric coordinate system）详解 - Yinli的文章 - 知乎 https://zhuanlan.zhihu.com/p/149836719