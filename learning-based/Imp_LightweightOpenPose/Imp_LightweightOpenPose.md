# Implementation of LightweightOpenPose
This repository is about the implementation of a open-source project **Lightweight Open Pose**.
Including the installation of [Intel® OpenVINO ToolKit](https://software.intel.com/content/www/us/en/develop/tools/openvino-toolkit/choose-download.html) 
and demo testing with the pretrained model. OpenVINO is the toolkit which could accelerate your CPU to do the inference.

| Projects  |  date |  author |  github |  framework | highlight | paper |
|---|---|---|---|---|---|---|
| **Lightweight OpenPose**  | 2018  | Daniil-Osokin  Intel  |[github](https://github.com/Daniil-Osokin/lightweight-human-pose-estimation.pytorch)| PyTorch   | -CPU enabled | Real-time 2D Multi-Person Pose Estimation on CPU: Lightweight OpenPose (ArXiv 2018)|

<div align=center><img src="https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/images/lightweight_CPU_2D.png" height="360" width="481"/></div>
  
## Environments
- Ubuntu 18.04
- Python 3.6
- PyTorch 1.4.0 

This project will start from a demo using the [pretrained model](https://download.01.org/opencv/openvino_training_extensions/models/human_pose_estimation/checkpoint_iter_370000.pth),
re-training might be done in the future.

## 0.Download Project Repo 
```
git clone https://github.com/Daniil-Osokin/lightweight-human-pose-estimation.pytorch.git
```

## 1.Install OpenVINO 
[[installation guide]](https://docs.openvinotoolkit.org/2020.4/openvino_docs_install_guides_installing_openvino_linux.html) 
[[Chinese blog]](https://blog.csdn.net/weixin_43741611/article/details/89365458)
[[Chinese blog 2]](https://blog.csdn.net/weixin_43741611/article/details/89365458)
- **download the zipped file and extract it**  
  Download the [toolkit](https://software.intel.com/content/www/us/en/develop/tools/openvino-toolkit/choose-download/linux.html) into your computer. 
  My version is **l_openvino_toolkit_p_2020.4.287**
  ```
  cd ~/Downloads/
  tar -xvzf l_openvino_toolkit_p_<version>.tgz
  ```
- **install through the GUI**
  ```
  cd l_openvino_toolkit_p_<version>
  sudo ./install_GUI.sh
  ```
  Click next until the installation finished.
  I just ignore the warning of "Intel Graphics Computer Runtime for OpenCL Driver is missing but you will be..." cause I only want to use CPU.
  
- **install the dependencies**
  ```
  cd /opt/intel/openvino/install_dependencies
  sudo -E ./install_openvino_dependencies.sh
  ```
- **add environment variables**   
  You have 2 choices here, choose one as you like:
  - load OpenVINO everytime when start bash
    ```
    sudo gedit ~/.bashrc
    ## add this line to the file
    source /opt/intel/openvino/bin/setupvars.sh
    ```
    Save the file and run`source ~/.bashrc`.Configuration completes if you can see
    ```
    [setupvars.sh] OpenVino environment initialized
    ```
    
  - load OpenVINO manually everytime when you need it (setting this as a short command)
    ```
    sudo gedit ~/.bashrc
    ## add this line to the file
    alias ov='source /opt/intel/openvino/bin/setupvars.sh'
    ```
    Save the file and run `ov`. Configuration completes if you can see
    ```
    [setupvars.sh] OpenVino environment initialized
    ```
    
- **configure model optimizer**
  OpenVINO employs network models called IR*(Intermediate Representation), 
  while `.xml` file saves the topology structure of the network
  and `.bin` file saves the weights w and bias b of the model.
  So we need to configure model optimizer if we want to use models from Tensorflow, Caffe and others.
  
  ```
  cd /opt/intel/openvino/deployment_tools/model_optimizer/install_prerequisites
  # install all the model optimizers
  sudo ./install_prerequisites.sh
  ```
  
- **check installation**
  - run the image classification verification script
  ```
  cd /opt/intel/openvino/deployment_tools/demo
  ./demo_squeezenet_download_convert_run.sh
  ```
  You will see the `demo completed successfully` displayed on the screen.  
  - run the inference pipeline verification script
  ```
  cd /opt/intel/openvino/deployment_tools/demo
  ./demo_security_barrier_camera.sh
  ```
  You will see a car picture with the text `<Hebei>MD711`


## 2. Set the Pre-trained Model
- Download the pretrained model
  ```
  cd path-to-your-project-repo/lightweight-human-pose-estimation.pytorch
  mkdir checkpoints
  cd checkpoints
  wget https://download.01.org/opencv/openvino_training_extensions/models/human_pose_estimation/checkpoint_iter_370000.pth
  ```
- Conversion to onnx format
  ```
  cd path-to-your-project-repo/lightweight-human-pose-estimation.pytorch
  cp scripts/convert_to_onnx.py ./ # if you do not move the script to the root directory, it will fail to import models
  python3 convert_to_onnx.py --checkpoint-path checkpoints/checkpoint_iter_370000.pth
  ```
  You will see a model file called `human-pose-estimation.onnx` in `lightweight-human-pose-estimation.pytorch`
  
- Conversion to OpenVINO format
  ```
  cd path-to-your-project-repo/lightweight-human-pose-estimation.pytorch
  python3 /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model human-pose-estimation.onnx --input data --mean_values data[128.0,128.0,128.0] --scale_values data[256] --output stage_1_output_0_pafs,stage_1_output_1_heatmaps
  ```
  This produces model `human-pose-estimation.xml` and weights `human-pose-estimation.bin` in single-precision floating-point format (FP32).
  
  -------------
  **: PyTorch version higher than 1.5 would fail in this procedure with an error called `No node with name stage_1_output_0_pafs`.  
  You can install the older version in this step just for transverting the model file and reinstall your previous pytorch after this step.
  Try to reinstall your PyTorch to 1.4.0 using 
  ```
  pip3 install torch===1.4.0+cu100 torchvision===0.5.0+cu100 -f https://download.pytorch.org/whl/torch_stable.html
  ```
  After this step, execute **Conversion to onnx format** and **Conversion to OpenVINO format** again.  
  **: Here is a code for installing my previous pytorch
  ```
  pip3 install torch==1.6.0+cu101 torchvision==0.7.0+cu101 -f https://download.pytorch.org/whl/torch_stable.html
  ```
  
## 3. Run a Demo!
- build c++ samples
  ```
  cd /opt/intel/openvino_2020.4.287/inference_engine/demos/
  bash build_demos.sh
  ```
  you will get executable files in `<user_path>/omz_demos_build/intel64/Release`
- run a c++ demo
  ```
  cd <user_path>/omz_demos_build/intel64/Release
  ```
  run your demo with a webcam
  ```
  ov #start your openvino environment
  ./human_pose_estimation_demo -m <pathTo>/human-pose-estimation.xml -i cam
  ```
  run your demo with video file
  ```
  ov #start your openvino environment
  ./human_pose_estimation_demo -m <pathTo>/human-pose-estimation.xml -i <pathTo>/yourvideo.mp4
  ```  
- run a python demo
  ```
  python3 demo.py --checkpoint-path <path_to>/checkpoint_iter_370000.pth --video 0
  ```
