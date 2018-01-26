---
layout: post
title:  ORB2 Development
date:   2017-11-02 13:09:10 +0100
categories: issues
issue_id: 53
---

To coordinate the development of the extended functionality within ORB_SLAM2 it was decided that a central repository should be created. At first we had the idea of creating a OS specific branch; one for Windows, one for MacOs and one for Linux. But in the end we have decided not to as it would be a mess to orchestrate development across all platforms. Instead we have opted to develop the algorithm by utilizing the server that was available to us. 

This enabled everyone to clone the repository in their home directory and run it from there. This would ensure efficiency in running experiments as they could be quite lengthy. The only downside of this approach was that ORB_SLAM2 was no longer able to output video footage of it analyzing a frame. 

The repository can be found on the Urbinn github: [ORB_SLAM2 repository](https://github.com/urbinn/orb2)