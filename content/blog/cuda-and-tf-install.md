+++
author = "Gary Gong"
categories = ["deeplearning"]
tags = ["tensorflow", "cuda"]
date = "2018-08-25"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Compiling TensorFlow-GPU on Ubuntu 16.04 with CUDA 9.1(9.2) and Python3"
type = "post"

+++

## Install CUDA:

https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=ppc64le&target_distro=Ubuntu&target_version=1604&target_type=deblocal

1. Here, we used "Linux" -> "ppc64le"(64-bit) -> "16.04" -> "deb(local)", and downloaded the "Base Installer" (approx. 1.0GB)
   <!-- more --> 

2. Following the guidance which given by the nVidia webpage to install the CUDA.

3. During installation, the installation program will ask you to create a "==symbolic link==" of CUDA library folder; remember to create.

4. Create the environment variable to make these work.

   ```sh
   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/ local/cuda/extras/CUPTI/lib64:/usr/lib/nvidia-381
   # Remember that nvidia-381 is the driver version, if you install 390, please modifiy it to nvidia-390
   export CUDA_HOME=/usr/local/cuda
   export PATH=$PATH:/usr/local/cuda/bin
   ```

5. Execute `source ~/.bashrc` (`source ~/.zshrc` if you use zsh instead of bash) to apply the environment variable.

## Install cuDNN:

https://developer.nvidia.com/rdp/cudnn-download

==You need have a nVidia developer membership account (free to signup) to coutinue to download the cuDNN package.==

1. After logining to the download webpage, select the corresponding CUDA version.

2. Select `cuDNN v7.?.? Library for Linux`

3. Unzip the package and copy the file into the system folder by following command:

   ```sh
   tar -zxvf cudnn-9.?-linux-x64-v7.?.tgz
   sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
   sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/ -d
   sudo chmod a+r /usr/local/cuda/include/cudnn.h
   sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
   ```




## First Check:

1. Check your CUDA version by following command:

   ```bash
   cat /usr/local/cuda/version.txt
   ```

2. Check your cuDNN version by following command:

   ```BASH
   cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
   ```

   ​

## Preparing Your Dependency:

```bash
sudo apt-get install python3-numpy python3-dev python3-pip python3-wheel
sudo apt-get install cuda-command-line-tools 
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/extras/CUPTI/lib64 
```



## Clone the Tensorflow:

```bash
git clone https://github.com/tensorflow/tensorflow 
cd tensorflow
git checkout Branch # where Branch is the desired branch
```



## Configure Your Tensorflow:

```bash
cd tensorflow
./configure

You have bazel 0.11.1 installed.
Please specify the location of python. [Default is /home/garygone/python-dev/dl/bin/python]:


Traceback (most recent call last):
  File "<string>", line 1, in <module>
AttributeError: module 'site' has no attribute 'getsitepackages'
Found possible Python library paths:
  /home/garygone/python-dev/dl/lib/python3.5/site-packages
Please input the desired Python library path to use.  Default is [/home/garygone/python-dev/dl/lib/python3.5/site-packages]

Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]:
jemalloc as malloc support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: n
No Google Cloud Platform support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Hadoop File System support? [Y/n]:
Hadoop File System support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: n
No Amazon S3 File System support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Apache Kafka Platform support? [y/N]:
No Apache Kafka Platform support will be enabled for TensorFlow.

Do you wish to build TensorFlow with XLA JIT support? [y/N]: Y
XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with GDR support? [y/N]:
No GDR support will be enabled for TensorFlow.

Do you wish to build TensorFlow with VERBS support? [y/N]:
No VERBS support will be enabled for TensorFlow.

Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]:
No OpenCL SYCL support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: Y
CUDA support will be enabled for TensorFlow.

Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 9.0]: 9.1


Please specify the location where CUDA 9.1 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:


Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]:


Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:


Do you wish to build TensorFlow with TensorRT support? [y/N]:
No TensorRT support will be enabled for TensorFlow.

Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 6.1,6.1,6.1,6.1,6.1,6.1,6.1,6.1]


Do you want to use clang as CUDA compiler? [y/N]:
nvcc will be used as CUDA compiler.

Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]:


Do you wish to build TensorFlow with MPI support? [y/N]:
No MPI support will be enabled for TensorFlow.

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]:


Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]:
Not configuring the WORKSPACE for Android builds.

Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See tools/bazel.rc for more details.
        --config=mkl            # Build with MKL support.
        --config=monolithic     # Config for mostly static monolithic build.
Configuration finished
```



## Build Your Tensorflow with gcc5:

```bash
bazel build --config=opt --config=cuda --cxxopt="-D_GLIBCXX_USE_CXX11_ABI=0" //tensorflow/tools/pip_package:build_pip_package
```

If build sucessfully:

```bash
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

and exec.

```bash
pip3 install /tmp/tensorflow_pkg/tensorflow-1.7.0-cp35-cp35m-linux_x86_64.whl
```

(Please make sure you are in virtualenv if you want to install this in your virtualenv and do not use sudo.)



## What I have seen:

1. https://www.tensorflow.org/install/install_sources : Installing TF from Source
2. [How to find which version of TensorFlow is installed in my system?](https://stackoverflow.com/questions/38549253/how-to-find-which-version-of-tensorflow-is-installed-in-my-system)
3. https://github.com/tensorflow/tensorflow/issues/15604 : ImportError: libcublas.so.9.0: cannot open shared object file: No such file or directory
4. [How to get the cuda version?](https://stackoverflow.com/questions/9727688/how-to-get-the-cuda-version)
5. [[NV] How to check CUDA and cuDNN version] (https://medium.com/@changrongko/nv-how-to-check-cuda-and-cudnn-version-e05aa21daf6c) : 
6. http://windywinter.cn/2018/03/10/ML/tensorflow-gpu版本的编译/ : tensorflow-gpu版本的编译
7. [学习Tensorflow(1)](