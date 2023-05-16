# Lecture 6: Rasterization 2 (Antialiasing and Z-Buffering)

## Antialiasing

- 	### Sampling theory

### Sampling is ubiquitous in Computer Graphics

#### 	Resterization =  Sample 2D Positions	

#### 	Photograph =  Sample Image Sensor Plane	

#### 	Video = Sample Time

### Sampling Artifacts ( Errors / Mistakes / Inaccuracies ) in Computer Graphics

####		Jaggies ( Staircase Pattern )

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p1.png" alt="p1" style="zoom:33%;" />

#### 	Moire Patterns in Imaging

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p2.png" alt="p2" style="zoom: 33%;" />

#### 	Wagon Wheel Illusion ( False Motion )

​		<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p3.png" alt="p3" style="zoom:33%;" />

​		

#### 		Artifacts due to sampling - "Aliasing"    

###### 	 	**这里的Artifact指的并非是人工造物，而是一种非自然的感觉的意思 **

​	 

### 	$\color{red} Behind \ the \ Aliasing \ Artifacts$

#### 		$\color{red} Signals \ are \ Changing \ to \ fast \ ( \ height \ frequency \ ), but  \ sampled \ too \ slowly $





### 	Antialiasing Idea: Blurring ( Pre-Filtering ) Before Sampling

#### 		1. Rasterization: Point Sampling in Space

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p4.png" alt="p4" style="zoom: 50%;" />

#### 		Note jaggies in rasterized triangle where pixel values are pure red or white



#### 		2. Rasterization: Antialiased Sampling

![p5](C:\Users\userData\Desktop\GAMES101\Lecture6\p5.png)

###### 					去除Nyquist(奈奎斯特)采样频率之上的采样频率后    （预处理，先过滤后采样）

#### Note antialiasing edges in rasterized triangle where pixel values take intermediate values	

###### 把三角形模糊之后，再对其进行像素中心点的采样，可以解决锯齿问题



### Point Sampling vs Antialiasing

<center>
    <img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p6.png" alt="p6" style="zoom:50%;" />
    <img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p7.png" alt="p7" style="zoom:50%;" />
</center>

​			

### 	Point Sampling vs Antialiasing

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p8.png" alt="p8" style="zoom: 50%;" />



### Blurred Aliasing vs  Antialiasing 

![p9](C:\Users\userData\Desktop\GAMES101\Lecture6\p9.png)

   

### Why?

1. #### Why undersampling introduces aliasing?

2. #### Why pre-filtering then sampling can do antialiasing?

​	

#### 	Let's dig into fundamental reasons And look at how to implement antialiased rasterization



------





## Frequency Domain

### Sines and Cosines

##### 	从最简单的波形Sin 和 Cos 的图像开始，通过调整系数，可以得到不同的余弦波，下图中两个余弦波仅频率不同，频率定义了余弦波的变化有多快。 同样，可以定义余弦波的周期 T，周期 T 代表了多长时间余弦函数就会重复一次，周期 T 是频率的倒数。

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p10.png" alt="p10" style="zoom: 50%;" />

###### 							上图中可以看到，当频率是2时，函数图形的变化变得更加激烈迅速

​	

### Fourier Transform

#### Represent a function as a weighted sum of sines and cosines

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p11.png" alt="p11" style="zoom: 67%;" />

##### 			可以看出公式与图形的对应关系，可以得知，对于任何一个周期函数，都可以把它写成一系列正弦与余弦函数的线性组合与一个常数项。这就是傅里叶级数展开。

##### 			这说明傅里叶级数展开与傅里叶变换是紧密相连的，给定任何一个函数，都可以经过一个复杂的过程变为另一个函数，也可以将变换后的函数通过逆变换变为原来的函数 （例如各种音乐软件中提取高频或低频声音），这样的操作就被称为 傅里叶变换 Fourier transform 和 逆傅里叶变换 inverse Fourier transform 。

![p12](C:\Users\userData\Desktop\GAMES101\Lecture6\p12.png)

##### 		通过傅里叶级数展开可以将任何一个周期性函数分解为不同的频率，傅里叶变换是把函数变为不同的频率的段，并且把这些不同频率的段显示出来。



### Higher Frequencies Need Faster Sampling

![p13](C:\Users\userData\Desktop\GAMES101\Lecture6\p13.png)

###### 	对于图中对于不同频率信号进行相同的频率进行采样，可以看出采样的拟合程度是逐渐降低的，如果像是 图中f5(x) 的情况，采样的频率完全跟不上函数的变化，就无法将函数拟合出来。

![p14](C:\Users\userData\Desktop\GAMES101\Lecture6\p14.png)

#### Height-frequency signal is insufficiently sampled: samples erroneously  to be from a  low-frequency signal.

###### 如果把黑色采样曲线看作为另一函数，就会发现使用同一采样方法会得出两种截然不同的信号，会得到相同的结果，这样的现象被称为走样。

#### Two frequencies that are indistinguishable at a given sampling rate are called "aliases".



### Filtering = $Getting \ rid \ of \ certain \ frequency \ contents$





<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p15.png" alt="p15" style="zoom: 50%;" />

###### 																			将可见的图片转为频率信息

##### 在右边这幅图中，图片的中心被定义为左侧可见图片最低频的区域，四周为高频区域，从中心到四周，频率会越来越高，其亮度表达了左图在该频域的信息量。在右图中可以看出绝大部分的的信息都是集中于低频上的，高频的信息会比低频信息少很多。傅里叶变换可以让我们看到这个图形在各个不同的频率的样子，这被称为频谱。





##### 当我们获得了图片的频域信息，此时去除频域中心，也就是低频的内容保留高频的内容，再使用逆傅里叶变换将频域转化为时域，就会得到一个显示出图片内容边界的图片，也就是说，高频信息表示的实际上就是图像内容上的边界。

##### 高频与低频在图像中的理解：

​	低频对应于图像内缓慢变化的灰度分量。例如：在一副大草原的图像中，低频对应着广袤的颜色趋于一致的草原。

​	高频对应于图像内变换越来越快的灰度分量，是由灰度的尖锐过度造成的。例如：在一副大草原的图像中，其中狮子的边缘信息。

​	衰减高频而通过低频，低通滤波器，将会使图像模糊。

​	衰减低频而通过高频，高通滤波器，将增强尖锐的细节，但是会导致图像的对比度降低。

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p16.png" alt="p16" style="zoom: 50%;" />

###### 																			高通道滤波 保留高频信息

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p17.png" alt="p17" style="zoom:50%;" />

###### 																			低通道滤波 保留低频信息





### Filtering =  Convolution (= Averaging)

#### Convolution

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p18.png" alt="p18" style="zoom: 50%;" />

###### 																				一维度线性滤波

### Convolution Theorem

#### Convolution in the spatial domain is equal to multiplication in the frequency domain,and vice versa

#### Option 1:

- ##### Filter by convolution in the spatial domain

#### Option 2:

- ##### Transform to frequency domain (Fourier transform)

- ##### Multiply by Fourier transform of convolution kernel

- ##### Transform back to spatial domain (inverse Fourier)

​	<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p19.png" alt="p19" style="zoom: 67%;" />

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p20.png" alt="p20" style="zoom:67%;" />



<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p21.png" alt="p21" style="zoom:50%;" />



<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p22.png" alt="p22" style="zoom:50%;" />

##### 		观察这上下两张图片，左侧为spatial domain 右侧为 frequency domain，现在将上图左侧的矩形放大，得到下图的结果，频域的变化反而变小了。在将左侧图像模糊的过程中，对图像进行卷积的操作，如果使用一个较大的box filter 那么得到的结果会变得越来越模糊，也更加接近于低频。 如果使用一个非常小的 box filter 进行卷积操作，则对应频域的范围就会变大，也就会留下更高的频域，模糊的效果就不会明显。



### Sampling = Repeating Frequency Contents

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p23.png" alt="p23" style="zoom: 50%;" />

		##### 		在这幅图中，a，b，c 分别是 时域中原始函数图像，冲击函数图像，原始函数乘以冲击函数，就会得到 e 图中的函数，也就是采样的结果。在右侧的频域中，冲击函数的频域依然是冲击函数，不过间隔有所变大，通过 时域的乘积是频域的卷积 可以说，采样就是在重复原始信号的频谱。  

###### 这里对于傅里叶变换与卷积的理解存在问题，以后会补充修改

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p24.png" alt="p24" style="zoom: 33%;" />

##### 		这幅图中展示的分别是 密集采样 与 稀疏采样结果在频域上重复的图片展示，可以看出，在进行稀疏采样时，采样的冲击函数之间的距离较大导致其频谱过小。所以当需要进行重复原始信号时，导致重复信号出现了叠加，导致走样的产生。



## Antialiasing

### How can we Reduce Aliasing Error ?

#### Option 1: Increase sampling rate

- Essentially increasing the distance between replicas in the Fourier domain 更高的采样频率
- Higher resolution  displays，sensors， framebuffers
- But：costly & may need very high resolution

​	

#### Option 2: Antialiasing

- Making Fourier contents "narrower" before repeating
- i.e. Filtering out high frequencies before sampling



### Antialiasing = Limiting, then repeating

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p25.png" alt="p25" style="zoom: 50%;" />

			##### 	在上图中，通过低通滤波去除了高频信号。频率图像可以在更低一些的采样率下依然做到不互相重叠，这样就通过先模糊再采样的方法解决了走样问题。

##### 	对于走样问题，增加采样率是最直接的解决方案，在图形图像中可以使用更换分辨率更大的硬件来提高分辨率，越高的分辨率代表了更高的采样率，意味着频谱与频谱之间的间隔足够大，就不容易出现频谱之间的混叠。

​	



### A Practical Pre-Filter

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p26.png" alt="p26" style="zoom: 50%;" />

	### Antialiasing By Averaging Value in Pixel Area

#### Solution:

- #### Convolve f(x,y) by a 1-pixel box-blur

  - Recall: convolving  = filtering = averaging

- #### Then sample at every pixel‘s center

###### 	先通过对每个像素进行模糊（卷积）再对每个像素中心进行采样



### Antialiasing By Computing Average Pixel Value

#### 	In rasterizing one triangle, the average value inside a pixel area of f(x,y) = inside(triangle, x, y) is equal to the area of the pixel covered by the triangle.

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p27.png" alt="p27" style="zoom:33%;" />



### Antialiasing By Supersampling (MSAA)

### Supersampling

#### 	Approximate the effect of the 1-pixel box filter by sampling multiple locations within a pixel and averaging their values:

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p28.png" alt="p28" style="zoom:33%;" />



### Supersampling : Step 1

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p29.png" alt="p29" style="zoom:33%;" />



### Supersampling : Step 2

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p30.png" alt="p30" style="zoom:33%;" />

### Supersampling : Result

<img src="C:\Users\userData\Desktop\GAMES101\Lecture6\p31.png" alt="p31" style="zoom:33%;" />

##### 		MASS 的思想是将一个像素划分为多个小像素，每个小像素有一个中心，可以判断在三角形内的小像素，再将这些小像素平均起来，如果点足够多的情况下可以得出比较好的效果。等同于在一个像素的内部增加采样点，例如在一个像素里，如果采样点不在三角形内，它被覆盖的比例就为0，如果一个采样点在三角形内，那么四个点的覆盖率平均下来就是25%，通过这样的方法实现了反走样的效果。同时引入 MASS 需要增加更多的点来判断是否在三角形内，会付出更多的计算量。

​	

### 关于FXAA 和 TAA

##### 		其他的具有里程碑意义的反走样方法有：FXAA (Fast Approximate AA) 和 TAA (Temporal AA)。FXAA是图像的后期处理方法，先把有锯齿的图得到，再通过一种操作把锯齿消掉。但是如果先得到锯齿的图，再做模糊操作是不对的，FXAA是通过图像匹配的方法找到边界然后替换成没有锯齿的边界，效率非常高。TAA方法是最近几年兴起的方法，它可以寻找上一帧的信息，用像素内的一个点没有做 MSAA来感知，假如我们看到的是静止的场景，相邻两帧看到的信息一样，那么我们可以用不同位置上的点来感知覆盖信息，那么大家会发现在时间范围内，得到的边界会各不相同。TAA是复用上一帧得到的像素的值，相当于将MSAA的样本分布在时间上，这样就没有引入任何额外的操作。





笔记补充： https://blog.csdn.net/weixin_39903176/article/details/112321441

图片的傅里叶变换：https://blog.csdn.net/qq_29788741/article/details/126414345

信号卷积：https://www.zhihu.com/question/272008168

傅里叶变换的通常解释：https://zhuanlan.zhihu.com/p/19763358	





