---
layout: post
category: c++
title: Visual C++ OpenCV使用笔记（1）
tagline: by wt
tags: [c++, OpenCV]
---
从四月一号开始，我在杭州拓峰科技参加实习，部门是机器人视觉，顾名思义，是做机器视觉的，主要编程语言是c++和c#。因此，我以前学过的python和java就没了用武之地，而且我还要开始学习我最不情愿学习的语言：C++。每思及此，蛋莫疼焉。除此之外，我还要接触OpenCV，这个强大而闻名的函数库。俗话说，既来之则安之，那便好好学一学怎么用OpenCV做视觉吧，顺便把C++也好好学一学，滔神学编程，多多益善。

<!--more-->

### 1) 最简单的OpenCV应用程序示例：
	#include<iostream>
	#include<opencv2/highgui/highgui.hpp>
	using namespace cv;
	using namespace std;

	Mat image = imread("img.jpg");
	namedWindow("My image");//这行可以注释掉，直接用下一句
	imshow("My image", image);
	waitKey(0);
	cout<<image.size().height;//打印image的size信息

##### Mat**类建立可操作的矩阵，用**imread()**读入图片信息。
	cv：Mat ima(240,320,CV_8U,cv::Scalar(100));
在这个例子中，需要指定建立的矩阵的参数信息。`CV_8U`指定像素点为一个byte即8位的图片，其中的`U`表示无符号，当然也可以使用有符号的`S`。对于真彩图片，还需要使用`CV_8UC3`设定3通道。也可以使用`CV_16SC3`表示16位图片，甚至可以指定32或64位的图片。

当Mat创建的矩阵超出范围时，分配的内存将自动释放，这一点非常棒。另外，值得注意的是，Mat类实现了引用计数和浅复制，使用`image1 = image2`的时候，你实现的其实只是引用的复制，仍然指向同一个对象。当然，可以使用`image1.copyTo(image2)`方法实现image1到image2的对象的复制。
	

- **nameWindow**创建一个窗口，非必要。
- **imshow()**建立名为"My image"的窗口，并打开**image**。
- **waitKey(0)**用于在控制台关闭终端前等候用户输入，参数不为零则表示等待的毫秒数。

##### 使用imwrite将可操作文件对象输出到指定文件
	imwrite("output.bmp", image);
	
**所有用于C++ API的OpenCV类和函数都被定义在了名字空间cv中**

###2) 获取像素点，处理像素点
	void salt(Mat &image, int n)
	{
		for(int k=0; k<n; k++)
		{
			int i = rand() % image.cols;
			int j = rand() % image.rows;
			if(image.channels() == 1)
			{
				image.at<uchar>(j, i) = 255;
			}
			else if(image.channels() == 3)
			{
				image.at<Vec3b>(j, i)[0] = 255;
				image.at<Vec3b>(j, i)[1] = 255;
				image.at<Vec3b>(j, i)[2] = 255;
			}
		}
	}
	
上面这个函数实现了获随机像素点，并将n个像素点的值置为255。值得注意的是，我们对于灰度图和真彩图的处理是不一样的，真彩图有三个通道，而灰度图只有一个。

#####向这个函数传递一个图像即可调用它：
	void main()
	{
		void salt(Mat &image, int n);
		Mat image = imread("img.jpg");
		salt(image, 3000);
		imshow("Image", image);
		waitKey(0);
	}
	
`cv::Mat`包含了很多种方法来获取图片的属性信息。public成员变量`rows`和`cols`一幅图像像素点矩阵的行列数，`Mat`还提供了`at(int i, int j)`方法，实现对像素点的直接操作。然而这个方法必须指定在编译时产生的返回值的型别，即使Mat可以hold住所有数据类型的元素，程序也必须指定你所期望的返回类型。这也是为什么`at`方法被作为一个模板方法的原因。所以，当调用`at`方法时，必须通过这样的方式指定返回值类型：`image.at<uchar>(j, i) = 255`

程序员必须保证你所指定的返回值类型和Mat矩阵里面的元素类型相一致，`at`方法不会做任何类型转换。

在真彩图中，每一个像素点包含了R/G/B三个通道的信息。

######因此cv:Mat创建的真彩图会返回含三个8bit数据的向量值，操作真彩图的像素点应该这样写：
	image.at<Vec3b>(j, i)[channel] = 255//`channel`指定了一个颜色通道,取1、2、3中


