---
layout: post
title:  URB - Motion-Only BA
date:   2018-01-10 13:09:10 +0100
categories: issues
issue_id: 108
---

For calculating the pose of the camera we want to use the existing framework that ORB_SLAM2. This framework however, called g2o, is written in C++ while our implementation of SLAM is written in Python. Thankfully there are many opensource libraries that enable interfacing from and to C++ from within Python or vica-versa. After testing out `pyboost` and `pybind11` our preference went to `pybind11` due to it being lightweight and straightforward.

```cpp
#include <pybind11/pybind11.h>
#include <pybind11/eigen.h>

#include <Eigen/LU>
#include <Eigen/StdVector>
#include "urbg2o.h"

namespace py = pybind11;

PYBIND11_PLUGIN(urbg2o)
{
    py::module m("urbg2o", "pybind11 opencv example plugin");

    m.def("poseOptimization", &poseOptimization, "pose-only bundle adjustment",
	py::arg("coords").noconvert(), py::arg("pose"));
    
    return m.ptr();
}
```

## Docker
To quickly test the bindings in an isolated environment a new repository was made; [Urbinn g2o](https://github.com/urbinn/urbinn-g2o). For development purpose I've also made a Docker container setup to enable virtualization for local development. This enables the developer to seemlessly run the bindings on his local machine. I've also written a guide on how to use Docker:

<div class="fluid-media">
  <iframe id="dynamic" frameborder="0" src="https://docs.google.com/document/d/e/2PACX-1vSvC3BVVAGe2tjXQ9_hgI-yrRpjfrepT8CKnesb3nepnPDw1YHxyQgvkVljXO1N_VKEyNFkcktWtZOT/pub?embedded=true" width="100%" height="100%"></iframe>
</div>

## Development

After setting up the repository with a basic setup for building and launching the Docker container, it was time to implement Motion Only Bundle Adjustment. By using the (available demo code)[https://github.com/RainerKuemmerle/g2o/blob/master/g2o/examples/ba/ba_demo.cpp] and examining the implementation of motion only bundle adjustment within ORB_SLAM2 we created the first version of motion only bundle adjustment.

