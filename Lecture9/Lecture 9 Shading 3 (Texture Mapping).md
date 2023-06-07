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



#### for each rasterized screen sample (x, y):       (Usually a pixel's center)

#### 		(u, v) = evaluate texture coordinate at (x, y)   (Using barycentric coordinates!)

#### 		texcolor = texture.sample(u, v);

#### 		set sample's color to texcolor;

#### 		(usually the diffuse albedo $Kd$ recall the Blinn-Phong reflectance model)



### Texture Magnification (What if the texture is too small?)

### 	Texture Magnification - Easy Case

##### 		Generally don't want this - insufficient texture resolution

##### 		A pixel on a texture - a texel

​		<img src="C:\Users\userData\Desktop\GAMES101\Lecture9\p6.png" alt="p6" style="zoom:33%;" />







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