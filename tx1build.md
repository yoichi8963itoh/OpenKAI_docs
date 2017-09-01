---
title: Build & Run on NVIDIA Jetson TX1/TX2
keywords: NVIDIA Jetson TX1 TX2
tags: [Jetson_TX1/TX2]
sidebar: openkai
permalink: tx1build.html
summary: These brief instructions will help you build and run OpenKAI on NVIDIA Jetson TX1
---
# Build & Run on NVIDIA Jetson TX1/TX2 (Ubuntu 16.04LTS / JetPack3.0)

## JetPack install & flash
Download the latest [JetPack](https://developer.nvidia.com/embedded/jetpack) and run the installer, choose the following options to be installed and flashed into your Jetson TX1/TX2:

<div style="text-align:center">
{% include image.html file="JetPackInstall.png" alt="JetPack install option" caption="JetPack install option" %}
</div>

## Prerequisites

```shell
sudo apt-get update
sudo apt-get -y install git cmake build-essential cmake-curses-gui libatlas-base-dev libprotobuf-dev libleveldb-dev libsnappy-dev libboost-all-dev libhdf5-serial-dev libgflags-dev libgoogle-glog-dev liblmdb-dev protobuf-compiler libgtk2.0-dev pkg-config libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libtbb-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 v4l-utils unzip python-protobuf libeigen3-dev
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

## (Optional) Install ZED driver

For Jetson TX1:
```shell
wget --no-check-certificate https://www.stereolabs.com/developers/downloads/ZED_SDK_Linux_JTX1_v2.1.2.run
chmod u+x ZED_SDK_Linux_JTX1_v2.1.2.run
./ZED_SDK_Linux_JTX1_v2.1.2.run
```

For Jetson TX2:
```shell
wget --no-check-certificate https://www.stereolabs.com/developers/downloads/ZED_SDK_Linux_JTX2_v2.1.2.run
chmod u+x ZED_SDK_Linux_JTX2_v2.1.2.run
./ZED_SDK_Linux_JTX2_v2.1.2.run
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

## Jetson-inference

```shell
git clone https://github.com/dusty-nv/jetson-inference.git
cd jetson-inference/
mkdir build
cd build
cmake ../
make
```

## Build OpenKAI

```shell
git clone https://github.com/yankailab/OpenKAI.git
cd OpenKAI
mkdir build
cd build
ccmake ../
make all -j4
```

## Run samples

```shell
cd OpenKAI/build
sudo ./OpenKAI ~/OpenKAI/kiss/apCopter.kiss
```

"apmCopter.kiss" part is the configuration script for each OpenKAI application, replace it to your own kiss file.


