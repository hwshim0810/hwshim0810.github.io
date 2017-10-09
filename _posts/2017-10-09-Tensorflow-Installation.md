---
layout: post
title: Tensorflow 설치과정 (Ubuntu 16.04)
description: "Tensorflow install"
tags: [Linux, Tensorflow, Installation]
---
# Nvidia 드라이버 설치
- Cuda 지원확인
`https://developer.nvidia.com/cuda-gpus`
- 드라이버 설치
```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt-get update
$ sudo apt-get install nvidia-375
```
- 설치확인
`$ nvidia-smi`

# Cuda toolkit 설치
- `sudo apt-get install cuda-8-0`
 
 - 환경변수
 ```
 $ echo -e "\n## CUDA and cuDNN paths"  >> ~/.bashrc
$ echo 'export PATH=/usr/local/cuda-8.0/bin:${PATH}' >> ~/.bashrc
$ echo 'export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}' >> ~/.bashrc
```

- 확인
` $ nvcc --version`
- cuda 경로확인
` $ which nvcc`

# CuDnn 설치
`https://developer.nvidia.com/rdp/cudnn-download`
- CUDA 버전맞는 library for linux 다운로드

- 압축풀고 which nvcc 경로에 복사후 권한
```
$ tar xzvf cudnn-8.0-linux-x64-v5.1.tgz
$ which nvcc
/usr/local/cuda-8.0/bin/nvcc
$ sudo cp cuda/lib64/* /usr/local/cuda-8.0/lib64/
$ sudo cp cuda/include/* /usr/local/cuda-8.0/include/
$ sudo chmod a+r /usr/local/cuda-8.0/lib64/libcudnn*
$ sudo chmod a+r /usr/local/cuda-8.0/include/cudnn.h
```

- 설치확인
```
$ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2  
#define CUDNN_MAJOR      5
#define CUDNN_MINOR      1
#define CUDNN_PATCHLEVEL 10
--
#define CUDNN_VERSION    (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)

#include "driver_types.h"
```

# NVIDIA CUDA Profiler Tools Interface 설치
`$ sudo apt-get install libcupti-dev`
