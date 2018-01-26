---
layout: post
title:  Kitti Point Cloud
date:   2017-11-02 13:09:10 +0100
categories: issues
issue_id: 70
---

With this issue we had to run the different KITTI sequences through the ORB_SLAM2 algorithm in order to extract a pointcloud in `csv` format. At first there were some installation problems which I've assisted with in order to get a functional algorithm. After resolving the installation problems we created a new method to save the landmarks indentified by ORB_SLAM2 to a `csv` file. The end result was a `csv` file with the following format:

```
x, y, z
1.422089,0.565656,10.932565
-0.961294,-0.454188,3.225033
-0.953023,-1.210474,4.256725
-0.934926,-1.382938,4.195824
2.647709,-1.207784,7.269565
2.674868,-0.523064,13.203098
2.215391,-1.841818,9.004871
-1.980812,-1.812535,23.361725
-1.064424,-0.930619,3.535723
1.773993,-1.518770,4.366801
```

This could then be used by importing the `csv` into CloudCompare, a program that used for analyzing Point Clouds, to experiment with the available features within the program.