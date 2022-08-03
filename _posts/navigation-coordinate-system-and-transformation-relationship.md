---
title: 'Navigation coordinate system and transformation relationship'
date: 2022-08-03
permalink: /posts/2022/08/navigation_coordinate_system_and_transformation_relationship/
tags:
  - Coordinate Transformation
---

```
    def lla2enu(self,init_lla, point_lla):
        lat_o = math.radians(init_lla[0])
        lon_o = math.radians(init_lla[1])
        alt_o = math.radians(init_lla[2])
        lat_i = math.radians(point_lla[0])
        lon_i = math.radians(point_lla[1])
        alt_i = math.radians(point_lla[2])

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
        return point_enu
```