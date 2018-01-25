---
layout: post
title:  Depth Map Evaluation
date:   2017-11-02 13:09:10 +0100
categories: issues
issue_id: 64
---

```python
import cv2
import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline  
```

```python
left = cv2.imread('/data/urbinn/datasets/kitti/sequences/00/image_2/000000.png', 0)
right = cv2.imread('/data/urbinn/datasets/kitti/sequences/00/image_3/000000.png', 0)
```

```python
stereo = cv2.StereoBM_create(numDisparities=16, blockSize=15)
disparity = stereo.compute(left, right)

class Pixel:
    x = 0
    y = 0
    disparity = 0
    
    def as_list(self):
        return [(self.x, self.y), self.disparity]
    
    def __init__(self, x, y, disparity):
        self.x = x
        self.y = y
        self.disparity = disparity
    
    def __str__(self):
        return '{}: x:{}, y:{}, disparity'.format(self.x, self.y, self.disparity)
    
sp = left.shape
rows,cols = sp
pixels = []

for i in range(rows):
    for j in range(cols):
        px = Pixel(i, j, disparity[i][j])
        pixels.append(px)
```

```python
pixels[3].as_list()
highest_first = sorted(pixels, key=lambda p: p.as_list()[1], reverse=True) 
coord, _ = highest_first[0].as_list()

rgb = np.dstack([left, left, left])
rgb[coord[0], coord[1], :] = [0, 0, 255]

cv2.imwrite('image.png', rgb)
cv2.imwrite('disparity.png', disparity)
x, y = coord
print(x, y)
#show2(rgb, disparity)
```