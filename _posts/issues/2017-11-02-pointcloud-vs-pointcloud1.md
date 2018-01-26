---
layout: post
title:  Pointcloud vs Pointcloud
date:   2017-11-02 13:09:10 +0100
categories: issues
issue_id: 64
---

For evaluating a pointcloud based on a groundtruth we have tried different approaches. At first we have attempted to evaluate the generated point cloud a KITTI sequence through the ORB_SLAM2 algorithm by using the publicly available LiDAR data from the KITTI dataset. Unfortunately this was not practically feasible for us to use within the project as we did not have a LiDAR available. Nonetheless we still examined the possibilities through experiments by the off chance that we could borrow a LiDAR from the Technical University of Delft with which we cooperate. (Unfortunately the experiments conducted are lost and cannot be showcased in this issue.)

After coming to the conclusion that LiDAR based evaluation would not work, we identifed the possibilities of our ZED camera. This in turn gave us the idea to evaluate the point cloud by using the depth image that the camera could create. A quick experiment showed that depth images are not sufficient because they are always relative to a frame and the perspective of the camera onto the frame would result in varying depth images. Therefore we have decided that depth images are not practical for the evaluation of a point cloud.

```python
import xml.etree.ElementTree as ET
import os

FOLDER = "keyframes"

if not os.path.exists(FOLDER):
    os.makedirs(FOLDER)

keyframes = ET.parse("../mapXML01.xml")
```

```python
SKIP_KEYFRAMES = 1
TRESHOLD = 10

keyframe_list = []

for keyframe in keyframes.getroot():
    index = int(keyframe[1].text)
    frameId = keyframe.find("mnFrameId").text
    map_points = []
    
    for mapPoint in keyframe.find("mvpMapPoints"):
        x, y, z = mapPoint[0].text, mapPoint[1].text, mapPoint[2].text
        map_points.append([x, y, z])
    
    frame_list.append(frameId)
    keyframe_list.append((int(index), int(frameId), map_points))

# Sort the keyframe_list
def getKey(item):
    return item[0]

sorted_keyframe_list = sorted(keyframe_list, key=getKey)

```

```
336
(336,
  396,
  [['-2.68436313', '4.32813549', '130.885834'],
  ['11.8897705', '8.87325001', '110.120392'],
  ['13.8909149', '9.95214748', '113.765594'],
  ...
```
```python
from matplotlib import pyplot as plt
import numpy as np
import matplotlib 
import seaborn as sns

number_of_mappoints = []
for keyframe in sorted_keyframe_list:
    number_of_mappoints.append(len(keyframe[2]))
    
plt.figure(figsize=[16,10])    
plt.hist(number_of_mappoints)
plt.xlabel("Aantal punten")
plt.ylabel("Aantal keyframes")
plt.legend(loc=3)
plt.show()    
```