---
title: 'Dot product and Cross product'
date: 2022-11-27
author: Lai Tat Hei
permalink: /posts/2022/11/dot_product_and_cross_product/
excerpt: ''
tags:
  - Numpy
---

1. Cross product<br/>

<br/><img src='/images/cross_product_picture_1.png'><br/>

Cross product have a lot of application in engineering purpose, such as check line intersections and check point in polygon<br/>
Cross product should be included 3-axis, so we need to expand 2-axis point to 3-axis point<br/>

<br/><img src='/images/cross_product_picture_2.png'><br/>

If `res > 0`, it means AC located in AB anticlockwise direction<br/>
If `res = 0`, it means AC is parallel with AB<br/>
If `res < 0`, it means AC located in AB clockwise direction<br/>

2. Cross Product Python implementation<br/>

```
import numpy as np
a = np.array([0,1])
b = np.array([-1,1])
res = (a[0])*(b[1]) - (a[1])*(b[0])
print(res)
print(np.cross(a,b))
```
<br/>

3. Dot product<br/>

Dot product can measure the angle between two vector

<br/><img src='/images/dot_product_picture_1.png'><br/>

If `res > 0`, it means the angle is 0<θ<90<br/>
If `res = 0`, it means the angle is θ=90, which means two vector is perpendicular<br/>
If `res < 0`, it means the angle is 90<θ<180<br/>

4. Dot Product Python implementation<br/>

```
import numpy as np
a = np.array([4,6])
b = np.array([-3,7])
res = (a[0])*(b[0]) + (a[1])*(b[1])
print(res)
print(np.dot(a,b))
```
<br/>

5. Rotation Matrix Python implementation

import numpy as np
import math
p = np.array([1,0])
rotation_angle = 90 # refers to angle between X axis and the line (clockwise is negative) (anticlockwise is positive)
new_x = p[0]*math.cos(math.radians(rotation_angle)) - p[1]*math.sin(math.radians(rotation_angle))
new_y = p[0]*math.sin(math.radians(rotation_angle)) + p[1]*math.cos(math.radians(rotation_angle))
new_p = np.array([new_x,new_y])
print(new_p)
