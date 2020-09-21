# Implement librealsense+OpenNI2+Nite2 on Ubuntu 18.04
This is a tutorial about how to implement RealSense SDK 2.0, OpenNI2 and Nite2 on your Ubuntu 18.04.
| Required Info               |                      |
| --------------------------- | -------------------- |
| Camera Model                | D435               |
| Operating System & Version  | Linux (Ubuntu 18.04) |
| Platform                    | PC                   |
| SDK Version                 | v2.24.0              |

Intel® RealSense™ SDK 2.0[[github]](https://github.com/IntelRealSense/librealsense)  
OpenNI2 [[sdk download]](https://structure.io/openni)[[github]](https://github.com/occipital/openni2)  
Nite2[[sdk download]](https://sourceforge.net/projects/roboticslab/files/External/nite/NiTE-Linux-x64-2.2.tar.bz2) 

## 1.install Intel® RealSense™ SDK 2.0
- 1.1 install Intel® RealSense™ SDK 2.0 [reference](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md)
	
	- Register the server's public key:
	```
	sudo apt-key adv --keyserver keys.gnupg.net --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE
	```

	- Add the server to the list of repositories
	```
	# Ubuntu 16
	sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial main" -u
	# Ubuntu 18
	sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo bionic main" -u
	```

	- Install the libraries (see section below if upgrading packages):
	```
	sudo apt-get install librealsense2-dkms
	sudo apt-get install librealsense2-utils
	```
	
	- Optionally install the developer and debug packages:
	```
	sudo apt-get install librealsense2-dev
	sudo apt-get install librealsense2-dbg 
	```

- 1.2 Reconnect the Intel RealSense depth camera and verify the installation.
	```
	# open a new terminal window and run
	realsense-viewer 
	```

- 1.3 Download the source files of librealsense for future use
	```
	git clone https://github.com/IntelRealSense/librealsense
	```

## 2. install OpenNI2

- 2.1 Download openNI2 from the official [website](https://structure.io/openni)
	- [32 bits](https://s3.amazonaws.com/com.occipital.openni/OpenNI-Linux-x86-2.2.0.33.tar.bz2)
	- [64 bits](https://s3.amazonaws.com/com.occipital.openni/OpenNI-Linux-x64-2.2.0.33.tar.bz2)  
	Extract the files to wherever you want  

- 2.2 Configure the environmental variables  
	```
	cd OpenNI-Linux-x64-2.2
	sudo ./install.sh
	```
- 2.2.5 CMake version update for Ubuntu16.04 (if you are using Ubuntu 18.04, skip this step)  
	Default CMake version for Ubuntu 16.04 is 3.5.x. To compile OpenNI2, you need a higher version than 3.8.x  
	
	- Download the `.tar.gz` file
	```
	sudo apt-get install build-essential
	wget http://www.cmake.org/files/v3.10/cmake-3.10.3.tar.gz	
	```
	- Extract the files and install
	```
	tar xf cmake-3.12.3.tar.gz
	cd cmake-3.12.3
	./configure
	make
	sudo make install
	```
	- Add Path to source 
	```
	sudo gedit ~/.bashrc
	```
	Add two lines below to `.bashrc`
	```
	export PATH=/usr/local/bin:$PATH
	export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
	```
	update source 
	```
	source ~/.bashrc
	```
	- check cmake version
	```
	cmake --version
	```

- 2.3 Integration for openni and librealsense [reference](https://github.com/IntelRealSense/librealsense/tree/master/wrappers/openni2)  
	- Compile librealsense  
	```
	cd librealsense/CMake
	cmake ..
	make
	``` 
	- configure path in CMakelists  
	```
	cd librealsense/wrappers/openni2
	gedit CMakeLists.txt 
	```
	set the **OPENNI2_DIR** to your openni lib path(you can find it in your "path-to-your-OpenNI-Linux-x86-2.2/OpenNIDevEnvironment")  
	set the **REALSENSE2_DIR** to your lib path
	```
	# DEPS
	set(OPENNI2_DIR "/home/pro/realsense_project/OpenNI-Linux-x86-2.2/Include" CACHE FILEPATH "OpenNI2 SDK directory")
	set(REALSENSE2_DIR "/home/pro/realsense_project/librealsense" CACHE FILEPATH "RealSense2 SDK directory")
	
	# if an error like [#error This file requires compiler and library support for the ISO C++ 2011 standard.] occurs, please add one line in your CMakeLists.txt
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
	``` 
	
	- Compile the realsense driver for OpenNI2  
	```
	cd librealsense/wrappers/openni2
	mkdir build
	cd build
	cmake ..
	make
	```

	- copy driver from librealsense directory to openni2 directory  
	For Linux, copy `librs2driver.so` in `librealsense/wrappers/openni2/build/_out` and `librealsense2.so` in `librealsense/CMake` to `OpenNI-Linux-x64-2.2/Samples/Bin/OpenNI2/Drivers/`    
	(if you want to launch NiViewer,copy `librs2driver.so` in `librealsense/wrappers/openni2/build/_out` and `librealsense2.so` in `librealsense/CMake` to `OpenNI-Linux-x64-2.2/Tools/OpenNI2/Drivers/` )

- 2.4 Try Launch any OpenNI2 example
	```
	cd OpenNI-Linux-x64-2.2/Samples/bin
	./SimpleViewer
	```
- 2.5 Try Launch NiViewer
	```
	cd OpenNI-Linux-x64-2.2/Samples/bin
	./SimpleViewer
	```

## 3. install Nite2.2
- 3.1 Download Nite2.2 from [here](https://sourceforge.net/projects/roboticslab/files/External/nite/NiTE-Linux-x64-2.2.tar.bz2)
	```
	wget https://sourceforge.net/projects/roboticslab/files/External/nite/NiTE-Linux-x64-2.2.tar.bz2
	```
	Extract it to your expected folder.  
	```
	cd NiTE-Linux-x64-2.2
	sudo ./install.h
	```
- 3.2 copy driver from librealsense directory to openni2 directory  
	For Linux, copy `librs2driver.so` in `librealsense/wrappers/openni2/build/_out` and `librealsense2.so` in `librealsense/CMake` to `NiTE-Linux-x64-2.2/Samples/Bin/OpenNI2/Drivers/`  

- 3.3 Configuration for Nite2 [reference](http://robots.uc3m.es/gitbook-installation-guides/install-openni-nite.html)
	```
	sudo ln -s $PWD/NiTE-Linux-x64-2.2/Redist/libNiTE2.so /usr/local/lib/  # $PWD should be /yourPathTo/NiTE-Linux-x64-2.2/..
	sudo ln -s $PWD/NiTE-Linux-x64-2.2/Include /usr/local/include/NiTE-Linux-x64-2.2  # $PWD should be /yourPathTo/NiTE-Linux-x64-2.2/..
	sudo ldconfig
	```
- 3.4 Try Launch some Nite2 Example
	```
	cd NiTE-Linux-x64-2.2/Samples/Bin
	./UserViewer
	```
	stand in front of your camera for a while for the camera to track you.

![Alt text](https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/images/openni_rs.png "optional title")
