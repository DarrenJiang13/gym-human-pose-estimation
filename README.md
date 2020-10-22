# gym-human-pose-estimation
This repository is for a project detecting static human joint parameters in the gym. The basic skill used here is called human pose estimation/human skeleton tracking.

There are several ways to accomplish skeleton tracking.   
All methods can mainly be divided into two types: **RGBD-based** and **RGB-based**.   
- RGBD-based type means getting 3D human joint keypoints from the combination of a 2D image and a depth image.
- RGB-based type means getting 2D/3D human joint keypoints from a 2D image (with neural networks) and calculating the joint coordinates with depth data.  

Here are some tutorials to implement the methods.
- [SDK based](https://github.com/DarrenJiang13/gym-human-pose-estimation/tree/master/realsense-SDK-based) 
- [Learning based](https://github.com/DarrenJiang13/gym-human-pose-estimation/tree/master/learning-based)

Hopefully I can get an EI conference paper for my master's degree with this project.
