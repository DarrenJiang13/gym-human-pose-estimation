# learning-based-methods-for-human-pose-estimation
This folder is about how to implement your learning-based method for skeleton tracking in Linux system(like Ubuntu 16.04, Ubuntu18.04), including detailed tutorials.

Human pose estimation/human skeleton tracking problem can be divided into 4 parts:
- Single-Person Skeleton Estimation
- Multi-person Pose Estimation
- Video Pose Tracking
- 3D Skeleton Estimation

In this project, I would focus on some open-source, fast and lightweight neural networks for human pose estimation. Maybe I would move forward in two directions:
 - 2D human pose estimation + depth image 
 - 3D human pose estimation from a webcam

## 1. Review
- [Paper Review](https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/learning-based/%E4%BA%BA%E4%BD%93%E9%AA%A8%E6%9E%B6%E6%A3%80%E6%B5%8B%E6%96%87%E7%8C%AE%E8%B0%83%E7%A0%94.md)
- [Open-source Code Repo](https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/learning-based/%E4%BA%BA%E4%BD%93%E9%AA%A8%E6%9E%B6%E6%A3%80%E6%B5%8B%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%E8%B0%83%E7%A0%94.md)

## 2. Implementation
- 2D Pose
    - [Detectron2](https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/learning-based/webcam_detectron2.md)
    - [LightweightPose2D](https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/learning-based/Imp_LightweightOpenPose/Imp_LightweightOpenPose.md)
- 3D Pose
    - [LightweightPose3D](https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/learning-based/Imp_LightweightOpenPose3D/Imp_LightweightOpenPose3D.md)
