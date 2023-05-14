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

​	



------

​			

## Antialiasing



How can we Reduce Aliasing Error ?

Option 1: Increase sampling rate

- Essentially 



​			

​		



笔记补充： https://blog.csdn.net/weixin_39903176/article/details/112321441

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

