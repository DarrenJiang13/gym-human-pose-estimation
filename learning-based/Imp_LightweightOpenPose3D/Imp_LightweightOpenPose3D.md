# Implementation of LightweightOpenPose3D
This repository is about the implementation of a open-source project **Lightweight Open Pose 3D**.
Including the installation of [OpenCV 4](https://opencv.org/releases/) 
and demo testing with the pretrained model. 

| Projects  |  date |  author |  github |  framework | highlight | paper |
|---|---|---|---|---|---|---|
| Real-time 3D Multi-person Pose Estimation  | 2019  | Daniil-Osokin Intel  |[github](https://github.com/Daniil-Osokin/lightweight-human-pose-estimation-3d-demo.pytorch)| PyTorch   | - CPU enabled;- OpenVINO for speedup; - **Cubemos** related? | Single-Shot Multi-Person 3D Pose Estimation From Monocular RGB (ArXiv 2019)  |

<div align=center><img src="https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/images/lightweight_CPU_3D.png" height="360" width="481"/></div>
  
## Environments
- Ubuntu 18.04
- Python 3.6
- OpenCV 4.0
- PyTorch 1.4.0 when you need to convert model to onnx and OpenVINO format.

This project will start from a demo using the [pretrained model](https://drive.google.com/file/d/1niBUbUecPhKt3GyeDNukobL4OQ3jqssH/view?usp=sharing),
re-training might be done in the future.

## 0.Download Project Repo 
  ```
  git clone https://github.com/Daniil-Osokin/lightweight-human-pose-estimation-3d-demo.pytorch.git
  ```

## 1.Install OpenCV 4 [Chinese installation guide](https://blog.csdn.net/new_delete_/article/details/84797041)
- install dependencies
  ```
  sudo apt-get install cmake #ignore this 
  sudo apt-get install build-essential libgtk2.0-dev libgtk-3-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev
  ```
- Download source file from [OpenCV release](https://opencv.org/releases/), I choosed [OpenCV 4.3.0](https://github.com/opencv/opencv/archive/4.3.0.zip).
For Chinese friends, if you feel it too slow to download it with browser, you can copy this link into 迅雷.
- Extract the source files and build it 
  ```
  cd opencv-4.3.0
  mkdir build
  cd build
  cmake -D CMAKE_BUILD_TYPE=Release -D OPENCV_GENERATE_PKGCONFIG=YES ..
  make # may take a while to make
  sudo make install
  ```
  You may need to wait a few minutes for the computer to download the ippicv during cmake process

## 2.Configure OpenCV 4 [Chinese installation guide](https://blog.csdn.net/new_delete_/article/details/84797041)
- Config the `pkg-config` of OpenCV
  - find the location of `opencv4.pc`
    ```
    sudo find / -iname opencv4.pc
    ```
    You can find the location as `/usr/local/lib/pkgconfig/opencv4.pc`
  - Add the path to `PKG_CONFIG_PATH`
    ```
    sudo gedit /etc/profile.d/pkgconfig.sh
    # add one line in this file
    export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH
    ```
    save and exit, run 
    ```
      source /etc/profile
    ```
  - Check the activiation
    ```
    pkg-config --libs opencv4
    ```
- configure OpenCV dynamic library
  ```
  sudo gedit /etc/ld.so.conf.d/opencv4.conf
  # add one line as:
  /usr/local/lib
  ```
  run `sudo ldconfig` to activate the settings
  
## 3.Test OpenCV
- Compile with makefile
   ```
   cd path-to-your-opencv-source-file/opencv-4.3.0/samples/cpp/example_cmake
   sudo gedit Makefile
   ```
   Change the `Makefile` to 
   ```
   CXX ?= g++
 
   CXXFLAGS += -c -std=c++11 -Wall $(shell pkg-config --cflags opencv4)
   LDFLAGS += $(shell pkg-config --libs --static opencv4)

   all: opencv_example

   opencv_example: example.o; $(CXX) $< -o $@ $(LDFLAGS)

   %.o: %.cpp; $(CXX) $< -o $@ $(CXXFLAGS)

   clean: ; rm -f example.o opencv_example
   ```
- Make and run
   ```
   cd path-to-your-opencv-source-file/opencv-4.3.0/samples/cpp/example_cmake
   make
   ./opencv_example
   ```
## 4.Prerequisites
1.Install requirements:
  ```
  pip3 install -r requirements.txt
  ```
2.Build `pose_extractor` module:
  ```
  python3 setup.py build_ext
  ```
3. Add build folder to PYTHONPATH:
  ```
  export PYTHONPATH=pose_extractor/build/:$PYTHONPATH
  ```
## 5.Run the demo
- Download model from [google drive](https://drive.google.com/file/d/1niBUbUecPhKt3GyeDNukobL4OQ3jqssH/view?usp=sharing) and put it in `lightweight-human-pose-estimation-3d-demo.pytorch/models`
- Running on GPU with a webcam
  ```
  python3 demo.py --model ./models/human-pose-estimation-3d.pth --video 0
  ```
  > Camera can capture scene under different view angles, so for correct scene visualization, please pass camera extrinsics and focal length with `--extrinsics` and `--fx` options correspondingly (extrinsics sample format can be found in data folder). In case no camera parameters provided, demo will use the default ones.

## 6.Inference with OpenVINO
- Install OpenVINO, see [this](https://github.com/DarrenJiang13/gym-human-pose-estimation/edit/master/learning-based/Imp_LightweightOpenPose/Imp_LightweightOpenPose.md)
- start OpenVINO environment
  ```
  ov
  ```
- Conversion to onnx format (Remember to roll your pytorch version back to 1.4, you can install a higher version after next step)
  ```
  cd path-to-your-project-repo/lightweight-human-pose-estimation-3d.pytorch
  cp scripts/convert_to_onnx.py ./ # if you do not move the script to the root directory, it will fail to import models
  python3 convert_to_onnx.py --checkpoint-path ./models/human-pose-estimation-3d.pth
  ```
  You will see `human-pose-estimation-3d` files with suffix like `.onnx`, `.bin`,`.mapping`,`.xml`
- Conversion to OpenVINO format(Remember to roll your pytorch version back to 1.4, you can install a higher version after this step)
  ```
  python3 <OpenVINO_INSTALL_DIR>/deployment_tools/model_optimizer/mo.py --input_model human-pose-estimation-3d.onnx --input=data --mean_values=data[128.0,128.0,128.0] --scale_values=data[255.0,255.0,255.0] --output=features,heatmaps,pafs
  ```
- Run the script with OpenVINO inference
  ```
  python3 demo.py --model human-pose-estimation-3d.xml --device CPU --use-openvino --video 0
  ```
