---
title: Getting started with OpenKAI
keywords: sample homepage
tags: [getting_started]
sidebar: openkai
permalink: index.html
summary: These brief instructions will help you get started quickly with the theme. The other topics in this help provide additional information and detail about working with other aspects of this theme and Jekyll.
---
# OpenKAI

## What is it?
OpenKAI (Kinetic AI) is a framework that combines AI and robot controllers. OpenKAI is designed to be highly customizable for versatile applications, yet light weight to run on embedded hardwares, and a simple code architecture that is easy for expansion and maintenance.

## Platform
OpenKAI is supposed to behave as a companion computer that commands an external low-level robotic controller. Supported hardware for the companiton computer currently are
* x86 PC & Ubuntu (14.04, 16.04)
* NVIDIA JetsonTX1 & Ubuntu (JetPack 2.3.1)

## External controller
* Pixhawk (via Mavlink/UART)
* Controllers with CAN bus I/F (via UART/USB<->CAN bus converter)

## Sample apps
Visual obstacle avoidance using ZED camera and Pixhawk (APMcopter 3.5-dev and above).

On-board camera:
<iframe width="640" height="360" src="https://www.youtube.com/embed/MOFullt5k3g"> </iframe>

External camera:
<iframe width="640" height="360" src="https://www.youtube.com/embed/qk_hEtRASqg"> </iframe>


## Build & Run
[Build on NVIDIA Jetson TX1](/tx1build.md)

[Build on Ubuntu desktop systems](/x86build.md)

## Application implementation
There are two ways of implement an application based on OpenKAI: compilation vs. script.

### Compilation
OpenKAI is implemented purely in C++, it is very easy to implement your application as C/C++ and compile with OpenKAI together. Refer to "OpenKAI/src/Application/Startup.cpp" as a starting point for application integration, and this guide:

[Develop OpenKAI apps with Eclipse CDT](/eclipse.md)

### Kiss (OpenKai Simple Script)
If the existing function Modules from OpenKAI already meet your needs, you can write a json-like Kiss script to define your application. Note that Kiss is treated as a config file and will be parsed all at once at the beginning of OpenKAI program execution. The Modules defined by Kiss file are statically created and get started to run. Therefore there is no difference in execution speed between the Compilation way and application defined by Kiss (To be updated).

## System architecture
OpenKAI is organized as a combination of multiple functional Modules. Each Module runs in an individual thread, so that modules can be ran simultaneously over multi-core CPUs efficiently. Each Module contains multiple sub-Modules, each sub-Module may handle certain part of functions divided by its category, and the Module switches between each of its sub-Modules to form a complete function.

<div style="text-align:center">
{% include image.html file="F1.png" alt="OpenKAI Modules" caption="OpenKAI Modules" %}
</div>

At source code level, each Module and sub-Module is implemented by an individual C++ class. A Module class is inherited from kai::ThreadBase class that handles thread interface and timing control (desired frames per second) etc.

A typical example of Modular designed OpenKAI system for an automatic visual guided landing system is shown below.
<div style="text-align:center">
{% include image.html file="F2.png" alt="OpenKAI autotmatic visual guided landing system diagram" caption="OpenKAI autotmatic visual guided landing system diagram" %}
</div>
Refer [here](http://ardupilot.org/dev/docs/companion-computer-nvidia-tx1.html) for hardware connections of the above example, and [here](https://youtu.be/5AVb2hA2EUs) is a video showing the example in action.

The overall system implemented by OpenKAI is a single-process multi-threads program. This architecture aims to provide an easy expansion similar to ROS, meanwhile keeping the program to be simple and provide an efficient interface between Modules by eliminating the socket communication in ROS. A reference of currently implemented Modules and sub-Modules will be provided soon.

