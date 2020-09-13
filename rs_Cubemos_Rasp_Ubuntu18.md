# Implement librealsense+Cubemos on Raspberry Pi 4B with Ubuntu 18.04 Desktop
This is a tutorial about how to implement RealSense SDK 2.0, Cubemos on your Raspberry Pi 4B with Ubuntu 18.04 Desktop.
| Required Info               |                      |
| --------------------------- | -------------------- |
| Camera Model                | D435               |
| Operating System & Version  | Linux (Ubuntu 18.04) |
| Platform                    | Raspberry Pi 4B      |
| SDK Version                 | v2.24.0              |

Intel® RealSense™ SDK 2.0[[github]](https://github.com/IntelRealSense/librealsense)  
Cubemos [[sdk download]](https://www.intelrealsense.com/skeleton-tracking/) 
Ubuntu 18.04 for Raspberry Pi [img download](https://ubuntu.com/download/raspberry-pi)  


## 1. Install Ubuntu18.04 on a Raspberry Pi 4B
  [installation guide](https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04.5&architecture=arm64+raspi4)  
  In this project, I installed a **Ubuntu18.04 (64-bit)** image with desktop.
  
  Run `sudo apt install ubuntu-desktop` to get a traditional gnome desktop.
  
## 2. Install realsense SDK [(Linux Ubuntu Installation)](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md)
- 2.0 Make Ubuntu Up-to-date
```
sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
```
- 2.1 Install cmake
```
sudo apt-get install cmake
```
- 2.2 Download librealsense sdk from github
```
git clone https://github.com/IntelRealSense/librealsense.git
```
- 2.3 Prepare Linux Backend and the Dev. Environment
for ubuntu 18.04
```
# Install the core packages required to build librealsense binaries and the affected kernel modules:
sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev
# Distribution-specific packages for ubuntu 18.04
sudo apt-get install libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev
```
- 2.4 Run Intel Realsense permissions script, build and apply patched kernel modules:
```
cd librealsense
./scripts/setup_udev_rules.sh
./scripts/patch-realsense-ubuntu-lts.sh
```

- 2.5 Building librealsense2 SDK
```
cd librealsense
mkdir build && cd build
cmake ../
sudo make uninstall && make clean && make && sudo make install
```
