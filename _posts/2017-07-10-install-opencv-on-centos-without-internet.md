---
layout: post
title: Centos7无网络环境安装配置Opencv 
modified: 2017-07-18
categories: [articles, 环境配置, Opencv]
tags: 
  - 环境配置
comments: true
---

Centos无网络环境安装配置Opencv
==============================

总得来讲，opencv在安装过程失败，往往是因为缺少相应库。我们这里详细讨论并介绍opencv
安装过程中库的依赖关系。

这里主要针对
Opencv进行安装配置时，下面给出**不能正常联网时**，Opencv安装的详细过程，并提供了软件安装过程中需要的依赖包。

**系统环境（centos 7, 服务器无联网环境）：**

*Linux version 3.10.0-514.el7.x86_64 (builder\@kbuilder.dev.centos.org) (gcc
version 4.8.5 20150623 (Red Hat 4.8.5-11) (GCC) ) \#1 SMP Tue Nov 22 16:42:41
UTC 2016*

\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~

Opencv安装：
============

Opencv安装需要的库：
--------------------

-   GCC 4.4.x or later

-   CMake

-   Git

-   GTK+2.x or higher, including headers (libgtk2.0-dev)

-   pkg-config

-   Python 2.7 or later and Numpy 1.8 or later with developer packages
    (python-dev, python-numpy)

-   ffmpeg or libav development packages: libavcodec-dev, libavformat-dev,
    libswscale-dev

-   \*[optional] libtbb2 libtbb-dev

-   \*[optional] libdc1394 2.x

-   \*[optional] libjpeg-dev, libpng-dev, libtiff-dev, libjasper-dev,
    libdc1394-22-dev

### ffmpeg 安装（这里附件提供了依赖包下载的地址)

>   ffmpeg （ffmpeg.x86_640:2.6.8-3.el7.nux）安装时需要的依赖库，总共需要18个依赖库：

>   (1/18): SDL-1.2.15-14.el7.x86_64.rpm \| 204 kB (2/18):
>   fdk-aac-0.1.4-1.x86_64.rpm \| 375 kB (3/18):
>   faac-1.28-6.0.el7.nux.x86_64.rpm \| 269 kB (4/18):
>   fribidi-0.19.4-6.el7.x86_64.rpm \| 63 kB (5/18):
>   lame-libs-3.99.5-8.el7.x86_64.rpm \| 343 kB (6/18):
>   libass-0.13.4-1.el7.x86_64.rpm \| 92 kB (7/18):
>   ffmpeg-2.6.8-3.el7.nux.x86_64.rpm \| 1.5 MB (8/18):
>   libvdpau-1.1-2.el7.x86_64.rpm \| 32 kB (9/18):
>   libdc1394-2.2.2-3.el7.x86_64.rpm \| 121 kB (10/18):
>   schroedinger-1.0.11-4.el7.x86_64.rpm \| 291 kB (11/18):
>   soxr-0.1.2-1.el7.x86_64.rpm \| 77 kB (12/18):
>   libavdevice-2.6.8-3.el7.nux.x86_64.rpm \| 71 kB (13/18):
>   openal-soft-1.16.0-3.el7.x86_64.rpm \| 282 kB (14/18):
>   libva-1.2.1-3.el7.x86_64.rpm \| 68 kB (15/18):
>   x265-libs-1.9-1.el7.nux.x86_64.rpm \| 1.5 MB (16/18):
>   x264-libs-0.142-11.20141221git6a301b6.el7.nux. \| 570 kB (17/18):
>   xvidcore-1.3.2-5.el7.nux.x86_64.rpm \| 258 kB (18/18):
>   ffmpeg-libs-2.6.8-3.el7.nux.x86_64.rpm \| 5.0 MB

由于安装ffmpeg 过程中需要的依赖库较多，可以先将一台装有centos版本的服务器联网，然后用yum
或者 （apt-get）等包管理工具将依赖库进行保存。

#### 方案一：用包安装工具进行依赖库关联下载

以Centos上自带的yum为例。yum是一个优秀的软件获取与系统更新的工具，主要应用于Redhat系列的发行版本上。

yum有一个plugin叫做yum-downloadonly，它就可以为用户实现只下载软件包的功能。完成安装后，yum就多了两个命令参数，分别是：--downloadonly、--downloaddir=/path/to/dir

#### 方案二：直接修改包管工具配置文件，对各个库安装的依赖库保存到本地

也可直接修改/etc/yum.conf 将keepcache=0 修改为 keepcache=1，
安装或者升级后，在目录 /var/cache/yum 下就会有下载的 rpm 包了

Opencv编译过程中出现的问题：
============================

安装过程SIZE_MAX宏定义缺失问题：
--------------------------------

In file included from /usr/include/jasper/jasper.h:77:0,

from /home/njuzxy/Desktop/opencv/modules/highgui/src/grfmt_jpeg2000.cpp:59:

/usr/include/jasper/jas_math.h: In function ‘bool jas_safe_size_mul(size_t,
size_t, size_t\*)’:

/usr/include/jasper/jas_math.h:143:15: error: ‘SIZE_MAX’ was not declared in
this scope

if (x && y \> SIZE_MAX / x) {

\^

/usr/include/jasper/jas_math.h: In function ‘bool jas_safe_size_add(size_t,
size_t, size_t\*)’:

/usr/include/jasper/jas_math.h:170:10: error: ‘SIZE_MAX’ was not declared in
this scope

if (y \> SIZE_MAX - x) {

\^

make[2]: \*\*\*
[modules/highgui/CMakeFiles/opencv_highgui.dir/src/grfmt_jpeg2000.cpp.o] Error 1

make[1]: \*\*\* [modules/highgui/CMakeFiles/opencv_highgui.dir/all] Error 2

make: \*\*\* [all] Error 2

由于错误提示没有SIZE_MAX这个宏，通过修改__STDC_LIMIT_MACROS，并没有解决以上问题，这里我们直接对文件/usr/include/jasper/jas_math.h进行修改，在\#include
\<stdint.h\>添加SIZE_MAX宏定义：

\#if ! defined SIZE_MAX

\#define SIZE_MAX (4294967295U)

\#endif

Opencv环境配置
==============

1、添加路径
-----------

如果不添加路径到环境变量当运行测试程序时会出错如下：

./drawing: error while loading shared libraries: libopencv_calib3d.so.2.4:
cannot open shared object file: No such file or directory

这里主要讨论一般安装不考虑，root权限下安装。

针对当前用户（用户名：username）：

输入命令：vi /home/username/.bashrc

PKG_CONFIG_PATH=\$PKG_CONFIG_PATH:/where/you/install/opencv/lib/pkgconfig】

Export PKG_CONFIG_PATH

export LD_LIBRARY_PATH=\$LD_LIBRARY_PATH:/ where/you/install/opencv/lib

更新环境变量：source /etc/profile; 更新系统库缓存： ldconfig

2、难查看opencv 是否安装成功
----------------------------

输入命令：pkg-config—cflags opencv

pkg-config—libs opencv

如果没有出错，说明安装成功。

3、测试
-------

生成 可执行文件：g++demo.cpp \`pkg-config --cflags --libs opencv\` -o demo

​​生成动态链接库可输入:gcc demo.c \`pkg-config --cflags --libs opencv\` -fPIC
-shared -o libdemo.so

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#include "opencv/cv.h"  
#include "opencv/highgui.h"  
#include <iostream>
int main()  
{  
        IplImage* pImg = NULL;  
        if((pImg = cvLoadImage("/home/njuzxy/Desktop/demo.png")))  
        {  
               cvNamedWindow( "Image");//创建窗口  
               cvShowImage( "Image", pImg);//显示图像  
               cvWaitKey(); //等待按键  
               cvDestroyWindow( "Image");//销毁窗口  
               cvReleaseImage( &pImg ); //释放图像  
               std::cout<<"success"<<std::endl;
               return 0;  
       }  
        return -1;  
}  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
