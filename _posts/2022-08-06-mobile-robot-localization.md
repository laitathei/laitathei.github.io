---
title: 'Mobile robot localization'
date: 2022-08-06
author: Lai Tat Hei
permalink: /posts/2022/08/mobile_robot_localization/
excerpt: 'Robot localization is a hot topic in the research field, especially now that it is increasingly important for commercial use. Therefore, this blog will discuss some simple robot positioning methods, with code explanation.'
tags:
  - Robot Localization
---

1. Introduction <br/>
In general, today's robots are already equipped with a lot of sensors for better localization capabilities. Thanks to the efforts of precise sensor data, people can easily obtain the current position of the robot through algorithms. The algorithm can provide two kinds of data, namely odometry and dead reckoning. The most notable difference between them is that odometry relies on wheel sensors, while dead reckoning relies on heading sensors such as gyroscope. Also, sensor data included error, which means the algorithm result will have drifting. Therefore, filtering is required to have better result.<br/>

2. Velocity to odom <br/>
One of the ways is to calculate the position from the robot velocity and yaw angle. The robot speed can be measured by the wheel encoder, and the yaw angle can be measured by the IMU or calculated by the wheel encoder. Therefore, the robot pose vector is represented like this:<br/>
<br/><img src='/images/pose_vector.png'><br/>
As the sensor data are coutinuous, which means there is a sampling time between two consecutive data for each sensor. The robot pose vector can convert to discrete form as follow:<br/>
<br/><img src='/images/discrete_pose_vector.png'><br/>
Finally, we can get the robot current position and its orientation. There is a code implementation of the above algorithm in ROS and the robot coordinate system illustration. In the code example, the velocity data come from wheel encoder and the yaw angle come from IMU.<br/>

```
#!/usr/bin/env python3
import rospy
import math
from geometry_msgs.msg import Twist,PoseStamped
from nav_msgs.msg import Odometry,Path
from sensor_msgs.msg import Imu
from tf.transformations import quaternion_from_euler,euler_from_quaternion

class vel2odom():
    def __init__(self):
        rospy.init_node('vel2odom')
        self.odom_pub = rospy.Publisher("/car_odom",Odometry,queue_size=1)
        self.path_pub = rospy.Publisher("/path",Path,queue_size=1)
        rospy.Subscriber("/cmd_vel", Twist,self.cmd_callback)
        rospy.Subscriber("/imu", Imu, self.imu_callback)
        self.pos_x = 0
        self.pos_y = 0
        self.yaw = 0
        self.path = Path()

    def cmd_callback(self,data):
        self.vx = data.linear.x
        self.vy = data.linear.y
        self.vz = data.linear.z
        self.ax = data.angular.x
        self.ay = data.angular.y
        self.az = data.angular.z

    def imu_callback(self,data):
        self.ox = data.orientation.x
        self.oy = data.orientation.y
        self.oz = data.orientation.z
        self.ow = data.orientation.w
        self.rpy = euler_from_quaternion([self.ox,self.oy,self.oz,self.ow])
        self.yaw = math.degrees(self.rpy[2])
        print("yaw: {}".format(self.yaw))

    def loop(self):
        if self.vx!=0.0:
            self.current_time = rospy.Time.now().to_sec()
            self.dt = self.current_time-self.last_time

            #update the position and yaw
            front_and_back = self.vx
            pos_x = front_and_back*math.cos(math.radians(self.yaw))*self.dt
            pos_y = front_and_back*math.sin(math.radians(self.yaw))*self.dt
            self.pos_x = self.pos_x + pos_x
            self.pos_y = self.pos_y + pos_y
            orientation = quaternion_from_euler(0,0,self.yaw)

            posestamp = PoseStamped()
            self.path.header.frame_id = "odom"
            self.path.header.stamp = rospy.Time.now()
            posestamp.pose.position.x = self.pos_x
            posestamp.pose.position.y = self.pos_y
            posestamp.pose.position.z = 0
            posestamp.pose.orientation.x = orientation[0]
            posestamp.pose.orientation.y = orientation[1]
            posestamp.pose.orientation.z = orientation[2]
            posestamp.pose.orientation.w = orientation[3]
            self.path.poses.append(posestamp)
            self.path_pub.publish(self.path)

if __name__=="__main__":
    start = vel2odom()
    r = rospy.Rate(10) # 10hz
    while not rospy.is_shutdown():
        rospy.wait_for_message("/cmd_vel", Twist)
        rospy.wait_for_message("/imu", Imu)
        start.last_time = rospy.Time.now().to_sec()
        start.loop()
        r.sleep()
```
