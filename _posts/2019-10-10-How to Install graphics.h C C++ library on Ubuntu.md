---
layout: post
title: "How to Install graphics.h C/C++ library on Ubuntu"
tags:  [Graphics, C++, Ubuntu, Linux, HowTo]
date: 2019-10-10
---

In this post we are going to install `graphics.h``D3D3D3`library on Ubuntu and to use it with `gcc` or `g++` compiler. `graphics.h` `D3D3D3` provides access to a simple graphics library that makes it possible to draw lines, rectangles, ovals, arcs, polygons, images, and strings on a graphical window. To know about functions in `graphic.h` `D3D3D3` library in C/C++ visit [here](https://web.stanford.edu/class/archive/cs/cs106b/cs106b.1126/materials/cppdoc/graphics.html) `##4400ff`.

To use `graphics.h``D3D3D3`on Ubuntu platform we need to compile and install `libgraph``D3D3D3`. `[libgraph](https://savannah.nongnu.org/projects/libgraph/)` `##4400ff`is an implementation of the TurboC graphics API (`graphics.h` `D3D3D3`) on GNU/Linux using SDL.

1. First we need to add the *Universe* repository because some packages are not available in main repository.
  ```
  sudo add-apt-repository universe
  sudo apt-get update
  ``` `D3D3D3`

2. Now we will install build-essential and some additional packages.
   **For versions prior to 18.4**
    ```
    sudo apt-get install libsdl-image1.2 libsdl-image1.2-dev guile-1.8 \
    guile-1.8-dev libsdl1.2debian libart-2.0-dev libaudiofile-dev \
    libesd0-dev libdirectfb-dev libdirectfb-extra libfreetype6-dev \
    libxext-dev x11proto-xext-dev libfreetype6 libaa1 libaa1-dev \
    libslang2-dev libasound2 libasound2-dev build-essential
    ``` `D3D3D3`

   **For 18.04** we need to add repositories of `xenial` `D3D3D3` in `sources.list` `D3D3D3`.
    ```
    sudo nano /etc/apt/sources.list
    ``` `D3D3D3`
   
   Now add these lines
    ```
    deb http://us.archive.ubuntu.com/ubuntu/ xenial main universe
    deb-src http://us.archive.ubuntu.com/ubuntu/ xenial main universe
    ``` `D3D3D3`
  
  Run `sudo apt-get update` and then install packages using 
    ```
    sudo apt-get install libsdl-image1.2 libsdl-image1.2-dev guile-2.0 \
    guile-2.0-dev libsdl1.2debian libart-2.0-dev libaudiofile-dev \
    libesd0-dev libdirectfb-dev libdirectfb-extra libfreetype6-dev \
    libxext-dev x11proto-xext-dev libfreetype6 libaa1 libaa1-dev \
    libslang2-dev libasound2 libasound2-dev
    ```
    
3. Download `libgraph` from [here](download.savannah.gnu.org/releases/libgraph/libgraph-1.0.2.tar.gz) and extract `libgraph-1.0.2.tar.gz` file.    

4. Go to the extracted folder and run following commands.
  ```
  ./configure
  make
  sudo make install
  sudo cp /usr/local/lib/libgraph.* /usr/lib
  ```
  
Now we can use `graphics.h` library after adding following lines to the code.
  ```
  int gd = DETECT, gm; 
  initgraph(&gd, &gm, NULL);
  ```
  
Here is a program of [Simple Pendulum Animation on Ubuntu](https://programmercave0.github.io//blog/2019/10/09/C++-Simple-Pendulum-Animation-on-Ubuntu-Machine).  
    
