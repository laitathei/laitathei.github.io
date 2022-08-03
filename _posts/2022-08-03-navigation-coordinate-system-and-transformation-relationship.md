---
title: 'Navigation coordinate system and transformation relationship'
date: 2022-08-03
author: Lai Tat Hei
permalink: /posts/2022/08/navigation_coordinate_system_and_transformation_relationship/
excerpt: 'Nowadays, navigation is very popular in our life, and many applications use GNSS as one of the reliable data sources. Therefore, this blog will discuss the mathematics implemented for navigation purposes.'
tags:
  - Coordinate Transformation
---


1. Introduction
In this blog, I will talk about the most common navigation coordinate system (frame) used in application level with code implementation. The reference book for this blog is `Fundamentals of Inertial Navigation, Satellite-based Positioning and their Integration` and `Principles of GNSS, Inertial, and Multisensor Integrated Navigation Systems`<br/>

2. Earth-Centered Earth-Fixed Frame (ECEF frame)
ECEF frame share the same origin and z-axis as the ECI frame (Earth-Centered Inertial Frame), but the main difference between these two navigation frame is ECEF frame rotates along with the Earth. In application level, most of the GNSS default data will follow ECEF frame, which means it provided latitude, longitude, altitude data in ECEF frame. Therefore, most people will use LLA as short forms to describe latitude, longitude, altitude data, but they are actually refering to ECEF frame. <br/>
<br/><img src='/images/ECI_ECEF_difference.PNG'><br/>

3. Local-Level Frame (LLF frame)
LLF frame aim to represent a vehicle’s attitude and velocity when on or near the surface of the Earth. Sometimes, people will also call this frame as local geodetic or navigation frame. Also, there are two kind of coordinate representations which are NED (north, east and down) and ENU (east, north and up) to describe xyz position order. <br/>
<br/><img src='/images/ned_enu_description.PNG'><br/>

4. World Geodetic System (WGS)
As the earth is not perfect sphere, many scientists spend a lot of effort figuring out the parameters and equations best suited to approximate the shape of the Earth. In application level, most people will use WGS84 in calculation.<br/>
```
a = 6378137.0  # equatorial radius / semimajor axis (m)
b = 6356752.3142 # polar radius / semiminor axis (m)
f = 0.00335281066 # flattening
e = 0.08181919 # Eccentricity
gm = 3.986004418E14  # Gravitational constant (m^3/s^2)
omega_ie = 7.2921151467E-05  # Earth rotation rate (rad/s)
```
<br/>
5. Code Example
<br/>There are some code samples to demonstrate coordinate transformation. <br/>

```
      def lla2enu(self,current_lla, reference_lla): # those lla in degrees
          lat_o = math.radians(current_lla[0]) 
          lon_o = math.radians(current_lla[1])
          alt_o = math.radians(current_lla[2])
          lat_i = math.radians(reference_lla[0])
          lon_i = math.radians(reference_lla[1])
          alt_i = math.radians(reference_lla[2])

          x_o = ((a/((1-e_square*(math.sin(lat_o))**2)*0.5)) + alt_o)*math.cos(lat_o)*math.cos(lon_o)
          y_o = ((a/((1-e_square*(math.sin(lat_o))**2)*0.5)) + alt_o)*math.cos(lat_o)*math.sin(lon_o)
          z_o = ((a/((1-e_square*(math.sin(lat_o))**2)*0.5))*(1-e_square) + alt_o)*math.sin(lat_o)
          x_i = ((a/((1-e_square*(math.sin(lat_i))**2)*0.5)) + alt_i)*math.cos(lat_i)*math.cos(lon_i)
          y_i = ((a/((1-e_square*(math.sin(lat_i))**2)*0.5)) + alt_i)*math.cos(lat_i)*math.sin(lon_i)
          z_i = ((a/((1-e_square*(math.sin(lat_i))**2)*0.5))*(1-e_square) + alt_i)*math.sin(lat_i)

          rotation_matrix = np.array([[-math.sin(lon_o), math.cos(lon_o),0],
                                      [-math.sin(lat_o)*math.cos(lon_o), -math.sin(lat_o)*math.sin(lon_o), math.cos(lat_o)],
                                      [math.cos(lat_o)*math.cos(lon_o), math.cos(lat_o)*math.sin(lon_o), math.sin(lat_o)]])

          delta_position = np.array([x_i,y_i,z_i]) - np.array([x_o,y_o,z_o])
          point_enu = np.matmul(rotation_matrix, delta_position)
          return point_enu # return the local cartesian position
      def enu2lla(self,point_enu,reference_lla)
          rotation_matrix = np.array([[-math.sin(lon_o), math.cos(lon_o),0],
                                      [-math.sin(lat_o)*math.cos(lon_o), -math.sin(lat_o)*math.sin(lon_o), math.cos(lat_o)],
                                      [math.cos(lat_o)*math.cos(lon_o), math.cos(lat_o)*math.sin(lon_o), math.sin(lat_o)]])
          point_lla = np.matmul(rotation_matrix.T, point_enu) + reference_lla
          return point_lla 
```
