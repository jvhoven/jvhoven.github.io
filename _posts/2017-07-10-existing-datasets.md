---
layout: post
title:  Literature scan - Existing datasets
date:   2017-07-10 13:09:10 +0100
categories: issues
issue_id: 2
---

A literature scan for publicy available datasets for evaluation an autonomous vehicle or a SLAM algorithm. Along with the datasets we've established a list of the sensors used to record the dataset. 
By indexing the datasets we can find the most suitable one for our project. In order to thoroughly test our algorithm it was necessary to create a list based on the features of the dataset. We have found four prominent datasets:

|   |  | Oxford (5) | Munich (6) | Munich (7) | KITTI (8) |
|---|:----------:|:----------:|:----------:|:---------:|
| **Video** | Camera | Front + Back | Front | Front | Front |
| | Stereo | Front | No | No | Front |
| | Color | Color | Mono | Mono | 2 Mono 2 Color |
| | Resolution | 1024x1024 | N.a | N.a | 1382 x 512 |
| | FPS | N.a | 50 | 50 | 10 |
| | Images | ~20.000.000 | 30.950 | 190.576 | N.a |
| **LIDAR** | 2D | Yes | No | No | Yes |
| | 3D | Yes | No | No | Yes |
| **Map** | Route (2D) | Yes | Yes | Yes | Yes |
| | Semantic (3D) | No | Yes | Yes | Yes |
| | GPS | Yes | No | No | Limited |
| **Time** | Day | Yes | Yes | Yes | Yes |
| | Night | Yes | No | No | No |
| **Weather** | Sunny | Yes | Yes | Yes | Yes |
| | Cloudy | Yes | No | No | Yes |
| | Rainy | Yes | No | No | Yes |
| **Road** | City | Yes | No | No | Yes |
| | Remote | No | No | No | Yes |
| | Highway | No | No | No | Yes |


