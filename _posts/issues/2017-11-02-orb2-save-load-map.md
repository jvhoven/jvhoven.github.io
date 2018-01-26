---
layout: post
title:  ORB2 Save/Load map
date:   2017-11-02 13:09:10 +0100
categories: issues
issue_id: 40
---

Because the first results of the ORB_SLAM2 algorithm were rather promising we wanted to try and experiment with incremental learning. Our initial thought was to extend ORB_SLAM2 functionality by adding a function to enable saving and loading data from the algorithm. Because the output of ORB_SLAM2 was a rather sparse 3D point cloud, we wanted to try and make it more dense so we could clearly identify objects by examining the point cloud. 

Luckily for us there was already a fork of the ORB_SLAM2 algorithm which incorporated many of the functionalities we had envisioned. Therefore we have decided to build upon the work of others (yay for opensource!) and test out the implemented functionality to begin with. 