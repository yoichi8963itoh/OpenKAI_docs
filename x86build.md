---
title: Installation for Ubuntu 16.04 LTS on x86_64 systems
keywords: Ubuntu 16.04 LTS
tags: [Ubuntu 16.04 LTS]
sidebar: openkai
permalink: x86build.html
summary: These brief instructions will help you build and run OpenKAI on Ubuntu 16.04 LTS on x86_64 systems, tested on Ubuntu Desktop 16.04.2
---
# Installation for Ubuntu 16.04LTS on x86_64 systems

## Prerequisites

```shell
sudo apt-get update
sudo apt-get -y install git cmake build-essential cmake-curses-gui libatlas-base-dev libprotobuf-dev libleveldb-dev libsnappy-dev libboost-all-dev libhdf5-serial-dev libgflags-dev libgoogle-glog-dev liblmdb-dev protobuf-compiler libgtk2.0-dev pkg-config python-dev python-numpy libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libtbb-dev libqt4-dev libtheora-dev libxvidcore-dev x264 v4l-utils unzip python-protobuf python-scipy python-pip libeigen3-dev uuid-dev libusb-1.0-0-dev libudev-dev
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
Download the latest [CUDA](https://developer.nvidia.com/cuda-downloads) and run

```shell
chmod u+x cuda_8.0.61_375.26_linux.run
./cuda_8.0.61_375.26_linux.run
sudo echo -e "export PATH=/usr/local/cuda/bin:\$PATH\nexport LD_LIBRARY_PATH=/usr/local/cuda/lib64:\$LD_LIBRARY_PATH" >> ~/.bashrc
bash
```

## cuDNN
Download the latest [cuDNN](https://developer.nvidia.com/cudnn) and run

```shell
tar xzvf cudnn-8.0-linux-x64-v5.1.tgz
cd cuda
sudo cp lib64/lib* /usr/local/cuda/lib64/
sudo cp include/* /usr/local/cuda/include/
sudo ldconfig
```

## TensorRT
Download and install [TensorRT](https://developer.nvidia.com/tensorrt). If you came across the "Broken package" error, extract the files directly from TensorRT's deb file and copy all the include files and lib files into the correspondent system directories. Optionally, download the [archive]() of the exctracted file from the original .deb package, and run

```shell
tar xzvf nv-gie_1.0.0-1+cuda8.0_amd64.tar.gz
cd nv-gie_1.0.0-1+cuda8.0_amd64
sudo cp -rp libgie1/usr/* /usr/
sudo cp -rp libgie-dev/usr/* /usr/
```

## (Optional) Edit OpenCV
If you are going to use ZED camera with OpenKAI, the latest ZED driver (v1.2) is built with OpenCV3.1, however, the latest OpenCV git marks the version with 3.2, which may brings difficulties in linking OpenCV .so files. A simple work-around is to change the version definition in the OpenCV repository. Open

opencv/modules/core/include/opencv2/core/version.hpp

and replace the following part

```cpp
#define CV_VERSION_MAJOR    3
#define CV_VERSION_MINOR    2
#define CV_VERSION_REVISION 0
```

with

```cpp
#define CV_VERSION_MAJOR    3
#define CV_VERSION_MINOR    1
#define CV_VERSION_REVISION 0
```

## Build OpenCV

```shell
git clone https://github.com/Itseez/opencv.git
git clone https://github.com/Itseez/opencv_contrib.git
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

```shell
wget --no-check-certificate https://www.stereolabs.com/developers/downloads/ZED_SDK_Linux_Ubuntu16_v2.1.2.run
chmod u+x ZED_SDK_Linux_Ubuntu16_v2.1.2.run
./ZED_SDK_Linux_Ubuntu16_v2.1.2.run
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

## jetson-inference
```shell
git clone https://github.com/dusty-nv/jetson-inference.git
```

Open the CMakeLists.txt in jetson-inference directory and replace the CUDA compute ability version in the following part with your own [CUDA device'](https://en.wikipedia.org/wiki/CUDA#GPUs_supported)

```cmake
set(
	CUDA_NVCC_FLAGS
	${CUDA_NVCC_FLAGS}; 
    -O3 -gencode arch=compute_53,code=sm_53
)
```

Next, replace the following part

```shell
include_directories(/usr/include/gstreamer-1.0 /usr/lib/aarch64-linux-gnu/gstreamer-1.0/include /usr/include/glib-2.0 /usr/include/libxml2 /usr/lib/aarch64-linux-gnu/glib-2.0/include/)
```

with

```shell
include_directories(/usr/include/gstreamer-1.0 /usr/lib/x86_64-linux-gnu/gstreamer-1.0/include /usr/include/glib-2.0 /usr/include/libxml2 /usr/lib/x86_64-linux-gnu/glib-2.0/include/)
```

then

```shell
cd jetson-inference/
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
