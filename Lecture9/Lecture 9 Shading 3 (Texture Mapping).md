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

​											<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p1.png" alt="p1" style="zoom: 25%;" />

#### 	What's the barycentric coordinate of A?

<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p2.png" alt="p2" style="zoom:25%;" />

#### 	Geometric viewpoint — proportional areas

​												<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p3.png" alt="p3" style="zoom:25%;" />

#### 	What's the barycentric coordinate of the centroid?

​														<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p4.png" alt="p4" style="zoom: 25%;" />

#### 	Barycentric Coordinates: Formulas

​													<img src="C:\Users\userData\AppData\Roaming\Tencent\QQ\Temp\L2[_TA]{@7WR[S8[J%~EF]J.png" alt="L2[_TA]{@7WR[S8[J%~EF]J" style="zoom:25%;" />

#### 	Using Barycentric Coordinates

##### 		Linearly interpolate values at vertices

​												<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p5.png" alt="p5" style="zoom:25%;" />

​	质心(重心)坐标并非一个固定的值，通过三角形内不同位置的质心，可以得到不同位置与三个顶点属性的比例。





## Applying Textures



### Simple Texture Mapping: Diffuse Color



f`or each rasterized screen sample (x, y):       (Usually a pixel's center)`

`(u, v) = evaluate texture coordinate at (x, y)   (Using barycentric coordinates!)`

`texcolor = texture.sample(u, v);`

`set sample's color to texcolor;`

(usually the diffuse albedo $Kd$ recall the Blinn-Phong reflectance model)



### Texture Magnification (What if the texture is too small?)

### 	Texture Magnification - Easy Case

##### 		Generally don't want this - insufficient texture resolution

##### 		A pixel on a texture - a texel

​		<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p6.png" alt="p6" style="zoom:33%;" />



#### Bilinear Interpolation

<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p8.png" alt="p8" style="zoom:33%;" />

###### 								want to sample texture value at red point



<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p9.png" alt="p9" style="zoom:33%;" />

###### 				take 4 nearest sample locations, use these location to interpolation

​							   <img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p10.png" alt="p10" style="zoom:33%;" />

​		

<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p11.png" alt="p11" style="zoom:33%;" />

###### 								lerp is Linear interpolation

<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p12.png" alt="p12" style="zoom:33%;" />

###### 							get sample point horizontal coordinate

​							<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p13.png" alt="p13" style="zoom:33%;" />

###### 							use thrice interpolation (horizontal twice, vertical once) get result.

###### 以上流程描述了双线性插值的一个处理流程，现在来对该流程进行分析：

​	单线性插值：

​		

双线性插值： https://blog.csdn.net/qq_58664081/article/details/129079354

​						https://blog.csdn.net/Exception_3212536934/article/details/125141459

## Texture Magnification (hard case) 

#### 	(What if the texture is too large?)

### Point Sampling Textures - Problem

![p7](C:\Users\userData\Desktop\GAMES101\Lecture9\p7.png)

###### 									图中可以看到经过点采样后的的图片在近处出现了锯齿 在远处出现了摩尔纹











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