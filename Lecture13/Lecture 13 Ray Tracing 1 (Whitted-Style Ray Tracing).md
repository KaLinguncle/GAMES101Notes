## Lecture 13 Ray Tracing 1 (Whitted-Style Ray Tracing)

### Why Ray Tracing?

- #### Rasterization couldn't handle global effects well

  - (Soft) shadows
  - And especially when the light bounces more than once

- #### Rasterization is fast, but quality is relatively low

- #### Ray tracing is accurate, but is very slow

  - Rasterization: real-time, ray tracing: offline
  - ~10K CPU core hours to render one frame in production



	### Light Rays

​	Three ideas about light rays

1. Light
2. Light
3. Light

"And if you gaze long into an abyss, the abyss also gazes into you." 



### Recursive (Whitted-Style) Ray Tracing

### Question 1:

#### 	Ray-Surface Intersection

​		Ray is defined by its origin and direction vector

​		Ray equation:

​			$r(t) =  o +td \quad 0 \le t \le \infin $

​			t is time , o is origin, d is diriction

​		

​		Sphere: $p: (p-c)^{2} -R^{2} = 0$



​		Solve for intersection:

​				 $(o +td-c)^{2} -R^{2} = 0$

​		

​		



###  