# 服务器使用规范

## 服务器文档使用

- 目前我们学习的主要文件为
> tutorials/basic/Opencv/opencv-and-python.ipynb
- 该文档为全英文，建议安装一个浏览器翻译的插件，或者直接使用 Chrome
- 服务器上的示例代码可以直接运行，但每次打开文档后都要先运行第一段代码载入函数

## 服务器环境使用

- 不要随意创建 文件&文件夹，不要随意修改他人的文件，学习目录下的文件已设置只读权限不可修改
- 每个同学可以在 “16级” 或 “17级” 目录下建一个自己的文件夹，作为开发环境
- 我们服务器已经配置好 python 环境可直接运行代码，在自己的文件夹下创建开发文件：
> New -> Python3
- 服务器使用 IPython 编译器要在服务器上显示图片需使用 IPython 的 display 函数
- 建议直接将学习文档的第一段代码（display_image() 和 display_frame()）复制过来使用
- 在关闭自己建的文件的时候，注意点击 Files -> Close and Halt 关闭，不要直接关闭网页，避免浪费服务器资源

## 其他学习途径

- 除了服务器上的资源，最主要的是[OpenCV 官方文档](https://docs.opencv.org/master/d9/df8/tutorial_root.html)
- 除此之外还有各大博客：
- [Python 教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)
- [Python 中文文档](https://yiyibooks.cn/xx/python_352/index.html)
- [numpy 中文文档](https://yiyibooks.cn/xx/NumPy_v111/index.html)

## IDE

- [PyCharm](https://pan.baidu.com/s/1hwVJe4oZ5DJW0Ij-MDwzYg)提取码: f1s8
    1. 安装pycharm（pycharm-professional-2018.2.3.exe）
    1. 安装pycharm时，选择Activation code激活，在下方填入激活码（激活码.txt）
    1. 关闭pycharm，将汉化包放到pycharm路径的 lib 目录中（resources_zh_CN_PyCharm_2018.2_r1.jar）
- [Anaconda](https://repo.anaconda.com/archive/Anaconda3-5.2.0-Windows-x86_64.exe)

## 本地环境安装

- [Pyhton-3.7.0](https://www.python.org/ftp/python/3.7.0/python-3.7.0.exe)
- 安装 Python 时记得勾选将 Python 添加到 PATH 环境里

```bash
# 更行 pip
python -m pip  install --upgrade pip

# 安装 numpy、opencv、matplotlib 环境
pip install numpy
pip install opencv-contrib-python==3.4.2.17   # 注意要使用这个版本的 OpenCV 库，最新版 SIFT 函数不能用
pip install matplotlib
```