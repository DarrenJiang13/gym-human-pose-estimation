# realsense-for-skeleton-tracking-linux
This repository is about how to implement your Intel realsense camera for skeleton tracking in Linux system(like Ubuntu 16.04, Ubuntu18.04), including detailed tutorials for some SDK configurations.     
**Skeleton tracking** means getting your joint keypoints in 3D format.

There are several ways to accomplish skeleton tracking with realsense d400 series.   
All methods can mainly be divided into two types: **RGBD-based** and **RGB-based**.   
- RGBD-based type means getting 3D human joint keypoints from the combination of a 2D image and a depth image.
- RGB-based type means getting 2D human joint keypoints from a 2D image (with neural networks) and calculating the joint coordinates with depth data.  

Here are some tutorials to configure the librealsense SDK 2.0 and some open-source or commercial skeleton tracking SDKs.(as RGB-based methods are too slow and needs GPU,so they are less discussed in this repo.)  
- RGBD based
  - [realsense+OpenNI2+Nite2](https://github.com/DarrenJiang13/realsense-for-skeleton-tracking-linux/blob/master/rs_OpenNI2_Nite2.md)
  - [realsense+Cubemos](https://github.com/DarrenJiang13/realsense-for-skeleton-tracking-linux/blob/master/rs_Cubemos.md)
  - [realsense+Nuitrack](https://github.com/DarrenJiang13/realsense-for-skeleton-tracking-linux/blob/master/rs_Nuitrack.md)
- RGB based
  - [Detectron2](https://github.com/DarrenJiang13/realsense-for-skeleton-tracking-linux/blob/master/webcam_detectron2.md)
  - others
