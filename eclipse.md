---
title: Develop OpenKAI apps with Eclipse CDT
keywords: Eclipse CDT
tags: [Eclipse CDT]
sidebar: openkai
permalink: eclipse.html
summary: These brief instructions will help you how to start develop OpenKAI apps with Eclipse CDT systems
---
# Develop OpenKAI apps with Eclipse CDT

{% include note.html content="Firstly complete the setup process following [this guide](/x86build.md)"%}

## Prerequisites
Assume you put all the git repositories inside the "/home/ubuntu/src" directory as in above link,run

```shell
sudo apt-get install default-jre
sudo su -c "echo -e '/usr/local/cuda/lib64\n/home/ubuntu/src/jetson-inference/build/x86_64/lib' >> /etc/ld.so.conf"
sudo ldconfig
```

## Eclipse install
Download, extract and launch the latest [Eclipse IDE for C/C++ Developers (CDT)](http://www.eclipse.org/downloads/packages/), 

```shell
tar xzvf eclipse-cpp-neon-2-linux-gtk-x86_64.tar.gz
cd eclipse
sudo ./eclipse
```

Following the instruction to setup the worksapce in the first launch of Eclipse. Here we suppose to make and set "/home/ubuntu/src/workspace" directory as the workspace.

## Eclipse project
Make a new C++ Project in Eclipse

<div style="text-align:center">
{% include image.html file="eclipse_newProj1.png" alt="Eclipse" caption="Eclipse new C++ Project" %}
</div>

Here we assume to use the project name "OpenKAI", check "Use default location" or set the Location as "/home/ubuntu/src/workspace", choose "Empty Project", Toolchains "Linux GCC", and click Next>
<div style="text-align:center">
{% include image.html file="eclipse_newProj2.png" alt="Eclipse" caption="Choose Empty Project" %}
</div>

Make sure both "Debug" and "Release" are checked, and click "Finish"
<div style="text-align:center">
{% include image.html file="eclipse_newProj3.png" alt="Eclipse" caption="Debug and Release are checked" %}
</div>

## Git clone into eclipse workspace
Open Terminal and run
```shell
cd ~/src/workspace/OpenKAI
git clone https://github.com/yankailab/OpenKAI.git
```

## Eclipse project settings
Back to Eclipse CDT, in the left pane click "OpenKAI" and press F5 key, now you should see there is an "OpenKAI" directory in the project.

<div style="text-align:center">
{% include image.html file="eclipse_OpenKAI.png" alt="Eclipse OpenKAI" caption="OpenKAI directory in the project" %}
</div>

Right click project "OpenKAI" and click "Properties", click "C/C++ Build", and choose the Configuration to be "[All configurations]". Setup each page as following:

<div style="text-align:center">
{% include image.html file="eclipse_cxxBuild.png" alt="Eclipse C++ Builder Settings" caption="C/C++ Builder Settings" %}
</div>

<div style="text-align:center">
{% include image.html file="eclipse_cxxBuildBehavior.png" alt="Eclipse C++ Build Behavior" caption="Eclipse C++ Build Behavior" %}
</div>

<div style="text-align:center">
{% include image.html file="eclipse_cxxBuildArtifact.png" alt="Eclipse C++ Build Behavior" caption="Eclipse C++ Build Artifact" %}
</div>

<div style="text-align:center">
{% include image.html file="eclipse_cxxDialect.png" alt="Eclipse C++ Dialect" caption="Eclipse C++ Dialect" %}
</div>

<div style="text-align:center">
{% include image.html file="eclipse_cxxIncludes.png" alt="Eclipse C++ Includes" caption="Eclipse C++ Includes" %}
</div>

<div style="text-align:center">
{% include image.html file="eclipse_cxxLibraries.png" alt="Eclipse C++ Libraries" caption="Eclipse C++ Libraries" %}
</div>


### Note:
If you experienced non-reponding or freezing in Eclipse on Ubuntu 16.04 etc., try to force Eclipse using GTK2 instead of GTK3. Open "/home/ubuntu/src/eclipse/eclipse.ini" and add

```ini
--launcher.GTK_version
2
```

just above the "--launcher.appendVmargs" line, so the "eclipse.ini" should somehow look like this:

```ini
-startup
plugins/org.eclipse.equinox.launcher_1.3.201.v20161025-1711.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.gtk.linux.x86_64_1.1.401.v20161122-1740
-product
org.eclipse.epp.package.cpp.product
--launcher.defaultAction
openFile
-showsplash
org.eclipse.platform
--launcher.defaultAction
openFile
--launcher.GTK_version
2
--launcher.appendVmargs
-vmargs
-Dosgi.requiredJavaVersion=1.8
-XX:+UseG1GC
-XX:+UseStringDeduplication
-Dosgi.requiredJavaVersion=1.8
-Xms256m
-Xmx1024m
```

## Launch OpenKAI from Eclipse
Open the run configuration setting by

<div style="text-align:center">
{% include image.html file="eclipse_runConfig.png" alt="Eclipse run configuration" caption="Eclipse run configuration" %}
</div>

Click "New launch configuration" icon in the left pane, put a new name and setting up like following:
<div style="text-align:center">
{% include image.html file="eclipse_runConfig2.png" alt="Eclipse run configuration" caption="Eclipse run configuration 2" %}
</div>

<div style="text-align:center">
{% include image.html file="eclipse_runConfig3.png" alt="Eclipse run configuration" caption="Eclipse run configuration 3" %}
</div>

Click "Apply", "Run" and you are done.

Enjoy.


