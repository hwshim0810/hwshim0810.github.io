---
layout: post
title: Tensorflow 설치과정 (Ubuntu 16.04)
description: "Tensorflow install"
tags: [Linux, Tensorflow, Installation]
---
# Nvidia 드라이버 설치
1. Cuda 지원확인  
[링크](https://developer.nvidia.com/cuda-gpus)
2. 드라이버 설치
```bash
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt-get update
$ sudo apt-get install nvidia-375
```
3. 설치확인  
```bash
    $ nvidia-smi
````

# Cuda toolkit 설치
1. 설치
```bash
    $ sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
    $ sudo apt-get update
    $ sudo apt-get install cuda-8-0
````
 
2. 환경변수
```bash
$ echo -e "\n## CUDA and cuDNN paths"  >> ~/.bashrc
$ echo 'export PATH=/usr/local/cuda-8.0/bin:${PATH}' >> ~/.bashrc
$ echo 'export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}' >> ~/.bashrc
```

3. 버전확인  
```bash
    $ nvcc --version
```
4. cuda 경로확인  
```bash
    $ which nvcc
```

# CuDnn 설치
1. 다운로드  
[링크](https://developer.nvidia.com/rdp/cudnn-download)
2. CUDA 버전맞는 library for linux 다운로드

3. 압축풀고 which nvcc 경로에 복사후 권한
```bash
$ tar xzvf cudnn-8.0-linux-x64-v5.1.tgz
$ which nvcc
/usr/local/cuda-8.0/bin/nvcc
$ sudo cp cuda/lib64/* /usr/local/cuda-8.0/lib64/
$ sudo cp cuda/include/* /usr/local/cuda-8.0/include/
$ sudo chmod a+r /usr/local/cuda-8.0/lib64/libcudnn*
$ sudo chmod a+r /usr/local/cuda-8.0/include/cudnn.h
```

4. 설치확인
```bash
    $ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2  
    #define CUDNN_MAJOR      5
    #define CUDNN_MINOR      1
    #define CUDNN_PATCHLEVEL 10
    #define CUDNN_VERSION    (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)

    #include "driver_types.h"
```

# NVIDIA CUDA Profiler Tools Interface 설치
1. 설치  
```bash
    $ sudo apt-get install libcupti-dev
```
