# Implement librealsense+Cubemos on Raspberry Pi OS
This is a tutorial about how to implement RealSense SDK 2.0, Cubemos on your Raspberry Pi OS.
| Required Info               |                      |
| --------------------------- | -------------------- |
| Camera Model                | D435               |
| Operating System & Version  | Linux (Raspberry Pi OS) |
| Platform                    | Raspberry Pi 3+      |
| SDK Version                 | v2.24.0              |

Intel® RealSense™ SDK 2.0[[github]](https://github.com/IntelRealSense/librealsense)  
Cubemos [[sdk download]](https://www.intelrealsense.com/skeleton-tracking/) 

## 1. Install Raspberry Pi OS on a Raspberry Pi 3B+
  [installation guide](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)  
  In this project, I installed a **Raspberry Pi OS (32-bit) with desktop** image without recommended softwares.
  
## 2. Install realsense SDK [reference](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation_raspbian.md)
- 2.1 install cmake
```
sudo apt-get install cmake
```
- 2.2 Add swap
Initial value is 100MB, but we need to build libraries so initial value isn't enough for that.
In this case, need to switch from 100 to `2048` (2GB).  
```
$ sudo vim /etc/dphys-swapfile
CONF_SWAPSIZE=2048

$ sudo /etc/init.d/dphys-swapfile restart swapon -s
```

-2.3 Install packages
```
$ sudo apt-get install -y libdrm-amdgpu1 libdrm-amdgpu1-dbgsys libdrm-dev libdrm-exynos1 libdrm-exynos1-dbgsys libdrm-freedreno1 libdrm-freedreno1-dbgsys libdrm-nouveau2 libdrm-nouveau2-dbgsys libdrm-omap1 libdrm-omap1-dbgsys libdrm-radeon1 libdrm-radeon1-dbgsys libdrm-tegra0 libdrm-tegra0-dbgsys libdrm2 libdrm2-dbgsys

$ sudo apt-get install -y libglu1-mesa libglu1-mesa-dev glusterfs-common libglu1-mesa libglu1-mesa-dev libglui-dev libglui2c2

$ sudo apt-get install -y libglu1-mesa libglu1-mesa-dev mesa-utils mesa-utils-extra xorg-dev libgtk-3-dev libusb-1.0-0-dev
```

-2.4 update udev rule
Now we need to get librealsense from the repo(https://github.com/IntelRealSense/librealsense).
```
$ cd ~
$ git clone https://github.com/IntelRealSense/librealsense.git
$ cd librealsense
$ sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/ 
$ sudo udevadm control --reload-rules && udevadm trigger 

```

-2.5 set path
```
$ sudo gedit ~/.bashrc
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

$ source ~/.bashrc

```

- 2.6 install `protobuf`
```
$ sudo apt install autoconf automake libtool
$ cd ~
$ git clone --depth=1 -b v3.5.1 https://github.com/google/protobuf.git
$ cd protobuf
$ ./autogen.sh
$ ./configure
$ make -j1
$ sudo make install
$ cd python
$ export LD_LIBRARY_PATH=../src/.libs
$ python3 setup.py build --cpp_implementation 
$ python3 setup.py test --cpp_implementation
$ sudo python3 setup.py install --cpp_implementation
$ export PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION=cpp
$ export PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION_VERSION=3
$ sudo ldconfig
$ protoc --version
```
-2.7 install `TBB`
```
$ cd ~
$ wget https://github.com/PINTO0309/TBBonARMv7/raw/master/libtbb-dev_2018U2_armhf.deb
$ sudo dpkg -i ~/libtbb-dev_2018U2_armhf.deb
$ sudo ldconfig
$ rm libtbb-dev_2018U2_armhf.deb
```

-2.8 install `OpenCV`
You can build from source code, but it takes so much time. In this case, we will use pre-build version to save time.
```
Remove previous version
$ sudo apt autoremove libopencv3

Install 
$ wget https://github.com/mt08xx/files/raw/master/opencv-rpi/libopencv3_3.4.3-20180907.1_armhf.deb
$ sudo apt install -y ./libopencv3_3.4.3-20180907.1_armhf.deb
$ sudo ldconfig
```

-2.9 install `RealSense` SDK/librealsense
```
$ cd ~/librealsense
$ mkdir  build  && cd build
$ cmake .. -DBUILD_EXAMPLES=true -DCMAKE_BUILD_TYPE=Release -DFORCE_LIBUVC=true
$ make -j1
$ sudo make install
```

-2.10 install `pyrealsense2`
```
$ cd ~/librealsense/build

for python2
$ cmake .. -DBUILD_PYTHON_BINDINGS=bool:true -DPYTHON_EXECUTABLE=$(which python)

for python3
$ cmake .. -DBUILD_PYTHON_BINDINGS=bool:true -DPYTHON_EXECUTABLE=$(which python3)

$ make -j1
$ sudo make install

add python path
$ vim ~/.zshrc
export PYTHONPATH=$PYTHONPATH:/usr/local/lib

$ source ~/.zshrc

```

-2.11 change `pi` settings (enable OpenGL)
```
$ sudo apt-get install python-opengl
$ sudo -H pip3 install pyopengl
$ sudo -H pip3 install pyopengl_accelerate
$ sudo raspi-config
"7.Advanced Options" - "A7 GL Driver" - "G2 GL (Fake KMS)"
```

Finally, need to reboot pi
```
$ sudo reboot
```


-2.12 Try RealSense D435
Connected D435 to the pi and open terminal
```