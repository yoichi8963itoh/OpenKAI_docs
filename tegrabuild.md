---
title: Build & Run on NVIDIA Jetson TX1/TX2
keywords: NVIDIA Jetson TX1 TX2
tags: [Jetson_TX1/TX2]
sidebar: openkai
permalink: tegrabuild.html
summary: These brief instructions will help you build and run OpenKAI on NVIDIA Jetson TX1/TX2
---
# Build & Run on NVIDIA Jetson TX1/TX2 (Ubuntu 16.04LTS / JetPack3.1)

## JetPack install & flash
Download the latest [JetPack](https://developer.nvidia.com/embedded/jetpack) and run the installer, choose the following options to be installed and flashed into your Jetson TX1/TX2:

```
Package                                                Action
>── Target - Jetson TX1/TX2
    >── Linux for Tegra Host Side Image Setup..........no action
    │   >── ...
    >── Flash OS Image to Target.......................install
    >── Install on Target
        >── VisionWorks Pack...........................no action
        │   >── ...
        >── CUDA Toolkit...............................install
        >── Compile CUDA Samples.......................no action
        >── TensorRT...................................install
        >── OpenCV for Tegra...........................install
        >── Multimedia API package.....................install
        >── cuDNN Package..............................install
```

<div style="text-align:center">
{% include image.html file="JetPackInstall.png" alt="JetPack install option" caption="JetPack install option" %}
</div>

## Change the performance setting
```shell
sudo rm /etc/rc.local
set +H
sudo sh -c "echo '#!/bin/sh\n/home/ubuntu/jetson_clocks.sh\nnvpmodel -m 0\nexit 0\n' >> /etc/rc.local"
set -H
sudo chmod a+x /etc/rc.local
sudo chmod a+x /home/ubuntu/jetson_clocks.sh
```

## Prerequisites

Base:
```shell
sudo apt-get update
sudo apt-get -y install build-essential cmake cmake-curses-gui git libboost-all-dev libgflags-dev libgoogle-glog-dev uuid-dev libboost-filesystem-dev libboost-system-dev libboost-thread-dev ncurses-dev
```

Blas:
```shell
sudo apt-get -y install libatlas-base-dev libopenblas-base libopenblas-dev liblapack-dev liblapack3
```

Codecs:
```shell
sudo apt-get -y install libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libpng-dev libtiff-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libtheora-dev libxvidcore-dev x264 v4l-utils gstreamer1.0 gstreamer1.0-tools gstreamer1.0-plugins-ugly libturbojpeg libvorbis-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev
```

IO:
```shell
sudo apt-get -y install libusb-1.0-0-dev libudev-dev
```

GUI:
```shell
sudo apt-get -y install libgtk2.0-dev libglew-dev
```

Python (Optional):
```shell
sudo apt-get -y install python-protobuf python-scipy python-pip python-dev python-numpy libboost-python-dev python-all-dev python-h5py python-matplotlib python-numpy python-pil python-pip python-pydot python-scipy python-skimage python-sklearn
```

Optional:
```shell
sudo apt-get -y install libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev liblmdb-dev protobuf-compiler unzip libtbb-dev libtbb2 pkg-config gfortran
```

## Update Eigen

```shell
wget --no-check-certificate http://bitbucket.org/eigen/eigen/get/3.3.4.tar.gz
tar xzvf 3.3.4.tar.gz
cd eigen
mkdir build
cd build
cmake ../
sudo make install
```

## Install OpenCV (3.1.0 or latest version)

```shell
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
cd ~/opencv
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules ..
make -j3
sudo make install
```

## Pangolin (Needed by ORB_SLAM2)

```shell
git clone https://github.com/yankailab/Pangolin.git
cd Pangolin
mkdir build
cd build
cmake ..
cmake --build .
sudo make install
```

## ORB_SLAM2

GPU version:
```shell
git clone https://github.com/yankailab/orb_slam2_gpu.git
cd orb_slam2_gpu
chmod +x build.sh
./build.sh
```

CPU version:
```shell
git clone https://github.com/yankailab/orb_slam2.git
cd orb_slam2
chmod +x build.sh
./build.sh
```

## jetson-inference-batch

```shell
git clone https://github.com/yankailab/jetson-inference-batch.git
cd jetson-inference-batch/
cp CMakeLists_tegra.txt CMakeLists.txt
mkdir build
cd build
cmake ../
make
```

## (Optional) Install ZED driver

For Jetson TX1:
```shell
wget --no-check-certificate https://www.stereolabs.com/developers/downloads/archives/ZED_SDK_Linux_JTX1_v2.2.1.run
chmod u+x ZED_SDK_Linux_JTX1_v2.2.1.run
./ZED_SDK_Linux_JTX1_v2.2.1.run
```

For Jetson TX2:
```shell
wget --no-check-certificate https://www.stereolabs.com/developers/downloads/archives/ZED_SDK_Linux_JTX2_v2.2.1.run
chmod u+x ZED_SDK_Linux_JTX2_v2.2.1.run
./ZED_SDK_Linux_JTX2_v2.2.1.run
```

## Build OpenKAI

```shell
git clone https://github.com/yankailab/OpenKAI.git
cd OpenKAI
mkdir build
cd build
ccmake ../
make all -j3
```

## Run samples

```shell
cd OpenKAI/build
sudo ./OpenKAI ~/OpenKAI/kiss/apCopter.kiss
```

"apmCopter.kiss" part is the configuration script for each OpenKAI application, replace it to your own kiss file.


