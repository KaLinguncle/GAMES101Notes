### Lecture 7: Shading 1 (Illumination, Shading and Graphics Pipeline)

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













