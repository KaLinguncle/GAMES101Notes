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



笔记补充： https://blog.csdn.net/weixin_39903176/article/details/112321441

图片的傅里叶变换：https://blog.csdn.net/qq_29788741/article/details/126414345

信号卷积：https://www.zhihu.com/question/272008168

​	





## Antialiasing



How can we Reduce Aliasing Error ?

Option 1: Increase sampling rate

- Essentially 



​			

​		





​	





- ### Antialiasing in practice



## Visibility /occlusion

- ### Z-buffering







#### Sampling Artifacts in Computer Graphics

Artifacts due to sampling - Aliasing



Behind the Aliasing Artifacts

​	Signals are changing too fast (high frequency) but sampled too slowly





### Antialiasing

### Frequency Domain



Filtering  =  Getting rid of certain frequency Domain

Filtering = Convolution (= Averaging)

Sampling  = Repeating Frequency Contents

Aliasing = Mixed Frequency Contents



How Can We Reduce Aliasing Error ?

Option 1:  Increase sampling rate

Option 2: Antialiasing    Filtering out high frequencies before sampling





### Anitaliasing By Supersampling (MSAA)

