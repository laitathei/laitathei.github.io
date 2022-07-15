---
title: "Real time Object Tracking"
excerpt: "Short description of portfolio item number 1<br/><img src='/images/real_time_object_tracking.png'>"
collection: portfolio
---

Hardware:
1. Jetson Xavier NX
2. Intel RealSense D455 depth camera

Software:
1. TensorRT
2. ONNX
3. YOLOv4 (Darknet)

Theory Explaination:
1. Coordinate transformation

The object detection model can provide the object pixel position in the image, which is under pixel coordinate system. If we want to get the corresponding object position in the real world, coordinate transformation are required. The first transformation can convert the coordinate from pixel coordinate to image coordinate. Below picture described the mathematical relationship between two different coordinate system.  <br />
![image](https://github.com/laitathei/laitathei.github.io/blob/master/images/pixel_to_image.png) <br />

The second transformation is related to image coordinate to camera coordinate, which can provide the real world position. The mathematical relationship is illustrated below.  <br />
![image](https://github.com/laitathei/laitathei.github.io/blob/master/images/camera_to_image.png) <br />
After combined these two transformation equation, we can got the relationship between image coordinates and pixel coordinates. <br />
![image](https://github.com/laitathei/laitathei.github.io/blob/master/images/2D_to_3D.jpg) <br />
Fx and Fy refers camera intrinsics, U and V refers center coordinate. As we are using Intel RealSense depth camera, we can easily to get the Fx and Fy via the realsense-viewer SDK in ubuntu. Also, you can use ROS to get the Fx and Fy via Intel official ROS wrapper. The Intel RealSense Depth camera is stereo camera, which can provide the depth information that already perpendicular to the camera plane. Therefore, the transformation equation between image coordinates and pixel coordinates can further simplify to two equations <br />
![image](https://github.com/laitathei/laitathei.github.io/blob/master/images/simplify_equation.jpg) <br />

2. 
