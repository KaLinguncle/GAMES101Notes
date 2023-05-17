# Lecture 7: Shading 1 (Illumination, Shading and Graphics Pipeline)

## Visibility / occlusion



### Painter's Algorithm

#### 	Inspired by how painters paint

#### 	Paint from back to front, overwrite in the framebuffer

###### 	画家算法 一种来自于绘画过程中的直观体现，画家在绘画时，总会先画远处的物体，再去画近处的物体，如果反过来 front to back 那么远处的物体就会把近处物体掩盖。

###### 按照图形学的流程来说：

1. ###### 对于一个场景，先把场景远处的内容光栅化到屏幕上，然后再光栅化近处的内容

2. ###### 若此时新光栅化的内容与已经光栅化的内容产生重叠，则新的内容会覆盖旧的内容

3. ###### 场景内所有内容从远到近光栅化完毕，就可以得到一个具有近处遮盖远处内容的结果

​	

<img src="C:\Users\userData\Desktop\GAMES101\Lecture7\p1.png" alt="p1" style="zoom:33%;" />

#### 	Requires sorting in depth (O(n log n) for n triangles)

#### 	Can have unresolvable depth order

​	<img src="C:\Users\userData\Desktop\GAMES101\Lecture7\p2.png" alt="p2" style="zoom:33%;" />

###### 		通过画家算法难以判断这三个三角形的深度顺序，三个三角形两两覆盖从而形成了一个环，在深度上存在互相遮挡关系，所以无法定义三角形们的前后顺序。当我们无法使用Painter‘s Algorithm时，引入 Z-Buffer来解决这个问题。

​	

### Z-Buffer

#### 	This is the algorithm that eventually won.

#### 	Idea:

- ##### Store current min.  z-value for each sample (pixel)

- ##### Needs an additional buffer for depth values

  - ##### frame buffer stores color value

  - ##### depth buffer (z-buffer) stores depth

#### 	IMPORTANR : For simplicity  we suppose

#### 		$ \textcolor{red}{ z \ is \ always \ positive} $  (smaller z -> close, larger z -> further) 		

##### 由于相机是放在坐标原点指向 -z轴，因此我们看到的图像深度都是负值，因此一个物体的z坐标越小，该物体就离相机越远，z的坐标越大，离相机就越近。 当我们计算物体深度时，将深度转化为一个大于0的数值，所以我们取 深度值 z的绝对值作为一个物体的深度，z越大则离相机越远，z越小则离相机越近。

### Z-Buffer Example

![p3](C:\Users\userData\Desktop\GAMES101\Lecture7\p3.png)

#####  这幅图的左侧是frame buffer 右侧为 depth buffer。左侧为物体最终渲染的结果，右侧的 depth buffer 存储每个像素点的深度值 z。在Depth /  Z-Buffer 上一个像素的深度值越大，则那个点越白，像素的深度值越小，则点越黑。我们设取色范围为[ 0, 1 ]，color = 0为黑，color = 1 为白,当我们的深度值小，对应的颜色值也就小，因此就接近黑色。当我们的深度值大,对应的颜色值也就大，因此也就接近于白色。



### Z-Buffer Algorithm

#### 	Initialize depth buffer to $\infty$ (重要的一步：先把所有像素的深度值都初始化为无限大)

#### 	During rasterization:

```
for (each triangle T)
	for (each sample (x,y,z) in T)
		if (z < zbuffer[x,y])		// closest sample so far 如果像素点的深度值小于深度缓冲中的深度值
			framebuffer[x,y] = rgb;	// update color 在该像素点绘制深度值小的三角形颜色
			zbuffer[x,y] = z; 		// update depth 将深度缓冲的的深度值更新为当前绘制三角形的在该点的深度值
			
			; 						// do nothing, this sample is occluded 如果深度值大于之前，则什么都不做。每个采样点展示的就是最靠近屏幕的（最近的）信息，后面的被前面覆盖了 这就是back to front。
```

​	<img src="C:\Users\userData\Desktop\GAMES101\Lecture7\p4.png" alt="p4" style="zoom:50%;" />

###### 可以看出，紫色三角形深度值大于5的部分在重叠时被红色三角形遮盖住了，这样就形成了图像覆盖的层次关系



### Z-Buffer Complexity

#### Complexity

- #### O(n) for n triangles (assuming constant coverage)

- #### How is it possible to sort n triangles in linear time?

  - ##### 这个算法能够在线性时间内完成的原因是算法并没有对三角形进行排序，而是仅仅在每个像素上做了一次深度值的判断，如果深度值小于之前就进行覆盖，如果深度值大于之前就什么都不做。所以这个算法所执行的就是：三角形的个数  $\times$  n个三角形所覆盖的像素个数； 得到一个 $O(n)$复杂度的算法。 

- #### Drawing triangles in different orders?

  - ##### 在这个过程中，三角形的绘制顺序并不重要，因为Z-Buffer只记录了当前绘制图像的深度值，和顺序并没有关系。在实际操作中并不是对每个像素进行深度值统计，例如在 MSAA 中对于每个像素分成更小的采样点也会进行统计，因此实际上是对每个采样点进行深度值比较。
  
- #### Most important visibility algorithm

  - ##### Implemented in hardware for all GPUs






## Shangding

###			Illumination & Shading

### 		Graphics Pipeline





### What We've Covered So Far

​		<img src="C:\Users\userData\Desktop\GAMES101\Lecture7\p5.png" alt="p5" style="zoom: 50%;" />





### Shading: Definition

- 	#### In Merriam-Webster Dicitionary

  - ##### The darkening or coloring of an illustration or diagram with parallel lines or a block of color.

- #### In this course

  - ##### The process of applying a material to an object.

##### 	darkening:	 引入明暗的不同，有些地方明亮一些，有些地方暗一些

##### 	coloring:	有些地方有颜色，有些地方有不同的颜色，有些地方没有颜色。

##### 	在图形学上：对其的定义是：对不同的物体应用不同的材质进行着色。



### A Simple Shading Model (Blinn-Phong Reflectance Model)

<img src="C:\Users\userData\Desktop\GAMES101\Lecture7\p6.png" alt="p6" style="zoom:33%;" />

##### 	光源在右上的方向，光源照亮了所有的茶杯，可以看到茶杯上有一些颜色不同的地方

##### 	茶杯上有一个 高光（Specular highlights), 高光是由于光线照射到了光滑的表面，其光线反射方向接近镜面反射方向，因此形成了高光。 茶杯表面除了高光外，其余颜色变化并不剧烈的地方，我们称之为漫反射(diffuse reflection) 光源在右上，在茶杯的背面应该看不到这个光源，那么茶杯的背面应该是黑色。但是我们看到 了这个茶杯的背面并不是黑色，也就是说有一些的光从茶杯的背面反射到了我们的眼里，那么 这个点一定是接受到了光。但是这个点接受到的并不是直接光照，而是间接光照。比如说光找 到墙上，墙面发生了一个漫反射将光反射到了桌面上，光再经过桌面的反射就能到茶杯的背面。假如说任何一个点都能够接收到来自四面八方的反射光，这个反射光就是环境关照 (Ambient lighting)，且这个光是一个常量。

​	

### Shading is Local 着色点的光照

<img src="C:\Users\userData\Desktop\GAMES101\Lecture7\p7.png" alt="p7" style="zoom:33%;" />





笔记补充 https://blog.csdn.net/weixin_43391563/article/details/111313567





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













