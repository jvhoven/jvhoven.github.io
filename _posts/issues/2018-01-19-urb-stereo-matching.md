---
layout: post
title:  URB - Stereo Matching
date:   2018-01-10 13:09:10 +0100
categories: issues
issue_id: 108
---

Stereo matching is the process of matching a landmark in the left-hand-frame on the same spot in the right-hand-frame. In stereo images this can be done by calculating the disparity of the landmark. For each landmark identified in the left-hand-frame we create a patch we move along the X-axis to try and match the same landmark in the right-hand-frame. This is mostly referred to as a sliding window approach, to which you bruteforce a patch until you find a match you're most confident about that it exactly matches a spot on the right-hand-frame. We calculate the L2 norm between two patches to give us an indication of the similarities between the two.

```python
def patch_disparity(framepoint, frame_right):
    frame_left = framepoint.frame
    if framepoint.cy < PATCH_SIZE or framepoint.cy > frame_left.getHeight() - PATCH_SIZE or \
        framepoint.cx < PATCH_SIZE or framepoint.cx > frame_left.getWidth() - PATCH_SIZE:
            return None
    startx = framepoint.startx
    best_disparity = 0
    best_distance = sys.maxsize
    distances = []
    for disparity in range(0, startx):

        patchL = framepoint.get_patch()
        patchR = frame_right.get_image()[starty:starty+PATCH_SIZE, startx-disparity:startx+PATCH_SIZE-disparity]
        #print(patchL.shape, patchR.shape,leftxstart,patchSize, disparity)
        distance = cv2.norm(patchL, patchR, NORM)
        distances.append(distance)
        if distance < best_distance:
            best_distance = distance
            best_disparity = disparity
    
    # bepaal minimale distance op disparities meer dan 1 pixel van lokale optimum 
    minrest = sys.maxsize
    if best_disparity > 1:
        minrest = min(distances[0:best_disparity-1])
    if best_disparity < startx - HALF_PATCH_SIZE - 2:
        minrest = min([minrest, min(distances[best_disparity+2:])])
        
    # de disparity schatting is onbetrouwbaar als die dicht bij 1 komt
    # gebruik hier als threshold 1.4 om punten eruit te filteren waarvoor we
    # geen betrouwbare disparity estimates kunnen maken
    confidence = minrest / best_distance
    
    # Geef de beste disparity op pixel niveau terug, met de twee neighbors om subpixel disparity uit te rekenen
    if best_disparity == 0:
            return (best_disparity, [best_distance, distances[1], distances[2]], confidence)
    elif best_disparity == startx -1:
        return (best_disparity, [distances[best_disparity-2], distances[best_disparity-1], best_distance], confidence)
    return (best_disparity, [distances[best_disparity-1], best_distance, distances[best_disparity+1]], confidence)
```

*At the time of writing was still called framepoint within the code, by referring to a landmark I'm referring to frame_point within the code*

<img style="width: 80% !important" alt="Landmark detection" src="/public/images/urb/stereo-matching.png" class="diagram" />


