---
layout: post
title:  URB - Landmark selection
date:   2018-01-10 13:09:10 +0100
categories: issues
issue_id: 108
---

We present a novel approach to Simultaneous Localization and Mapping algorithms... I'm just kidding. URB is the experiment in which we have taken the general structure of ORB_SLAM2 and try to optimize the implementations and/or design decisions it makes. For the first part of URB, Tracking, we have identified actions ORB_SLAM2 takes in order to succesfully identify a landmark and estimate its depth. The first step in this process is landmark selection. 

ORB_SLAM2 uses an implementation from the opensource library OpenCV2 in order to identify and match landmarks a stereo image. Our idea was based on using a canny edge filter, e.g the Sobel operator, to select landmarks across a vertical line. 

```python
SOBELV = np.array([-1, 0, 1, -2, 0, 2, -1, 0, 1]).reshape((3,3))
SOBELH = np.array([-1, -2, -1, 0, 0, 0, 1, 2, 1]).reshape((3,3))
# these 5x3 filters promote pixels that are at the bottom/bottom of a vertical edge 
BOTTOMEDGE = np.array([-1, 0, 1, -1, 0, 1, -1, 0, 1, 1, 0, -1, 1, 0, -1]).reshape((5,3))
TOPEDGE = np.array([1, 0, -1, 1, 0, -1, -1, 0, 1, -1, 0, 1, -1, 0, 1]).reshape((5,3))
# this 3x3 filters identify tge top/bottom of binary edges
BOTTOMV = np.array([0, 0, 0, -1.0, 1.0, -1.0, -1.0, -1.0, -1.0]).reshape((3,3))
TOPV = np.array([-1.0, -1.0, -1.0, -1.0, 1.0, -1.0, 0, 0, 0]).reshape((3,3))

def filterImage(img, kernel):
    return cv2.filter2D(img, -1, kernel)

# filter image absolute if you do not care about direction
def filterImageAbs(img, kernel, threshold):
    img = filterImage(img, kernel)
    img = np.abs(img)
    if threshold is not None:
        _, img = cv2.threshold(img, threshold, 255, cv2.THRESH_BINARY)
    return img

def sobelv(img, threshold = None):
    return filterImageAbs(img, SOBELV, threshold)

def sobelh(img, threshold = None):
    return filterImageAbs(img, SOBELH, threshold)

def lowerVerticalEdge(img, threshold = None):
    return filterImageAbs(img, BOTTOMEDGE, threshold)

def higherVerticalEdge(img, threshold = None):
    return filterImageAbs(img, TOPEDGE, threshold)

def topVerticalEdge(img):
    return filterImage(img, TOPV)

def bottomVerticalEdge(img):
    return filterImage(img, BOTTOMV)

def getKeypoints(img):
    # compute the median of the single channel pixel intensities for thresholding
    v = np.median(img) * 0.95

    # blur the image to supress noise from being detected
    smoothed = cv2.GaussianBlur(img,(5,5),0)
    
    higherVerticalEdges = higherVerticalEdge(smoothed, v)
    lowerVerticalEdges = lowerVerticalEdge(smoothed, v)
    veTop = topVerticalEdge(higherVerticalEdges)
    veBottom = bottomVerticalEdge(lowerVerticalEdges)

    return veTop + veBottom
```
*This code has been created by Jeroen Vuurens*

Early implementations show promising results:

<img style="width: 80% !important" alt="Landmark detection" src="/public/images/urb/early-landmark-detection.png" class="diagram" />