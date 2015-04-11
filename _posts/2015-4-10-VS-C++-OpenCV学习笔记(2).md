---
layout: post
category: c++
title: VS C++ OpenCV使用笔记（2）
tagline: by wt
tags: [c++, OpenCV]
---
领导给的第一个任务是学习怎么利用角点检测做棋盘标定，学完了以后就敲一段代码实现长度测量，估计是想让我借此练练手罢。于是我花了一个下午和一个晚上把这个任务搞定了，
结果今天上班领导不在，那就先做一下笔记好了，也不算偷懒吧。

<!--more-->

##功能要求

- 相机拍照后，用鼠标点击照片上两点，即可获得两点之间位移信息，实现长度测量。
- 所用相机成像质量很高，测量前无需做图像校正处理。
- 暂未做精度要求。

##设计分析

- 工作前，需要标定长度。
- 定标后，被测表面和相机距离保持不变。
- 读取照片并打开。
- 鼠标点击图片两点，输出长度信息。

那么程序设计就可以大概分成两步：标定和测量。

####标定：

翻了一下`Learning OpenCV`这本堪称经典的书，里面第11章介绍了如何进行摄像机标定，但是它的标定目的是为了消除摄像机畸变和坐标旋转，而老板给我说的是相机成像质量很高，无需做校正处理。也就是说，相机内参数为理想值，而坐标旋转对于我要实现的测量并无影响。这样一来，事情就简单多了，我只需要建立像素点和实际长度之间的对应关系，即可实现测量。

如何求取像素点和实际长度的对应关系呢？OpenCV里面有一个非常方便的方法：棋盘（Chessboard）。

给定一个棋盘图像，可以用`findChessboardCorners()`来定位棋盘角点。

	bool findChessboardCorners(
		InputArray image,
		Size patternSize, 
		OutputArray corners,
		int flag = CALIB_CB_ADAPTIVE_THRESH
		+ CALIB_CB_NORMALIZE_IMAGE
		+ CALIB_CB_FAST_CHECK)

- **image** 棋盘图，必须是一个8位灰度或真彩图，一般我们都要将图片转换为8位灰度图。
- **patternSize** 棋盘图内部角点数量，包含行和列： `patternSize = cvSize(points_per_row,points_per_colum) = cvSize(columns,rows)`
- **corners** 用来存储检测到的角点位置信息：`vector<Point2f> corners；`
- **flag** 标志位信息详见API。

使用这个函数，可以将所有的角点坐标值都存储在`corners`当中，可以通过`corners[i].x`和`corners[i].y`调用，真是非常方便。

角标坐标找到后，剩下的就只是建立映射关系了，这都是很简单的东西。

####测量：

在这部分遇到的问题主要是**如何获取鼠标点击事件的像素坐标值**。

######在OpenCV中提供了这样一个函数

	void setMouseCallback(
		const string& winname, 
		MouseCallback onMouse, 
		void* userdata=0 )

- **winname** 是你要操作的窗口，我们在这个窗口中打开一个图片
- **onMouse** 是一个鼠标反馈函数，控制着鼠标事件发生时我们需要做的事情。具体使用方法参看[例子](https://github.com/Itseez/opencv/tree/master/samples/cpp/ffilldemo.cpp)。
- **userdata** 是传递给事件反馈的参数，可选，默认为0。

使用这个函数，我们需要定义`onMouse()`函数，每当鼠标点击事件发生时，再结合`CvScalar cvGet2D(const CvArr* arr, int idx0, int idx1)`即可获得像素坐标，剩下的就只是把这些像素值转换为长度值。更详细的使用方法请看[OpenCV响应鼠标函数cvSetMouseCallback（）和其副程式onMouse（）的使用（OpenCV2.4.5）](http://blog.csdn.net/glb562000520/article/details/8938582)

那么到此基本思路就搞定了。可以发现，根本就没什么算法上面的难点，只是一些C++的语法和OpenCV库函数的使用而已，非常简单。但是在做这个任务的过程中，我看了很多OpenCv的API文档，直接相关的间接相关的都看了一眼，收获还是蛮大的。
