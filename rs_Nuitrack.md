# Implement librealsense+Nuitrack on Ubuntu 18.04
This is a tutorial about how to implement RealSense SDK 2.0, Nuitrack on your Ubuntu 18.04.
| Required Info               |                      |
| --------------------------- | -------------------- |
| Camera Model                | D435               |
| Operating System & Version  | Linux (Ubuntu 18.04) |
| Platform                    | PC                   |
| SDK Version                 | v2.24.0              |

Intel® RealSense™ SDK 2.0[[github]](https://github.com/IntelRealSense/librealsense)  
Nuitrack [[sdk download]](http://download.3divi.com/Nuitrack/platforms/)  [[github]](https://github.com/3DiVi/nuitrack-sdk)

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
  
## 2.install Nuitrack SDK [reference](http://download.3divi.com/Nuitrack/doc/Installation_page.html)
- 2.1 Download Nuitrack
   Download Nuitrack from [here](http://download.3divi.com/Nuitrack/platforms/), here I choose `nuitrack-ubuntu-amd64.deb` though my system is x64	
- 2.2 [For Ubuntu 18.04] Install the libpng12-0 package. 
   ```
   sudo apt install libpng12-0
   ```
- 2.3 Install the downloaded package using the following command: 
  ```
  sudo dpkg -i <downloaded-package-name>.deb
  ```
  - if you encounter the error like
  ```
  WARNING: Can not load library module: /usr/etc/nuitrack/middleware/libNuitrackModule.so
  ERROR: Empty factory for DepthProvider
  ```
  check the solution in **3D Sensor Specific Requirements** [here](https://download.3divi.com/Nuitrack/doc/Installation_page.html)  
  - if there is an error related to `NiViewer`, try to remove `openni-utils`
  ```
  sudo apt remove --purge openni-utils
  ```

- 2.4 Log out to let the system changes take effect. Check that the environment variables **NUITRACK_HOME** and **LD_LIBRARY_PATH** are set correctly using the following commands
  ```
  echo $NUITRACK_HOME
  echo $LD_LIBRARY_PATH
  ```
  NUITRACK_HOME should be equal to `/usr/etc/nuitrack`. LD_LIBRARY_PATH should include `/usr/local/lib/nuitrack` path.

- 2.5 Launch an example and run Activation with your license
  - Turn on a new terminal and run the command `nuitrack`, you will see a window.   
  - Pick your device in drop-down menu，and click test. 	
  - Entry your license acquired from [nuitrack website](https://nuitrack.com/#pricing) and choose `activate`.	
  - If you keep failed in activation and your test program can only last for 3-4 seconds, try step 2.6.

- 2.6 Launch an example and run Activation with your license
  - Turn on a new terminal and run the command `nuitrack_device_api_sample`, you will see a window.   
  - Pick your device in drop-down menu，and click test. 	
  
