---
layout: post
title:  URB - Semantic Map
date:   2018-01-10 13:09:10 +0100
categories: issues
issue_id: 121
---

```python
import sys
import matplotlib.pyplot as plt
import matplotlib.colors as colors
import matplotlib.patches as mpatches
sys.path.append('..')
from src.kitti import *
import numpy as np
import cv2
import json
import glob
from src.boundingbox import format_yolo_output
from src.trajectory import Trajectory
from src.sequence import load_keyframes

%matplotlib inline
```

### Analyzing KITTI sequence 00 image_2


```python
LEFTDIR = '/data/urbinn/datasets/kitti/sequences/00/image_0'
RIGHTDIR = '/data/urbinn/datasets/kitti/sequences/00/image_1'

yolo_data = format_yolo_output("/data/urbinn/darknet/output/seq00_image02/objects.json")
                
frames = []
for filename in sorted(glob.glob(LEFTDIR + '/*')): 
    image_name = filename.split('/')[-1]
    frame = None
    
    if not image_name in yolo_data:
        print("frame {} has no yolo data available".format(image_name))
        frame = Frame(filename, RIGHTDIR)
    else:
        frame = Frame(filename, RIGHTDIR, classifications = yolo_data[image_name])
        
    frames.append(frame)
```

    frame 004540.png has no yolo data available


Note: For some reason frame 004540 has no objects detected. This might be possible but requires further investigation.

#### Setting up a sequence


```python
seq = Sequence()
for f in ProgressBar()(frames):
    seq.add_keyframe(f)
```
```python
DIMENSION = 400

poses = [kf.get_pose() for kf in seq.keyframes]
```

#### Evaluating trajectory plot


```python
def load(file):
    keyframeids, frameids, poses = load_keyframes(file)
    return keyframeids, frameids, poses.reshape(poses.shape[0], 4, 4)

sequence = 0
folder = '/home/jeroen/notebooks/urb/resultspose'
keyframeids, frameids, poses_test = load(folder + '/keyframes_%02d_all_17_1.6_1.6.npy'%(sequence))
```

#### Update found observations with classifications


```python
points_per_frame = []
for kf in seq.keyframes:
    kf.update_observations_per_classification()
    kf.filter_no_classification()
    obs = kf.get_observations()
    
    points_per_frame.append(obs)
```

#### Visualize observations with classifications per frame


```python
show(draw_observations_classification(points_per_frame[4]))
```


<img style="width: 100% !important;" alt="" src="/public/images/urb/output_11_0.png" class="diagram" />

## Semantic Map

#### Plot trajectory with observations


```python
flatten = lambda l: [item for sublist in l for item in sublist]
trajectory = Trajectory(poses, (400, 400), points_per_frame)
trajectory2 = Trajectory(poses_test, (400, 400), [])

#show2(trajectory.draw(True, False), trajectory2.draw(True, False))
img = trajectory.draw(True, False)
#trajectory._poses[:10]
```

#### Analyze found classes for setting up a legend


```python
classifications = []
for obs in flatten(points_per_frame):
    if obs.classification not in classifications:
        classifications.append(obs.classification)

classifications
```
    ['Window',
     'Cyclist',
     'Car',
     'Van',
     'Traffic_lights',
     'Lamppost',
     'Pedestrian',
     'Door',
     'Fence',
     'Pole',
     'Forbidden_to_stand_still',
     'Guidance_beacon_right']

```python
LEGEND = {
    'Window': (255, 0, 0),
    'Cyclist': (0, 255, 0),
    'Car': (0, 0, 255),
    'Van': (255, 100, 0),
    'Traffic_lights': (255, 255, 0),
    'Lamppost': (255, 255, 100),
    'Pedestrian': (100, 255, 0),
    'Door': (50, 50, 255),
    'Fence': (255, 50, 50),
    'Pole': (50, 255, 50),
    'Forbidden_to_stand_still': (0, 0, 0),
    'Guidance_beacon_right': (0, 100, 0)
}

def create_legend_entry(key, from_dict=LEGEND):
    r, g, b = from_dict[key]
    return mpatches.Patch(color=(b/255.0, g/255.0, r/255.0), label=key)

PATCHES = []
for obj in LEGEND.keys():
    PATCHES.append(create_legend_entry(obj)) 
    
plt.legend(handles=PATCHES)
plt.figure(figsize=(10, 20))
plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
```

<img style="width: 50% !important;" alt="" src="/public/images/urb/sym_map.png" class="diagram" />
