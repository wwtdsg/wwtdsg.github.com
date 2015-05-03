---
layout: post
category: jottings
title: 结构光表面标定精度测试记录
tagline: by wt
tags: [Accuracy Test, jottings]
---

##测试条件
- 镜头焦距： 12mm
- 相机像素： 30万（659x494）
- 光源： 630mm普通红色线激光
- 工作距离： 景深约60mm，最低端约150mm。（设备最下端到桌面的距离）

<!--more-->

##测试步骤

###相机标定

用棋盘格标定系统，获取相机内参和畸变系数。

**标定代码**：[CameraCalibrate.cpp](https://github.com/wwtdsg/My-code-in-c-/blob/master/cameraCalibrationTestInC.cpp)

###结构光平面标定

使用相机标定得到的内参数矩阵和畸变系数矩阵，采用5*3棋盘格配合黑色激光投影区域的特制标定模板。

**标定代码**：[StrucLightCalibrate.cpp](https://github.com/wwtdsg/My-code-in-c-/blob/master/StrucLightCalibrate.cpp)

###平面精度测试

用刻度尺当做被测物，测量实际距离，计算测量误差。

**刻度尺姿态**：平放桌上。

**测试代码**：[threeDRebuild.cpp](https://github.com/wwtdsg/My-code-in-c-/blob/master/threeDRebuild.cpp)

##测试结果：

###Test1:： 测试长度100mm，测量值为100.013mm，误差 < 1mm。
![image](https://raw.githubusercontent.com/wwtdsg/My-code-in-c-/master/picture/test1.png)

###Test2：测试长度100mm，测量值为100.77mm，误差 < 1mm。
![image](https://raw.githubusercontent.com/wwtdsg/My-code-in-c-/master/picture/test2.jpg)

###Test3：测试长度100mm，测量值为100.88mm，误差 < 1mm。
![image](https://raw.githubusercontent.com/wwtdsg/My-code-in-c-/master/picture/test3.jpg)

由于测量方法采用手动选点，所以测量精度受人为因素影响较大。在实际结构光三维重构过程中，结构光中心线提取的精度也对重构精度具有很大影响。

与之前同事所设计的结构光平面标定程序比较，精度差别不大，但是程序运行速度更快，代码更少。

#刻度尺倾斜姿态下，精度测量还没有做，有待回到公司后取像测试。
