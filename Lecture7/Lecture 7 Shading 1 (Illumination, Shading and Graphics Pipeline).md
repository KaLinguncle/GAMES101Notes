# Lecture 7: Shading 1 (Illumination, Shading and Graphics Pipeline)

## Visibility / occlusion

	### 	Z-buffering



### Painter's Algorithm

#### 	Inspired by how painters paint

#### 	Paint from back to front, overwrite in the framebuffer

###### 	画家算法 一种来自于绘画过程中的直观体现，画家在绘画时，总会先画远处的物体，再去画近处的物体，如果反过来 front to back 那么远处的物体就会把近处物体掩盖。

<img src="C:\Users\userData\Desktop\GAMES101\Lecture7\p1.png" alt="p1" style="zoom:33%;" />

#### 	Requires sorting in depth (O(n log n) for n triangles)

#### 	Can have unresolvable depth order

​	<img src="C:\Users\userData\Desktop\GAMES101\Lecture7\p2.png" alt="p2" style="zoom:33%;" />

###### 		通过画家算法难以判断这三个三角形的深度顺序，sad

​	



### Z-Buffer

#### 	This is the algorithm that eventually won.

#### 	Idea:

- ##### Store current min.  z-value for each sample (pixel)

- ##### Needs an additional buffer for depth values

  - ##### frame buffer stores color value

  - ##### depth buffer (z-buffer) stores depth

#### 	IMPORTANR : For simplicity  we suppose

##### 		$ \textcolor{red}{ z \ is \ always \ positive} $  (smaller z -> close, larger z -> further) 		



### Z-Buffer Example

![p3](C:\Users\userData\Desktop\GAMES101\Lecture7\p3.png)

 















​	







​	

​	

​		

​	





## Shangding

###		Illumination % Shading

### 	Graphics Pipeline











#### Rasterization  

1. ####  Rasterizing one triangle

2. ####  Sampling theory

3. ####  Antialiasing



#### Today

##### Visibility / occlusion

- Z - buffer

##### Shading

- Illumination % Shading
- Graphics Pipeline



##### Painter's Algorithm

​	Inspired by how painters paint. 

​	Paint from back to front, overwrite in the framebuffer.

​	Requires sorting in depth (O(n log n) for n triangles) Can have unresolvable depth order



### Z-Buffer  

​	This is the algorithm that eventually won.

​	Idea:

- ​	Store current min. z-value for each sample (pixel)
- ​	Needs an additional buffer for depth values
  - frame buffer stores color values
  - depth buffer (z-buffer) stores depth

IMPORTANT: For simplicity we suppose

​			z is always positive

​			(smaller z -> closer, larger z -> further)

#### Z-Buffer Complexity

​	Complexity

​		O(n) for n triangles (assuming constant coverage)

​		How is it possible to sort n triangles in linear time?

	##### Drawing triangles in different orders?

Most important visibility algorithm

​	Implemented in hardware for all GPUs





### Shading ： Definition

​	In this course

​		The process of applying a material to an object.

​	

	#### A Simple Shading Model (Blinn-Phong Reflectance Model)



#### Diffuse Reflection

​	Light is scattered uniformly in all directions 

​		- Surface color is the same for all view directions

​		Shading independent of view direction













