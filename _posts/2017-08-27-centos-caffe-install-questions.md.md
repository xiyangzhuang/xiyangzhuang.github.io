---
layout: post
title: 在Centos安装Caffe时遇到的错误
modified: 2017-08-27
categories: [articles, configuration]
tags: 
  - study
comments: true
---

1 引言 

2 常用OpenCV库与机器学习平台Caffe 

>   2.1 OpenCV在图像处理中的使用介绍 

>   2.2 机器学习平台：Caffe 

3 软件安装及错误解决方案 

>   3.1 Centos7无网络环境安装配置Opencv 

>   3.2 Centos7无网络环境安装配置Caffe 

>   3.3 软件安装中遇到的问题 

>   3.3.1 错误 libprotobuf.so.14缺失问题 

>   3.3.2 missing: Atlas_CLAPACK_INCLUDE_DIR Atlas_CBLAS_LIBRARY
>   Atlas_BLAS_LIBRARY Atlas_LAPACK_LIBRARY 

>   3.3.3 libraries: libprotobuf.so.8 缺失问题: 

>   3.3.4 仅编译matcaffe 编译问题（编译参数设置） 

>   3.3.5 Invalid MEX-file '/matlab/+caffe/private/caffe_.mexa64':
>   libopenblas.so.0: cannot open shared object file: No such file or directory

>   3.3.6 MATLAB/R2014a/bin/glnxa64/../../sys/os/glnxa64/libstdc++.so.6: version
>   \`GLIBCXX_3.4.18' not found (required by /usr/local/lib/libprotobuf.so.14)

>   3.3.7 INSTALL NCCL，注意gencode architecture,不然编译会出现问题 

>   3.3.8 NCCL与MATLAB识别问题，如果未识别 


常用OpenCV库与机器学习平台Caffe
===============================

OpenCV在图像处理中的使用介绍
----------------------------

OpenCV是一个基于BSD许可（开源）发行的跨平台计算机视觉库，可以运行在Linux、Windows、Android和Mac
OS操作系统上。它轻量级而且高效——由一系列 C 函数和少量 C++
类构成，同时提供了Python、Ruby、MATLAB等语言的接口，实现了图像处理和计算机视觉方面的很多通用算法。

OpenCV用C++语言编写，它的主要接口也是C++语言，但是依然保留了大量的C语言接口。该库也有大量的Python,
Java and MATLAB/OCTAVE
(版本2.5)的接口。这些语言的API接口函数可以通过在线文档获得。如今也提供对于C\#,Ch,
Ruby的支持。所有新的开发和算法都是用C++接口。一个使用CUDA的GPU接口也于2010年9月开始实现。

机器学习平台：Caffe
-------------------

Caffe的全称应该是Convolutional Architecture for Fast Feature
Embedding，它是一个清晰、高效的深度学习框架，它是开源的，核心语言是C++，它支持命令行、Python和Matlab接口，它既可以在CPU上运行也可以在GPU上运行。它的license是BSD
2-Clause。

软件安装及错误解决方案
======================

这里主要是针对软件安装过程中，公司内部网络通过网上搜索的等形式快速下载相应的软件包，因此这里给出了相应软件包，依赖包的下载使用方法。

由于linux的包依赖关系较为复杂，因些这里给出了几种依赖包的关联下载使用方法。

方案一：用包安装工具进行依赖库关联下载。

以Centos上自带的yum为例。yum是一个优秀的软件获取与系统更新的工具，主要应用于Redhat系列的发行版本上。

yum有一个plugin叫做yum-downloadonly，它就可以为用户实现只下载软件包的功能。完成安装后，yum就多了两个命令参数，分别是：--downloadonly、--downloaddir=/path/to/dir

方案二：直接修改包管工具配置文件，对各个库安装的依赖库保存到本地。

也可直接修改/etc/yum.conf 将keepcache=0 修改为 keepcache=1，
安装或者升级后，在目录 /var/cache/yum 下就会有下载的 rpm 包了


<!-- more -->


Centos7无网络环境安装配置Opencv
-------------------------------

总得来讲，opencv在安装过程失败，往往是因为缺少相应库。我们这里详细讨论并介绍opencv
安装过程中库的依赖关系。

这里主要针对
Opencv进行安装配置时，下面给出不能正常联网时，Opencv安装的详细过程，并提供了软件安装过程中需要的依赖包。注意这里只给出该系统版软件包有依赖安装的情况进行说明，对于不需要依赖安装的软件不做说明，直接下载对应源进行安装即可（地址
密码:uy8a）。

系统环境（centos 7, 服务器无联网环境，**注此服务器版本为工作服务器版本信息**）：

Linux version 3.10.0-514.el7.x86_64 (builder\@kbuilder.dev.centos.org) (gcc
version 4.8.5 20150623 (Red Hat 4.8.5-11) (GCC) ) \#1 SMP Tue Nov 22 16:42:41
UTC 2016

Opencv安装：

Opencv安装需要的库：

• GCC 4.4.x or later

• CMake

• Git

• GTK+2.x or higher, including headers (libgtk2.0-dev)

• pkg-config

• Python 2.7 or later and Numpy 1.8 or later with developer packages
(python-dev, python-numpy)

• ffmpeg or libav development packages: libavcodec-dev, libavformat-dev,
libswscale-dev

• \*[optional] libtbb2 libtbb-dev

• \*[optional] libdc1394 2.x

• \*[optional] libjpeg-dev, libpng-dev, libtiff-dev, libjasper-dev,
libdc1394-22-dev

Gcc安装（下载 地址 密码:uy8a）

\> 该Centos 7版本上gcc缺少的依赖库只有一个：

(1/2): libstdc++-devel-4.8.5-11.el7.x86_64.rpm \| 1.5 MB

(2/2): gcc-c++-4.8.5-11.el7.x86_64.rpm \| 7.2 MB

Lib-gtk（下载 地址 密码:uy8a）

\> 该Centos 7版本上Lib-gtk依赖库有13个：

(1/13): glib x86_64 1:1.2.10-41.el7 137 k

(2/13): glib-devel x86_64 1:1.2.10-41.el7 40 k

(3/13): gtk+ x86_64 1:1.2.10-77.el7 841 k

(4/13): libICE-devel x86_64 1.0.9-2.el7 49 k

(5/13): libSM-devel x86_64 1.2.2-2.el7 13 k

(6/13):libX11-devel x86_64 1.6.3-3.el7 980 k

(7/13):libXau-devel x86_64 1.0.8-2.1.el7 14 k

(8/13):libXext-devel x86_64 1.3.3-3.el7 75 k

(9/13): libXfixes-devel x86_64 5.0.1-2.1.el7 13 k

(10/13): libXi-devel x86_64 1.7.4-2.el7 105 k

(11/13): libXt-devel x86_64 1.1.4-6.1.el7 445 k

(12/13):libxcb-devel x86_64 1.11-4.el7 1.1 M

(13/13): xorg-x11-proto-devel noarch 7.7-13.el7 281 k

libav development[optional] （下载 地址 密码:uy8a）

gstreamer-devel libavc1394-devel libraw1394-devel libdc1394-devel jasper-devel
jasper-utils

安装软件版本：

gstreamer-devel.x86_64 0:0.10.36-7.el7

jasper-devel.x86_64 0:1.900.1-30.el7_3

jasper-utils.x86_64 0:1.900.1-30.el7_3

libavc1394-devel.x86_64 0:0.5.3-14.el7

libdc1394-devel.x86_64 0:2.2.2-3.el7

libraw1394-devel.x86_64 0:2.1.0-2.el7

对应依赖库：

check-devel.x86_64 0:0.9.9-5.el7

freeglut.x86_64 0:2.8.1-3.el7

glib2-devel.x86_64 0:2.46.2-4.el7

jasper.x86_64 0:1.900.1-30.el7_3

libdc1394.x86_64 0:2.2.2-3.el7

libjpeg-turbo-devel.x86_64 0:1.2.90-5.el7

libxml2-devel.x86_64 0:2.9.1-6.el7_2.3

xz-devel.x86_64 0:5.2.2-1.el7

zlib-devel.x86_64 0:1.2.7-17.el7

ffmpeg 安装（下载 地址 密码:uy8a）

ffmpeg （ffmpeg.x86_64
0:2.6.8-3.el7.nux）安装时需要的依赖库，总共需要18个依赖库：

(1/18): SDL-1.2.15-14.el7.x86_64.rpm \| 204 kB

(2/18): fdk-aac-0.1.4-1.x86_64.rpm \| 375 kB

(3/18): faac-1.28-6.0.el7.nux.x86_64.rpm \| 269 kB

(4/18): fribidi-0.19.4-6.el7.x86_64.rpm \| 63 kB

(5/18): lame-libs-3.99.5-8.el7.x86_64.rpm \| 343 kB

(6/18): libass-0.13.4-1.el7.x86_64.rpm \| 92 kB

(7/18): ffmpeg-2.6.8-3.el7.nux.x86_64.rpm \| 1.5 MB

(8/18): libvdpau-1.1-2.el7.x86_64.rpm \| 32 kB

(9/18): libdc1394-2.2.2-3.el7.x86_64.rpm \| 121 kB

(10/18): schroedinger-1.0.11-4.el7.x86_64.rpm \| 291 kB

(11/18): soxr-0.1.2-1.el7.x86_64.rpm \| 77 kB

(12/18): libavdevice-2.6.8-3.el7.nux.x86_64.rpm \| 71 kB

(13/18): openal-soft-1.16.0-3.el7.x86_64.rpm \| 282 kB

(14/18): libva-1.2.1-3.el7.x86_64.rpm \| 68 kB

(15/18): x265-libs-1.9-1.el7.nux.x86_64.rpm \| 1.5 MB

(16/18): x264-libs-0.142-11.20141221git6a301b6.el7.nux. \| 570 kB

(17/18): xvidcore-1.3.2-5.el7.nux.x86_64.rpm \| 258 kB

(18/18): ffmpeg-libs-2.6.8-3.el7.nux.x86_64.rpm \| 5.0 MB

由于安装ffmpeg
过程中需要的依赖库较多，可以先将一台装有centos版本的服务器联网，然后用yum 或者
（apt-get）等包管理工具将依赖库进行保存。

安装软件版本：

libpng-devel.x86_64 2:1.5.13-7.el7_2 libtiff-devel.x86_64 0:4.0.3-27.el7_3

libtool.x86_64 0:2.4.2-22.el7_3 nasm.x86_64 0:2.10.07-7.el7

swig.x86_64 0:2.0.10-5.el7

相应依赖库：

autoconf.noarch 0:2.69-11.el7

automake.noarch 0:1.13.4-3.el7

m4.x86_64 0:1.4.16-10.el7

perl-Data-Dumper.x86_64 0:2.145-3.el7

perl-Test-Harness.noarch 0:3.28-3.el7

perl-Thread-Queue.noarch 0:3.02-2.el7

Opencv编译过程中出现的问题：

安装过程SIZE_MAX宏定义缺失问题：

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

**解决方法：**

由于错误提示没有SIZE_MAX这个宏，通过修改__STDC_LIMIT_MACROS，并没有解决以上问题，这里我们直接对文件/usr/include/jasper/jas_math.h进行修改，在\#include
\<stdint.h\>添加SIZE_MAX宏定义：

\#if ! defined SIZE_MAX

\#define SIZE_MAX (4294967295U)

\#endif

**Opencv环境配置**

**1、添加路径**

如果不添加路径到环境变量当运行测试程序时会出错如下：

./drawing: error while loading shared libraries: libopencv_calib3d.so.2.4:
cannot open shared object file: No such file or directory

这里主要讨论一般安装不考虑，root权限下安装。

针对当前用户（用户名：username）：

输入命令：vi /home/username/.bashrc

PKG_CONFIG_PATH=\$PKG_CONFIG_PATH:/where/you/install/opencv/lib/pkgconfig

Export PKG_CONFIG_PATH

export LD_LIBRARY_PATH=\$LD_LIBRARY_PATH:/ where/you/install/opencv/lib

更新环境变量：source /etc/profile; 更新系统库缓存： ldconfig

**2、难查看opencv 是否安装成功**

输入命令：pkg-config—cflags opencv

pkg-config—libs opencv

如果没有出错，说明安装成功。

**3、测试**

生成 可执行文件：g++demo.cpp \`pkg-config --cflags --libs opencv\` -o demo

生成动态链接库可输入:gcc demo.c \`pkg-config --cflags --libs opencv\` -fPIC
-shared -o libdemo.so

Centos7无网络环境安装配置Caffe
------------------------------

这里主要针对
caffe进行安装配置时，下面给出**不能正常联网时**，caffe安装的详细过程，并提供了软件安装过程中需要的依赖包。注意这里只给出该系统版软件包有依赖安装的情况进行说明，对于不需要依赖安装的软件不做说明，直接下载对应源，通过其它途径拷贝到相应安装位置即可。

**系统环境（centos 7, 服务器无联网环境）：**

*Linux version 3.10.0-514.el7.x86_64 (builder\@kbuilder.dev.centos.org) (gcc
version 4.8.5 20150623 (Red Hat 4.8.5-11) (GCC) ) \#1 SMP Tue Nov 22 16:42:41
UTC 2016*

**Caffe 安装依赖库：**

\> protobuf-devel leveldb-devel snappy-devel
[OpenCV](http://lib.csdn.net/base/opencv)-devel boost-devel hdf5-devel gflags
devel glog-devel lmdb-devel

这里我们可以先使用联网的机器和没有访问限制的服务器，使用上述介绍的方法下载相应依赖库，然后再进行安装。

1） glog

wget https://google-glog.googlecode.com/files/glog-0.3.3.tar.gz

tar zxvf glog-0.3.3.tar.gz

cd glog-0.3.3

./configure

make && make install

2） gflags

wget https://github.com/schuhschuh/gflags/archive/master.zip

unzip master.zip

cd gflags-master

mkdir build && cd build

export CXXFLAGS=”-fPIC” && cmake .. && make VERBOSE=1

make && make install

3） lmdb

Git clone https://github.com/LMDB/lmdb

cd lmdb/libraries/liblmdb

make && make install

4） protobuf重要

安装步骤请参考 http://blog.csdn.net/tdmyl/article/details/31811317

5） hdf5安装

hdf5安装请查看官方文档

http://www.hdfgroup.org/HDF5/release/obtainsrc.html\#src

6） leveldb安装

leveldb下载请见https://github.com/google/leveldb

安装请见http://blog.sina.com.cn/s/blog_560e310001015jfx.html

缺失的步骤：

leveldb文件夹下依次执行

make

cd out-shared

cp lib\* /usr/local/lib

以下是联网进行安装的方法，由于本文说的是不联网安装，因此本文给出对于本版本服务器的依赖库的下载地址[http://pan.baidu.com/s/1bpB2AYF]
密码：21r4

由于在不联网环境下很难对软件包的依赖关系，利用包管理工具（yum,
apt-get等）做出更新，因此本文强调的是进行不联网更新安装。由于本文说的是不联网安装，因此本文给出对于本版本服务器的依赖库的下载[地址](http://pan.baidu.com/s/1bpB2AYF)*[http://pan.baidu.com/s/1bpB2AYF]*
密码：21r4，也在公司服务器上下载有相应代码。通过将相应版本的代码放置到一个目录，通过Linux
yum,rpm命令进行更新安装。其它步骤按照官方文档进行配置就不会出错。

软件安装中遇到的问题
--------------------

软件安装过程中出现的编译问题进行了整理，这里由于贴出了出错时的相应代码，因此该部分请结合目录进行搜索查看，这里主要针对Matcaffe和Pycaffe的编译安装问题进行了整理回顾。

### 错误 libprotobuf.so.14缺失问题

**问题描述：**

PROTOC src/caffe/proto/caffe.proto

protoc: error while loading shared libraries: libprotobuf.so.14: cannot open
shared object file: No such file or directory

make: \*\*\* [.build_release/src/caffe/proto/caffe.pb.cc] Error 127

**解决方案:**

sudo ldconfig or export LD_LIBRARY_PATH=/usr/local/lib

### missing: Atlas_CLAPACK_INCLUDE_DIR Atlas_CBLAS_LIBRARY Atlas_BLAS_LIBRARY Atlas_LAPACK_LIBRARY

**问题描述：**

CMake Error at
/usr/local/share/cmake-2.8/Modules/FindPackageHandleStandardArgs.cmake:108
(message):

Could NOT find Atlas (missing: Atlas_CLAPACK_INCLUDE_DIR

Atlas_CBLAS_LIBRARY Atlas_BLAS_LIBRARY Atlas_LAPACK_LIBRARY)

Solution:

Cmake –DBLAS=open ..

**解决方案：**

cmake -DBLAS=open -DCMAKE_INCLUDE_PATH=/home/17070718/packages/OpenBLAS/include
-DCMAKE_LIBRARY_PATH=/home/17070718/packages/OpenBLAS/lib .

### libraries: libprotobuf.so.8 缺失问题:

**问题描述：**

/bin/protoc: error while loading shared libraries: libprotobuf.so.8: cannot open
shared object file: No such file or directory

make[2]: \*\*\* [include/caffe/proto/caffe.pb.cc] Error 127

make[1]: \*\*\* [src/caffe/CMakeFiles/caffeproto.dir/all] Error 2

**解决方案：**

Git clone –recursive [源码地址]

### 仅编译matcaffe 编译问题（编译参数设置）

**问题描述：**

gnu++11 [enabled by default]

}), solvers_.end());

\^

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:208:4: error: no
matching function for call to
?.emove_if(std::vector\<boost::shared_ptr\<caffe::Solver\<float\> \>
\>::iterator, std::vector\<boost::shared_ptr\<caffe::Solver\<float\> \>
\>::iterator, delete_solver(int, mxArray\*\*, int, const
mxArray\*\*)::__lambda0)?

}), solvers_.end());

\^

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:208:4: note:
candidate is:

In file included from /usr/include/c++/4.8.2/algorithm:62:0,

from ./include/caffe/blob.hpp:4,

from ./include/caffe/caffe.hpp:7,

from /home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:18:

/usr/include/c++/4.8.2/bits/stl_algo.h:1134:5: note: template\<class \_FIter,
class \_Predicate\> \_FIter std::remove_if(_FIter, \_FIter, \_Predicate)

remove_if(_ForwardIterator \__first, \_ForwardIterator \__last,

\^

/usr/include/c++/4.8.2/bits/stl_algo.h:1134:5: note: template argument
deduction/substitution failed:

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp: In substitution
of ?.emplate\<class \_FIter, class \_Predicate\> \_FIter std::remove_if(_FIter,
\_FIter, \_Predicate) [with \_FIter =
\__gnu_cxx::__normal_iterator\<boost::shared_ptr\<caffe::Solver\<float\> \>\*,
std::vector\<boost::shared_ptr\<caffe::Solver\<float\> \> \> \>; \_Predicate =
delete_solver(int, mxArray\*\*, int, const mxArray\*\*)::__lambda0]?.

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:208:4: required
from here

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:208:4: error:
template argument for ?.emplate\<class \_FIter, class \_Predicate\> \_FIter
std::remove_if(_FIter, \_FIter, \_Predicate)?.uses local type
?.elete_solver(int, mxArray\*\*, int, const mxArray\*\*)::__lambda0?

}), solvers_.end());

\^

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:208:4: error:
trying to instantiate ?.emplate\<class \_FIter, class \_Predicate\> \_FIter
std::remove_if(_FIter, \_FIter, \_Predicate)?

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp: In function
?.oid delete_net(int, mxArray\*\*, int, const mxArray\*\*)?.

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:293:3: warning:
lambda expressions only available with -std=c++11 or -std=gnu++11 [enabled by
default]

}), nets_.end());

\^

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:293:4: error: no
matching function for call to
?.emove_if(std::vector\<boost::shared_ptr\<caffe::Net\<float\> \> \>::iterator,
std::vector\<boost::shared_ptr\<caffe::Net\<float\> \> \>::iterator,
delete_net(int, mxArray\*\*, int, const mxArray\*\*)::__lambda1)?

}), nets_.end());

\^

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:293:4: note:
candidate is:

In file included from /usr/include/c++/4.8.2/algorithm:62:0,

from ./include/caffe/blob.hpp:4,

from ./include/caffe/caffe.hpp:7,

from /home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:18:

/usr/include/c++/4.8.2/bits/stl_algo.h:1134:5: note: template\<class \_FIter,
class \_Predicate\> \_FIter std::remove_if(_FIter, \_FIter, \_Predicate)

remove_if(_ForwardIterator \__first, \_ForwardIterator \__last,

\^

/usr/include/c++/4.8.2/bits/stl_algo.h:1134:5: note: template argument
deduction/substitution failed:

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp: In substitution
of ?.emplate\<class \_FIter, class \_Predicate\> \_FIter std::remove_if(_FIter,
\_FIter, \_Predicate) [with \_FIter =
\__gnu_cxx::__normal_iterator\<boost::shared_ptr\<caffe::Net\<float\> \>\*,
std::vector\<boost::shared_ptr\<caffe::Net\<float\> \> \> \>; \_Predicate =
delete_net(int, mxArray\*\*, int, const mxArray\*\*)::__lambda1]?.

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:293:4: required
from here

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:293:4: error:
template argument for ?.emplate\<class \_FIter, class \_Predicate\> \_FIter
std::remove_if(_FIter, \_FIter, \_Predicate)?.uses local type ?.elete_net(int,
mxArray\*\*, int, const mxArray\*\*)::__lambda1?

}), nets_.end());

\^

/home/17070718/packages/caffe/matlab/+caffe/private/caffe_.cpp:293:4: error:
trying to instantiate ?.emplate\<class \_FIter, class \_Predicate\> \_FIter
std::remove_if(_FIter, \_FIter, \_Predicate)?

make: \*\*\* [matlab/+caffe/private/caffe_.mexa64] Error 255

**解决方案：**

>   CXXFLAGS += -MMD -MP

>   CXXFLAGS += -std=c++11

### Invalid MEX-file '/matlab/+caffe/private/caffe_.mexa64': libopenblas.so.0: cannot open shared object file: No such file or directory

**解决方案：**

Add the openblas lib dir to LD_LIBRARY_PATH

### MATLAB/R2014a/bin/glnxa64/../../sys/os/glnxa64/libstdc++.so.6: version \`GLIBCXX_3.4.18' not found (required by /usr/local/lib/libprotobuf.so.14)

**解决方案：**

Maybe this issue on caffe is helpful to you.
[BVLC/caffe\#827](https://github.com/BVLC/caffe/issues/827)  
In my case, export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libstdc++.so.6 solved
the error.

strings /usr/anaconda2/pkgs/libgcc-5.2.0-0/lib/libstdc++.so.6 \| grep
GLIBCXX(符合要求)

strings /usr/local/MATLAB/R2014a/sys/os/glnxa64/libstdc++.so.6 \| grep GLIBCXX

strings /usr/anaconda2/lib/libstdc++.so.6 \| grep GLIBCXX(符合要求)

也即添加上包含GLIBCXX_3.4.18版本的libstdc++.so.6

### INSTALL NCCL，注意gencode architecture,不然编译会出现问题

**问题描述：**

F0920 17:05:47.221035 23925 caffe.cpp:254] Multi-GPU execution not available -
rebuild with USE_NCCL

\*\*\* Check failure stack trace: \*\*\*

\@ 0x7f00dbada84d google::LogMessage::Fail()

\@ 0x7f00dbadc61c google::LogMessage::SendToLog()

\@ 0x7f00dbada43c google::LogMessage::Flush()

\@ 0x7f00dbadcf2e google::LogMessageFatal::\~LogMessageFatal()

\@ 0x40b72e train()

\@ 0x40861c main

\@ 0x7f00cef57af5 \__libc_start_main

USE_NCCL,编译方式来执行程度

nvcc fatal : Unsupported gpu architecture 'compute_60'

make: \*\*\* [/home/17070718/packages/nccl/build/obj/libwrap.o] Error 1

[17070718\@233184 nccl]\$ echo \$LD_LIBRARY_PATH

Myself try to modify the architecture of the cuda

CUDA_ARCH := -gencode arch=compute_20,code=sm_20 \\

\-gencode arch=compute_20,code=sm_21 \\

\-gencode arch=compute_30,code=sm_30 \\

\-gencode arch=compute_35,code=sm_35 \\

\-gencode arch=compute_50,code=sm_50 \\

\-gencode arch=compute_52,code=sm_52 \\

\#-gencode arch=compute_60,code=sm_60 \\

\#-gencode arch=compute_61,code=sm_61 \\

\#-gencode arch=compute_61,code=compute_61

### NCCL与MATLAB识别问题，如果未识别

NCCL对于已经安装的NCCL, 如果nccl的lib和include
库不在搜索路径里面，记得把相应CMake Dependency
路径,前提是NCCL已经安装lib已经加入LD_LIBRARY_PATH里面（成功案例，Caffe-reid(/prjdocs/person_reids/caffe-reid)

SOLUTION：

cmake -DBLAS=open -DBUILD_matlab=ON -DUSE_NCCL=ON
-DCMAKE_INCLUDE_PATH=/home/17070718/packages/OpenBLAS/include
-DCMAKE_LIBRARY_PATH=/home/17070718/packages/OpenBLAS/lib .
