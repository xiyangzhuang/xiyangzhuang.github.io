layout: post
title: Centos7无网络环境安装配置Caffe
modified: 2017-07-19
categories: [articles, 环境配置, Caffe]
tags: 
  - 环境配置
comments: true
---

这里主要针对
caffe进行安装配置时，下面给出**不能正常联网时**，caffe安装的详细过程，并提供了软件安装过程中需要的依赖包。注意这里只给出该系统版软件包有依赖安装的情况进行说明，对于不需要依赖安装的软件不做说明，直接下载对应源，通过其它途径拷贝到相应安装位置即可。

**系统环境（centos 7, 服务器无联网环境）：**

*Linux version 3.10.0-514.el7.x86_64 (builder\@kbuilder.dev.centos.org) (gcc
version 4.8.5 20150623 (Red Hat 4.8.5-11) (GCC) ) \#1 SMP Tue Nov 22 16:42:41
UTC 2016*

Caffe安装：
===========

Caffe 安装依赖库：
------------------

> protobuf-devel leveldb-devel
snappy-devel [OpenCV](http://lib.csdn.net/base/opencv)-devel boost-devel
hdf5-devel gflags glog-devel lmdb-devel

可以先使用联网的机器和没有访问限制的服务器下载相应依赖库，然后再进行安装。

1.  glog

   wget <https://google-glog.googlecode.com/files/glog-0.3.3.tar.gz>   
   tar zxvf glog-0.3.3.tar.gz   
   cd glog-0.3.3   
   ./configure   
   make && make install

2.  gflags

   wget <https://github.com/schuhschuh/gflags/archive/master.zip>   
   unzip master.zip   
   cd gflags-master   
   mkdir build && cd build   
   export CXXFLAGS=”-fPIC” && cmake .. && make VERBOSE=1   
   make && make install

3.  lmdb

   [Git](http://lib.csdn.net/base/git) clone <https://github.com/LMDB/lmdb>   
   cd lmdb/libraries/liblmdb   
   make && make install

4.  protobuf重要

   安装步骤请参考 <http://blog.csdn.net/tdmyl/article/details/31811317>

5.  hdf5安装

   hdf5安装请查看官方文档   
   <http://www.hdfgroup.org/HDF5/release/obtainsrc.html#src>

6.  leveldb安装 

   leveldb下载请见<https://github.com/google/leveldb>   
   安装请见<http://blog.sina.com.cn/s/blog_560e310001015jfx.html>   
   缺失的步骤：   
   leveldb文件夹下依次执行   
   make   
   cd out-shared   
   cp lib\* /usr/local/lib
然则，安装如下这些库往往需要大量的依赖库
  atlas-devel.x86_64 0:3.10.1-10.el7   boost-devel.x86_64 0:1.53.0-26.el7    
  gflags.x86_64 0:2.1.1-6.el7          gflags-devel.x86_64 0:2.1.1-6.el7     
  glog.x86_64 0:0.3.3-8.el7            glog-devel.x86_64 0:0.3.3-8.el7       
  hdf5.x86_64 0:1.8.12-8.el7           hdf5-devel.x86_64 0:1.8.12-8.el7      
  leveldb.x86_64 0:1.12.0-11.el7       leveldb-devel.x86_64 0:1.12.0-11.el7  
  lmdb.x86_64 0:0.9.18-1.el7           lmdb-devel.x86_64 0:0.9.18-1.el7      
  opencv.x86_64 0:2.4.5-3.el7          opencv-devel.x86_64 0:2.4.5-3.el7     
  protobuf.x86_64 0:2.5.0-8.el7        protobuf-devel.x86_64 0:2.5.0-8.el7   
  snappy-devel.x86_64 0:1.1.0-3.el7
在上述库的安装过程中会相继要下载其它的依赖库
  OpenEXR-libs.x86_64 0:1.7.1-7.el7                                          
  atlas.x86_64 0:3.10.1-10.el7                                               
  boost.x86_64 0:1.53.0-26.el7                                               
  boost-atomic.x86_64 0:1.53.0-26.el7                                        
  boost-chrono.x86_64 0:1.53.0-26.el7                                        
  boost-context.x86_64 0:1.53.0-26.el7                                       
  boost-filesystem.x86_64 0:1.53.0-26.el7                                    
  boost-graph.x86_64 0:1.53.0-26.el7                                         
  boost-locale.x86_64 0:1.53.0-26.el7                                        
  boost-math.x86_64 0:1.53.0-26.el7                                          
  boost-program-options.x86_64 0:1.53.0-26.el7                               
  boost-python.x86_64 0:1.53.0-26.el7                                        
  boost-random.x86_64 0:1.53.0-26.el7                                        
  boost-regex.x86_64 0:1.53.0-26.el7                                         
  boost-serialization.x86_64 0:1.53.0-26.el7                                 
  boost-signals.x86_64 0:1.53.0-26.el7                                       
  boost-test.x86_64 0:1.53.0-26.el7                                          
  boost-timer.x86_64 0:1.53.0-26.el7                                         
  boost-wave.x86_64 0:1.53.0-26.el7                                          
  ilmbase.x86_64 0:1.0.3-7.el7                                               
  libgfortran.x86_64 0:4.8.5-11.el7                                          
  libquadmath.x86_64 0:4.8.5-11.el7                                          
  lmdb-libs.x86_64 0:0.9.18-1.el7                                            
  opencv-core.x86_64 0:2.4.5-3.el7                                           
  protobuf-compiler.x86_64 0:2.5.0-8.el7  

***由于本文说的是不联网安装，因此本文给出对于本版本服务器的依赖库的下载***[地址
](http://pan.baidu.com/s/1bpB2AYF) 密码：21r4

Caffe 安装配置
----------------

Caffe 的安装配置，请参考官方文档。
