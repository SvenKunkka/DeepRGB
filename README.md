<!-- SVENKUNKKA-README-NAV:BEGIN -->
[English overview](#project-overview-en) · [中文项目说明](#project-overview-zh) · [Maintainer on X / 维护者 X](https://x.com/Svenu1s5)
<!-- SVENKUNKKA-README-NAV:END -->

## DeepRGBD

by Jonathan Balloch and David Bishai as class project for Advanced Computer Vision at Georgia Tech. Credit for the original deep model to the Pyramid Scene Parsing Network (PSPNet) from the guys over at Chinese University of Hong Kong.

## Pyramid Scene Parsing Network

by Hengshuang Zhao, Jianping Shi, Xiaojuan Qi, Xiaogang Wang, Jiaya Jia, details are in [project page](http://www.cse.cuhk.edu.hk/~hszhao/projects/pspnet/index.html).  '[Pyramid Scene Parsing Network](https://arxiv.org/abs/1612.01105)'

### Introduction

This repository is for Deep RGBD, a deep framework that leverages a state of the art scene parsing with other techniques to segement scenes in of RGBD video. The network uses a modified version of Caffe [yjxiong](https://github.com/yjxiong/caffe/tree/mem) and [DeepLab v2](https://bitbucket.org/aquariusjay/deeplab-public-ver2) for evaluation. Pere PSPNet, the bach norm layer has parameters as 'slope,bias,mean,variance' while the typical batch_norm layer contains two parameters as 'mean,variance'.


### Installation

For installation, please follow the instructions of [Caffe](https://github.com/BVLC/caffe) and [DeepLab v2](https://bitbucket.org/aquariusjay/deeplab-public-ver2). To enable cuDNN for GPU acceleration, cuDNN v5 is needed as required in 'yjxiong'. If you meet error related with 'matio', please download and install [matio](https://sourceforge.net/projects/matio/files/matio/1.5.2) as required in 'DeepLab v2'.

The code has been tested successfully on Ubuntu 14.04 and 12.04 with CUDA 7.0, 7.5 and 8.0

### Usage

1. Clone the repository:

   ```shell
   git clone https://github.com/dbishai/DeepRGB.git 
   ```

2. Build Caffe and matcaffe:

   ```shell
   cd $DeepRGBD_ROOT
   cp Makefile.config.example Makefile.config
   vim Makefile.config
   make -j8 && make matcaffe
   ```

3. Evaluation:

   - Evaluation code is in folder 'evaluation'.
   - Download trained models and put them in folder 'evaluation/model':
     - pspnet50\_ADE20K.caffemodel: [GoogleDrive](https://drive.google.com/open?id=0BzaU285cX7TCN1R3QnUwQ0hoMTA)
     - pspnet101\_VOC2012.caffemodel: [GoogleDrive](https://drive.google.com/open?id=0BzaU285cX7TCNVhETE5vVUdMYk0)
     - pspnet101\_cityscapes.caffemodel: [GoogleDrive](https://drive.google.com/open?id=0BzaU285cX7TCT1M3TmNfNjlUeEU)
   - Modify the related paths in 'eval_all.m':
     - Mainly variables 'data_root' and 'eval_list', and your image list for evaluation should be similarity to that in folder 'evaluation/samplelist' if you use this evaluation code structure. 
     - Matlab 'parfor' evaluation is used and the default GPUs are with ID [0:3]. Modify variable 'gpu_id_array' if needed. We assume that number of images can be divided by number of GPUs; if not, you can just pad your image list or switch to single GPU evaluation by set 'gpu_id_array' be length of one, and change 'parfor' to 'for' loop.

   ```shell
   cd evaluation
   vim eval_all.m
   ```

   - Run the evaluation scripts:

   ```
   ./run.sh
   ```

4. Results: 

## Setup ElasticFusion (Ubuntu 16.04)

Clone ElasticFusion, edit build.sh script to delete lines 6-21, then run it

Copy libFreenectDriver.so from the "lib" directory of this repo and place it in  ElasticFusion/deps/OpenNI2/Bin/x64-Release/OpenNI2/Drivers

Run these commands:
```bash
export LD_LIBRARY_PATH=/usr/local/lib:<OpenNI2 LOCATION>/Bin/Intermediate/x64-Release:$LD_LIBRARY_PATH
sudo ln -s /usr/lib/x86_64-linux-gnu/libudev.so /lib/x86_64-linux-gnu/libudev.so.1.6.4
```

if you want to globally install ElasticFusion:
```bash
cd ElasticFusion/Core
sudo make install
cd ../GUI
sudo make install
sudo cp -R ElasticFusion/deps/OpenNI2/Bin/x64-Release/OpenNI2 /usr/local/lib
sudo cp ElasticFusion/deps/OpenNI2/Bin/x64-Release/libOpenNI2.so /usr/local/lib
```

now try running ElasticFusion
```bash
sudo usermod -a -G video YOUR_USERNAME
ElasticFusion
```

### To build your own FreenectDriver
Grab the packages you'll need to compile libusb and libfreenect:
```bash
sudo apt-get install git cmake build-essential
sudo apt-get install freeglut3-dev libxmu-dev libxi-dev
sudo apt-get install libudev-dev
```
Remove the existing libusb, if it's there:
```bash
sudo apt-get remove libusb-1.0-0-dev
```

Grab the sources for libusb-1.0.21:
```bash
wget http://sourceforge.net/projects/libusb/files/libusb-1.0/libusb-1.0.21/libusb-1.0.21.tar.bz2
tar -xvf libusb-1.0.21.tar.bz2
```
Build and install the updated libusb:
```bash
cd libusb-1.0.21/
./configure --prefix=/usr --disable-static
make
sudo make install
```
Then you should be able to build libfreenect.
```bash
cmake -L .. -DLIBUSB_1_LIBRARY:FILEPATH=/usr/lib/libusb-1.0.so
```
modified from <http://stackoverflow.com/questions/28835794/undefined-reference-to-libusb-get-parent-compiling-freenect>


Now follow [these](https://github.com/OpenKinect/libfreenect/tree/master/OpenNI2-FreenectDriver) instructions to compile libFreenectDriver.so

###Troubleshoot
For all other questions please reach out or consult the Issues area over at [PSPNet](https://github.com/hszhao/PSPNet/issues)

### Questions

Please contact 'balloch@gatech.edu'




---

<!-- SVENKUNKKA-PROJECT-INFO:BEGIN -->

<a id="project-overview-zh"></a>
## 中文项目说明

[dbishai/DeepRGB](https://github.com/dbishai/DeepRGB) 的分支，是 Jonathan Balloch 与 David Bishai 的计算机视觉课程项目，结合 PSPNet、Caffe 与 RGB-D 视频处理开展场景分割研究；保留相关原作者归属。

**使用入口：** 按照 README 配置 Caffe 与 MATLAB 接口，编辑 `Makefile.config` 后运行 `make -j8`、`make matcaffe`；模型和评估路径在 `evaluation/` 配置。

**范围说明：** 文档依赖旧版 Ubuntu、CUDA、cuDNN 和外部模型下载，属于历史研究环境；不据此保证现代系统兼容或模型链接仍可用。

<a id="project-overview-en"></a>
## English project overview

A fork of [dbishai/DeepRGB](https://github.com/dbishai/DeepRGB), a computer-vision class project by Jonathan Balloch and David Bishai combining PSPNet, Caffe and RGB-D video processing for scene segmentation, with original research credits preserved.

**Getting started:** Configure Caffe and its MATLAB interface following the README, edit `Makefile.config`, then run `make -j8` and `make matcaffe`. Configure models and evaluation paths in `evaluation/`.

**Scope:** The documentation relies on older Ubuntu, CUDA and cuDNN versions and external model downloads. This historical research environment does not establish modern-system compatibility or continued availability of model links.

**本 Fork 的维护者 / Maintainer of this fork:** [SvenKunkka](https://github.com/SvenKunkka) · [X / Twitter](https://x.com/Svenu1s5)

**上游项目 / Upstream:** [dbishai/DeepRGB](https://github.com/dbishai/DeepRGB)。保留原作者署名与原许可证 / Original authorship and license notices are retained.

<!-- SVENKUNKKA-PROJECT-INFO:END -->
