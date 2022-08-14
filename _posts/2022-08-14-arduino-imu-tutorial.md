---
title: 'Arduino IMU tutorial'
date: 2022-08-14
author: Lai Tat Hei
permalink: /posts/2022/08/arduino_imu_tutorial/
excerpt: ''
tags:
  - IMU
  - Arduino
---

1. Arduino UNO with MPU6500<br/>
<br/><img src='/images/arduino_mpu6500_connection.png'><br/>
`
git clone https://github.com/jrowberg/i2cdevlib
`
<br/>
Place `Arduino` folder into `C:\Program Files (x86)\Arduino\libraries`<br/>
Copy `I2Cdev` header file and source file from `I2Cdev` folder to `MPU6050`<br/>
Connect arduino to PC and upload `MPU6050_DMP6` to arduino