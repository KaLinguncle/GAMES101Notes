# Lecture 10: Geometry 1 (Introduction)



环境贴图： https://www.cnblogs.com/KillerAery/p/15106770.html#%E5%A4%A9%E7%A9%BA%E7%9B%92skybox

球面环境贴图：https://zhuanlan.zhihu.com/p/84494845



# Applications of Texture

- #### In modern GPUs, texture  = memory + range query (filtering)

  - #### General method to bring data to fragment calculations 

  - Many applications

    - #### Environment Lighting

    - #### Store microgeometry

    - #### Procedural textures

    - #### Solid modeling
    
    - #### Volume rendering
    
    - #### ...



### Environment Map

###### 		现实世界的环境光是非常复杂的，存在大量的经过多次反射后到达着色物体后再到达人眼的间接光（例如光线照在窗户上，窗户再反射到杯子上，杯子再将光线返回到人眼），这个过程中会把反射经过的物体颜色按一定权重混合在一起（因此看到杯子混合了窗户的颜色），最后再着色物体形成近似 “镜面反射” 的效果（本质上就算接受了复杂环境光的结果 也被称作为 “环境映射" Environment Mapping）

![p3](./p3.png)

###### 																		左图中展示的为环境中的光线，右图为在物体上展示的环境光线效果

###### 		为了实现接受环境光的效果，我们可以用一个贴图来存放环境光信息，即 环境贴图 Environment Map 或 反射贴图 Reflection Map，再给物体着色的时候不仅采样普通纹理，也采样环境贴图，按一定比例混合这两者的颜色（例如物体材质越光滑，镜面反射效果越强）



### Environmental Lighting

![p4](./p4.png)





### Spherical Environment Map

![p5](./p5.png)

### Spherical Map — Problem

![p6](./p6.png)

###### 																									球面贴图带来的问题，易于失真，在贴图上下可以看出

###### 在某些情况下，我们可能需要将空间中的曲面 （一般是指凸多面体）映射到纹理平面，即 :   $f: \R^3 \rightarrow \R^2; \ (x,y,z) \rightarrow (uv)$

###### 那么就需要借助 球形贴图 Spherical Map 或者 立方体贴图 Cube Map



### Spherical Map

###### 	球形贴图映射的思路：

   1. ###### 假设一个单位求包围了物体中心，当物体中心看向物体表面的某个位置 $(x,y,z)$ 时，从物体中心朝这个位置发出一条射线，此时射线会与单位球相交于某点。

			1. ###### 根据射线与单位球体的交点坐标 $(x_o,y_o,z_o)$ ，推算出焦点所在的偏航角与俯仰角 $(yaw, pitch)$，然后来映射成在球形贴图对应的 $(u,v)$ 坐标点。

###### 

### Cube Map

<img src="./p7.png" alt="p7" style="zoom: 33%;" />

![p8](./p8.png)

###### 	立方体贴图映射的思路：

  1. ###### 假设一个单位立方体包围了物体中心，当物体中心看向物体表面某个位置 $(x,y,z)$ 时，从物体中心朝这个位置发出一条射线与立方体交于某点。

  2. ###### 根据射线与单位立方体的交点坐标 $(x_o,y_o,z_o)$，在 $x_o,y_o,z_o$ 中取绝对值最大的那个分量，根据它的符号来判定来确定要映射在哪一个面。

  3. ###### 在确定映射第几个面后，剩余另外两个分量便是来映射成在第几张立方体贴图中的 $(u,v)$ 的坐标点。



### Textures can affect shading!

##### 	Texture doesn't have to only represent colors

  - ##### What of ot stores the height / normal ?

  - ##### Bump / normal mapping

		- ##### Fake the detailed geometry





Bump Mapping：【GAMES101-现代计算机图形学课程笔记】Lecture 10 Geometry 1 （介绍） - marsggbo的文章 - 知乎 https://zhuanlan.zhihu.com/p/147354628

### Bump Mapping

	#### Adding surface detail without adding more triangles

### Displacement Mapping  

#### a more advanced approach





### 3D procedural Noise + Solid Modeling

### Provide Precomputed Shading

### 3D Texture and Volume Rendering







# Introduction to Geometry

## 	Example of geometry

![p1](./p1.png)

​	

![p2](./p2.png)



Many Ways to Represent Geometry

Implicit

- algebraic surface
- level sets
- distance funcitons
- ...

Explicit

- point cloud
- polygon mesh
- subdivsion, NURBS
- ...

Each choice best suited to a different task/type of geometry



### "Implicit" Representations of Geometry

E.g. sphere: all point in 3D, where  $x^2 + y^2 + z^2 =1$



### Explicit Representations of Geometry



#### Distance Functions （Implicit）

An Example: Blending a moving boundary

### Level Set Methods (Also implicit)



#### Fractals









​	