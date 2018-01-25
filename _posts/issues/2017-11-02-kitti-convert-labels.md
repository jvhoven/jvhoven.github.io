---
layout: post
title:  Convert Kitti Labels
date:   2017-11-02 13:09:10 +0100
categories: issues
issue_id: 0
---
```python
labels = { 'Car': 0, 'Cyclist': 1, 'Pedestrian': 2, 'Truck': 3 }
file = open('labels/01.txt', 'r')
output = open('labels/output-01.txt', 'w')

def convert(size, box):
    dw = 1./size[0]
    dh = 1./size[1]
    x = (box[0] + box[1])/2.0
    y = (box[2] + box[3])/2.0
    w = box[1] - box[0]
    h = box[3] - box[2]
    x = x*dw
    w = w*dw
    y = y*dh
    h = h*dh
    return (x,y,w,h)

while 1:
    line = file.readline()
    if not line: break
    
    columns = line.strip('\n').split(' ')
    
    xmin, ymin, xmax, ymax = columns[4], columns[5], columns[6], columns[7]
    b = (float(xmin), float(xmax), float(ymin), float(ymax))
    bb = convert((1242,375), b)
    
    output.write((str(labels[columns[0]]) + " " + " ".join([str(a) for a in bb]) + '\n'))
                 
file.close()
output.close()
```