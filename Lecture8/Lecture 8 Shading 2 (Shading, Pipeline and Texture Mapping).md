## Lecture 8: Shading 2 (Shading, Pipeline and Texture Mapping)    还差部分内容关于Per-Vertex Normal Vectors



### Specular Term (Blinn-Phong)

#### Intensity depends on view direction

- #### Birght near mirror reflection direction

<img src="./p1.png" alt="p1" style="zoom: 25%;" />

###### 		高光的产生：当观察者观察物体的视角与光源角度正好是镜面对称时，即可看到高光。在上图中可以发现，与光线 $l$  基于法线对称的 $R$ 是镜面反射方向 (但高光并非完全只有一条反射光线，在 视角 $v$ 与 $R$ 越来越接近时，镜面反射效果越明显)

### V close  to mirror direction $\Leftrightarrow$ half vector near normal

- ### Measure "near" by dot product of unit vectors

<img src="./p2.png" alt="p2" />

###### 半程向量(半角向量) $h$ 为了精简计算观察角度$v$ 与 反射角度$R$ 的接近率，从图一可以看出，物体的法线方向正好是光源向量 $l$  和反射向量 $R$ 的角平分向量，通过对比法向量$n$与半程向量$h$的接近程度，使用$cos(\alpha) = n\cdot h$表示高光强度， $cos\alpha \in [0,1]$可以对反射点的高光强度归一化，综合以上可以得到高光模型公式。

###					$L_{s} = K_{s}(\frac{I}{r^{2}})max(0, n\cdot h)^{p}$

<img src="./p3.png" alt="p3" style="zoom:50%;" />

###### 指数p用于加快函数的衰减程度，由于在现实世界中高光在物体表面上只存在很小的一部分，而$cos\alpha$的衰减速度太慢，通过携带指数，可以促进衰减速度，使得高光只能在与法线向量$n$ 非常接近的情况下才能被视角看见。

<img src="./p4.png" alt="p4" style="zoom:33%;" />

### Ambient Term

#### Shading that does not depend on anything

- #### Add constant color to account for disregarded illumination and fill in black shadows
- #### This is approximate / fake!<img src="./p5.png" alt="p5" style="zoom: 50%;" />

###### 环境光照射在其他物体反射到观察物体上再通过观察物体表面反射至观察者视角。在Blinn-Phong模型中，假设物体表面接收到的各种环境光都是相同强度的。这样的模型大大简化了计算环境光的步骤，强度相同，意味着反射光的强度也相同，由于环境光来自四面八方的物体反射，所以该物体表面反射环境光的方向也是四面八方的。在这样的模型中，环境光与光源的角度无关，与观察角度也无关，是一个常数。

### 														$L_{a}= K_{a}I_{a}$

### Blinn-Phong Reflection Model

#### Ambient + Diffuse + Specular = Blinn-Phong Reflection

###### Ambient 由于环境光是常数，所以作用在物体的每一个点上都有相同的光照强度

###### Diffuse 漫反射光与观察角度相关，当观察视角与物体表面顶点法线越接近，光照强度越大

###### Specular 高光与观察角度相关，只有半程向量与顶点法线偏差非常小时，才能看到高光效果

#### <img src="./p6.png" alt="p6" />

#### 							$L = L_{a} +L_{d}+L{s} \\ =k_{a}I_{a} + k_{d}(\frac{I}{r^{2}})max(0, n\cdot l) + k_{s}(\frac{I}{r^{2}})max(0, n\cdot h)^{p}$





### Shading Frequencies

#### What caused the shading difference？

<img src="./p7.png" alt="p7" style="zoom:33%;" />

### Shade each triangle (flat shading)

#### Flat shading	

- #### Triangle face is flat — one normal vector

- #### Not good for smooth surfaces

<img src="./p8.png" alt="p8" style="zoom: 50%;" />



### Shade each vertex (Gouraud shading)

#### Gouraud shading

- #### Interpolate colors from vertices across triangle

- #### Each vertex has a normal vector 

<img src="./p9.png" alt="p9" style="zoom:50%;" />



### Shade each pixel (Phong shading)

#### Phong shading

- #### imterpolate normal vectors across each triangle

- #### Compute full shading model at each pixel

- #### Not the Blinn-Phong Reflectance Model

<img src="./p10.png" alt="p10" style="zoom:50%;" />





<img src="./p11.png" alt="p11" />



### Defining Per-Vertex Normal Vectors

#### Best to get vertex normals from the underlying geometry

- e.g. consider a sphere

#### Otherwise have to infer vertex normals from triangle faces

- Simple scheme: average surrounding face normals

### 	$N_{v} = \frac{\sum_{i}N_{i}}{|| \sum_{i}N_{i}||}$



### Defining Per-Pixel Normal Vectors

#### barycentric interpolation (introducing soon) of vertex normals

<img src="./p12.png" alt="p12" style="zoom:33%;" />







### Graphics (Real-time Rendering) Pipeline

#### Graphics Pipeline

<img src="./p16.png" alt="p16" />









### Texture Mapping

#### Surfaces are 2D

- ##### Surface lives in 3D world space

- ##### Every 3D surface point also has a place where it goes in the 2D image (texture).

<img src="./p13.png" alt="p13" />



### Visualization of Texture Coordinates

#### Each triangle vertex is assigned a texture coordinate (u, v)

<img src="./p14.png" alt="p14" style="zoom:33%;" />

<img src="./p15.png" alt="p15" style="zoom:33%;" />





补充笔记 https://zhuanlan.zhihu.com/p/352209183













