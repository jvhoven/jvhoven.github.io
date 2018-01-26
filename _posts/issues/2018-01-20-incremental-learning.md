---
layout: post
title:  ORB2 Incremental Learning
date:   2017-11-02 13:09:10 +0100
categories: issues
issue_id: 41
---

Using the implementation we had developed in issue 40, we attempted to apply incremental learning to the KITTI dataset. 
Our initial results were looking promising but that was only when we ran the algorithm with small datasets. This is where the various problems of ORB_SLAM2 unfolded. By making the algorithm analyze a rather large dataset we have found that alot of memory leaks were created, resulting in a crash of the algorithm during runtime.

This is a major flaw in the implementation and the outcome of this issue resulted in us evaluating if it was still worth going on with the ORB_SLAM2 algorithm.