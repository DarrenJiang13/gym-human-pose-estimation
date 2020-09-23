# Implement detectron2+webcam on Ubuntu 18.04
This is a tutorial about how to implement detectron2, webcam on your Ubuntu 18.04.
| Required Info               |                      |
| --------------------------- | -------------------- |
| Camera Model                | D435               |
| Operating System & Version  | Linux (Ubuntu 18.04) |
| Platform                    | PC                   |
| SDK Version                 | v2.24.0              |

1. Requirements
  Install Nvidia Driver,CUDA10.1,cuDNN 7.6.5,pytorch, [here](https://github.com/DarrenJiang13/VideoPose3DwithDetectron2/blob/master/documents/GPUConfiguration.md)
  
2. Install detectron2 [here](https://github.com/facebookresearch/detectron2/blob/master/INSTALL.md)
  Check your cuda and torch version

3. Download detectron2 project from [github](https://github.com/facebookresearch/detectron2) 
  ```
  git clone https://github.com/facebookresearch/detectron2.git
  ```
4. Run a demo [reference](https://github.com/facebookresearch/detectron2/blob/master/GETTING_STARTED.md)
  ```
  cd detectron2/demo
  python3 demo.py --config-file ../configs/COCO-Keypoints/keypoint_rcnn_R_50_FPN_1x.yaml \
  --webcam \
  --opts MODEL.WEIGHTS detectron2://COCO-Keypoints/keypoint_rcnn_R_50_FPN_1x/137261548/model_final_04e291.pkl
  ```
  ![Alt text](https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/images/detectron2_webcam.png "detectron2_webcam")
