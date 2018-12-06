# OpenCV

## OpenCV 简介

* OpenCV 的结构
- 根据功能和需求的不同，OpenCV中的函数接口大体可以分为如下部分：
1. core：核心模块，主要包含了OpenCV中最基本的结构（矩阵，点线和形状等），以及相关的基础运算/操作
1. imgproc：图像处理模块，包含和图像相关的基础功能（滤波，梯度，改变大小等），以及一些衍生的高级功能（图像分割，直方图，形态分析和边缘/直线提取等）
1. highgui：提供了用户界面和文件读取的基本函数，比如图像显示窗口的生成和控制，图像/视频文件的IO等
1. video：用于视频分析的常用功能，比如光流法（Optical Flow）和目标跟踪等
1. calib3d：三维重建，立体视觉和相机标定等的相关功能
1. features2d：二维特征相关的功能，主要是一些不受专利保护的，商业友好的特征点检测和匹配等功能，比如ORB特征
1. object：目标检测模块，包含级联分类和Latent SVM
1. ml：机器学习算法模块，包含一些视觉中最常用的传统机器学习算法
1. flann：最近邻算法库，Fast Library for Approximate Nearest Neighbors，用于在多维空间进行聚类和检索，经常和关键点匹配搭配使用
1. photo：计算摄像学（Computational Photography）相关的接口，当然这只是个名字，其实只有图像修复和降噪而已
1. stitching：图像拼接模块，有了它可以自己生成全景照片
1. nonfree：受到专利保护的一些算法，其实就是SIFT和SURF
1. contrib：一些实验性质的算法，考虑在未来版本中加入的
1. legacy：字面是遗产，意思就是废弃的一些接口，保留是考虑到向下兼容
1. ocl：利用OpenCL并行加速的一些接口
1. superres：超分辨率模块，其实就是BTV-L1（Biliteral Total Variation – L1 regularization）算法
1. viz：基础的3D渲染模块，其实底层就是著名的3D工具包VTK（Visualization Toolkit）

### OpenCV-Python

- OpenCV-Python是为解决计算机视觉问题而设计的 Python 库
- 与 C/C++ 等语言相比，Python 的速度要慢一些。也就是说，Python 可以很容易地用 C/C++ 进行扩展，这允许我们用 C/C++ 编写计算复杂的代码，并创建可以用作 Python 模块的 Python 包装器。这给了我们两个优势：首先，代码和原来的 C/C++ 代码一样快(因为它是在后台工作的实际 C++ 代码)，其次，用 Python 编写代码比用 C/C++ 更容易。OpenCV-Python 是原始 OpenCV C++ 实现的 Python 包装器
- OpenCV-Python 使用 Numpy，这是一个高度优化的库，用于具有 matlab 风格语法的数值操作。所有 OpenCV 数组结构都被转换为 Numpy 数组。这也使得与其他使用 Numpy 的库(如 SciPy 和 Matplotlib)集成更加容易

### 在 Windows 上安装 OpenCV-Python

- 先安装 Python
> pip install opencv-python

## 处理文件、摄像头和图形用户界面

### 基本 I/O

####  图像在 Python 中的表示

- 无论使用哪种格式表示图像，每一个像素都会有一个值，但不同的格式表示像素的方式不同
- 对于灰度图，每个像素都由一个 8 位整数表示，即每个像素值的范围是 0 ~ 255
- 对与彩色图，如 BGR 格式的图像，每一个像素由一个三元数组表示，，每个整型（integer）向量分别表示一个 B、G 和 R 通道。其它色彩空间（如 HSV）也以同样的方式来表示像素，只是取值范围和通道数不同（HSV 色彩空间的色度值范围是 0 ~ 180）
- 可以通过 shape 属性来查看图像的结构，它会返回行和列。如果有一个以上的通道，还会返回通道数

```python
>>> import cv2, numpy

# 创建一个黑色的正方形灰度图
>>> img = numpy.zeros((3,3), dtype=numpy.uint8)
array([[0, 0, 0],
       [0, 0, 0],
       [0, 0, 0]], dtype=uint8)
>>> img.shape
(3,3)   # 3行3列1通道

# 利用 cv.cvtColor() 函数将该图像转换成 BGR 格式：
>>> img = cv2.cvtColor(img, cv2.COLOR_GRAY2BGR)
array([[[0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]],
       
       [[0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]],
        
       [[0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]]], dtype=uint8)
>>> img.shape()
(3,3,3) #3行3列3通道
```

- Tip：大多数常用的 OpenCV 函数都在 cv2 模块内。可能也会遇到其他基于 cv 或 cv2.cv 模块的 OpenCV 帮助，这些都是之前的版本。
- Python 模块被称为 cv2 并不表示该模块是针对 OpenCV 2.x.x 版本的，而是因为该模块引入了一个更好的 API 接口，它们采用了面向对象的编程方式，这与以前的 cv 模块有所不同

#### 读取图像

- OpenCV 提供函数 cv.imread() 读取图像，函数支持各种静态图像文件格式。不同系统支持的文件格式不一样，但都支持 BMP 格式，通常还应该支持 PNG、JPEG 和 TIFF 格式
> img = cv.imread(filename[, flags])
- filename：图像路径
- flags：读取方式常用 flags 有：
    - cv.IMREAD_COLOR = 1 : 默认标志‎，将图像转换为3通道 BGR 彩色图像，透明度信息（alpha 通道）会被删除
    - cv.IMREAD_GRAYSCALE = 0 : 将图像转换为单个通道灰度图像
    - cv.IMREAD_UNCHANGED = -1 : 读取图像，包括图像的 alpha 通道
    - cv.IMREAD_ANYDEPTH = 2 ：‎当输入具有相应的深度时返回16位/32 位图像, 否则将其转换为8位
    - cv.IMREAD_ANYCOLOR = 4 ：‎图像以任何可能的颜色格式读取
    - cv.IMREAD_LOAD_GDAL = 8：使用GDAL驱动程序通过支持以下格式来解码图像Raster（光栅）、Vector（向量）
    - [其他](https://docs.opencv.org/3.4.3/d4/da8/group__imgcodecs.html#gga61d9b0126a3e57d9277ac48327799c80af660544735200cbe942eea09232eb822)
- 默认以 BGR 格式读取，即使是图像文件是灰度格式函数也会返回 BGR 格式的图像；如果无法读取图像 (由于缺少文件、权限不正确、不支持或格式无效), 则函数返回空矩阵 NULL

```python
import cv2 as cv

img = cv.imread("test.jpg", 0)
```

#### 显示图像

- 使用函数 cv.imshow() 显示图像，窗口会自动适应图像大小
> cv.imshow(winname, mat)
- winname：窗口名称（字符串）
- mat：要显示的图像
- 可以根据需要创建尽可能多的窗口, 但窗口的名称必须不同

```python
cv.imshow('image', img)
cv.waitKey(0)
cv.destroyAllWindows()
```

- cv.waitKey () 是一个键盘绑定函数，其参数为等待键盘触发的时间，单位为毫秒
- 特定的几毫秒内，如果按下任意键，函数就会返回按键的 ASCII 码值，程序将继续执行。如果没有键盘输入，返回 -1。如果设置函数的参数是 0，它将会无限的等待键盘输入
- 此函数可以用于检测特定键是否被按下
- Tip：Python 提供了一个标准函数 ord()，可以将字符转换为 ASCII 码，如 ord('a') = 97
- OpenCV 的窗口函数和 waitKey() 函数相互依赖，OpenCV 的窗口只有在调用 waitKey() 函数时才会更新，waitKey() 函数只有在 OpenCV 窗口成为活动窗口时，才能捕获输入信息
- 除了绑定键盘事件之外，此函数还处理许多其他 GUI 事件，因此必须使用它来实际显示图像
- cv.‎‎destroyAllWindows() 函数可以销毁创建的所有窗口，如果要销毁任何特定窗口，如果只想销毁特定的窗口可以使用 cv.destroyWindow() 函数，参数为想要销毁的窗口名
- 有一种情况是要先创建窗口后向其加载图像，可以指定窗口是否可调整大小
- 使用 cv.namedWindow() 函数创建窗口，第一个参数是窗口名称，第二个参数是 flag
- 默认情况下 flag 为 cv.WINDOW_AUTOSIZE 窗口大小会自动调整以适应所显示的图像，但是不能更改大小
- 如果指定的标志是 cv.WINDOW_NORMAL 就可以调整窗口大小，当图像尺寸太大, 会有滚动条

```python
cv.namedWindow('image', cv.WINDOW_NORMAL)
cv.imshow('image', img)
cv.waitKey(0)
cv.destroyAllWindows()
```

#### 保存图像

- OpenCV 提供函数 cv.imwrite() 函数保存图像
> cv.imwrite(filename, img[, params])
- filename：保存为的文件名
- img：要保存的图像
- [params](https://docs.opencv.org/master/d4/da8/group__imgcodecs.html#ga292d81be8d76901bff7988d18d2b42ac)：使用特定的编码格式

```python
import numpy as np
import cv2 as cv

img = cv.imread('test.jpg',0) # 读取灰度图像
cv.imshow('image',img)
k = cv.waitKey(0)
if k == 27:                   # 按 ESC 键退出
    cv.destroyAllWindows()
elif k == ord('s'):           # 按 's' 键保存退出
    cv.imwrite('messigray.png',img)
    cv.destroyAllWindows()
```

- ‎如果使用的是64位计算机, 则必须将 k = cv.waitKey(0) 改成 k = cv.waitKey(0) & 0xFF

#### 使用 Matplotlib

- Matplotlib 是 Python 的绘图库, 它提供了多种绘图方法，可以使用 Matplotlib 缩放图像、保存图片等

```python
import numpy as np
import cv2 as cv
from matplotlib import pyplot as plt

img = cv.imread('messi5.jpg',0)
plt.imshow(img, cmap = 'gray', interpolation = 'bicubic')
plt.xticks([]), plt.yticks([])  # ‎在 X 和 Y 轴上隐藏刻度值‎
plt.show()
```

- OpenCV 是以 BGR 格式加载的彩色图像，但 Matplotlib 是以 RGB 模式显示图像。因此，如果用 OpenCV 读取图像后直接使用 Matplotlib 彩色图像将无法正确显示，需要先进行转换：

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

img = cv2.imread('messi4.jpg')
b,g,r = cv2.split(img)
img2 = cv2.merge([r,g,b])
plt.subplot(121);plt.imshow(img) # expects distorted color
plt.subplot(122);plt.imshow(img2) # expect true color
plt.show()

cv2.imshow('bgr image',img) # expects true color
cv2.imshow('rgb image',img2) # expects distorted color
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- [matplotlib.pyplot API](https://matplotlib.org/api/pyplot_api.html)

#### 图像和原始字节之间的转换

- 字节的范围是 [0, 255]，像素通常由每个信道的一个字节表示，一个像素通常由每个通道的一个字节表示
- OpenCV 图像是一个 numpy.array() 类型的二维或三维数组，8位的灰度图像是一个含有字节值的二维数组，一个 24 位的 RGB 图像是一个三维数组，也包含字节值
- 可以使用 image[0, 0, 0] 访问这些值，第一个索引是 y 坐标（行），0 表示顶部；第二个索引是 x 坐标（列），0 表示最左边；第三个索引（在 RGB 图像中）为颜色通道（灰度图中没有）
- 可以使用 setitem 方法为每个像素点赋值
- 若一幅图像的每个通道为 8 位，可以将其显式转换为标准的一维 Python bytearray 格式：
> byteArray = bytearray(image)
- 反之，含有特定顺序的 bytearray，也可以通过显式转换和重构，得到 numpy.array 形式的图像：
> Gray_image = numpy.array (gray_byte_aray) .reshape (height, width)

> Bgr_image = numpy.array (bgr_byte_array) .reshape (height, width, 3)
- 将含有随即字节的 bytearray 转换为灰度图像和 BGR 图像

```python
import cv2
import numpy
import os

# 生成一个大小为 120,000 的随机 bytearray.
random_byte_array = bytearray(os.urandom(120000))
flat_numpy_array = numpy.array(random_byte_array)

# 将该数组转换为 400x300 的灰度图像.
gray_image = flat_numpy_array.reshape(300, 400)

# 将该数组转换为 400x100 的彩色图像
bgr_image = flat_numpy_array.reshape(100, 400, 3)

cv2.imshow('Random gray', gray_image)
cv2.imshow('Random color', bgr_image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

- 使用 Python 标准的 os.urandom() 函数可随机生成原始字节，然后将该字节转换为 NumPy 数组
- 使用 numpy.random.random(0, 256, 120000).reshape(300, 400) 也能直接（且更高效）随机生成 NumPy 数组
- 使用 os.urandom() 函数是为了展示原始字节的转换

### 视频文件处理

- OpenCV 提供了 VideoCapture 对象用于捕获视频
- 其参数可以是设备索引或视频文件，设备索引是指定照相机的编号默认从 0 开始，视频文件直接传入文件路径的字符串
- 通过 cap.isOpened() 函数确保正确的打开相机设备或正确的读取视频文件
- 通过 cap.read() 函数读取每一帧图像，函数先返回一个 bool 值标志着是否正确读取，如果正确读取会同时返回每一帧图像
- 最后处理完后不要忘记释放捕获（关闭设备或文件）

### 从照相机捕获视频

```python
import numpy as np
import cv2 as cv

cap = cv.VideoCapture(0)

while(cap.isOpened()):                              # 确保成功打开设备/文件
    ret, frame = cap.read()
    if ret == True:                                 # 确保捕获到图像
      gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)  # 转换为灰度图
      cv.imshow('frame',gray)
      if cv.waitKey(1) & 0xFF == ord('q'):          # 按 q 键退出
          break

cap.release()                                       # 释放捕获
cv.destroyAllWindows()
```

- 通过 cap.get(propid) 函数获取视频的属性 prop_id 的范围是[0, 18] 每个数字表示视频的一个属性，具体属性查阅[官方文档](https://docs.opencv.org/master/d4/d15/group__videoio__flags__base.html#gaeb8dd9c89c10a5c63c139bf7c4f5704d)
- 通过 cap.set(propid, value) 可以更改部分属性
- 例如将视频宽、高都缩小为原来的 0.5 倍：

```python
width = cap.get(cv.CAP_PROP_FRAME_WIDTH)    # 获取宽度 
heigth = cap.get(cv.CAP_PROP_FRAME_HEIGHT)  # 获取高度
cap.set(cv.CAP_PROP_FRAME_WIDTH, width/2)   # 设置宽度
cap.set(cv.CAP_PROP_FRAME_HEIGHT, height/2) # 设置高度
```

### 从文件播放视频

- 与从相机中捕获视频相同，只是输入的是视频文件名
- 通过 cv.waitKey() 函数设置视频播放的速度，正常情况下设置 25 毫秒即可

```python
import numpy as np
import cv2 as cv

cap = cv.VideoCapture('test.avi')

while(cap.isOpened()):
    ret, frame = cap.read()
    cv.imshow('frame', frame)
    if cv.waitKey(25) & 0xFF == ord('q'):
        break

cap.release()
cv.destoryAllWindows()
```

- 请确保安装了 ffmpeg 或 gstreamer 的正确版本。有时, 使用视频捕获主要是由于安装了错误的 ffmpeg/gstreamer
- Tip：一般处理图像并不需要对每一帧进行处理，可以设置一个 flag 每 10 帧左右处理一次图像

### 保存视频

- OpenCV 提供了 VideoWriter 对象保存视频，需要指定输出文件名、FourCC 码、每秒保存的帧数、每帧的大小以及 isColor 的标识
- 如果 isColor 设置为 True 每帧图像将以彩色保存，否则以灰度图保存
- FourCC 是一个4字节的代码，用于指定视频编解码器。可用代码的列表可以在 fourcc.org 找到
  - 在 Fedora 系统使用：DIVX, XVID, MJPG, X264, WMV1, WMV2 （XVID 是最好的，MJPG 适用于大视频， X264 适用于小视频）
  - 在 Windows 系统使用：DIVX
  - 在 OSX 系统使用：MJPG (.mp4), DIVX (.avi), X264 (.mkv)
- FourCC 码的传参形式：
> cv.VideoWriter_fourcc('M','J','P','G')
- 或
> cv.VideoWriter_fourcc(*'MJPG')
- 下面的代码捕获从相机, 翻转每帧垂直方向, 并保存它‎：

```python
import numpy as np
import cv2 as cv

cap = cv.VideoCapture(0)

# 定义编解码器并创建VideoWriter对象
fourcc = cv.VideoWriter_fourcc(*'XVID')
out = cv.VideoWriter('output.avi',fourcc, 20.0, (640,480))

while(cap.isOpened()):
    ret, frame = cap.read()
    if ret==True:
        frame = cv.flip(frame,0)

        # 保存翻转的坐标系
        out.write(frame)

        cv.imshow('frame',frame)
        if cv.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break

# 工作完成后释放一切
cap.release()
out.release()
cv.destroyAllWindows()
```

## OpenCV 中的绘图

- 常用的绘图函数有：cv.line(), cv.circle() , cv.rectangle(), cv.ellipse(), cv.putText()
- 这些函数都需要下列参数：
    - img：要绘制图形的那幅图
    - color：使用的颜色，以 RGB 为例，需要传入一个元组，如 (255,0,0) 代表蓝色。对于灰度图只需要传入灰度值
    - thickness：线条的粗细，默认值是 1。如果给一个闭合图形设置为 -1，这个图形就会被填充。
    - linetype：线条的类型，8 连接（8-connected，默认），抗锯齿等。cv.LINE_AA 为抗锯齿的线条，很适合曲线

### 画线

- 画一条线你只告函数条线的起点和终点
> cv.line(img, pt1, pt2, color[, thickness[, lineType[, shift]]])
- pt1：起点
- pt2：终点

### 画矩形

- 画一个矩形你告函数的左上点和右下点的坐标
> cv.rectangle(img, pt1, pt2, color[, thickness[, lineType[, shift]]])
- pt1：左上点
- pt2：右下点

### 画圆

- 画圆的只指定圆形的中心点坐标和半径大小
> cv.circle(img, center, radius, color[, thickness[, lineType[, shift]]])
- center：圆心
- radius：半径

### 画椭圆

- 画椭圆比较复杂，需要多几个参数。一个参数是中心点的位置坐标。 下一个参数是长轴和短轴的长度。椭圆沿逆时针方向旋转的角度。椭圆弧沿顺时针方向起始的角度和结束的角度如，果是 0 或 360就是整个椭圆
> cv.ellipse(img, center, axes, angle, startAngle, endAngle, color[, thickness[, lineType[, shift]]])
- center：圆心
- axes：长轴和短轴的长度（二元组）
- angle：椭圆旋转角度
- startAngle：椭圆弧的起始角度
- endAngle：椭圆弧结束的角度

### 画多边形

- 画多边形，需要指定每个顶点的坐标。将这些点的坐标组成 N 行的二维数组，N 是顶点的数量，它应该是 int32 型的
> cv.polylines(img, pts, isClosed, color[, thickness[, lineType[, shift]]])
- pts：所有点坐标的数组
- isClosed：是否闭合（首尾相连）
- Tip：可以使用 cv.polylines() 函数绘制多条直线。只需创建一个想要绘制的所有线的数组，并将其传递给函数。所有的线将被单独画出。与为每条线都调用 cv.line() 相比，这是一种更好更快的绘制一组线条的方法

### 在图片上添加文字

> cv.putText(img, text, org, fontFace, fontScale, color[, thickness[, lineType[, bottomLeftOrigin]]])
- text：要绘制的文字
- org：绘制的文字的左下角的位置坐标
- fontFace：[字体样式](https://docs.opencv.org/3.4.3/d0/de1/group__core.html#ga0f9314ea6e35f99bb23f29567fc16e11)
- fontScale：字体大小
- lineType：[线条样式](https://docs.opencv.org/3.4.3/d0/de1/group__core.html#gaf076ef45de481ac96e0ab3dc2c29a777)，如颜色、粗细、线条类型等，推荐使用 linetype=cv.LINE_AA

```python
import cv2 as cv
import numpy as np

# 创建一个黑色图像
img = np.zeros((512,512,3), np.uint8)

# 画一条粗细为 5px 的对角蓝线
cv.line(img,(0,0),(512,512),(255,0,0),5)

# 在右上角画一个绿色的矩形
cv.rectangle(img,(384,0),(510,128),(0,255,0),3)

# 在矩形中画一个圆
cv.circle(img,(447,63), 63, (0,0,255), -1)

# 在图片中心画半个椭圆
cv.ellipse(img,(256,256),(100,50),0,0,180,255,-1)

# 在左上角画一个黄色的具有四个顶点的多边形
pts = np.array([[10,5],[20,30],[70,20],[50,10]], np.int32)
pts = pts.reshape((-1,1,2))     # 这里 reshape 的第一个参数为 -1,表明这一维的长度是根据后的维度的算出来的
cv.polylines(img,[pts],True,(0,255,255))

# 在图像上绘制白色的“OpenCV”
font = cv.FONT_HERSHEY_SIMPLEX
cv.putText(img,'OpenCV',(10,480), font, 4,(255,255,255),2,cv.LINE_AA)

cv.imshow('final', img)
cv.waitKey(0)
cv.destroyAllWindows()
```

- Tip：所有的绘图函数的回值是 None 所以不能使用 img = cv2.line(img,(0,0),(511,511),(255,0,0),5)

## 用鼠标作图

- 首先，要创建一个鼠标回调函数，该函数在鼠标事件发生时执行（监听鼠标事件）
- 鼠标事件可以是鼠标上的任何动作，比如左键按下、左键松开、左键双击等
- 可以通过鼠标事件获取鼠标对应图像上的坐标，有了事件和坐标就可以做任何想要做的事情
> cv.setMouseCallback(winname, onMouse[, userdata])
- winname：窗口名字
- onMouse：鼠标事件的回调函数（MouseCallback）
- userdata：传递给回调的可选参数
- 所有的鼠标事件回调函数都有一个统一的格式，他们所不同的地方仅仅是被调用后的功能
> MouseCallback(event, x, y, flags[, userdata])
- event：[鼠标事件](https://docs.opencv.org/3.4.3/d7/dfc/group__highgui.html#ga927593befdddc7e7013602bca9b079b0)
- x：x 坐标
- y：y 坐标
- flags：[事件标志](https://docs.opencv.org/3.4.3/d7/dfc/group__highgui.html#gaab4dc057947f70058c80626c9f1c25ce)
- userdata：可选参数

```python
import numpy as np
import cv2 as cv

drawing = False # true if mouse is pressed
mode = True # if True, draw rectangle. Press 'm' to toggle to curve
ix,iy = -1,-1

# mouse callback function
def draw_circle(event,x,y,flags,param):
    global ix,iy,drawing,mode

    if event == cv.EVENT_LBUTTONDOWN:
        drawing = True
        ix,iy = x,y

    elif event == cv.EVENT_MOUSEMOVE:
        if drawing == True:
            if mode == True:
                cv.rectangle(img,(ix,iy),(x,y),(0,255,0),-1)
            else:
                cv.circle(img,(x,y),5,(0,0,255),-1)

    elif event == cv.EVENT_LBUTTONUP:
        drawing = False
        if mode == True:
            cv.rectangle(img,(ix,iy),(x,y),(0,255,0),-1)
        else:
            cv.circle(img,(x,y),5,(0,0,255),-1)

img = np.zeros((512,512,3), np.uint8)
cv.namedWindow('image')
cv.setMouseCallback('image',draw_circle)

while(1):
    cv.imshow('image',img)
    k = cv.waitKey(1) & 0xFF
    if k == ord('m'):
        mode = not mode
    elif k == 27:
        break

cv.destroyAllWindows()
```

## 处理图像

### 色彩空间转换

- OpenCV 中有超过 150 种进行色彩空间转换的方法。但在计算机视觉中最常用的就三种：灰度（Gray）↔ BGR，BGR ↔ HSV
    - 灰度色彩空间是通过除去彩色信息来将其转换成灰阶，灰度色彩空间对在对图像的中间处理时更为高效
    - BGR 即蓝-绿-红色彩空间，每一个像素点都由一个三元数组来表示，分别代表蓝、绿、红三种颜色。它与 RGB 只是在颜色的顺序上不同
    - HSV：H（Hue）是色调取值范围[0, 179]，S（Saturation）是饱和度取值范围[0, 255]，V（Value）表示亮度取值范围[0, 255]
- 对色彩空间进行转换使用 cv.cvtColor() 函数
> cv2.cvtColor(src, code[, dst[, dstCn]])
- src：输入图像地址
- code：颜色空间转换代码
- dst：输出图像的大小和深度与‎‎ ‎‎src ‎‎相同
- dstCn：目标图像中的通道数，如果参数为 0, 则通道的数目将从 src 和‎‎ code 自动派生


### 对象跟踪

- 通过色彩空间转换，可以提取带有某种特定色彩的物体。在 HSV 色彩空间中要比在 RGB 空间中更容易表示某一特定颜色
- 提取某一颜色对象的步骤:
    1. 获得视频的每一帧
    1. 颜色空间转换:从 RGB 到HSV
    1. 设置 HSV 阈值
    1. 提取图像中那些被定义颜色的对象

```python
import cv2 as cv
import numpy as np

cap = cv.VideoCapture('test.avi')

while cap.isOpened():
    # 获取每一帧
    res, frame = cap.read()
    if res:
        # 转换到 HSV
        hsv = cv.cvtColor(frame, cv.COLOR_BGR2HLS)
        # 设定蓝色的阈值
        lower_color = np.array([85, 50, 50])
        upper_color = np.array([145, 255, 255])
        # 根据阈值构建掩模 mask
        mask = cv.inRange(hsv, lower_color, upper_color)
        # 对原始图像和 mask 进行位运算  
        res = cv.bitwise_and(frame, frame, mask=mask)

        cv.imshow("Frame", frame)
        cv.imshow("Mask", mask)
        #cv.imshow("Color", res)

        if cv.waitKey(25) & 0xFF == ord("q"):
            break
    else:
        break
cv.destroyAllWindows()
cap.release()
```

### 找到跟踪对象的 HSV 值

- 使用 cv.cvtColor() 查找需要的 HSV 阈值，如绿色的阈值：
```python
green = np.uint8([[[0, 255, 0]]])   # 这里的三层分别对应与 cvArray, cvMat, IplImage
hsv_green = cv.cvtColor(green, cv.COLOR_BGR2HSV)
print(hsv_green)
[[[60, 255, 255]]]
```

- 分别将 [H-10, 100, 100] 和 [H+10, 255, 255] 作为下限和上限

## 几何变换

- OpenCV 提供了两个转换函数，cv.warpAffine 和 cv.warpPerspective，来实现各种转换。cv.warpAffine 接收的参数是 2x3 的变换矩阵，而 cv.warpPerspective 接收的参数是 3x3 的变换矩阵

### 缩放

- OpenCV 提供 cv.resize() 函数调整图像大小，可以直接设置图像尺寸，也可以指定缩放系数
> dst = cv.resize(src, dsize[, dst[, fx[, fy[, interpolation]]]])
- dsize：输出图像尺寸，如果传 None 就会这样计算：
> dsize = Size(round(fx*src.cols), round(fy*src.rows))
- fx：水平轴的缩放比例系数，如果不传，相当于：(double)dsize.width/src.cols
- fy：垂直轴的缩放比例系数，如果不传，相当于：(double)dsize.height/src.rows
- dsize 或 fx 和 fy 必须不同时为 0
- interpolation：插值方法，在压缩图像时推荐使用 cv.INTER_AREA，在扩展时推荐使用 cv.INTER_CUBIC（慢）和 cv.INTER_LINEAR，默认是 cv.INTER_LINEAR

```python
import numpy as np
import cv2 as cv

img = cv.imread('messi5.jpg')
# 使用 X，Y 轴都扩展两倍的方法
res = cv.resize(img,None,fx=2, fy=2, interpolation = cv.INTER_CUBIC)
# 使用指定图像大小的方法
height, width = img.shape[:2]
res = cv.resize(img,(2*width, 2*height), interpolation = cv.INTER_CUBIC)
```

### 平移

- 如果要沿(x, y)方向移动，移动的距离是（tx, ty），可以使用 Numpy 构建转换矩阵 M（数据类型是 np.float32）：
$$ M = \begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ \end{bmatrix} $$
- 然后传给函数 cv.warpAffine() 实现平移
> dst = cv.warpAffine(src, M, dsize[, dst[, flags[, borderMode[, borderValue]]]])
- M：2x3 的变换矩阵
- dsize：输出图像大小
- flag：插值方法，可选标志 WARP_INVERSE_MAP 意味着 M 是逆变换（dst → src）
- borderMode：像素推断方法，当 borderMode = BORDER_TRANSPARENT 时，意味着该函数不会修改与源图像中的“异常值”对应的目标图像中的像素
- borderValue：在边界不变的情况下使用的价值; 默认为0

```python
import numpy as np
import cv2 as cv

img = cv.imread('test.jpg',0)
rows,cols = img.shape   # 获取图像行、列数

M = np.float32([[1,0,100],[0,1,50]])
dst = cv.warpAffine(img,M,(cols,rows))

cv.imshow('img',dst)
cv.waitKey(0)
cv.destroyAllWindows()
```

### 旋转

- 对图像旋转角度 θ 需要使用下面形式的旋转矩阵：
$$ M = \begin{bmatrix} \sin\theta & -\sin\theta \\ \sin\theta & \cos\theta \\ \end{bmatrix} $$
- 如果想要以任意旋转中心进行旋转，矩阵要修正为：
$$ \begin{bmatrix} \alpha & \beta & (1-\alpha)*center.x-\beta*center.y \\ -\beta & \alpha & \beta*center.x+(1-\alpha)*center.y \end{bmatrix} $$
- 其中
$$ \alpha = scale*\cos\theta $$
$$ \beta = scale*\sin\theta $$
- OpenCV 提供了 cv.getRotationMatrix2D 函数用于构建此矩阵
> retval = cv.getRotationMatrix2D(center, angle, scale)
- center：图像的旋转中心
- angle：旋转角度，以度为单位，正值为逆时针旋转
- scale：各向同性比例因子

```python
img = cv.imread('test.jpg',0)
rows,cols = img.shape

# cols-1 和 rows-1 是坐标的极限
M = cv.getRotationMatrix2D(((cols-1)/2.0,(rows-1)/2.0),90,1)    # 这里旋转 90°
dst = cv.warpAffine(img,M,(cols,rows))
```

### 仿射变换

- 在仿射变换中，原始图像中的所有平行线仍将在输出图像中平行
- 为了找到变换矩阵，我们需要从原图像中找到三个点及其在输出图像中的相应位置，然后通过 cv.getAffineTransform() 创建一个2x3矩阵，将该矩阵传递给 cv.warpAffine()
- cv.getAffineTransform() 从三对对应点计算仿射变换
$$ \begin{bmatrix} x'_i \\ y'_i \end{bmatrix} =map\_matrix*\begin{bmatrix} x_i \\ y_i \\ 1 \end{bmatrix} $$
$$ dst(i) = (x',y'),src(i) = (x_i,y_i),i = 0,1,2 $$
> retval = cv.getAffineTransform(src, dst)
- src：原图像中三角形顶点的坐标
- dst：目标图像中相应三角形顶点的坐标

```python
img = cv.imread('test.png')
rows,cols,ch = img.shape

pts1 = np.float32([[50,50],[200,50],[50,200]])
pts2 = np.float32([[10,100],[200,50],[100,250]])

M = cv.getAffineTransform(pts1,pts2)

dst = cv.warpAffine(img,M,(cols,rows))

plt.subplot(121),plt.imshow(img),plt.title('Input')
plt.subplot(122),plt.imshow(dst),plt.title('Output')
plt.show()
```

### 透视变换

- 对于透视变换，我们需要通过 cv.getPerspectiveTransform() 函数构建一个 3x3 的变换矩阵
- 要构建这个变换矩阵，需要在输入图像和输出图像上找 4 个相应的点，这 4 个点中的任意三个都不能共线
$$ \begin{bmatrix} t_ix'_i \\ t_iy'_i \\ t_i \end{bmatrix} =map\_matrix*\begin{bmatrix} x_i \\ y_i \\ 1 \end{bmatrix} $$
$$ dst(i) = (x'_i,y'_i),src(i) = (x_i,y_i),i = 0,1,2,3 $$
> retval = cv.getPerspectiveTransform(src, dst[, solveMethod])
- src：原图像中 4 个点的坐标
- dst：目标图像中相应 4 个点的坐标
* 函数 cv.warpPerspective() 使用该变换矩阵转换原图像：
$$ dst(x,y) = src\left(\frac{M_{11}x+M_{12}y+M_{13}}{M_{31}x+M_{32}y+M_{33}},\frac{M_{21}x+M_{22}y+M_{23}}{M_{31}x+M_{32}y+M_{33}} \right) $$
> dst = cv.warpPerspective(src, M, dsize[, dst[, flags[, borderMode[, borderValue]]]])
- M：3x3 的变换矩阵
- dsize：输出图像大小
- flag：插值方法（INTER_LINEAR or INTER_NEAREST）和可选标志 WARP_INVERSE_MAP 将 M 设置为逆变换（dst → src）

```python
img = cv.imread('test.png')
rows,cols,ch = img.shape

pts1 = np.float32([[56,65],[368,52],[28,387],[389,390]])
pts2 = np.float32([[0,0],[300,0],[0,300],[300,300]])

M = cv.getPerspectiveTransform(pts1,pts2)

dst = cv.warpPerspective(img,M,(300,300))

plt.subplot(121),plt.imshow(img),plt.title('Input')
plt.subplot(122),plt.imshow(dst),plt.title('Output')
plt.show()
```

### 图像阈值

#### 简单阈值

### 傅里叶变换

- 在 OpenCV 中，对图像和视频的大多数处理多少会涉及傅里叶变换的概念：所有的波形都可以由一系列简单且频率不同的正弦曲线叠加得到。这样就可以区分图像里哪些区域的信号（图像像素）变化特别强，哪些区域的信号变化平缓，从而可以任意地标记噪声区域、特征区域、前景和背景等
- 在 Numpy 库中有快速傅里叶变换（FFT）包，它包含了 fft2() 函数，可以计算一幅图像的离散傅里叶变换（DFT）
- 图像的幅度谱（magnitude spectrum）是另一种图像，幅度谱图像呈现了原始图像在变化方面的一种表示：把一幅图中最明亮的像素放到图像中央，然后逐渐变暗，在边缘上的像素最暗。这样可以发现图像中有多少亮的像素和暗的像素，以及它们分布的百分比

#### 高通滤波器（HPF）

- 高通滤波器是检测图像的某个区域，然后根据像素与周围像素的亮度差值来提升（boost）该像素的亮度的滤波器
- 如下的核为例：

```python
[[0, -0.25, 0],
 [-0.25, 1, -0.25],
 [0, -0.25, 0]]
```

- 核（Kernel）（滤波器矩阵）是指一组权重的集合，它会应用在原图像的一个区域，并由此生成目标图像的一个像素。比如大小为 7 的核意味着每 49（7×7）个源图像的像素会产生目标图像的一个像素。可把核看作一块覆盖在源图像上可移动的毛玻璃片，玻璃片覆盖区域的光线会按某种方式进行扩散混合后透过去
- 在计算完中央像素与周围邻近像素的亮度差值之和以后，如果亮度变化很大，中央像素的亮度会增加（反之则不会）。换句话说，如果一个像素比它周围的像素更突出，就会提升它的亮度
- 这在边缘检测上尤其有效，它会采用一种高频提升滤波器（high boost filter）的高通滤波器
- 高通和低通滤波器都有一个称为半径（radius）的属性，它决定了多大面积的邻近像素参与滤波运算
- 一个高通滤波器的例子：

```python
import cv2
import numpy as np
from scipy import ndimage

kernel_3x3 = np.array([[-1, -1, -1],
                   [-1,  8, -1],
                   [-1, -1, -1]])

kernel_5x5 = np.array([[-1, -1, -1, -1, -1],
                       [-1,  1,  2,  1, -1],
                       [-1,  2,  4,  2, -1],
                       [-1,  1,  2,  1, -1],
                       [-1, -1, -1, -1, -1]])
# 这些滤波器中所有值加起来为0
img = cv2.imread("photoes/image6.jpg", 0)

k3 = ndimage.convolve(img, kernel_3x3)
k5 = ndimage.convolve(img, kernel_5x5)

blurred = cv2.GaussianBlur(img, (17,17), 0)
g_hpf = img - blurred

cv2.imshow("3x3", k3)
cv2.imshow("5x5", k5)
cv2.imshow("g_hpf", g_hpf)
cv2.waitKey()
cv2.destroyAllWindows()
```

- 我们定义了一个 3×3 和一个 5×5 的核，然后将读入的图像转换为灰度格式，用给定的核与图像进行卷积（convolve）
- 使用 SciPy 模块可以更方便的完成多维数组的卷积运算（相较于 numpy）

#### 低通滤波器（Low Pass Filter, LPF）

- 高通滤波器是根据像素与邻近像素的亮度差值来提升该像素的亮度。低通滤波器则是在像素与周围像素的亮度差值小于一个特定值时，平滑该像素的亮度。它主要用于去噪和模糊化，比如说，高斯模糊是最常用的平滑滤波器之一，它是削弱高频信号强度的低通滤波器

## 滤波和平滑处理图像

### 图像滤波 2D 卷积

- 滤波过程可以在频率和/或空间域上进行。这里我们将关注空间域中的过滤器
- 滤波空间操作定义在待变换点附近(x, y)，要在空间域中进行滤波，对图像进行卷积(sweep)。对于这个，我们遵循空间中的卷积定理
- OpenCV 提供了函数 cv2.filter2D() ‎使用给定核执行图像的卷积
- 过滤过程如下: 对于图像的每个像素, 一个大小3x3 的窗口以它为中心, 由连续像素执行的核乘法求和, 除以9。这相当于在窗口中执行像素值的平均值。对图像中的每个像素执行此操作
> dst = cv.filter2D(src, Ddepth, Kernel[, dst[, anchor[, delta[, borderType]]]])
  - src：输入图像
  - Ddepth：目标图像所需深度;如果是负数，它将与 src.depth() 相同
  - Kernel：卷积的核
- dst：输出的图像，大小相同、通道数与 src 相同‎

```python
import cv2 as cv
import numpy as np

img = cv.imread("test.jpg")

kernel = np.ones((3, 3), np.float32)/9
dst = cv.filter2D(src = img, ddepth = -1, kernel = kernel)

cv.imshow("Original", img)
cv.imshow("Filtered", blur)
cv.waitKey(0)
cv.destroyAllWindows()
```

#### 平均

- 媒体过滤器是最简单、直观、易于实现平滑图像。这个操作是通过图像与标准滤波器的卷积来完成的。它只是计算核区域下像素的平均值，并替换中心元素的值。在OpenCV中，这个操作是由函数 cv2.blur() 实现，在调用中，必须指定核的大小(宽度和高度)

```python
import cv2 as cv

img = cv.imread("test.jpg")
k = 5
blur = cv.blur(img, (k,k))

cv.imshow("Original", img)
cv.imshow("Filtered", blur)
cv.waitKey(0)
cv.destroyAllWindows()
```

- 对于平均滤波器或平均滤波器的缺点时它对局部变化相当敏感。另外，可以创建在原始图像中没有出现的新的颜色强度

#### 高斯滤波器

- 高斯滤波器用于模糊图像和消除噪声。它类似于媒体过滤器，但使用不同的掩码，建模高斯函数
- 如果我们想使用高斯核，我们必须使用函数 cv.GaussianBlur() 与前一种情况一样，我们必须指定核的宽度和高度，它必须是正数
- 此外，标准差还必须通过 sigmaX 和 sigmaY 参数分别在X和Y方向上指定。如果只指定了sigmaX 参数的值，函数将为 sigmaY 参数分配相同的值

```python
import cv2 as cv

img = cv.imread("test.jpg")

k = 5
sigma = 0
blur = cv.GaussianBlur(img, (k,k), sigma)

cv.imshow("Original", img)
cv.imshow("Filtered", blur)
cv.waitKey(0)
cv.destroyAllWindows()
```

- 高斯滤波器之所以如此重要的原因之一是它能有效地去除图像中的高斯噪声，这个过滤器比普通过滤器产生更多的平滑

#### 中值滤波器

- 中值滤波器由函数 cv.medianBlur() 它计算核区域下所有像素的中值，中间的元素被中值取代，核大小的值必须是正的奇数，这种过滤器非常有效地消除了脉冲噪声

```python
import cv2 as cv

img = cv.imread("test.jpg")

k = 5
blur = cv.medianBlur(img, k)

cv.imshow("Original", img)
cv.imshow("Filtered", blur)
cv.waitKey(0)
cv.destroyAllWindows()
```

## 渐变

- 在本节中，我们将展示通过高通过滤器过滤图像、提取边缘和梯度

### Sobel

- Sobel 运算符（也称为 Sobel-Feldman 运算符）对图像计算二维空间梯度，从而突出边缘对应的空间高频率区域
- Sobel 运算符计算图像在每个点(像素)的梯度。因此，对于每个点，这个算子给出了变化最大的幅度、方向以及从暗到明的方向
- 在每个分析点上图像多么突然或平稳的变化，来判断图像上最有可能的边界以及边界延伸的方向。在实践中，计算边的大小的概率比计算方向和方向更可靠，也更容易解释
- Sobel 运算符使用两个尺寸为3x3的掩模或核与原始图像卷积来计算导数的近似，一个用于计算横轴的变化，另一个用于计算纵轴的变化
- 这些核被设计成响应垂直轴和水平轴。它们可以分别应用于输入图像以产生单独的测量值，或者，另一方面，可以在两个方向上组合起来查找和划界坐标轴
- OpenCV 中执行这种过滤的函数是 cv.Sobel() 可以分别使用 xorder/dx 和 yorder/dy 参数指定梯度（垂直或水平）的方向。此外，还可以使用ksize参数指定核大小
> dst = cv.Sobel(src, ddepth, dx, dy[, dst[, ksize[, scale[, delta[, borderType]]]]])
- src：输入图像
- dst：与 src 具有相同大小和通道数的输出图像
- ddepth：输出图像深度，在 8位输入图像的情况下，它将导致截断的导数
- dx：x 导数的阶数
- dy：y 导数的阶数
- ksize：Sobel 核的大小，必须是 1 或 3 或 5 或 7
- scale：计算导数值的可选尺度参数，默认情况下不进行缩放

```python
import cv2 as cv

img = cv.imread("test.jpg", 0)

k = 3
sobelX = cv.Sobel(img, ddepth=cv.CV_64F, dx=1, dy=0, ksize=k)
sobelY = cv.Sobel(img, ddepth=cv.CV_64F, dx=0, dy=1, ksize=k)

cv.imshow("Original", img)
cv.imshow("SobelX", sobelX)
cv.imshow("SobelY", sobelY)
cv.waitKey(0)
cv.destroyAllWindows()
```

### 拉普拉斯

- 拉普拉斯算子是2维图像的二阶空间导数的各向同性测量。在图像上使用拉普拉斯滤波器可以提取出发生剧烈变化的区域，因此用于边缘检测
- 通常拉普拉斯滤波器应用于先前用高斯滤波器过滤过的图像，以降低对噪声的敏感性
- OpenCV 提供 cv.Laplacian() 函数做拉普拉斯滤波，必选参数是输入图像和输出图像的深度
> dst = cv.Laplacian(src, ddepth[, dst[, ksize[, scale[, delta[, borderType]]]]])
- src：输入图像
- ddepth：输出图像的深度
- ksize：用于计算二阶导数滤波器的孔径大小
- scale：计算拉普拉斯值的可选比例因子，默认情况下进行缩放

```python
import cv2 as cv

img = cv.imread("test.jpg")

blur = cv.Laplacian(img, 3)

cv.imshow("Original", img)
cv.imshow("Filtered", blur)
cv.waitKey(0)
cv.destroyAllWindows()
```

## Canny 边缘检测器

- Canny算法是由 John F. Canny 在1986年开发的一种方法，使用了多级算法来检测图像中的大量边缘
- 算法的步骤如下:
1. 降噪：由于边缘检测会受到图像中包含的噪声的影响，第一步是使用5x5大小的核的高斯滤波器消除图像中的噪声
2. 计算梯度：图像的边缘可以指向不同的方向，因此Canny算法使用了四个滤波器来检测图像的水平、垂直和对角线方向的边缘
3. 在边缘上使用非最大抑制（NMS）：在得到梯度的方向和大小后，对整个图像进行分析，去除不构成任何轴的不需要的像素。为了做到这一点，要检查每个像素在其梯度方向上的邻域是否为局部最大值来检查每个像素。如果像素不是局部最大值，则设置为零。简而言之，结果就是一个“窄轴”的二值图像
4. 阈值：在算法的最后一个阶段，它取决于得到的所有坐标轴哪些是真的坐标轴，哪些不是。为此，设置了两个阈值minVal和maxVal。所有梯度强度大于 maxVal 阈值的轴都被视为坐标轴。所有梯度强度小于 minVal 阈值的轴都被认为是非轴，被丢弃。在 minVal 和 maxVal 值之间的值根据它们之间的连接进行分类。如果它们连接到值大于 maxVal 的像素就是坐标轴的一部分。在任何其他情况下都将被舍弃
- OpenCV 提供 cv.Canny() 函数实现 Canny 边缘检测器
> edges = cv.Canny(image, threshold1, threshold2[, edges[, apertureSize[, L2gradient]]])
- image：8位的输入图像
- edges：输出边缘图像，单通道8位图像，其大小与输入图像相同
- threshold1：第一个阈值 minVal
- threshold2：第二个阈值 maxVal
- apertureSize：Sobel 过滤器核大小（默认设置为3）

```python
import cv2 as cv
import numpy as np

cap = cv.VideoCapture("test.avi")

minVal = 50
maxVal = 150
apertureSize = 3

while(cap.isOpened()):
    ret, frame = cap.read()
    if ret == True:
        edges = cv.Canny(frame, minVal, maxVal, apertureSize)

        cv.imshow("Original", frame)
        cv.imshow("Canny edge detection", edges)
        c = cv.waitKey(25)
        if c == 27:
            break
    else:
        break
cap.release()
cv.destroyAllWindows()
```

## 直方图

- 直方图是一个图形，它给了我们关于图像强度分布的一般概念。一般来说，它是在X轴上设置范围[0,255]的图形，在Y轴上显示具有该强度的像素数量
- 这是理解图像的另一种方式，通过观察图像的直方图，我们可以直观地看到图像的对比度、亮度和强度分布
- 在直方图的基本参数范围内，以下几点非常突出:
    - BINS：X 轴上显示的值的范围。通常我们将显示256个值(从 0 到 255)，但可能会希望对强度进行分组。例如，我们可以显示从 0 到 15，从16到31，……，从 240 到 255，这样，在直方图中只需要表示16个值
    - DIMS：维度，它是进行计算的参数。在这种情况下，维度是1，因为我们只计算强度
    - RANGE：你要测量的强度的范围，通常整个取值范围是[0,255]
- OpenCV提供 cv.calcHist() 函数计算图像直方图
> hist = cv.calcHist(images, channels, mask, histSize, ranges[, hist[, accumulate]])
- images：图像源，参数是一个图像列表
- channels：用于计算直方图的 dims 通道列表，第一个数组通道从 0 数到 images[0].channels()-1，第二个数组通道从 images[0].channels() 数到 images[0].channels() + images[1].channels()-1，依此类推
- mask：可选参数，如果要计算整个图像的直方图，应该将其设置为None，如果矩阵不是空的，它必须是一个与图像[i]大小相同的8位数组。非零掩码元素标记直方图中计数的数组元素
- histSize：每个维度中直方图大小的数组，它也是一个列表，在我们的示例中将使用[256]
- ranges：‎RANGE，每个维度中直方图bin边界的 dims 数组的数组‎，通常，我们将使用[0,255]

```python
# 计算灰度图像上的直方图并显示图形
import cv2 as cv
import numpy as np
from matplotlib import pyplot as plt

img = cv.imread("test.jpg", cv.IMREAD_ANYCOLOR)
hist = cv.calcHist([img], channels=[0],mask=None, histSize=[256], ranges=[0,255])

cv.imshow("img", img)
plt.figure(figsize=(6,6))
plt.plot(hist)
plt.xlim([0,255])
plt.show()
cv.waitKey(0)
cv.destroyAllWindows()
```

### 图和 Matplotlib

- Matplotlib 库中有一个显示直方图的函数：matplot.pyplot.hist()，函数本身直接计算直方图并生成它。因此, 没有必要利用 OpenCV 的‎‎ cv.calcHist()‎‎ 函数
- 但是 OpenCV 算法的优化和更快‎

```python
import cv2 as cv
import numpy as np
from matplotlib import pyplot as plt

img = cv.imread("hdu.jpg")

cv.imshow("img", img)
plt.hist(img.ravel(), 256, [0,255])
plt.xlim([0,255])
plt.show()
cv.waitKey(0)
cv.destroyAllWindows()
```

## 模板匹配

- 模板匹配是一种数字图像处理方法，用于在另一幅图像中搜索和查找模板图像的位置，这是一种寻找适合模板图像的小部分的技术
- OpenCV 提供函数 cv.matchTemplate() 实现模板匹配。简单地说，该算法只是简单地将模板图像放到一般图像上，就好像它是一个二维卷积，然后将模板与图像的那个区域进行比较。结果是一个灰度图像，每个像素表示它与为您的邻居服务的模板图像的距离
> result = cv.matchTemplate(image, templ, method[, result[, mask]])
- image：要进行匹配的源图像，它必须是8位或32位浮点数
- templ：匹配模板，它必须不大于源图像，并且具有相同的数据类型
- result：比较结果图，它一定是单通道32位浮点数。如果源图像大小是 W × H 的，模板是 w × h 的，结果图就是 (W-w+1)×(H-h+1) 的
- method：指定比较[方法](https://docs.opencv.org/master/df/dfb/group__imgproc__object.html#ga3a7850640f1fe1f58fe91a2d7583695d)
- 通过 cv.minMaxLoc() 函数可以找到能够定位图像的最大值/最小值

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

img = cv2.imread('test.jpg', 0)
img2 = img.copy()
template = cv2.imread('template.png', 0)
w, h = template.shape[::-1]

# 列出了所有 6 种方法
methods = ['cv2.TM_CCOEFF', 'cv2.TM_CCOEFF_NORMED', 'cv2.TM_CCORR',
           'cv2.TM_CCORR_NORMED','cv2.TM_SQDIFF', 'cv2.TM_SQDIFF_NORMED']

for meth in methods:
    img = img2.copy()
    method = eval(meth)

    # 进行模板匹配
    res = cv2.matchTemplate(img,template,method)
    min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(res)

    # 如果使用的方法是 TM_SQDIFF 或 TM_SQDIFF_NORMED，则取最小值
    if method in [cv2.TM_SQDIFF, cv2.TM_SQDIFF_NORMED]:
        top_left = min_loc
    else:
        top_left = max_loc
    bottom_right = (top_left[0] + w, top_left[1] + h)

    cv2.rectangle(img, top_left, bottom_right, 255, 2)

    plt.subplot(121),plt.imshow(res,cmap = 'gray')
    plt.title('Matching Result'), plt.xticks([]), plt.yticks([])
    plt.subplot(122),plt.imshow(img,cmap = 'gray')
    plt.title('Detected Point'), plt.xticks([]), plt.yticks([])
    plt.suptitle(meth)
    plt.show()
```

## 人脸检测

- 基于 Haar 基波滤波器特征提取的级联分类器的目标检测法，是 Paul Viola 和 Michael Jones 在 2001 年提出的一种有效的目标检测方法
- 它是一种基于学习技术的自动级联函数，由大量的正、负图像对级联函数进行训练，使其他图像中的目标检测成为可能，图像必须具有默认大小
- 首先，对一个分类器进行训练，使用几百个特定对象的例子，在本例中是正面例子。此外，它还被训练与任意其它大小相同的图像，称为负面例子
- 分类器经过训练后，可应用于输入图像中关注的区域。如果该区域是搜索对象，分类器输出“1”，否则输出“0”。要在完整图像中搜索对象，搜索窗口将在图像中移动。此外，分类器被设计成能够调整大小，以查找不同大小的对象
- 之所以称为“in cascade”，是因为分类器是由几个简单的分类器在不同的阶段形成的，这些分类器依次应用，直到一个阶段被否决或者另一个阶段被推进到最后
- OpenCV 有一个训练器和一个检测器。如果想为任何类型的对象(火车、汽车、狗等)训练自己的分类器，可以使用 OpenCV 来创建它。除此之外，OpenCV还提供了许多之前训练过的人脸、眼睛、微笑等分类器

```python
import cv2 as cv
import sys

# 加载 XML 分类器
cas_path = 'haarcascade/haarcascade_frontalface_default.xml'
eye_path = 'haarcascade/haarcascade_eye.xml'
face_cascade = cv.CascadeClassifier(cas_path)
eye_cascade = cv2.CascadeClassifier(eye_path)

capture = cv.VideoCapture(0)

# 根据 OpenCV 版本选择标志
if cv.__version__.startswith('2.4'):
    dmf_flag = cv.cv.CV_HAAR_SCALE_IMAGE
else:
    dmf_flag = cv.CASCADE_SCALE_IMAGE

while(capture.isOpened()):
    ret, frame = capture.read()
    if ret == True:
        # 灰度化图像
        gray_image = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
        # 检测面部
        faces = face_cascade.detectMultiScale(
            gray_image,
            scaleFactor=1.1,
            minNeighbors=5,
            minSize=(30, 30),
            flags=dmf_flag
        )
        # 在脸上画一个长方形
        for (x, y, w, h) in faces:
            cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 4)
        cv.imshow('Video', frame)
        if cv.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break
capture.release()
cv.destroyAllWindows()
```

## 图像检索以及基于图像描述符的搜索

- 特征是有意义的图像区域，该区域具有独特性或易于识别性。因此，角点及高密度区域是很好的特征；边缘可以将图像分为两个区域，也可以看作好的特征；斑点（与周围有很大差别的图像区域）也是有意义的特征
- OpenCV 可以检测图像的主要特征，然后提取这些特征，使其成为图像描述符，这些图像特征可作为图像搜索的数据库。此外，人们可以利用关键点将图像拼接（stitch）起来，组成一个更大的图像（例如全景图）

### 特征检测算法

- OpenCV 中最常用的特征检测和提取算法有：
    - 用于检测角点的：Harris、FAST、
    - 用于检测斑点（blob）：SIFT、SURF、BRIEF
    - ORB：该算法代表带方向的 FAST 算法与具有旋转不变性的 BRIEF 算法
- 通过以下方法进行特征匹配：
    - 暴力（Brute-Force）匹配法
    - 基于 FLANN 的匹配法
- 特征检测和提取过程是一个底层的操作，有时计算成本很高

### 角点特征检测

#### cornerHarris

- OpenCV提供 cv.cornerHarris() 函数用于角点检测
$$ dst(x,y) = detM^{(x,y)}-k*(trM^{(x,y)})^2 $$
> dst = cv.cornerHarris(src, blockSize, ksize, k[, dst[, borderType]])
- src：‎输入单通道8位或浮点图像。
- dst：哈里斯探测器返回的图像，它具有CV_32FC1类型，大小与src相同
- blockSize：‎邻域大小‎
- ksize：Sobel 算子的孔（aperture）
- k：哈里斯探测器自由参数，对应上面公式里的 k

```python
import cv2 as cv
import numpy as np

img = cv.imread("hdu.jpg")
# RGB -> 灰度图
gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
# 输入图像应该是灰度和浮点32类型
gray = np.float32(gray)
# 检测角点
dst = cv.cornerHarris(gray, blockSize=2, ksize=3, k=0.04)
# 阈值为一个最优值，它可能根据图像的不同而变化
img[dst>0.01*dst.max()]=[0,0,255]

cv.imshow('dst',img)
if cv.waitKey(0) & 0xFF == ord('q'):
    cv.destroyAllWindows()
```

### ORB（快速定向和）

- ORB是在“ OpenCV 实验室”中创建的一种算法，该算法于2011年由 Ethan Rublee、Vincent Rabaud、Kurt Konolige 和 Gary R. Bradski 在文章《SIFT或SURF的有效替代品》中发表。正如这篇文章的标题所指出的那样，这是一种替代算法筛选和免费浏览的方法，因为要获得这些算法的专利，使用这些算法是需要付费的
- 可以说ORB是快速角检测器和简单算法的融合，经过一定的修改，也使用了Harris角检测器
- 为了创建 ORB 对象，OpenCV提供了cv.ORB() 函数。创建完后，为了检测关键点和描述符，必须使用 orb.detectAndCompute() 方法
- [官方文档](https://docs.opencv.org/3.1.0/d1/d89/tutorial_py_orb.html)

```python
import numpy as np
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('simple.jpg',0)

# Initiate ORB detector
orb = cv2.ORB_create()

# find the keypoints with ORB
kp = orb.detect(img,None)

# compute the descriptors with ORB
kp, des = orb.compute(img, kp)

# draw only keypoints location,not size and orientation
img2 = cv2.drawKeypoints(img,kp,color=(0,255,0), flags=0)
plt.imshow(img2),plt.show()
```

## 特征匹配

- 暴力匹配：在第一个集合中选择一个特征的描述符，通过距离的计算将其与第二个集合的所有特征进行比较，返回最接近他的特征
- 在OpenCV中，我们必须首先使用 cv2.BFMatcher() 函数创建 BFMatcher 对象。该方法有两个参数：normType (距离计算方法)和 crossCheck(返回两个集合的描述符)。创建之后，BFMatcher.match() 方法将返回最佳匹配。最后，cv2.drawMatches() 方法将帮助我们绘制对应的图像，将两幅图像水平地组合在一起，并在两幅图像之间画出最佳对应的线条
- 匹配方法的结果是类型为DMatch的对象列表。该对象具有以下属性：
1. DMatch.distance - 描述符之间的距离，越少越好
1. DMatch.trainldx - 图像索引列上的描述符索引
1. DMatch.queryldx - 图像查询中描述符的索引
1. DMatch.imgldx - 图像索引列

### 基础的 Brute-Force Matcher（暴力匹配）

- 暴力匹配很简单。它在第一个集合中获取一个特征的描述符，并使用一定的距离计算与第二个集合中的所有其它特征进行匹配，返回最接近的一个
- 对于 BF matcher，首先我们必须用 cv2.BFMatcher() 创建 BFMatcher 对象，它需要两个可选参数：
- 第一个是 normType，它指定要使用的距离度量。默认情况下是 cv.NORM_L2，它适用于 SIFT、SURF 等（cv.NORM_L1 也适用）
- 对于基于二进制字符串的描述符，如 ORB、BRIEF、BRISK 等，应该使用 cv.NORM_HAMMING，它使用 Hamming 距离作为度量
- 如果 ORB 使用的是 WTA_K == 3 or 4，那么应该使用 cv2.NORM_HAMMING2
- 第二个参数是布尔变量 crossCheck，默认为 false，如果为 true，Matcher 只返回与值(i,j)匹配的值，这样集合A中的第i个描述符与集合B中的第j个描述符匹配最好，反之亦然
- 也就是说，两个集合中的两个特性应该相互匹配。给出了一致的结果，是 D.Lowe 在 SIFT 提出的比值检验的一种较好的替代方法
- 创建 BFMatcher 对象之后，有两个重要的方法 BFMatcher.match() 和 bfmatcher.knmatch()。第一个返回最佳匹配。第二个方法返回用户指定的 k 的最佳匹配。当我们需要在这方面做额外的工作时，它可能是有用的
- 就像我们使用 cv2.drawKeypoints() 来绘制关键点一样，cv2.drawMatches() 帮助我们绘制匹配。它在水平方向堆叠两个图像，并从第一个图像到第二个图像绘制线条，以显示最佳匹配
- 还有 cv2.drawMatchesKnn 它能拉出所有 k 个最好的匹配。如果 k=2，它将为每个关键点绘制两条匹配线。所以我们必须传递一个 mask 如果我们想有选择地画它

- Tip：对于大值，np.math.factorial 返回的数据类型是 long，而不是 int，具有长值的数组是 dtype 对象，因为不能使用 NumPy 的类型存储，可以通过以下方式重新转换最终结果
> WeightMesh = np.array(AyMesh*AxMesh, dtype=float)

## Video Analysis（视频分析）

### Optical Flow（光流）

- 光流的概念是指在连续的两帧图像中由于图像中的物体移动或者摄像头的移动导致的图像中目标像素的移动
- 由观察者和场景之间的相对运动引起的视觉场景中物体表面和边缘的明显运动模式
- 光流是二维矢量场，表示了一个点从第一帧到第二帧的位移
- ![image](image/optical_flow_basic1.jpg)
- 上面的图表示了一个球在连续的5帧图像中的运动。箭头表示了它的位移矢量
- 光流法的工作原理基于如下假设：
    1. 场景的像素强度在相邻帧之间基本不变
    2. 相邻像素具有相似的运动
- 第一帧中的像素 I(x,y,t) 表示在时刻 t 时像素 I(x,y) 的值，在经过 dt 时间后，该像素在下一帧中移动了 (dx,dy)，由于这些像素是相同的且强度不变，因此可以表示成：
$$ I(x,y,t) = I(x+dx,y+dy,t+dt) $$
- 假设移动很小，使用泰勒公式可以表示成：
$$ I(x+dx,y+dy,t+dt)=I(x,y,t)+{\partial I\over\partial x}\Delta x+{\partial I\over\partial y}\Delta y+{\partial I\over\partial t}\Delta t+H.O.T$$
- $ H.O.T $ 是高阶无穷小
- 由第一个假设和使用泰勒公式展开的式子可以得到：
$$ {\partial I\over\partial x}\Delta x+{\partial I\over\partial y}\Delta y+{\partial I\over\partial t}\Delta t=0 $$
- 除以 $ \partial t $ 得
$$ f_xu+f_yu+f_t=0 $$
- 其中
$$ f_x={\partial f\over \partial x};f_y={\partial f\over \partial y};f_t={\partial f\over \partial t};u={dx\over dt};v={dy\over dt} $$
- 上述方程称为光流方程，$ f_x $ 和 $ f_y $ 是图像的梯度，$ f_t $ 是沿时间的梯度。但是 u 和 v 是未知的，我们无法用一个方程解两个未知数，那么就有 Lucas-Kanade 方法来解决这个问题

#### Lucas-Kanade 算法

- 基于第二条假设，就是所有的相邻像素都有相同的移动 Lucas-Kanade 方法使用了一个 3x3 的窗口，在这个窗口中的 9 个像素点满足方程
$$ f_xu+f_yu+f_t=0 $$
- 将点代入方程，现在的问题就变成了使用9个点求解两个未知量，解的个数大于未知数的个数，这是个超定方程，使用最小二乘的方法来求解最优值。如下为计算得到的结果
$$ \begin{bmatrix} u \\ v \\ \end{bmatrix} = \begin{bmatrix} \sum_if_{x_i}^2 & \sum_if_{x_i}f_{y_i} \\ \sum_if_{x_i}f_{y_i} & \sum_if_{y_i}^2 \\ \end{bmatrix}^{-1} \begin{bmatrix} -\sum_if_{x_i}f_{y_i} \\ -\sum_if_{y_i}f_{t_i} \\ \end{bmatrix} $$
- 检测逆矩阵与 Harris 角点检测很像，说明角点是适合用来做跟踪的
- 想法很简单，给出一些点用来追踪，从而获得点的光流向量。但是有另外一个问题需要解决，目前讨论的运动都是小步长的运动，如果有幅度大的运动出现，本算法就会失效
- 使用的解决办法是利用图像金字塔。在金字塔顶端的小尺寸图片当中，大幅度的运动就变成了小幅度的运动。所以使用LK算法，可以得到尺度空间上的光流

#### OpenCV 中的 LK 光流

- OpenCV 库提供函数 cv.calcOpticalFlowPyrLK()
- 一个简单应用示例：我们要跟踪视频中某些点，先使用 cv.goodFeaturesToTrack() 来获取这些点
- 首先取第一帧，检测其中的 Shi-Tomasi 角点，然后使用 Lucas-Kanade 算法来迭代地跟踪这些特征点。迭代的方式就是向 cv.calcOpticalFlowPyrLK() 函数传入上一帧图片和其中的特征点以及当前帧图片，函数会返回当前帧的特征点，每个点都带有一个状态值（0 或 1），如果在当前帧找到了上一帧中的点，这个点的状态值就是 1，否则就是 0。将状态值为 1 的点作为下次特征点的输入，不停迭代

> nextPts, status, err = cv.calcOpticalFlowPyrLK(prevImg, nextImg, prevPts, nextPts[, status[, err[, winSize[, maxLevel[, criteria[, flags[, minEigThreshold]]]]]]])
- 返回值：
    - nextPts：二维点的输出矢量（具有单精度浮点坐标），包含第二图像中输入特征的计算新位置; 当传递 OPTFLOW_USE_INITIAL_FLOW 标志时，向量必须与输入中的大小相同
    - status：状态值（无符号字符型）
    - err：向量中的每个特征对应的错误率
- 输入值：
    - prevImg：上一帧图片
    - nextImg：当前帧图片
    - prevPts：上一帧找到的特征点向量
    - winSize：在图像金字塔中计算局部连续运动的窗口的尺寸
    - maxLevel：图像金字塔层数，0 代表不使用金字塔（单极）
    - criteria：指定迭代搜索算法的终止条件（在指定的最大迭代次数criteria.maxCount之后或当搜索窗口移动小于criteria.epsilon时）
    - flags：选择计算方法：
        1. OPTFLOW_USE_INITIAL_FLOW：使用初始估计，存储在nextPts中; 如果未设置标志，则将prevPts复制到nextPts并将其视为初始估计
        2. OPTFLOW_LK_GET_MIN_EIGENVALS：使用最小特征值作为误差测量; 如果未设置标志，则将原始位置和移动点之间的色块之间的 L1 距离除以窗口中的像素数，用作误差测量
    - minEigThreshold：该算法计算光流方程的2×2正常矩阵的最小特征值，除以窗口中的像素数，如果此值小于 minEigThreshold，则会过滤掉相应的功能并且不会处理该光流，因此它允许删除坏点并获得性能提升

### Background Subtraction（背景消除）

#### Basics（基础）

- 背景减除算法是很多以机器视觉为基础的应用中，非常重要的预处理算法。例如，使用固定的摄像头来统计一个房间的进出人数或者交通摄像头提取关于交通工具的信息等等。在所有这些例子当中，首先要做的就是把人和交通工具单独提取出来。从技术上来讲，需要把移动的前景从静止的背景当中提取出来
- 如果你要一张单独的背景图片，例如一个没有游客的房间照片，没有任何交通工具的街道照片等等，这有一个很简单的方法，只要从新的图片当中减去背景图片即可。你就能得到单独的前景照片。但是在多数的例子当中，你不可能有这样的背景照片，所以我们需要从我们手头有的照片中提取前景。当存在阴影等效果的时候，这相当复杂了。因为影子是会移动的，简单的减除方法会将阴影部分同样当成是前景。这是个很糟糕的事情
- 如果已经有一张单独是背景的图像，就只需将新图像中的背景减去即可。但大多数情况下，单独的背景图像是没有的。所以需要从已有的图像中提取背景当存在阴影时，情况会变得更复杂，由于阴影也会移动，简单的减法会将阴影部分当作是前景，产生误判
- 下面介绍几个算法，OpenCV 已经实现了3种很好用的方法

#### BackgroundSubtractorMOG

- 这是一个基于高斯混合的背景/前景分割算法，来自 2001 年P. KadewTraKuPong 和 R. Bowden 发表的论文：“An improved adaptive background mixture model for real-time tracking with shadow detection”
- 它使用的方法是对每个背景像素由 k 个混合高斯模型进行建模，k 通常为 3 到 5。色彩信息在场景中存在时间的比例作为高斯混合模型的权重大小。最有可能的背景颜色信息是停留时间最长且更为静止的
- 使用时要先通过函数 cv.createBackgroundSubtractorMOG() 来创建一个背景对象，该函数有些可选参数，如历史长度、高斯混合数、阈值等，我们都使用默认值（缺省）。然后在视频循环中，使用 backgroundsubtractor.apply() 方法获取前景的蒙板
> cv.createBackgroundSubtractorMOG([, history[, nmixtures[, backgroundRatio[, noiseSigma]]]])
- history：历史的长度
- nmixtures：高斯混合数
- backgroundRatio：背景比率
- noiseSigma：噪声强度（亮度或每个颜色通道的标准差），0 表示一些自动值

```python
import numpy as np
import cv2 as cv

cap = cv.VideoCapture('vtest.avi')

fgbg = cv.createBackgroundSubtractorMOG2()

while(cap.isOpened()):
    ret, frame = cap.read()
    if ret == True:

        fgmask = fgbg.apply(frame)

        cv.imshow('frame',fgmask)
        k = cv.waitKey(30) & 0xff
        if k == 27:
            break

cap.release()
cv.destroyAllWindows()
```