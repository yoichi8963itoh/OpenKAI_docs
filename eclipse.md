---
title: Develop OpenKAI apps with Eclipse CDT
keywords: Eclipse CDT
tags: [Eclipse CDT]
sidebar: openkai
permalink: eclipse.html
summary: These brief instructions will help you how to start develop OpenKAI apps with Eclipse CDT systems
---
# Develop OpenKAI apps with Eclipse CDT

{% include note.html content="Firstly complete the setup process following [this guide](https://github.com/yankailab/OpenKAI/blob/master/doc/x86_64/Ubuntu/build.md)"%}

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
<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_newProj1.png" alt="Eclipse ">
</p>

Here we assume to use the project name "OpenKAI", check "Use default location" or set the Location as "/home/ubuntu/src/workspace", choose "Empty Project", Toolchains "Linux GCC", and click Next>
<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_newProj2.png" alt="Eclipse ">
</p>

Make sure both "Debug" and "Release" are checked, and click "Finish"
<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_newProj3.png" alt="Eclipse ">
</p>

## Git clone into eclipse workspace
Open Terminal and run
```shell
cd ~/src/workspace/OpenKAI
git clone https://github.com/yankailab/OpenKAI.git
```

## Eclipse project settings
Back to Eclipse CDT, in the left pane click "OpenKAI" and press F5 key, now you should see there is an "OpenKAI" directory in the project.
<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_OpenKAI.png" alt="Eclipse OpenKAI">
</p>

Right click project "OpenKAI" and click "Properties", click "C/C++ Build", and choose the Configuration to be "[All configurations]". Setup each page as following:

<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_cxxBuild.png" alt="Eclipse c++ Build">
</p>

<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_cxxBuildBehavior.png" alt="Eclipse c++ Build Behavior">
</p>

<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_cxxBuildArtifact.png" alt="Eclipse c++ Build Artifact">
</p>

<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_cxxDialect.png" alt="Eclipse c++ Dialect">
</p>

<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_cxxIncludes.png" alt="Eclipse c++ Includes">
</p>

<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_cxxLibraries.png" alt="Eclipse c++ Libraries">
</p>

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
<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_runConfig.png" alt="Eclipse run configuration">
</p>

Click "New launch configuration" icon in the left pane, put a new name and setting up like following:
<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_runConfig2.png" alt="Eclipse run configuration">
</p>

<p align="center">
<img src="https://github.com/yankailab/OpenKAI/raw/master/doc/x86_64/Ubuntu/img/eclipse_runConfig3.png" alt="Eclipse run configuration">
</p>

Click "Apply", "Run" and you are done.

Enjoy.

## Just a test
Test CPP css

```cpp
#include "Startup.h"

Startup* g_pStartup;
void onMouseGeneral(int event, int x, int y, int flags, void* userdata)
{
	g_pStartup->handleMouse(event, x, y, flags);
}

void signalHandler(int signal)
{
	if (signal != SIGINT)return;
	printf("\nSIGINT: Complete\n");
	g_pStartup->m_bRun = false;
}

namespace kai
{

Startup::Startup()
{
	for (int i = 0; i < N_INST; i++)
	{
		m_pInst[i] = NULL;
		m_pMouse[i] = NULL;
	}
	m_nInst = 0;
	m_nMouse = 0;

	m_name = "";
	m_bWindow = true;
	m_waitKey = 50;
	m_bRun = true;
	m_key = 0;
	m_bLog = true;
	m_winMouse = "";
}

Startup::~Startup()
{
}

string* Startup::getName(void)
{
	return &m_name;
}

bool Startup::start(Kiss* pKiss)
{
	g_pStartup = this;
	signal(SIGINT, signalHandler);

	int i;
	NULL_F(pKiss);
	Kiss* pApp = pKiss->root()->o("APP");
	if (pApp->empty())
		return false;

	F_INFO(pApp->v("appName", &m_name));
	F_INFO(pApp->v("bWindow", &m_bWindow));
	F_INFO(pApp->v("waitKey", &m_waitKey));
	F_INFO(pApp->v("winMouse", &m_winMouse));

	//create instances
	F_FATAL_F(createAllInst(pKiss));

	//link instances with each other
	for (i = 0; i < m_nInst; i++)
	{
		F_FATAL_F(m_pInst[i]->link());
	}

	if(m_winMouse != "" && m_bWindow)
	{
		setMouseCallback(m_winMouse, onMouseGeneral, 0 );
	}

	//UI thread
	m_bRun = true;

	if (m_bWindow)
	{
		while (m_bRun)
		{
			draw();
			m_key = waitKey(m_waitKey);
			handleKey(m_key);
		}
	}
	else
	{
		while (m_bRun)
		{
			draw();
		}
	}

	for (i = 0; i < m_nInst; i++)
	{
		m_pInst[i]->complete();
	}

	for (i = 0; i < m_nInst; i++)
		DEL(m_pInst[i]);

	return 0;
}
```


