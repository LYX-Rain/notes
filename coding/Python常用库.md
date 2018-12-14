# Jupyter notebook

## 安装

```bash
python3 -m pip install --upgrade pip
python3 -m pip install jupyter
```

- 设置密钥的sha1码：

```python
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:c88810a822cf:2a10d54101bf6b326351c15f0c7b9fede372ef23'
```

- 记下 sha1 码
- 配置 jupyter_notebook_config.py
> jupyter notebook --generate-config
```bash
vim .jupyter

## The IP address the notebook server will listen on.
c.NotebookApp.ip = '*'
  
#  The string should be of the form type:salt:hashed-password.
c.NotebookApp.password = u''    # 将 sha1 码复制到此
  
## The port the notebook server will listen on.
c.NotebookApp.port = 8888
 
c.NotebookApp.open_browser = False

c.NotebookApp.notebook_dir = u'/home/rain/jupyter'

c.NotebookApp.allow_remote_access = True
```

# NumPy

- NumPy 的主要对象是同类型的多维数组（array）。它是一张表，所有元素（通常是数字）的类型都相同，并通过正整数元组索引。在NumPy中，维度称为轴。轴的数目为rank
- Numpy 内部解除了 Python 的 PIL(全局解释器锁)，运算效率极好，是大量机器学习框架的基础库
- NumPy 的数组的类称为 ndarray。别名为 array。要注意，numpy.array 与标准 Python 库的类 array.array 不同，后者仅处理一维数组并提供较少的功能

### ndarray 对象的重要属性

> ndarray.ndim
- 数组的轴（维度）的个数。在 Python 中，维度的数量被称为 rank。
> ndarray.shape
- 数组的维度。这是一个整数的元组，表示每个维度中数组的大小。对于有 n 行和 m 列的矩阵，shape 将是(n,m)。因此，shape 元组的长度就是维度(rank)的个数(ndim)。
> ndarray.size
- 数组元素的总数。等于 shape 的元素的乘积
> ndarray.dtype
- 数组中的元素的数据类型。这些数据类型都是 NumPy 定义的，例如 numpy.int32、numpy.int16 和 numpy.float64。在创建数组的时候可以指定 dtype
> ndarray.itemsize
- 数组中每个元素的字节大小。例如，元素的 dtype 为 float64 类型的数组的 itemsize 为 8（=64/8），而 complex32 类型的数组的 itemsize 为 4（=32/8）。它等于 ndarray.dtype.itemsize
> ndarray.data
该缓冲区包含数组的实际元素。通常，我们不需要使用此属性，因为我们将使用索引访问数组中的元素

```python
>>> import numpy as np
>>> a = np.arange(15).reshape(3, 5)
>>> a
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
>>> type(a)
<type 'numpy.ndarray'>
>>> a.ndim
2
>>> a.shape
(3, 5)
>>> a.size
15
>>> a.dtype
dtype('float64')
>>> a.dtype.name
'int64'
>>> a.itemsize
8
```

## Numpy 创建数组

- 使用 array 函数从常规 Python 列表或元组中创建数组，得到的数组的类型从序列中元素的类型推导出

```python
>>> import numpy as np
>>> a = np.array([2,3,4])
>>> a
array([2, 3, 4])
>>> a.dtype
dtype('int64')
>>> b = np.array([1.2, 3.5, 5.1])
>>> b.dtype
dtype('float64')
```

- 一个常见的错误在于使用多个数值参数调用 array，而不是提供一个数字列表作为参数
> a = np.array(1,2,3,4)    # WRONG

> a = np.array([1,2,3,4])  # RIGHT

- array将序列的序列转换成二维阵列，将序列的序列的序列转换成三维数组，等等

```python
>>> b = np.array([(1.5,2,3), (4,5,6)])
>>> b
array([[ 1.5,  2. ,  3. ],
       [ 4. ,  5. ,  6. ]])
```

- 可以在创建时明确指定数组的类型：

```python
>>> c = np.array( [ [1,2], [3,4] ], dtype=complex )
>>> c
array([[ 1.+0.j,  2.+0.j],
       [ 3.+0.j,  4.+0.j]])
```

- 通常，数组的元素最初是未知的，但它的大小是已知的。因此，NumPy 提供了几个函数来创建具有初始占位符内容的数组。这就减少了数组增长的必要，因为数组增长的操作花费很大
- 函数 zeros 创建一个由 0 组成的数组，函数 ones 创建一个由 1 数组的数组，函数 empty 内容是随机的并且取决于存储器的状态。默认情况下，创建的数组的 dtype 是 float64

```python
>>> np.zeros((3,4))
array([[ 0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.]])
>>> np.ones((2,3,4), dtype=np.int16)  # 可以指定dtype
array([[[ 1, 1, 1, 1],
        [ 1, 1, 1, 1],
        [ 1, 1, 1, 1]],
       [[ 1, 1, 1, 1],
        [ 1, 1, 1, 1],
        [ 1, 1, 1, 1]]], dtype=int16)
>>> np.empty((2,3))   # 未初始化时，输出可能不同
array([[  3.73603959e-262,   6.02658058e-154,   6.55490914e-260],
       [  5.30498948e-313,   3.14673309e-307,   1.00000000e+000]])
```

- 要创建数字序列，NumPy提供了一个类似于range的函数，该函数返回数组而不是列表

```python
>>> np.arange( 10, 30, 5 )
array([10, 15, 20, 25])
>>> np.arange( 0, 2, 0.3 )  # 步长可以是浮点数
array([ 0. ,  0.3,  0.6,  0.9,  1.2,  1.5,  1.8])
```

- 当 arange 与浮点参数一起使用时，由于浮点数的精度是有限的，通常不可能预测获得的元素数量。出于这个原因，通常最好使用函数 linspace，它接收我们想要的元素数量而不是步长作为参数：

```python
>>> from numpy import pi
>>> np.linspace( 0, 2, 9 )          # [0,2]中9等分数
array([ 0.  ,  0.25,  0.5 ,  0.75,  1.  ,  1.25,  1.5 ,  1.75,  2.  ])
>>> x = np.linspace( 0, 2*pi, 100 ) # [0,2Π]中100等分数
>>> f = np.sin(x)
```

### Numpy 创建随机数组

- seed：随机因子，在随机数生成器的随机因子被确定后，随机生成的数字是固定的。就像把随机的过程变成一种伪随机的机制，有利于结果复现


#### 均匀分布

- np.random.rand(10, 10)    创建指定形状(示例为10行10列)的数组(范围在0至1之间)
- np.random.uniform(0, 100) 创建指定范围内的一个数
- np.random.randint(0, 100) 创建指定范围内的一个整数

#### 正态分布

- 生成一个满足平均值为 0 且方差为 1 的正态分布随机数样本
> np.random.randn()
- 给定均值/标准差/维度的正态分布
> np.random.normal(1.75, 0.1, (2, 3))

### 数组的打印

- 数组可以通过 print 打印输出，打印出的数组与嵌套列表类似，具有以下布局：
    - 最后一个轴从左到右打印
    - 倒数第二个从上到下打印
    - 其余的也从上到下打印，每个切片与下一个用空行分开
- 一维数组被打印为行、二维为矩阵和三维为矩阵列表

```python
>>> a = np.arange(6)                         # 1维数组
>>> print(a)
[0 1 2 3 4 5]
>>>
>>> b = np.arange(12).reshape(4,3)           # 2维数组
>>> print(b)
[[ 0  1  2]
 [ 3  4  5]
 [ 6  7  8]
 [ 9 10 11]]
>>>
>>> c = np.arange(24).reshape(2,3,4)         # 3维数组
>>> print(c)
[[[ 0  1  2  3]
  [ 4  5  6  7]
  [ 8  9 10 11]]
 [[12 13 14 15]
  [16 17 18 19]
  [20 21 22 23]]]
```

- 如果数组太大而无法打印，NumPy 将自动对数组的中心部分用省略号代替
- 要禁用此行为并强制 NumPy 打印整个数组，可以使用 set_printoptions 更改打印选项
> np.set_printoptions(threshold='nan')

## 多维数组的基本操作

### 算术运算

- 数组能够直接进行加减乘除算术运算

```python
>>> a = np.array([1，2，3])
>>> b = np.arange([4, 5, 6])
>>> a+b
array([5, 7, 9])
>>> a-b
array([-3, -3, -3])
>>> a*b
array([ 4, 10, 18])
>>> a/b
array([0.25, 0.4 , 0.5 ])
>>> b+2
array([6, 7, 8])
>>> b*2
array([ 8, 10, 12])
>>> b/2
array([2. , 2.5, 3. ])
```

- 矩阵是不存在除法运算的，但数组能够进行除法运算。和矩阵的乘法运算不同，数组的乘法运算机制是通过将位置对应的元素相乘来完成的，矩阵乘积可以使用 dot 函数或方法执行：

```python
>>> A = np.array( [[1,1],
...             [0,1]] )
>>> B = np.array( [[2,0],
...             [3,4]] )
>>> A*B                         # 按元素的积
array([[2, 0],
       [0, 4]])
>>> A.dot(B)                    # 矩阵积
array([[5, 4],
       [3, 4]])
>>> np.dot(A, B)                # 另一种方法算矩阵积
array([[5, 4],
       [3, 4]])
```

- 某些操作（例如+=和*=）适用于修改现有数组，而不是创建新数组

```python
>>> a = np.ones((2,3), dtype=int)
>>> b = np.random.random((2,3))
>>> a *= 3
>>> a
array([[3, 3, 3],
       [3, 3, 3]])
>>> b += a
>>> b
array([[ 3.417022  ,  3.72032449,  3.00011437],
       [ 3.30233257,  3.14675589,  3.09233859]])
>>> a += b                  # b 不会自动转换为整数类型
Traceback (most recent call last):
  ...
TypeError: Cannot cast ufunc add output from dtype('float64') to dtype('int64') with casting rule 'same_kind'
```

- 当使用不同类型的数组操作时，结果数组的类型对应于更一般或更精确的数组（称为向上转换的行为）

```python
>>> a = np.ones(3, dtype=np.int32)
>>> b = np.linspace(0,pi,3)
>>> b.dtype.name
'float64'
>>> c = a+b
>>> c
array([ 1.        ,  2.57079633,  4.14159265])
>>> c.dtype.name
'float64'
>>> d = np.exp(c*1j)
>>> d
array([ 0.54030231+0.84147098j, -0.84147098+0.54030231j,
       -0.54030231-0.84147098j])
>>> d.dtype.name
'complex128'
```

- 许多一元运算，例如计算数组中所有元素的总和，都是作为ndarray类的方法实现的

```python
>>> a = np.random.random((2,3))
>>> a
array([[ 0.18626021,  0.34556073,  0.39676747],
       [ 0.53881673,  0.41919451,  0.6852195 ]])
>>> a.sum()         # 对数组中的所有元素进行求和
2.5718191614547998
>>> a.min()         # 找出数组中最小的元素
0.1862602113776709
>>> a.max()         # 找出数组中最大的元素
0.6852195003967595
```

- 通过指定 axis 参数，你可以沿着数组的指定轴应用操作。当 axis 为 0 时，计算方向是针对数组的列的；当 axis 为 1 时，计算方向是针对数组的行的

```python
>>> b = np.arange(12).reshape(3,4)
>>> b
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> b.sum(axis=0)                            # 对每列进行求和
array([12, 15, 18, 21])
>>>
>>> b.min(axis=1)                            # 每行的最小值
array([0, 4, 8])
>>>
>>> b.cumsum(axis=1)                         # 每行的累计和
array([[ 0,  1,  3,  6],
       [ 4,  9, 15, 22],
       [ 8, 17, 27, 38]])
```

#### 基础函数运算

- NumPy 提供了常见的数学函数，如 sin，cos 和 exp，这些函数在数组上按元素级别操作，产生一个数组作为输出

```python
>>> B = np.arange(3)
>>> B
array([0, 1, 2])
>>> np.exp(B)
array([ 1.        ,  2.71828183,  7.3890561 ])
>>> np.sqrt(B)
array([ 0.        ,  1.        ,  1.41421356])
>>> C = np.array([2., -1., 4.])
>>> np.add(B, C)
array([ 2.,  0.,  6.])

# 绝对值，1
a = np.abs(-1)

# sin函数，1.0
b = np.sin(np.pi/2)

# tanh逆函数，0.50000107157840523
c = np.arctanh(0.462118)

# e为底的指数函数，20.085536923187668
d = np.exp(3)

# 2的3次方，8
f = np.power(2, 3)

# 点积，1*3+2*4=11
g = np.dot([1, 2], [3, 4])

# 开方，5
h = np.sqrt(25)

# 求和，10
l = np.sum([1, 2, 3, 4])

# 平均值，5.5
m = np.mean([4, 5, 6, 7])

# 标准差，0.96824583655185426
p = np.std([1, 2, 3, 2, 1, 3, 2, 0])
```

### 索引、切片和迭代

- NumPy 数组可以被索引、切片和迭代，就和其他 Python 序列一样

# Matplotlib

## 创建图

- 线型图

```python
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(42)          # 设置随机种子
x = np.random.randn(30)     # 生成随机数
plt.plot(x, "r--o")         # 绘图
plt.show()
```

### 线条颜色、标记形状和线型

- 用于设置线型图中线条颜色的常用参数：
    - “b”：线条颜色为蓝色
    - “g”：线条颜色为绿色
    - “r”：线条颜色为红色
    - “c”：线条颜色为蓝绿色
    - “m”：线条颜色为洋红色
    - “y”：线条颜色为黄色
    - “k”：线条颜色为黑色
    - “w”：线条颜色为白色
- 用于指定线型图中标记参数点形状的常用参数：
    - “o”：标记实际点使用的形状为圆形
    - “*”：标记实际点使用“ * ”符号
    - “+”：标记实际点使用“ + ”符号
    - “x”：标记实际点使用“ x ”符号
- 用于设置线型图中连接参数点线条形状的常用参数如下：
    - “-”：线条形状为实线
    - “--”：线条形状为虚线
    - “-.”：线条形状为点实线
    - “:”：线条形状为点线

```python
import numpy as np
import matplotlib.pyplot as plt

a = np.random.randn(30)
b = np.random.randn(30)
c = np.random.randn(30)
d = np.random.randn(30)
plt.plot(a, "r--o", b, "b-*", c, "g-.+", d, "m:x")
plt.show()
```

### 标签和图例

```python
import matplotlib.pyplot as plt
import numpy as np

np.random.seed(42)
x = np.random.randn(30)
y = np.random.randn(30)

plt.title("Example")               # 标题
plt.xlabel("x")                    # X 坐标
plt.ylabel("Y")                    # Y 坐标
X, = plt.plot(x, "r--o")
Y, = plt.plot(y, "b-*")
plt.legend([X,Y], ["X", "Y"])      # 显示函数，第一个参数是列表包含图中使用的标记和线形，第二个参数是对应图例的文字描述
plt.show()
```

### 子图（subplot）

- 同时显示多个图像

```python
import matplotlib.pyplot as plt
import numpy as np

a = np.random.randn(30)
b = np.random.randn(30)
c = np.random.randn(30)
d = np.random.randn(30)

fig = plt.figure()                 # 定义一个实例
ax1 = fig.add_subplot(2,2,1)       # 在两行两列的子图网格中添加第一个子图
ax2 = fig.add_subplot(2,2,2)       # 添加第二个子图
ax3 = fig.add_subplot(2,2,3)       # ···
ax4 = fig.add_subplot(2,2,4)       # ···

A, = ax1.plot(a, "r--o")
ax1.legend([A], ["A"])
B, = ax2.plot(b, "b-*")
ax2.legend([B], ["B"])
C, = ax3.plot(c, "g-.+")
ax3.legend([C], ["C"])
D, = ax4.plot(d, "m:x")
ax4.legend([D], ["D"])
plt.show()
```

### 散点图（scatter）

```python
import matplotlib.pyplot as plt
import numpy as np

np.random.seed(42)
x = np.random.randn(30)
y = np.random.randn(30)

plt.scatter(x, y, c="g", marker="o", label="(X,Y)")
plt.xlabel("X")
plt.ylabel("Y")
plt.legend(loc=1)
plt.show()
```

- 核心代码是：
> plt.scatter(x, y, c="g", marker="o", label="(X,Y)")
- c：指定散点图中绘制的参数点使用哪种颜色
- marker：指定散点图中绘制的参数点使用哪种形状
- label：指定在散点图中绘制的参数点使用的图例

### 直方图（histogram）

```python
import matplotlib.pyplot as plt
import numpy as np

np.random.seed(42)
x = np.random.randn(1000)
plt.hist(x, bins=20, color="g")    # bin 用于指定直方图条纹数量
plt.xlabel("X")
plt.ylabel("Y")
plt.show()
```

### 饼图

```python
import matplotlib.pyplot as plt

labels = ['Dogs', 'Cats', 'Birds']
sizes = [15, 50, 35]

plt.pie(sizes, explode=(0,0,0.1), labels=labels, autopct='%1.1F%%', startangle=90)
plt.axis('equal')    # 使 X 轴和 Y 轴的刻度保持一致，否者就不是圆饼
plt.show()
```

> plt.pie(sizes, explode=(0,0,0.1), labels=labels, autopct='%1.1F%%', startangle=90)
- sizes：指定每部分数据系列在整个圆形中的占比
- explode：每部分数据系列之间的间隔，如果设置两个 0 和一个 0.1，就能突出第 3 部分
- autopct：将 sizes 中的数据以所定义的浮点精度进行显示
- startangle：开始绘制第一块饼图与 X 轴正方向的夹角度数

# pandas

## 介绍

- Pandas 通常是被用在数据采集和存储以及数据建模和预测中间的工具，作用是数据挖掘和清理
- pandas提供了快速，灵活和富有表现力的数据结构，目的是使“关系”或“标记”数据的工作既简单又直观。它旨在成为在Python中进行实际数据分析的高级构建块
- pandas适合于许多不同类型的数据，包括：
  1. 具有异构类型列的表格数据，例如SQL表格或Excel数据
  2. 有序和无序（不一定是固定频率）时间序列数据。
  3. 具有行列标签的任意矩阵数据（均匀类型或不同类型）
  4. 任何其他形式的观测/统计数据集。
- 安装：
> pip3 install pandas
- 或者通过conda 来安装pandas：
> conda install pandas

## 数据结构入门

- Serise：1 维，带有标签的同构类型数组
- DataFrame：2 维，表格结构，带有标签，大小可变，且可以包含异构的数据列
- DataFrame 可以看作是 Series 的容器，即：一个 DataFrame 中可以包含若干个 Series

### Series（序列）

- Series 是一维结构的数据，能够保存任何数据类型（整数、字符串、浮点数、python 对象等）
> s = pd.Series(data, index=index)
- data：数据，可以是一个 python 字典、ndarray（NumPy中的数据结构）、标量（例如 5）

```python
import pandas as pd

series1 = pd.Series([1,2,3,4])
print(series1)
'''
0    1
1    2
2    3
3    4
dtype: int64
'''
```

- 第一列是数据的索引（index）第二列是数据