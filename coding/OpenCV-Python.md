# OpenCV-Python教程

## 简介

### OpenCV
- OpenCV 于1999年由 Gary Bradsky 在英特尔创立，第一个版本于2000年问世。Vadim Pisarevsky 加入了 Gary Bradsky，负责管理英特尔的俄罗斯软件OpenCV团队。2005年，OpenCV 被用于 Stanley ，这辆车赢得了2005年美国穿越沙漠DARPA机器人挑战大赛。后来，在 Willow Garage 的支持下，在 Gary Bradsky 和 Vadim Pisarevsky 主导下，OpenCV项目的开发工作变得活跃起来。OpenCV 现在支持与计算机视觉和机器学习相关的众多算法，并且每天都在拓展中。
- OpenCV 支持各种编程语言，如C++，Python，Java等，可在不同的平台上使用，包括Windows，Linux，OS X，Android和iOS。基于CUDA和OpenCL的高速GPU操作接口也在积极开发中。
- OpenCV-Python 是 OpenCV 的 Python API，结合了 OpenCV C++ API 和 Python 语言的最佳特性。

### OpenCV-Python
- OpenCV-Python是一个Python绑定库，旨在解决计算机视觉问题。
- Python 是一种由 Guido van Rossum 开发的通用编程语言，它很快就变得非常流行，主要是因为它的简单性和代码可读性。它使程序员能够用更少的代码表达思想，而不会降低可读性。
- 与 C/C++ 这类语言相比，Python 的速度更慢。好在，可以使用 C/C++ 轻松的拓展 Python ，我们可以在 C/C++ 中编写计算密集型代码，并用 Python 来封装。这给我们带来了两个好处：首先，代码像原始的 C/C++ 代码一样快（因为后台实际上就是 C/C++ 代码在工作），其次，在 Python 中编写代码比在 C/C++ 中更容易。OpenCV-Python 就是 OpenCV C++ 的Python封装。
- OpenCV-Python 使用了Numpy，这是一个有着MATLAB风格语法，高度优化的用于数值计算的库。所有OpenCV数组结构都与Numpy数组进行转换。这也使得与使用Numpy的其他库（如SciPy和Matplotlib）集成更容易。

## 安装

### 在Ubuntu系统中安装OpenCV-Python

- 在Ubuntu中，可以通过两种方式安装OpenCV-Python：
  1. 直接使用 Ubuntu 软件库中编译好的二进制文件进行安装
  2. 从源码编译安装

#### 二进制方式安装OpenCV-Python

- 使用终端中的以下命令安装 python-opencv（以root用户身份）。
> $ sudo apt-get install python-opencv

- 打开Python IDLE（或IPython）并在Python终端中键入以下命令。

```python
import cv2 as cv
print(cv.__version__)
```

- 如果没有打印任何错误，恭喜！你已成功安装 OpenCV-Python。
- 这种方法虽然简单，但是有个问题。apt软件库并非总是含有最新版本的OpenCV。

#### 从源码中编译OpenCV

- 首先，我们将安装一些依赖库。有些是必需的，有些是可选的。如果不需要，可以跳过可选的依赖库。

##### 必需的依赖库

- 首先，我们需要用 CMake 来配置安装，用 GCC 来编译，用 Python-devel 和 Numpy 来构建Python绑定，等等。

```bash
sudo apt-get install cmake
sudo apt-get install python-devel numpy
sudo apt-get install gcc gcc-c++
```

- 接下来，我们需要GTK库支持GUI功能，需要v4l库支持相机功能，需要 ffmpeg 库和 gstreamer 库支持媒体功能，等等。

```bash
sudo apt-get install gtk2-devel
sudo apt-get install ffmpeg-devel
sudo apt-get install gstreamer-plugins-base-devel
```

##### 可选的依赖库

- 上面的依赖库已经足以让你在Ubuntu系统上安装OpenCV。但是，根据你的需求，你可能还要装一些额外的库。
- OpenCV自带了PNG，JPEG，JPEG2000，TIFF，WebP 等图像格式的支持库，但它们可能有点老。如果你想要最新的库，你可以自己安装它们。

```bash
sudo apt-get install libpng-devel
sudo apt-get install libjpeg-turbo-devel
sudo apt-get install jasper-devel
sudo apt-get install openexr-devel
sudo apt-get install libtiff-devel
sudo apt-get install libwebp-devel
```

##### 下载 OpenCV

- 从OpenCV的[GitHub存储库](https://github.com/opencv/opencv)下载最新的源代码。

```bash
$ sudo apt-get install git
$ git clone https://github.com/opencv/opencv.git
```

- 进入刚下的“opencv”文件夹。创建一个“build”文件夹，进入“build”。

```bash
$ cd opencv
$ mkdir build
$ cd build
```

##### 配置和安装

- 现在，我们已安装好了所有的依赖库，让我们开始安装OpenCV。必须使用CMake配置安装。它指定了要安装哪些模块，安装路径，要使用的其他库，以及是否需要编译文档和示例等。默认参数已配置好，大部分工作都可以自动化完成。
- 下面的命令通常用于配置OpenCV库编译（在 build 文件夹里执行）：
> $ cmake ../

- OpenCV默认采用“Release”构建类型，安装路径为“/usr/local”。有关CMake选项的其他信息，请参阅OpenCV C++编译指南：
- 您应该能在CMake输出信息中看到这些行（这些意味着已正确安装了Python）：

```bash
--   Python 2:
--     Interpreter:                 /usr/bin/python2.7 (ver 2.7.6)
--     Libraries:                   /usr/lib/x86_64-linux-gnu/libpython2.7.so (ver 2.7.6)
--     numpy:                       /usr/lib/python2.7/dist-packages/numpy/core/include (ver 1.8.2)
--     packages path:               lib/python2.7/dist-packages
--
--   Python 3:
--     Interpreter:                 /usr/bin/python3.4 (ver 3.4.3)
--     Libraries:                   /usr/lib/x86_64-linux-gnu/libpython3.4m.so (ver 3.4.3)
--     numpy:                       /usr/lib/python3/dist-packages/numpy/core/include (ver 1.8.2)
--     packages path:               lib/python3.4/dist-packages
```

- 现在，使用“make”命令编译文件，使用“make install”来安装。

```bash
$ make
# sudo make install
```

- 安装完成后，所有文件都在“/usr/local/”目录。打开终端，并导入cv2。

```python
import cv2 as cv
print(cv.__version__)
```