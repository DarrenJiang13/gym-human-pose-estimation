# Implement librealsense+Cubemos on Ubuntu 18.04
This is a tutorial about how to implement RealSense SDK 2.0, Cubemos on your Ubuntu 18.04.
| Required Info               |                      |
| --------------------------- | -------------------- |
| Camera Model                | D435               |
| Operating System & Version  | Linux (Ubuntu 18.04) |
| Platform                    | PC                   |
| SDK Version                 | v2.24.0              |

Intel® RealSense™ SDK 2.0[[github]](https://github.com/IntelRealSense/librealsense)  
Cubemos [[sdk download]](https://www.intelrealsense.com/skeleton-tracking/)  

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
  
## 2.install Cubemos SDK [reference](https://dev.intelrealsense.com/docs/skeleton-tracking-sdk-installation-guide)
- 2.1 Download the installation package
     Apply for a 30-day free trail or buy commercial license of cubemos from [intel website](https://www.intelrealsense.com/skeleton-tracking/). After applying for the license, you will get a license and a download link sent to your mailbox.
- 2.2 installation 
    ```
    sudo apt-get install ./cubemos-SkeletonTracking_*.deb
    ```
- 2.3 set the environment variables
   Log out and log in again to make sure that all environment variables are loaded correctly.    
   When you log in to the system, turn on a new terminal, type `$CUBEMOS_SKEL_SDK`, and you will see 
   ```
   bash: /opt/cubemos/skeleton_tracking: Is a directory
   ```
   continue.
- 2.4 activate the software
  ```
  cd /opt/cubemos/skeleton_tracking/scripts/
  # do not use `sudo` here or you may encounter some environment variable problems.
  bash ./post_installation.sh
  ```
  when you see
  ```
  Please enter your license key (format: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX) and press [ENTER]
  ```
  input your license key. Continue.  
  If there is any question about the installation, please refer to the trouble shooting part in 
  [manual book](https://download-skeleton-tracking-sdk.s3.eu-central-1.amazonaws.com/GettingStartedGuide.pdf)(Global Proxy needed for our Chinese friends who use VPN).   

- 2.5 Launch an example
  ```
  cd ~/cubemos-samples/build/cpp/realsense
  ./cpp-realsense
  ```
  Congratulations!
  ![Alt text](https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/images/cubmos_rs.png "cubmos-realsense")
  
  You can also run skeleton tracking with a 2D webcam
  ```
  cd ~/cubemos-samples/build/cpp/webcam
  ./cpp-webcam
  ```
  ![Alt text](https://github.com/DarrenJiang13/gym-human-pose-estimation/blob/master/images/cubmos_webcam.png "cubmos-webcam")
  
  
  
  
  
  
  
  
