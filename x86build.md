---
title: Installation for Ubuntu 16.04 LTS on x86_64 systems
keywords: Ubuntu 16.04 LTS
tags: [Ubuntu 16.04 LTS]
sidebar: openkai
permalink: x86build.html
summary: These brief instructions will help you build and run OpenKAI on Ubuntu 16.04 LTS on x86_64 systems, tested on Ubuntu Desktop 16.04.3
---
# Installation for Ubuntu 16.04LTS on x86_64 systems

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
sudo apt-get -y install libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libpng-dev libtiff-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libtheora-dev libxvidcore-dev x264 v4l-utils gstreamer1.0 gstreamer1.0-tools gstreamer1.0-plugins-ugly libturbojpeg libturbojpeg-dev libvorbis-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev
```

IO:
```shell
sudo apt-get -y install libusb-1.0-0-dev libudev-dev
```

GUI:
```shell
sudo apt-get -y install libgtk2.0-dev libqt5-dev libglew-dev
```

Python:
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

## CUDA
Download [CUDA 8.0](https://developer.nvidia.com/cuda-toolkit-archive), follow the “CUDA_Quick_Start_Guid.pdf”, install with the deb file(more easier), and test with the examples in /NVIDIA_CUDA-8.0_Samples.

If you want to change the PATH permanently,run:

```shell
chmod u+x cuda_8.0.61_375.26_linux.run
./cuda_8.0.61_375.26_linux.run
sudo echo -e "export PATH=/usr/local/cuda-8.0/bin:\$PATH\nexport LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:\$LD_LIBRARY_PATH" >> ~/.bashrc
bash
```
Attention: It should be CUDA 8.0, CUDA 9.0 will cause some error later.

## cuDNN
Download [cuDNN v7.0 for CUDA 8.0](https://developer.nvidia.com/cudnn), follow the “CUDNN Installation Guide”, install with the deb file(more easier), and test with the examples.

To install from tar, run

```shell
tar xzvf cudnn-8.0-linux-x64-v7.tgz
cd cuda
sudo cp lib64/lib* /usr/local/cuda/lib64/
sudo cp include/* /usr/local/cuda/include/
sudo ldconfig
```

## TensorRT
Download and install [TensorRT 2.1](https://developer.nvidia.com/tensorrt), follow the Quick Start Instructions, install with the deb file(more easier). 

Attention: Should be TensorRT 2.1, TensorRT 3.0 is not stable for the moment.

To install manually from tar, run

```shell
tar xf TensorRT-2.1.2.x86_64.cuda-8.0-16-04.tar.bz2
cd TensorRT-2.1.2/
sudo cp -rp lib/* /usr/local/lib/
sudo cp -rp include/* /usr/local/include/
sudo cp -rp bin/* /usr/local/bin/
sudo cp -rp targets/x86_64-linux-gnu/* /usr/libx86_64-linux-gnu/
```

## (Optional) Edit OpenCV
If you are going to use ZED camera with OpenKAI, the latest ZED driver (v2.2.0) is built with OpenCV3.1, however, the latest OpenCV git marks the version with 3.3.1, which may brings difficulties in linking OpenCV .so files. A simple work-around is to change the version definition in the OpenCV repository. Open opencv/modules/core/include/opencv2/core/version.hpp and replace the following part:

```cpp
#define CV_VERSION_MAJOR    3
#define CV_VERSION_MINOR    3
#define CV_VERSION_REVISION 1
```

with

```cpp
#define CV_VERSION_MAJOR    3
#define CV_VERSION_MINOR    1
#define CV_VERSION_REVISION 0
```

## Build OpenCV

```shell
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
cd opencv
mkdir build
cd build
ccmake ../
```
Setup the [build options p1](/images/OpenCV_ccmake_1.png), [build options p2](/images/OpenCV_ccmake_2.png) and

```shell
make all -j
sudo make install
```

## (Optional) Install ZED driver

Go to the [ZED driver site](https://www.stereolabs.com/developers/downloads/) and download the related version, and run:

```shell
chmod u+x <ZED_SDK_Linux_Ubuntu16_YOUR_VERSION.run>
./<ZED_SDK_Linux_Ubuntu16_YOUR_VERSION.run>
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
cp CMakeLists_x86_64.txt CMakeLists.txt
mkdir build
cd build
cmake ../
make -j
```

## Build OpenKAI

```shell
git clone https://github.com/yankailab/OpenKAI.git
cd OpenKAI
mkdir build
cd build
ccmake ../
```
Setup the [build options](/images/OpenKAI_ccmake.png), and

```shell
make all -j
```
