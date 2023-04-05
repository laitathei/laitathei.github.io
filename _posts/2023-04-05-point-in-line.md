---
title: 'Point in Line'
date: 2023-04-02
author: Lai Tat Hei
permalink: /posts/2023/04/point_in_line/
excerpt: ''
tags:
  - geometry
---

1. Define Problem<br/>
<br/><img src='/images/point_not_in_line.png'><br/>
The problem of points in line is a very classical geometric problem. <br/>

2. Algorithm explaination<br/>
If the point located in the line, the following scenario will form. <br/>
<br/><img src='/images/point_in_line.png'><br/>
It will cause `dist(AC) + dist(CB) == dist(AB)`<br/>
<br/>
If the point not located in the line, the following scenario will form. <br/>
<br/><img src='/images/point_not_in_line.png'><br/>
It will cause `dist(AC) + dist(CB) != dist(AB)`<br/>
Therefore, we can create the function to solve the problem easily. <br/>

3. Code implementation<br/>

```
bool point_in_line(vector<vector<double>> line, vector<double> point)
{
    double distance_AC = sqrt(pow((line[0][0]-point[0]),2)+pow((line[0][1]-point[1]),2));
    double distance_BC = sqrt(pow((line[1][0]-point[0]),2)+pow((line[1][1]-point[1]),2));
    double distance_AB = sqrt(pow((line[0][0]-line[1][0]),2)+pow((line[0][1]-line[1][1]),2));
    if (distance_AC+distance_BC == distance_AB)
        return true;
    return false;
}
```
<br/>
