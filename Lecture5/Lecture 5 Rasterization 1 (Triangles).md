# Lecture 5: Rasterization 1 (Triangles)



## Perspective Projection

#### what's near plane's l, r, b, t then?

- ##### If explicitly specified, good

- ##### Sometimes people prefer:  vertical field-of-view (for Y) and aspect ratio (assume symmetry i.e.

  #####  l = -r, b = -t)

<img src="C:\Users\userData\Desktop\GAMES101\Lecture5\p1.png" alt="p1" style="zoom: 50%;" />

#### How to convert form fov Y and aspect to l, r, b, t？ 

![p6](C:\Users\userData\Desktop\GAMES101\Lecture4\p6.png)

![p2](C:\Users\userData\Desktop\GAMES101\Lecture5\p2.png)

#### We assume symmetry l = -r b = -t, so frustum width is two r ,frustum hight is two t

###      $aspect = \frac{2r}{2t} =\frac{r}{t}$

### 		$tan(\frac{fovY}{2})= \frac{t}{\lvert n\rvert}$

#### What's after MVP?

- Model transformation (placing objects)
- View transformation (placing camera)
- Projection transformation
  - Orthographic projection (cuboid to "canonical" cube $[-1, 1]^{3}$)
  - Perspective projection (frustum to "canonical" cube)
- Canonical cube to ?

------

------



## Canonical Cube to Screen

#### What is a screen?

- An array of pixels
- Size of the array: resolution
- A typical kind of raster display

#### Raster == screen in German

​	Rasterize == drawing onto the screen

#### Pixel (FYI, short for "picture element")

​	For now: A pixel is a little square with uniform color

​	Color is a mixture of(red, green, blue)



### Canonical Cube to Screen

![p3](C:\Users\userData\Desktop\GAMES101\Lecture5\p3.png)

### Irrelevant to z

### Transform in xy plane:  $[-1,1]^{2}$ to $[0, width] \times[0, height]$

#### Viewport transform matrix:

### 	$M_{viewport}=\begin{pmatrix} \frac{width}{2}&0 & 0&\frac{width}{2} \\  0 & \frac{height}{2}&0 &\frac{height}{2} \\ 0&0&1&0\\ 0&0&0&1\end{pmatrix}$

#### ( Orthographic cube width and height is 2 )

![p4](C:\Users\userData\Desktop\GAMES101\Lecture5\p4.png)

​			

### Triangles - Fundamental Shape Primitives

#### Why triangles?

- Most basic polygon
  - Break up other polygons



### What Pixel Values Approximate a Triangle？

![p5](C:\Users\userData\Desktop\GAMES101\Lecture5\p5.png)



## A Simple Approach: Sampling

##### Evaluating a function at a point is sampling.

##### We can discretize a function by sampling.

```c
for (input x = 0; x < xmax; ++x)
	output[x] = f(x);
```



### Rasterization As 2D Sampling

1. ### Sample if each Pixel Center is Inside Triangle

2. ### Define Binary Function: $inside(tri, x, y)$

   

   ### 	$inside(t,x,y) = \begin{cases} 1 \ Point \ (x,y)  \ in \ triangle \ t \\ 0  \  otherwise\end{cases}$

   

   ### Rasterization = Sampling A 2D Indicator Function

   ```c
   // 检查图形是否在像素中心点内 +0.5 +0.5 像素中心的实际位置是其坐标 +0.5得到
   for (int x = 0; x < xmax; ++x) 
   	for(int y = 0; y < ymax; ++y)
   		image[x][y] = inside(tri, x+0.5, y+0.5)
   ```




	   ### Evaluating inside ( tri, x, y )
	
	#### 	How to evaluate a point inside a Triangle ?

####		Use Cross Products , Cross Product in Graphics:

- Determine Left / Right 
- Determine Inside / Outside  (e.g.  a point in the triangle)

<img src="C:\Users\userData\Desktop\GAMES101\Lecture5\p6.png" alt="p6" style="zoom: 33%;" />

#### 		在图中，如果需要判断一个点Q是否在三角形内，可以对 [ 向量P1P2，向量P1Q ] , [ 向量P0P1, 向量 P0Q] , [ 向量P2P0, P2Q ] 三个向量组分别进行叉乘，通过右手螺旋定则判断叉乘得到垂直向量方向。方向(Z轴正负)都相同则代表点Q在三角形内。



### Checking All Pixels on the Screen 

#### Use a Bounding Box    (blue area)

<img src="C:\Users\userData\Desktop\GAMES101\Lecture5\p7.png" alt="p7" style="zoom: 33%;" />

###### 					蓝色的区域称为三角形的包围盒 Bounding Box ，或者称其为轴向包围盒 Axis Aligned Bounding



### We Send the Display the Sampled Signal 

### And 

### The Display Physically Emits This Signal

<center>
    <img src="C:\Users\userData\Desktop\GAMES101\Lecture5\p8.png" alt="p8" style="zoom:33%;" />
    <img src="C:\Users\userData\Desktop\GAMES101\Lecture5\p9.png" alt="p9" style="zoom:33%;" />
#### 	Compare: The Continuous Triangle Function

<img src="C:\Users\userData\Desktop\GAMES101\Lecture5\p10.png" alt="p10" style="zoom:33%;" />



### What's Wrong With This Picture? 

#### $\color{red} Aliasing (Jaggies)$

![p11](C:\Users\userData\Desktop\GAMES101\Lecture5\p11.png)



###### 关于光栅化的额外补充资料：https://zhuanlan.zhihu.com/p/450540827
