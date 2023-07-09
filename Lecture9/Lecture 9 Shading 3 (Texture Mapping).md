# Lecture 9: Shading 3 (Texture Mapping)

​	

## Interpolation Across Triangles: Barycentric Coordinates

### Why do we want to interpolate?

- #### Specify values at vertices

- #### Obtain smoothly varying values across triangles

### What do we want to interpolate?

- #### Texture coordinates, colors, normal vectors, …

### How do we interpolate?

- #### Barycentric coordinates





### Barycentric Coordinates

#### 	A coordinate system for triangles $(\alpha,\beta,\gamma)$

​											<img src="./p1.png" alt="p1" style="zoom: 25%;" />

#### 	What's the barycentric coordinate of A?

<img src="./p2.png" alt="p2" style="zoom:25%;" />

#### 	Geometric viewpoint — proportional areas

​												<img src="./p3.png" alt="p3" style="zoom:25%;" />

#### 	What's the barycentric coordinate of the centroid?

​														<img src="./p4.png" alt="p4" style="zoom: 25%;" />

#### 	Barycentric Coordinates: Formulas

​													<img src="C:\Users\userData\AppData\Roaming\Tencent\QQ\Temp\L2[_TA]{@7WR[S8[J%~EF]J.png" alt="L2[_TA]{@7WR[S8[J%~EF]J" style="zoom:25%;" />

#### 	Using Barycentric Coordinates

##### 		Linearly interpolate values at vertices

​												<img src="./p5.png" alt="p5" style="zoom:25%;" />

​	质心(重心)坐标并非一个固定的值，通过三角形内不同位置的质心，可以得到不同位置与三个顶点属性的比例。





## Applying Textures



### Simple Texture Mapping: Diffuse Color

```  c
for each rasterized screen sample (x, y):       (Usually a pixel's center)

(u, v) = evaluate texture coordinate at (x, y)   (Using barycentric coordinates!)

texcolor = texture.sample(u, v);

set sample's color to texcolor;

(usually the diffuse albedo $Kd$ recall the Blinn-Phong reflectance model)```
```

### Texture Magnification (What if the texture is too small?)

### 	Texture Magnification - Easy Case

##### 		Generally don't want this - insufficient texture resolution

纹理过小引起的问题：

​		比如说，把一张100 x 100 的纹理贴图应用在一个 500 x 500 的屏幕之上会导致走样与失真，因为屏幕空间的几个像素点对应在纹理贴图坐标上的一个纹素上，





##### 		A pixel on a texture - a texel

​		<img src="./p6.png" alt="p6" style="zoom:33%;" />



#### Bilinear Interpolation

<img src="./p8.png" alt="p8" style="zoom:33%;" />

###### 								want to sample texture value at red point



<img src="./p9.png" alt="p9" style="zoom:33%;" />

###### 				take 4 nearest sample locations, use these location to interpolation

​							   <img src="./p10.png" alt="p10" style="zoom:33%;" />

​		

<img src="./p11.png" alt="p11" style="zoom:33%;" />

###### 								lerp is Linear interpolation

<img src="./p12.png" alt="p12" style="zoom:33%;" />

###### 							get sample point horizontal coordinate

​							<img src="./p13.png" alt="p13" style="zoom:33%;" />

###### 													use thrice interpolation (horizontal twice, vertical once) get result.

###### 		以上流程描述了双线性插值的一个处理流程，可以看出通过这套流程下来，可以得出区域内任意一点的颜色，深度等信息，双线性插值不仅可以向上述流程中以横向进行两次lerp纵向一次lerp，也可以纵向两次lerp，横向一次lerp，可以得到一样的结果。从上述流程看出总共进行了三次插值。

###### 	单线性插值：

###### 				已知数据$(x_0, y_0)与(x_1,y_1)$要计算$[x_0, x_1]$区间内某一位置 $x$在直线上的$y$值，因为直线上的函数值是线性变化的，只需要通过计算 $x_0 \ x$两点斜率和$x_0 \ x_1$两点的斜率，令二者相等可以得到方程。

​				

​																<img src="./p14.png" alt="p14" style="zoom: 50%;" />

###### 				直线的斜率相等，所以可得  

### 												$\frac{y-y_0}{x-x_0}=\frac{y_1-y_0}{x_1-x_0}$

### 										$y =\frac{x_1-x}{x-x_0}y_0 +\frac{x-x_0}{x_1-x_0}y_1$

###### 		双线性插值：

###### 					为什么需要这样的双线性插值流程，首先，图中我们知道了4个顶点的属性，再通过两次插值得到了横	向两点的属性，最后插值得到这两点间的属性，总共进行了三次单线性插值。

​																

​																		<img src="./p15.png" alt="p15" style="zoom:33%;" />

###### 			在x方向做插值  ($f(Q_{11}) = y1$）：

#### 										$f(R_1) = f(x, y_1) \approx \frac{x_2-x}{x_2-x_1}f(Q_{11})+\frac{x-y_1}{x_2-x_1}f(Q_{21})$

#### 									   $f(R_2) = f(x, y_2) \approx \frac{x_2-x}{x_2-x_1}f(Q_{12})+\frac{x-y_1}{x_2-x_1}f(Q_{22})$

###### 			在y方向做插值 ：

#### 									  $f(x, y) \approx \frac{y_2-y}{y_2-y_1}f(R_{1})+\frac{y-y_1}{y_2-y_1}f(R_{2})$

​			

#### 		Bilinear interpolation usually gives pretty good results at reasonable costs





## Texture Magnification (hard case) 

#### 	(What if the texture is too large?)

### Point Sampling Textures - Problem

<img src="./p7.png" alt="p7" />

###### 																			图中可以看到经过点采样后的的图片在近处出现了锯齿 在远处出现了摩尔纹

### Screen Pixel "Footprint" in Texture

<img src="./p16.png" alt="p16" style="zoom: 50%;" />

###### 					为什么会出现这种走样的情况，在近处，一个像素所覆盖的纹理区域是比较小的，而在远处，一个像素覆盖的纹理区域非常大，再使用像素中心去采样就会出现问题。



### Will Supersampling Do Antialiasing?

###### 					为了消除上面的锯齿，可以通过超采样技术对每个像素进行512次采样，得出一个非常不错的结果，但是，这样做会有什么优点或缺点呢。

![p17](./p17.png)

### Antialiasing	Supersampling?

#### Will supersampling work?

- Yes, high quality, but costly
- When highly minified, many texels in pixel footprint
- Signal frequency too large in a pixel
- Need even higher sampling frequency

#### Let's understand this problem in another way

- What if we don't sample?
- Just need to get the average value within a range!



### Point Query vs. (Avg.) Range Query

![p18](./p18.png)



### Different Pixels  $\rightarrow$ Different-Sized Footprints

![p19](./p19.png)

###### 			从图中可以看出，图中近处的圆形区域中，每个像素所映射的纹素是比较少的，而在远处的圆形区域中，每个像素都需要映射大量的纹素，导致出现走样的问题。

### 问题： 通过范围查询使用加载不同的mipmap，可以实现位置移动视角变换时进行不同贴图的加载？（openGL是如何处理多级渐远纹理的？），当我们离上图中远处墙面的圆形区域越近，加载的mipmap也就越大，为什么看起来效果并不会走样。



## Mipmap   Allowing($\color{red}{fast, approx, suqare}$) range queries

​		什么是Mipmap，为什么使用Mipmap；

​		





##### 		为什么使用Mipmap仅仅只增加原texture内存的$\frac{1}{3}$, 正常来说，Mipmap会生成很多级别的纹理，每个级别的宽高都是上一级别的 $\frac{1}{2}$ (Mipmap的宽高需要相同且为 $2^n$)，也就是说面积为上一个级别的$\frac{1}{4}$ ,直到最后只有 1 x 1分别率的大小。

##### 		那么，Mipmap总共增加了多少大小呢：

#### 			$S = \frac{1}{4} + \frac{1}{16}+\frac{1}{64} + \cdots + \frac{1}{4^n}$

#### 			$4S = 1+ \frac{1}{4} + \frac{1}{16} + \cdots + \frac{1}{4^{n-1}}$

#### 			$4S - S = 3S = 1- \frac{1}{4^n}$

#### 			$\lim\limits_{n \to \infty}1-\frac{1}{4^n} =1  = 3S$

#### 			可以得出 $S = \frac{1}{3}$

##### 			   Mipmap使用的面积增加了$\frac{1}{3}$, 却减少了大量的即时计算消耗

mipmap的粗略介绍 https://blog.csdn.net/qq_42428486/article/details/118856697

mipmap占用的介绍 https://zhuanlan.zhihu.com/p/36939174

纹理映射补充1：纹理映射（Texture mapping） - 王江荣的文章 - 知乎 https://zhuanlan.zhihu.com/p/364045620

纹理映射补充2：图形学基础 - 纹理 - 纹理映射流程 - 杨鼎超的文章 - 知乎 https://zhuanlan.zhihu.com/p/369977849

### Mipmap(L. Williams 83)







Interpolation Across Triangles: 

#### Barycentric coordinate

​	why do we want to interpolate?

​		Specify values at vertices

​		Obtion smoothly varying values across triangles

​	

​	what do web want to interpolate?

​		Texture coordinates, colors, normal vectors,...



​	How do we interpolate?

​		Barycentric Coordinates

​		<u>However, barycentric coordinates are not invariant under projection!</u>



	#### Applying Textures
	
	#### Simple Texture Mapping : Diffuse Color

​	



#### Texture Magnification (what if the texture is too small?)

 	Bilinear interpolation

​	

	#### Linear interpolation (1D)

​		lerp(x, v0, v1) = v0 + x(v1 - v0)



#### Texture Magnification (hard case)(what if the texture is too large?)

​	Point Sampling Textures --- Problem

​	

	##### Mipmap

Allowing(fast, approx, square) range queries



### Anisotropic Filtering

​		Ripmaps and summed area tables