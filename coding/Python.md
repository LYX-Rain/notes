# Python3

## 基础语法

### 注释
- 单行注释以 # 开头
- 多行注释可以用多个 # 号，还有 ''' 和 """：

```python
#!/usr/bin/python3
 
# 第一个注释
# 第二个注释
 
'''
第三注释
第四注释
'''
 
"""
第五注释
第六注释
"""
```

### 缩进

- 使用缩进来表示代码块，不需要使用大括号 {}

```python
if True:
    print ("Answer")
    print ("True")
else:
    print ("Answer")
  print ("False")    # 缩进不一致，会导致运行错误
```

### 多行语句

- Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠(\)来实现多行语句

```python
total = item_one + \
    item_two + \
    item_three
```

- 在 [], {}, 或 () 中的多行语句，不需要使用反斜杠(\)

```python
total = ['item_one', 'item_two', 'item_three',
    'item_four', 'item_five']
```

- Python可以在同一行中使用多条语句，语句之间使用分号(;)分割

### end 关键字

- 关键字end可以用于将结果输出到同一行，或者在输出的末尾添加不同的字符

```python
a, b = 0, 1
while b < 1000:
    print(b, end=',')
    a, b = b, a+b
# 1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,
```

## Pip

- 升级
> python3 -m pip install --upgrade pip
- 升级pip后出现
> ImportError: cannot import name main


```python
sudo vim /usr/bin/pip3
# 把下面的三行
from pip import main
if __name__ == '__main__':
    sys.exit(main())

# 换成下面的三行
from pip import __main__
if __name__ == '__main__':

    sys.exit(__main__._main())
```

- 然后重启
- 获取pip支持的文件名还有版本

```python
# AMD64
import pip._internal
print(pip._internal.pep425tags.get_supported())
# WIN32
import pip
print(pip.pep425tags.get_supported())
```

## 数据类型

- Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。
- 在 Python 中，变量就是变量，它没有类型，我们所说的"类型"是变量所指的内存中对象的类型。

### 多个变量赋值

```python
a = b = c = 1
# 创建一个整型对象，值为 1，从后向前赋值，三个变量都指向同一个内存地址

a, b, c = 1, 2, "runoob"
# 两个整型对象 1 和 2 的分配给变量 a 和 b，字符串对象 "runoob" 分配给变量 c
```

### 数字(Number)类型

- python中数字有四种类型：整数、布尔型、浮点数和复数
- 在Python 3里，只有一种整数类型 int，表示为长整型，没有 python2 中的 Long
- 数据类型是不允许改变的,这就意味着如果改变数字数据类型的值，将重新分配内存空间
- 以通过使用del语句删除单个或多个对象的引用
> del var
- 内置的 type() 函数可以用来查询变量所指的对象类型
- 还可以用 isinstance 来判断：

```python
a = 111
isinstance(a, int)
True
```

- type()不会认为子类是一种父类类型。
- isinstance()会认为子类是一种父类类型。
- 数值的除法包含两个运算符：/ 返回一个浮点数，// 返回一个整数。
- 在混合计算时，Python会把整型转换成为浮点数。

#### 数学函数

| 函数 | 返回值 ( 描述 ) |
| ---- | -------------- |
| abs(x) | 返回数字的绝对值，如abs(-10) 返回 10 |
| ceil(x) | 返回数字的上入整数，如math.ceil(4.1) 返回 5 |
| cmp(x, y) | 如果 x < y 返回 -1, 如果 x == y 返回 0, 如果 x > y 返回 1。 Python 3 已废弃 。使用 使用 (x>y)-(x<y) 替换。 |
| exp(x) | 返回e的x次幂(ex),如math.exp(1) 返回2.718281828459045 |
| fabs(x) | 返回数字的绝对值，如math.fabs(-10) 返回10.0 |
| floor(x) | 返回数字的下舍整数，如math.floor(4.9)返回 4 |
| log(x) | 如math.log(math.e)返回1.0,math.log(100,10)返回2.0 |
| log10(x) | 返回以10为基数的x的对数，如math.log10(100)返回 2.0 |
| max(x1, x2,...) | 返回给定参数的最大值，参数可以为序列。 |
| min(x1, x2,...) | 返回给定参数的最小值，参数可以为序列。 |
| modf(x) | 返回x的整数部分与小数部分，两部分的数值符号与x相同，整数部分以浮点型表示。 |
| pow(x, y) | x**y 运算后的值。 |
| round(x [,n]) | 返回浮点数x的四舍五入值，如给出n值，则代表舍入到小数点后的位数。 |
| sqrt(x) | 返回数字x的平方根。 |

#### 随机数函数

| 函数 | 描述 |
| ---- | ---- |
| choice(seq) | 从序列的元素中随机挑选一个元素，比如random.choice(range(10))，从0到9中随机挑选一个整数。 |
| randrange ([start,] stop [,step]) | 从指定范围内，按指定基数递增的集合中获取一个随机数，基数缺省值为1 |
| random() | 随机生成下一个实数，它在[0,1)范围内。 |
| seed([x]) | 改变随机数生成器的种子seed。如果你不了解其原理，你不必特别去设定seed，Python会帮你选择seed |
| shuffle(lst) | 将序列的所有元素随机排序 |
| uniform(x, y) | 随机生成下一个实数，它在[x,y]范围内。 |

#### 三角函数

| 函数 | 描述 |
| ---- | ---- |
| acos(x) | 返回x的反余弦弧度值 |
| asin(x) | 返回x的反正弦弧度值 |
| atan(x) | 返回x的反正切弧度值 |
| atan2(y, x) | 返回给定的 X 及 Y 坐标值的反正切值 |
| cos(x) | 返回x的弧度的余弦值 |
| hypot(x, y) | 返回欧几里德范数 sqrt(x*x + y*y) |
| sin(x) | 返回的x弧度的正弦值 |
| tan(x) | 返回x弧度的正切值 |
| degrees(x) | 将弧度转换为角度,如degrees(math.pi/2) ， 返回90.0 |
| radians(x) | 将角度转换为弧度 |

#### 数学常量

- pi：π
- e：自然常数

### 字符串(String)

- python中单引号和双引号使用完全相同。
- 使用三引号('''或""")可以指定一个多行字符串。
- Python 不支持单字符类型，单字符在 Python 中也是作为一个字符串使用。
- 转义符 '\'
- 反斜杠可以用来转义，使用r可以让反斜杠不发生转义。。 如 r"this is a line with \n" 则\n会显示，并不是换行。
- 按字面意义级联字符串，如"this " "is " "string"会被自动转换为this is string。
- 字符串可以用 + 运算符连接在一起，用 * 运算符重复。
- Python 中的字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始。
- Python中的字符串不能改变。
- 字符串的截取的语法格式如下：变量[头下标:尾下标]

```python
str='Runoob'
 
print(str)                 # 输出字符串
print(str[0:-1])           # 输出第一个到倒数第二个的所有字符
print(str[0])              # 输出字符串第一个字符
print(str[2:5])            # 输出从第三个开始到第五个的字符
print(str[2:])             # 输出从第三个开始的后的所有字符
print(str * 2)             # 输出字符串两次
print(str + '你好')        # 连接字符串
 
print('hello\nrunoob')      # 使用反斜杠(\)+n转义特殊字符
print(r'hello\nrunoob')     # 在字符串前面添加一个 r，表示原始字符串，不会发生转义
```

- 在 Python 中，字符串格式化使用与 C 中 sprintf 函数一样的语法

```python
print ("我叫 %s 今年 %d 岁!" % ('小明', 10))
```

#### tip

- 反转字符串
> s=s[::-1]

#### 字符串内建函数

| 函数 | 描述 |
| ---- | ---- |
| capitalize() | 将字符串的第一个字符转换为大写 |
| center(width,fillchar) | 返回一个指定的宽度 width 居中的字符串，fillchar 为填充的字符，默认为空格 |
| count(str,beg=0,end=len(string)) | 返回 str 在 string 里面出现的次数，如果 deg 或者 end 指定则返回指定范围内 str 出现的次数 |
| bytes.decode(encoding="utf-8",errors="strict") | Python3 中没有 decode 方法，但我们可以使用 bytes 对象的 decode() 方法来解码给定的 bytes 对象，这个 bytes 对象可以由 str.encode() 来编码返回 |
| encode(encoding="utf-8",errors="strict") | 以 encoding 指定的编码格式编码字符串，如果出错默认报一个ValueError 的异常，除非 errors 指定的是'ignore'或者'replace' |
| expandtabs(tabsize=8) | 把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8 |
| find(str,beg=0,end=len(string)) | 检测 str 是否包含在字符串中，如果指定范围 beg 和 end ，则检查是否包含在指定范围内，如果包含返回开始的索引值，否则返回-1 |
| rfind(str, beg=0,end=len(string)) | 类似于 find()函数，不过是从右边开始查找 |
| index(str,beg=0,end=len(string)) | 跟find()方法一样，只不过如果str不在字符串中会报一个异常 |
| rindex( str, beg=0, end=len(string)) | 类似于 index()，不过是从右边开始 |
| isalnum() | 如果字符串至少有一个字符并且所有字符都是字母或数字则返 回 True,否则返回 False |
| isalpha() | 如果字符串至少有一个字符并且所有字符都是字母则返回 True, 否则返回 False |
| isdigit() | 如果字符串只包含数字则返回 True 否则返回 False |
| islower() | 如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False |
| isnumeric() | 如果字符串中只包含数字字符，则返回 True，否则返回 False |
| isspace() | 如果字符串中只包含空白，则返回 True，否则返回 False |
| istitle() | 如果字符串是标题化的(见 title())则返回 True，否则返回 False |
| isupper() | 如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False |
| join(seq) | 以指定字符串作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串 |
| len(string) | 返回字符串长度 |
| lower() | 转换字符串中所有大写字符为小写 |
| upper() | 转换字符串中的小写字母为大写 |
| swapcase() | 将字符串中大写转换为小写，小写转换为大写 |
| lstrip() | 截掉字符串左边的空格或指定字符 |
| rstrip() | 删除字符串字符串末尾的空格 |
| strip([chars]) | 在字符串上执行 lstrip()和 rstrip() |
| replace(old, new [, max]) | 把 将字符串中的 str1 替换成 str2,如果 max 指定，则替换不超过 max 次 |
| rjust(width,[, fillchar])  | 返回一个原字符串右对齐,并使用fillchar(默认空格）填充至长度 width 的新字符串 |
| split(str="", num=string.count(str)) | num=string.count(str)) 以 str 为分隔符截取字符串，如果 num 有指定值，则仅截取 num 个子字符串 |
| splitlines([keepends]) | 按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符 |
| startswith(str, beg=0,end=len(string)) | 检查字符串是否是以 str 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查 |
| title() | 返回"标题化"的字符串,就是说所有单词都是以大写开始，其余字母均为小写(见 istitle()) |
| zfill (width) | 返回长度为 width 的字符串，原字符串右对齐，前面填充0 |

### List(列表)

- 列表中元素的类型可以不相同，它支持数字，字符串甚至可以包含列表（所谓嵌套）

#### 访问列表中的值

- 使用下标索引来访问列表中的值，同样也可以使用方括号的形式截取字符

```python
list = [ 'abcd', 786 , 2.23, 'runoob', 70.2 ]
tinylist = [123, 'runoob']

print (list)            # 输出完整列表
print (list[0])         # 输出列表第一个元素
print (list[1:3])       # 从第二个开始输出到第三个元素
print (list[2:])        # 输出从第三个元素开始的所有元素
print (tinylist * 2)    # 输出两次列表
print (list + tinylist) # 连接列表
```

#### 函数&方法

| 函数 | 描述 |
| ---- | ---- |
| len(list) | 返回列表元素个数 |
| max(list) | 返回列表元素最大值 |
| min(lsit) | 返回列表元素最小值 |
| list(seq) | 将元组转换为列表 |
| list.append(obj) | 在列表末尾添加新的对象 |
| list.count(obj) | 统计某个元素在列表中出现的次数 |
| list.extend(seq) | 在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表） |
| list.index(obj) | 从列表中找出某个值第一个匹配项的索引位置 |
| list.insert(index, obj) | 将对象插入列表 |
| list.pop([index=-1]) | 移除列表中的一个元素（默认最后一个元素），并且返回该元素的值 |
| list.remove(obj) | 移除列表中某个值的第一个匹配项 |
| list.reverse() | 反向列表中元素 |
| list.sort(cmp=None, key=None, reverse=False) | 对原列表进行排序 |
| list.clear() | 清空列表 |
| list.copy() | 复制列表 |

### Tuple（元组）

- 元组（tuple）与列表类似，不同之处在于元组的元素不能修改
- 元组写在小括号 () 里，元素之间用逗号隔开

#### 创建元组

- 创建空元组
> tup1=()
- 元组中只包含一个元素时，需要在元素后面添加逗号，否则括号会被当作运算符使用
> tup1=(50,)
- 元组中的元素值是不允许修改的，只可以进行连接
> tup3 = tup1 + tup2
- 元组中的元素值是不允许删除的，只可以使用 del 语句来删除整个元组
> del tup1

#### 内置函数

- len()：计算元组元素个数
- max()：返回元组中元素最大值
- min()：返回元组中元素最小值
- tuple(seq)：将列表转换为元组

### Set（集合）

- 集合（set）是一个无序不重复元素的序列
- 使用大括号 { } 或者 set() 函数创建集合，注意：创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典

```python
student = {'Tom', 'Jim', 'Mary', 'Tom', 'Jack', 'Rose'} 
print(student)   # 输出集合，重复的元素被自动去掉
 
# 成员测试
if 'Rose' in student :
    print('Rose 在集合中')
else :
    print('Rose 不在集合中')

# set可以进行集合运算
a = set('abracadabra')
b = set('alacazam')

print(a)
print(a - b)     # a和b的差集
print(a | b)     # a和b的并集
print(a & b)     # a和b的交集
print(a ^ b)     # a和b中不同时存在的元素
```

#### 基本操作

##### 添加元素

> s.add(x)
- 将元素 x 添加到集合 s 中，如果元素已存在，则不进行任何操作
> s.update(x)
- 同样可以添加元素，且参数可以是列表，元组，字典等

##### 移除元素

> s.remove(x)
- 将元素 x 添加到集合 s 中移除，如果元素不存在，则会发生错误
> s.discard(x)
- 也是移除集合中的元素，且如果元素不存在，不会发生错误
> s.pop()
- 随机删除集合中的一个元素

### Dictionary（字典）

- 列表是有序的对象集合，字典是无序的对象集合。两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取
- 字典是一种映射类型，字典用"{ }"标识，它是一个无序的键(key) : 值(value)对集合
- 键(key)必须使用不可变类型，可以用数字，字符串或元组充当，在同一个字典中，键(key)必须是唯一的
- 字典值可以是任何的 python 对象，既可以是标准的对象，也可以是用户定义的，但键不行
- dict的第一个特点是查找速度快，无论dict有10个元素还是10万个元素，查找速度都一样。而list的查找速度随着元素增加而逐渐下降
- dict的缺点是占用内存大，还会浪费很多内容，list正好相反，占用内存小

```python
dict = {}
dict['one'] = "1"
dict[2]     = "2"

tinydict = {'name': 'runoob','code':1, 'site': 'www.runoob.com'}

print (dict['one'])       # 输出键为 'one' 的值
print (dict[2])           # 输出键为 2 的值
print (tinydict)          # 输出完整的字典
print (tinydict.keys())   # 输出所有键
print (tinydict.values()) # 输出所有值
dict.clear()              # 清空字典
del dict                  # 删除字典
```

- 如果key不存在，dict就会报错：

```python
d['Thomas']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'Thomas'

# 要避免key不存在的错误，有两种办法，一是通过in判断key是否存在
'Thomas' in d   #False

# 二是通过dict提供的get()方法，如果key不存在，可以返回None，或者自己指定的value
d.get('Thomas')         # None
d.get('Thomas', -1)     # -1
```

- 构造函数 dict() 可以直接从键值对序列中构建字典

```python
dict([('Runoob', 1), ('Google', 2), ('Taobao', 3)])
# {'Taobao': 3, 'Runoob': 1, 'Google': 2}

dict(Runoob=1, Google=2, Taobao=3)
# {'Taobao': 3, 'Runoob': 1, 'Google': 2}

{x: x**2 for x in (2, 4, 6)}
# {2: 4, 4: 16, 6: 36}
```

#### 内置函数&方法

| 函数 | 描述 |
| ---- | ---- |
| len(dict) | 计算字典元素个数，即键的总数 |
| str(dict) | 输出字典，以可打印的字符串表示 |
| type(variable) | 返回输入的变量类型，如果变量是字典就返回字典类型 |
| radiansdict.clear() | 删除字典内所有元素 |
| radiansdict.copy() | 返回一个字典的浅复制 |
| radiansdict.fromkeys() | 创建一个新字典，以序列seq中元素做字典的键，val为字典所有键对应的初始值 |
| radiansdict.get(key, default=None) | 返回指定键的值，如果值不在字典中返回default值 |
| key in dict | 如果键在字典dict里返回true，否则返回false |
| radiansdict.items() | 以列表返回可遍历的(键, 值) 元组数组 |
| radiansdict.keys() | 以列表返回一个字典所有的键 |
| radiansdict.values() | 以列表返回字典中的所有值 |
| radiansdict.setdefault(key, default=None) | 和get()类似, 但如果键不存在于字典中，将会添加键并将值设为default |
| radiansdict.update(dict2) | 把字典dict2的键/值对更新到dict里 |
| pop(key[,default]) | 删除字典给定键 key 所对应的值，返回值为被删除的值。key值必须给出。 否则，返回default值 |
| popitem() | 随机返回并删除字典中的一对键和值(一般删除末尾对) |

### 数据类型转换

| 函数 | 功能 |
| --- | ---- |
| int(x) | 将 x 转换为一个整数 |
| float(x) | 将x转换到一个浮点数 |
| complex(rea[,imag]) | 创建一个复数 |
| str(x) | 将对象 x 转换为字符串 |
| repr(x) | 将对象 x 转换为表达式字符串 |
| eval(str) | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
| tuple(s) | 将序列 s 转换为一个元组 |
| list(s) | 将序列 s 转换为一个列表 |
| set(s) | 转换为可变集合 |
| dict(d) | 创建一个字典，d 必须是一个序列 (key,value)元组 |
| chr(x) | 将一个整数转换为一个字符 |
| ord(x) | 将一个字符转换为它的整数值 |
| hex(x) | 将一个整数转换为一个十六进制字符串 |
| oct(x) | 将一个整数转换为一个八进制字符串 |

## 运算符

### 算术运算符

| 运算符 | 描述 | 实例 |
| ------ | ---- | --- |
| + | 加 - 两个对象相加 | a + b 输出结果 31 |
| - | 减 - 得到负数或是一个数减去另一个数 | a - b 输出结果 -11 |
| * | 乘 - 两个数相乘或是返回一个被重复若干次的字符串 | a * b 输出结果 210 |
| / | 除 - x 除以 y | b / a 输出结果 2.1 |
| % | 取模 - 返回除法的余数 | b % a 输出结果 1 |
| ** | 幂 - 返回x的y次幂 | a**b 为10的21次方 |
| // | 取整除 - 返回商的整数部分 | 9//2 输出结果 4 , 9.0//2.0 输出结果 4.0 |

### 成员运算符

| 运算符 | 描述 | 实例 |
| ----- | ---- | ---- |
| in | 如果在指定的序列中找到值返回 True，否则返回 False | x 在 y 序列中 , 如果 x 在 y 序列中返回 True |
| not in | 如果在指定的序列中没有找到值返回 True，否则返回 False | x 不在 y 序列中 , 如果 x 不在 y 序列中返回 True |

### 身份运算符

- 身份运算符用于比较两个对象的存储单元

| 运算符 | 描述 | 实例 |
| ------ | --- | ---- |
| is | is 是判断两个标识符是不是引用自一个对象 | x is y, 类似 id(x) == id(y) , 如果引用的是同一个对象则返回 True，否则返回 False |
| is not | 	is not 是判断两个标识符是不是引用自不同对象 | x is not y ， 类似 id(a) != id(b)。如果引用的不是同一个对象则返回结果 True，否则返回 False |

## 语句

### 条件控制语句

```python
if condition_1:
    statement_block_1
elif condition_2:
    statement_block_2
else:
    statement_block_3
```

- Python 中用 elif 代替了 else if，所以if语句的关键字为：if – elif – else
- 每个条件后面要使用冒号 :，表示接下来是满足条件后要执行的语句块
- 使用缩进来划分语句块，相同缩进数的语句在一起组成一个语句块
- 在Python中没有switch – case语句

### 循环语句

#### while 循环

```python
while 判断条件：
    语句
```

- while 循环使用 else 语句

```python
count = 0
while count < 5:
   print (count, " 小于 5")
   count = count + 1
else:
   print (count, " 大于或等于 5")
```

#### for 循环

```python
for <variable> in <sequence>:
    <statements>
else:
    <statements>
```

- 如果从 for 或 while 循环中终止，任何对应的循环 else 块将不执行
- 可以使用 enumerate() 在遍历序列的同时获得元素索引值

```python
for index,item in enumerate(['a','b','c']):
    print(index,item)
```

- 可以使用 zip() 函数同时遍历两个序列

```python
a=['test1','test2']
b=['test3','test4']
for x,y in zip(a,b):
    print(x,y)

# test1,test3
# test2,test4
```

#### range() 函数

- range([beg,] end, 步进) 函数用来遍历数字序列

```python
for i in range(5):
    print(i)
0
1
2
3
4
```

### pass 语句

- Python pass是空语句，是为了保持程序结构的完整性，pass 不做任何事情，一般用做占位语句

## 函数

- 函数代码块以 def 关键词开头，后接函数标识符名称和圆括号 ()
- 函数内容以冒号起始，并且缩进
- 不带表达式的return相当于返回 None

```python
def area(width, height):
    return width * height
w = 4
h = 5
print(area(w, h))
```

### 参数

- 在定义函数 时给定的名称称作“形参”（Parameters），在调用函数时你所提供给函数的值称作“实参”（Arguments）

#### 关键字参数

- 使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值

```python
def printinfo( name, age ):
    print ("名字: ", name)
    print ("年龄: ", age)
    return
printinfo( age=50, name="runoob" )
```

- 定义命名的关键字参数在没有可变参数的情况下不要忘了写分隔符*，否则定义的将是位置参数

#### 默认参数

- 调用函数时，如果没有传递参数，则会使用默认参数

```python
def printinfo( name, age = 35 ):
    print ("名字: ", name)
    print ("年龄: ", age)
    return
printinfo( name="runoob" )
# 名字:  runoob
# 年龄:  35
```

#### 可变参数

- 可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，和上述 2 种参数不同，声明时不会命名
- 加了星号 * 的参数会以元组(tuple)的形式导入，存放所有未命名的变量参数

```python
def printinfo( arg1, *vartuple ):
    print (arg1)
    print (vartuple)
printinfo( 70, 60, 50 )
# 70
# (60, 50)
```

- 加了两个星号 ** 的参数会以字典的形式导入

```python
def printinfo( arg1, **vardict ):
    print (arg1)
    print (vardict)
printinfo(1, a=2,b=3)
# 1
# {'a': 2, 'b': 3}
```

- 如果单独出现星号 * 后的参数必须用关键字传入

```python
def f(a,b,*,c):
    return a+b+c
f(1,2,3)   # 报错
f(1,2,c=3) # 正常 6
```

### 匿名函数

- python 使用 lambda 来创建匿名函数。
- lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去
- lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数
- 虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率
- 语法：
> lambda [arg1 [,arg2,.....argn]]:expression
- 返回值就是表达式（expression）的结果

```python
sum = lambda arg1, arg2: arg1 + arg2
print ("相加后的值为 : ", sum( 10, 20 ))
# 相加后的值为 :  30
```

- 匿名函数有个好处，因为函数没有名字，不必担心函数名冲突

### 变量作用域

- 所有变量的作用域是它们被定义的块，从定义它们的名字的定义点开始
- Python的作用域一共有4种：
- L （Local） 局部作用域
- E （Enclosing） 闭包函数外的函数中
- G （Global） 全局作用域
- B （Built-in） 内建作用域
- 以 L –> E –> G –>B 的规则查找，即：在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内建中找

```python
x = int(2.9)  # 内建作用域
 
g_count = 0  # 全局作用域
def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```

- Python 中只有模块（module），类（class）以及函数（def、lambda）才会引入新的作用域
- 其它的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的
- 也就是说这些语句内定义的变量，外部也可以访问

#### 全局变量和局部变量

```python
total = 0 # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
    #返回2个参数的和."
    total = arg1 + arg2 # total在这里是局部变量.
    print ("函数内是局部变量 : ", total)
    return total
 
#调用sum函数
sum( 10, 20 )
print ("函数外是全局变量 : ", total)

# 函数内是局部变量 :  30
# 函数外是全局变量 :  0
```

#### global 和 nonlocal关键字

- 当内部作用域想修改外部作用域的变量时，需要使用 global 关键字

```python
num = 1
def fun1():
    global num  # 需要使用 global 关键字声明
    print(num) 
    num = 123
    print(num)
fun1()
print(num)

# 1
# 123
# 123
```

- 如果要修改嵌套作用域（enclosing 作用域，外层非全局作用域）中的变量则需要 nonlocal 关键字

```python
def outer():
    num = 10
    def inner():
        nonlocal num   # nonlocal关键字声明
        num = 100
        print(num)
    inner()
    print(num)
outer()

# 100
# 100
```

- 另外有一种特殊情况

```python
a = 10
def test():
    a = a + 1
    print(a)
test()
```

- 执行会报错
- 错误信息为局部作用域引用错误，因为 test 函数中的 a 使用的是局部，未定义，无法修改
- 修改 a 为全局变量，通过函数参数传递，可以正常执行

```python
a = 10
def test(a):
    a = a + 1
    print(a)
test(a)
```

### 返回多个值

- 函数可以同时返回多个值，但其实就是一个tuple

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny
```

## 高级特性

### 切片

- Python提供了切片（Slice）操作符简化取指定索引范围的操作
> L[beg:end[:步进]]
- L[0:3] 表示，从索引0开始取，直到索引3为止，但不包括索引3。即索引0，1，2，正好是3个元素
- 如果第一个索引是0，还可以省略 L[:3]
- 它也支持倒数切片 L[-1:] 取倒数第一个元素

### 列表生成式

- 如果要生成 [1x1, 2x2, 3x3, ..., 10x10]

```python
[x * x for x in range(1, 11)]
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

- 写列表生成式时，把要生成的元素放到前面，后面跟for循环
- for循环后面还可以加上if判断，可以筛选

```python
[x * x for x in range(1, 11) if x % 2 == 0]
# [4, 16, 36, 64, 100]
```

- for 循环还可以是多层的

```python
[m + n for m in 'ABC' for n in 'XYZ']
# ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

### 生成器

- 通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的
- 并且如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了
- 所以，如果列表元素可以按照某种算法推算出来，那我们就不必创建完整的list，从而节省大量的空间
- 在Python中，这种一边循环一边计算的机制，称为生成器：generator

#### 一

- 要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的[]改成()，就创建了一个generator：

```python
L = [x * x for x in range(10)]
# 直接打印出list的每一个元素
print(L)
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

g = (x * x for x in range(10))
# 通过next()函数获得generator的下一个返回值
print(next(g))  # 0
print(next(g))  # 1
print(next(g))  # 4
# ······
```

- 创建L和g的区别仅在于最外层的[]和()，L是一个list，而g是一个generator
- generator保存的是算法，每次调用next(g)，就计算出g的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误
- 我们创建了一个generator后，基本上永远不会调用next()，而是通过for循环来迭代它，并且不需要关心StopIteration的错误

```python
for n in g:
    print(n)
# 0
# 1
# 4
# 9
# 16
# ······
```

#### 二

- 定义generator的第二种方法：如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator
- 在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行

```python
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()

# 0 1 1 2 3 5 8 13 21 34 55
```

- 但是用for循环调用generator时，发现拿不到generator的return语句的返回值。
- 如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中

### 迭代器

- 可以直接作用于for循环的对象统称为可迭代对象：Iterable
- 可以使用isinstance()判断一个对象是否是Iterable对象

```python
>>> from collections import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False
```

- 可以被next()函数调用并不断返回下一个值的对象称为迭代器：Iterator
- 可以使用isinstance()判断一个对象是否是Iterator对象

```python
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance('abc', Iterator)
False
```

- 集合数据类型如 list、dict、str 等是 Iterable 但不是 Iterator，不过可以通过 iter() 函数获得一个 Iterator 对象

```python
>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True
```

- Python的Iterator对象表示的是一个数据流，Iterator对象可以被next()函数调用并不断返回下一个数据，直到没有数据时抛出StopIteration错误
- 可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过next()函数实现按需计算下一个数据
- 所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算
- Iterator甚至可以表示一个无限大的数据流

```python
import sys         # 引入 sys 模块
 
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
 
while True:
    try:
        print (next(it))
    except StopIteration:
        sys.exit()

"""
1
2
3
4
"""
```

## 函数式编程

- 函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数

### 高阶函数（Higher-order function）

#### 变量可以指向函数

```python
>>> f = abs
>>> f(-10)
10
```

- 变量 f 现在已经指向了 abs 函数本身，直接调用 abs() 函数和调用变量 f() 完全相同

#### 函数名也是变量

- 对于abs()这个函数，完全可以把函数名abs看成变量，它指向一个可以计算绝对值的函数
- 如果把abs指向其他对象

```python
>>> abs = 10
>>> abs(-10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not callable
```

- 把abs指向10后，就无法通过abs(-10)调用该函数了，因为abs这个变量已经不指向求绝对值函数而是指向一个整数10

#### 传入函数

- 既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数
- 一个最简单的高阶函数：

```python
def add(x, y, f):
    return f(x) + f(y)
```

- 当我们调用 add(-5, 6, abs) 时，参数 x，y 和 f 分别接收 -5，6 和 abs

```python
print(add(-5, 6, abs))  # 11
```

### map/reduce

- Python内建了map()和reduce()函数
- map() 函数接收两个参数，一个是函数，一个是 Iterable，map 将传入的函数依次作用到序列的每个元素，并把结果作为新的 Iterator 返回
- map() 传入的第一个参数是f，即函数对象本身
- 由于结果r是一个 Iterator，Iterator 是惰性序列，因此通过 list() 函数让它把整个序列都计算出来并返回一个 list

```python
>>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))     # 把 list 所有数字转为字符串
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

- reduce 把一个函数作用在一个序列 [x1, x2, x3, ...] 上，这个函数必须接收两个参数，reduce 把结果继续和序列的下一个元素做累积计算
> reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
- 比如要把序列[1, 3, 5, 7, 9]变换成整数13579

```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> reduce(fn, [1, 3, 5, 7, 9])
13579
```

### filter

- Python内建的filter()函数用于过滤序列
- 和map()类似，filter()也接收一个函数和一个序列
- 和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素
- 例如，在一个list中，删掉偶数，只保留奇数

```python
def is_odd(n):
    return n % 2 == 1

list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
# 结果: [1, 5, 9, 15]
```

- 用filter()这个高阶函数，关键在于正确实现一个“筛选”函数
- filter()函数返回的是一个Iterator，也就是一个惰性序列，所以要强迫filter()完成计算结果，需要用list()函数获得所有结果并返回list

### sorted（排序算法）

- Python内置的sorted()函数就可以对list进行排序：

```python
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]
```

- 此外，sorted()函数也是一个高阶函数，它还可以接收一个key函数来实现自定义的排序，例如按绝对值大小排序：

```python
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```

- 默认情况下，对字符串排序，是按照ASCII的大小比较的，由于'Z' < 'a'，结果，大写字母Z会排在小写字母a的前面

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'])
['Credit', 'Zoo', 'about', 'bob']

# 给sorted传入key函数，即可实现忽略大小写的排序
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
['about', 'bob', 'Credit', 'Zoo']
```

- 要进行反向排序，不必改动key函数，可以传入第三个参数reverse=True

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```

- 用sorted()排序的关键在于实现一个映射函数

### 返回函数

- 高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```

- 当我们调用lazy_sum()时，返回的并不是求和结果，而是求和函数
- 调用函数f时，才真正计算求和的结果

```python
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
>>> f()
25
```

- 在函数lazy_sum中又定义了函数sum，并且，内部函数sum可以引用外部函数lazy_sum的参数和局部变量
- 当lazy_sum返回函数sum时，相关参数和变量都保存在返回的函数中，这种称为“闭包（Closure）”
- 当我们调用lazy_sum()时，每次调用都会返回一个新的函数，即使传入相同的参数

```python
>>> f1 = lazy_sum(1, 3, 5, 7, 9)
>>> f2 = lazy_sum(1, 3, 5, 7, 9)
>>> f1==f2
False
# f1()和f2()的调用结果互不影响
```

#### 闭包

- 返回的函数并没有立刻执行，而是直到调用了f()才执行

```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()

>>> f1()
9
>>> f2()
9
>>> f3()
9
```

- 返回的函数引用了变量i，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量i已经变成了3，因此最终结果为9
- 返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量
- 如果一定要引用循环变量就要再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变

```python
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs

>>> f1, f2, f3 = count()
>>> f1()
1
>>> f2()
4
>>> f3()
9
```

### 装饰器

- 函数对象有一个__name__属性，可以拿到函数的名字：

```python
>>> now.__name__
'now'
>>> f.__name__
'now'
```

- 要增强now()函数的功能，比如，在函数调用前后自动打印日志，但又不希望修改now()函数的定义
- 这种在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）
- 本质上，decorator就是一个返回函数的高阶函数

### 偏函数

- 当函数的参数个数太多，需要简化时，使用 functools.partial 可以创建一个新的函数，这个新函数可以固定住原函数的部分参数，从而在调用时更简单

## 模块

- 为了编写可维护的代码，把很多函数分组，分别放到不同的文件里，这样，每个文件包含的代码就相对较少
- 在 Python 中，一个.py文件就称之为一个模块（Module）
- 使用模块还可以避免函数名和变量名冲突，相同名字的函数和变量完全可以分别存在不同的模块中
- 为了避免模块名冲突 Python 引入了按目录来组织模块的方法，称为包（Package）
- 引入了包以后，只要顶层的包名不与别人冲突，那所有模块都不会与别人冲突
- 每一个包目录下面都会有一个__init__.py的文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包
- \_\_init__.py可以是空文件，也可以有Python代码，因为 \_\_init__.py 本身就是一个模块，而它的模块名就是包名

### 使用模块

- 导入模块
> import sys
- 导入sys模块后，我们就有了变量sys指向该模块，利用sys这个变量，就可以访问sys模块的所有功能
- 一个模块只会被导入一次，不管你执行了多少次import

### import 与 from...import

- 在 python 用 import 或者 from...import 来导入相应的模块。
- 将整个模块(somemodule)导入，格式为： import somemodule
- 从某个模块中导入某个函数,格式为： from somemodule import somefunction
- 从某个模块中导入多个函数,格式为： from somemodule import firstfunc, secondfunc, thirdfunc
- 将某个模块中的全部函数导入，格式为： from somemodule import *

#### from … import

- Python 的 from 语句让你从模块中导入一个指定的部分到当前命名空间中
> from modname import name1[, name2[, ... nameN]]

#### __name__属性

- 一个模块被另一个程序第一次引入时，其主程序将运行
- 如果我们想在模块被引入时，模块中的某一程序块不执行，我们可以用__name__属性来使该程序块仅在该模块自身运行时执行
- 每个模块都有一个__name__属性，当其值是'__main__'时，表明该模块自身在运行，否则是被引入

```python
if __name__ == '__main__':
   print('程序自身在运行')
else:
   print('我来自另一模块')
```

```bash
$ python using_name.py
程序自身在运行
```

```python
>>> import using_name
我来自另一模块
>>>
```

#### dir() 函数

- 内置的函数 dir() 可以找到模块内定义的所有名称。以一个字符串列表的形式返回
- 如果没有给定参数，那么 dir() 函数会罗列出当前定义的所有名称

#### 作用域

- 有的函数和变量仅仅在模块内部使用，在Python中，是通过_前缀来实现
- 正常的函数和变量名是公开的（public），可以被直接引用
- 类似__xxx__这样的变量是特殊变量，可以被直接引用，但是有特殊用途，比如__author__，__name__就是特殊变量，模块定义的文档注释也可以用特殊变量__doc__访问
- 类似_xxx和__xxx这样的函数或变量就是非公开的（private），不应该被直接引用

### 安装第三方模块

- 在 Python 中安装第三方模块通过包管理工具 pip 完成
> pip install xxxx

#### 模块搜索路径

- 当我们试图加载一个模块时，Python会在指定的路径下搜索对应的.py文件，如果找不到，就会报错

```python
>>> import mymodule
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named mymodule
```

- 默认情况下，Python解释器会搜索当前目录、所有已安装的内置模块和第三方模块，搜索路径存放在sys模块的path变量中
- 如果我们要添加自己的搜索目录，有两种方法
- 一是直接修改sys.path，添加要搜索的目录：

```python
>>> import sys
>>> sys.path.append('/Users/michael/my_py_scripts')
```

- 这种方法是在运行时修改，运行结束后失效
- 第二种方法是设置环境变量 PYTHONPATH，该环境变量的内容会被自动添加到模块搜索路径中。设置方式与设置Path环境变量类似
- 只需要添加你自己的搜索路径，Python自己本身的搜索路径不受影响

## IO

### 读取键盘输入

- Python提供了 input() 内置函数从标准输入读入一行文本，默认的标准输入是键盘
- input 可以接收一个Python表达式作为输入，并将运算结果返回
> str = input("请输入：")
- 通过 eval() 函数同时接收多个数据输入
> a,b,c = eval(input())

### 输出

- Python两种输出值的方式: 表达式语句和 print() 函数
- 第三种方式是使用文件对象的 write() 方法，标准输出文件可以用 sys.stdout 引用
- 如果要将输出的值转成字符串，可以使用 repr() 或 str() 函数来实现
- str()： 函数返回一个用户易读的表达形式
- repr()： 产生一个解释器易读的表达形式

```python
>>> s = 'Hello, Runoob'
>>> str(s)
'Hello, Runoob'
>>> repr(s)
"'Hello, Runoob'"
>>> str(1/7)
'0.14285714285714285'
```

#### print() 函数

- 语法格式:
> print( *objects, sep='', end='/n', file=sys.stdout, flush=False)
- 把 object 中的每个对象都转化为字符串的形式，然后写道 file 参数指定的文件中，默认是标准输出（sys.stdout）
- 每一个对象之间用 sep 所指的参数进行分隔，默认是空格；所有对象都写到文件后，写入 end 参数所指字符，默认是换行

#### format() 格式化输出

- 花括号声明 {}，用于渲染前的参数引用声明。花括号里可以用数字代表引用参数的序号，或者变量名直接引用
- 从 format 参数引入的变量名
- 冒号（:）后面带填充的字符，只能是一个字符，默认使用空格填充
- 字符位数声明，用数字表示

```python
import math
#括号及其里面的字符 (称作格式化字段) 将会被 format() 中的参数替换。
print('We are the {} who say "{}!"'.format('knights', 'Ni'))
#在括号中的数字用于指向传入对象在 format() 中的位置
print('{0} and {1}'.format('chicken', 'eggs'))
print('{1} and {0}'.format('chicken', 'eggs'))
#如果在 format() 中使用了关键字参数, 那么它们的值会指向使用该名字的参数。
print('This {food} is {adjective}.'.format(\
food='milk', adjective='absolutely horrible'))  
#位置及关键字参数可以任意的结合
print('The story of {0}, {1}, and {other}.'.\
format('Bill', 'John',other='Dan'))
#可选项 ':' 和格式标识符可以跟着字段名。 这就允许对值进行更好的格式化。 
#下面的例子将 Pi 保留到小数点后三位： 
print('The value of PI is approximately {0:.3f}.'.format(math.pi))
#在 ':' 后传入一个整数, 可以保证该域至少有这么多的宽度。 用于美化表格时很有用

print('{0:10} ==> {1:10d}'.format("Bill",8752))
# :冒号+空白填充+右对齐+固定宽度18+浮点精度.2+浮点数声明f
print('{:>18,.2f}'.format(76305784.0))
# 又对齐，使用空格填充
print('{:>8}'.format('286'))
# 又对齐，使用0填充
print('{:0>8}'.format('286'))
# 又对齐，使用*填充
print('{:*>8}'.format('286'))
# 用不同的进制输出数据
print('二进制输出{:b}'.format(17))
print('千分位输出{:,}'.format(1234567890))
# 铜鼓关键字输出
print('{name},{cardNo}'.format(cardNo=10012001,name='cat') )
# 输出正负号
print('{:+f}; {:+f}'.format(25.168, -98.705))
print('The rate is: {:.2%}'.format(0.7892))

fruit = ('apple','peach','orange')
# 通过下表匹配参数
print('fruit: {0[2]};  {0[0]}'.format(fruit))
```

### 读取键盘输入

- Python提供了 input() 内置函数从标准输入读入一行文本，默认的标准输入是键盘
- input 可以接收一个Python表达式作为输入，并将运算结果返回
> str = input("请输入：")
- 通过 eval() 函数同时接收多个数据输入
> a,b,c = eval(input())

### 文件读写

#### 读文件

- 要以读文件的模式打开一个文件对象，使用Python内置的open()函数，传入文件名和标示符
> open(filename, mode)
- filename：包含了你要访问的文件名称的字符串值
- mode：决定了打开文件的模式：只读，写入，追加等。所有可取值见如下的完全列表。这个参数是非强制的，默认文件访问模式为只读(r)

```python
>>> f = open('/Users/michael/test.txt', 'r')    # 标示符'r'表示读

# 如果文件不存在，open()函数就会抛出一个IOError的错误，并且给出错误码和详细的信息告诉你文件不存在
>>> f=open('/Users/michael/notfound.txt', 'r')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [Errno 2] No such file or directory: '/Users/michael/notfound.txt'

# 如果文件打开成功，调用 read() 方法可以一次读取文件的全部内容，Python 把内容读到内存，用一个 str 对象表示
>>> f.read()
'Hello, world!'

# 最后一步是调用close()方法关闭文件
>>> f.close()
```

- 文件使用完毕后必须关闭，因为文件对象会占用操作系统的资源，并且操作系统同一时间能打开的文件数量也是有限的
- 由于文件读写时都有可能产生IOError，一旦出错，后面的f.close()就不会调用
- 所以，为了保证无论是否出错都能正确地关闭文件，我们可以使用try ... finally来实现：

```python
try:
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```

- 但是每次都这么写实在太繁琐，所以，Python引入了with语句来自动帮我们调用close()方法：

```python
with open('/path/to/file', 'r') as f:
    print(f.read())
```

- 调用read()会一次性读取文件的全部内容，如果文件过大（大于内存），内存就爆了
- 保险起见，可以反复调用read(size)方法，每次最多读取size个字节的内容
- 调用 readline() 可以每次读取一行内容，调用 readlines() 一次读取所有内容并按行返回list

```python
for line in f.readlines():
    print(line.strip()) # 把末尾的'\n'删掉
```

#### file-like Object

- 像open()函数返回的这种有个read()方法的对象，在Python中统称为file-like Object
- 除了file外，还可以是内存的字节流，网络流，自定义流等等。file-like Object不要求从特定类继承，只要写个read()方法就行
- StringIO就是在内存中创建的file-like Object，常用作临时缓冲

#### 二进制文件

- 默认是读取文本文件，并且是UTF-8编码的文本文件。要读取二进制文件，比如图片、视频等等，用'rb'模式打开文件即可

```python
>>> f = open('/Users/michael/test.jpg', 'rb')
>>> f.read()
b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节
```

#### 字符编码

- 要读取非UTF-8编码的文本文件，需要给open()函数传入encoding参数，例如，读取GBK编码的文件

```python
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
>>> f.read()
'测试'
```

- 遇到有些编码不规范的文件，你可能会遇到UnicodeDecodeError，因为在文本文件中可能夹杂了一些非法编码的字符
- 遇到这种情况，open()函数还接收一个errors参数，表示如果遇到编码错误后如何处理。最简单的方式是直接忽略
> >>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')

### 写文件

- 写文件和读文件是一样的，唯一区别是调用open()函数时，传入标识符'w'或者'wb'表示写文本文件或写二进制文件

```python
>>> f = open('/Users/michael/test.txt', 'w')
>>> f.write('Hello, world!')
>>> f.close()
```

- f.write(string) 将 string 写入到文件中, 然后返回写入的字符数
- 可以反复调用write()来写入文件，但是务必要调用f.close()来关闭文件
- 当我们写文件时，操作系统往往不会立刻把数据写入磁盘，而是放到内存缓存起来，空闲的时候再慢慢写入
- 只有调用close()方法时，操作系统才保证把没有写入的数据全部写入磁盘。忘记调用close()的后果是数据可能只写了一部分到磁盘，剩下的丢失了
- 所以，还是用with语句来得保险：

```python
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
```

- 要写入特定编码的文本文件，请给open()函数传入encoding参数，将字符串自动转换成指定编码
- 以'w'模式写入文件时，如果文件已存在，会直接覆盖（相当于删掉后新写入一个文件）
- 如果希望追加到文件末尾，可以传入'a'以追加（append）模式写入

#### 常用函数

| 函数 | 描述 |
| ---- | ---- |
| file.close() | 关闭文件 |
| file.flush() | 刷新文件内部缓冲，直接把内部缓冲区的数据立刻写入文件, 而不是被动的等待输出缓冲区写入 |
| file.fileno() | 返回一个整型的文件描述符(file descriptor FD 整型), 可以用在如os模块的read方法等一些底层操作上 |
| file.isatty() | 如果文件连接到一个终端设备返回 True，否则返回 False |
| file.next() | 返回文件下一行 |
| file.read([size]) | 从文件读取指定的字节数，如果未给定或为负则读取所有 |
| file.readline([size]) | 读取整行，包括 "\n" 字符 |
| file.readlines([sizeint]) | 读取所有行并返回列表，若给定sizeint>0，返回总和大约为sizeint字节的行, 实际读取值可能比 sizeint 较大, 因为需要填充缓冲区 |
| file.seek(offset[, whence]) | 设置文件当前位置 |
| file.tell() | 返回文件当前位置 |
| file.truncate([size]) | 从文件的首行首字符开始截断，截断文件为 size 个字符，无 size 表示从当前位置截断；截断之后后面的所有字符被删除，其中 Widnows 系统下的换行代表2个字符大小 |
| file.write(str) | 将字符串写入文件，返回的是写入的字符长度 |
| file.writelines(sequence) | 向文件写入一个序列字符串列表，如果需要换行则要自己加入每行的换行符 |

### StringIO和BytesIO

### StringIO

- StringIO顾名思义就是在内存中读写str
- 要把str写入StringIO，我们需要先创建一个StringIO，然后，像文件一样写入即可

```python
>>> from io import StringIO
>>> f = StringIO()
>>> f.write('hello')
5
>>> f.write(' ')
1
>>> f.write('world!')
6
>>> print(f.getvalue()) # getvalue()方法用于获得写入后的str
hello world!
```

- 要读取StringIO，可以用一个str初始化StringIO，然后，像读文件一样读取

```python
>>> from io import StringIO
>>> f = StringIO('Hello!\nHi!\nGoodbye!')
>>> while True:
...     s = f.readline()
...     if s == '':
...         break
...     print(s.strip())
...
Hello!
Hi!
Goodbye!
```

### BytesIO

- StringIO操作的只能是str，如果要操作二进制数据，就需要使用BytesIO
- BytesIO实现了在内存中读写bytes，我们创建一个BytesIO，然后写入一些bytes

```python
>>> from io import BytesIO
>>> f = BytesIO()
>>> f.write('中文'.encode('utf-8'))
6
>>> print(f.getvalue())
b'\xe4\xb8\xad\xe6\x96\x87'
```

- 写入的不是str，而是经过UTF-8编码的bytes
- 和StringIO类似，可以用一个bytes初始化BytesIO，然后，像读文件一样读取

```python
>>> from io import BytesIO
>>> f = BytesIO(b'\xe4\xb8\xad\xe6\x96\x87')
>>> f.read()
b'\xe4\xb8\xad\xe6\x96\x87'
```

### 操作文件和目录

- Python内置的os模块也可以直接调用操作系统提供的接口函数

```python
>>> import os
>>> os.name # 操作系统类型
'posix'
```

- 如果是posix，说明系统是Linux、Unix或Mac OS X，如果是nt，就是Windows系统
- 要获取详细的系统信息，可以调用uname()函数

```python
>>> os.uname()
posix.uname_result(sysname='Darwin', nodename='MichaelMacPro.local', release='14.3.0', version='Darwin Kernel Version 14.3.0: Mon Mar 23 11:59:05 PDT 2015; root:xnu-2782.20.48~5/RELEASE_X86_64', machine='x86_64')
```

- uname()函数在Windows上不提供，也就是说，os模块的某些函数是跟操作系统相关的

#### 环境变量

- 在操作系统中定义的环境变量，全部保存在os.environ这个变量中，可以直接查看

```python
>>> os.environ
environ({'VERSIONER_PYTHON_PREFER_32_BIT': 'no', 'TERM_PROGRAM_VERSION': '326', 'LOGNAME': 'michael', 'USER': 'michael', 'PATH': '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/mysql/bin', ...})
```

- 要获取某个环境变量的值，可以调用 os.environ.get('key')

```python
>>> os.environ.get('PATH')
'/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/mysql/bin'
>>> os.environ.get('x', 'default')
'default'
```

#### 操作文件和目录

- 操作文件和目录的函数一部分放在os模块中，一部分放在os.path模块中

```python
# 查看当前目录的绝对路径:
>>> os.path.abspath('.')
'/Users/michael'
# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
>>> os.path.join('/Users/michael', 'testdir')
'/Users/michael/testdir'
# 然后创建一个目录:
>>> os.mkdir('/Users/michael/testdir')
# 删掉一个目录:
>>> os.rmdir('/Users/michael/testdir')
```

- 把两个路径合成一个时，不要直接拼字符串，而要通过os.path.join()函数，这样可以正确处理不同操作系统的路径分隔符
- 在Linux/Unix/Mac下，os.path.join()返回这样的字符串
> part-1/part-2
- 而Windows下会返回这样的字符串
> part-1\part-2
- 同样的道理，要拆分路径时，也不要直接去拆字符串，而要通过os.path.split()函数，这样可以把一个路径拆分为两部分，后一部分总是最后级别的目录或文件名

```python
>>> os.path.split('/Users/michael/testdir/file.txt')
('/Users/michael/testdir', 'file.txt')
```

- os.path.splitext()可以直接让你得到文件扩展名

```python
>>> os.path.splitext('/path/to/file.txt')
('/path/to/file', '.txt')
```

- 这些合并、拆分路径的函数并不要求目录和文件要真实存在，它们只对字符串进行操作
- 文件操作使用下面的函数。假定当前目录下有一个test.txt文件

```python
# 对文件重命名:
>>> os.rename('test.txt', 'test.py')
# 删掉文件:
>>> os.remove('test.py')
```

- 列出当前目录下的所有目录
> [x for x in os.listdir('.') if os.path.isdir(x)]
- 列出所有的.py文件
> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']

#### 常用函数

| 函数 | 描述 |
| ---- | ---- |
| os.access(path, mode) | 检验权限模式 |
| os.chdir(path) | 改变当前工作目录 |
| os.chmod(path, mode) | 更改权限 |
| os.chowm(path, uid, gid) | 更改文件所有者 |
| os.close(fd) | 关闭文件描述符 fd |
| os.getcwd() | 返回当前工作目录 |
| os.mkdir(path[, mode]) | 以数字mode的mode创建一个名为path的文件夹.默认的 mode 是 0777 (八进制) |
| os.makedirs(path[, mode]) | 递归文件夹创建函数。像mkdir(), 但创建的所有intermediate-level文件夹需要包含子文件夹 |
| os.open(file, flags[, mode]) | 打开一个文件，并且设置需要的打开选项，mode参数是可选的 |
| os.pathconf(path,name) | 返回相关文件的系统配置信息 |
| os.read(fd, n) | 从文件描述符 fd 中读取最多 n 个字节，返回包含读取字节的字符串，文件描述符 fd对应文件已达到结尾, 返回一个空字符串 |
| os.remove(path) | 删除路径为path的文件。如果path 是一个文件夹，将抛出OSError |
| os.removedirs(path) | 递归删除目录 |
| os.rename(src, dst) | 重命名文件或目录，从 src 到 dst |
| os.renames(old, new) | 递归地对目录进行更名，也可以对文件进行更名 |
| os.rmdir(path) | 删除path指定的空目录，如果目录非空，则抛出一个OSError异常 |
| os.stat(path) | 获取path指定的路径的信息，功能等同于C API中的stat()系统调用 |
| os.write(fd, str) | 写入字符串到文件描述符 fd中. 返回实际写入的字符串长度 |

### 序列化

- 把变量从内存中变成可存储或传输的过程称之为序列化，在Python中叫pickling
- 序列化之后，就可以把序列化后的内容写入磁盘，或者通过网络传输到别的机器上
- 反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，即unpickling
- Python提供了pickle模块来实现序列化

#### 一个对象序列化并写入文件

- Python 提供了一个叫作	 Pickle 的标准模块，通过它你可以将任何纯 Python 对象存储到一个文件中，并在稍后将其取回。这叫作持久地（Persistently）存储对象。

```python
>>> import pickle
>>> d = dict(name='Bob', age=20, score=88)
>>> pickle.dumps(d)
b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'
```

- pickle.dumps()方法把任意对象序列化成一个bytes，然后，就可以把这个bytes写入文件
- 或者用另一个方法pickle.dump()直接把对象序列化后写入一个file-like Object：

```python
>>> f = open('dump.txt', 'wb')
>>> pickle.dump(d, f)
>>> f.close()
```

- 要想将一个对象存储到一个文件中，我们首先需要通过 open 以写入（write）二进制（binary）模式打开文件，然后调用 pickle 模块的 dump 函数。这一过程被称作封装（Pickling）
- 当我们要把对象从磁盘读到内存时，可以先把内容读到一个 bytes，然后通过 pickle 模块的 load 函数接收返回的对象。这个过程被称作拆封（Unpickling）
- 也可以直接用 pickle.load() 方法从一个 file-like Object 中直接反序列化出对象

```python
>>> f = open('dump.txt', 'rb')
>>> d = pickle.load(f)
>>> f.close()
>>> d
{'age': 20, 'score': 88, 'name': 'Bob'}
```

#### JSON

- JSON表示的对象就是标准的 JavaScript 语言的对象，JSON 和 Python 内置的数据类型对应如下

| JSON类型 | Python类型 |
| -------- | ---------- |
| {} | dict |
| [] | list |
| "string" | str |
| 1234.56 | int或float |
| true/false | True/False |
| null | None |

- Python内置的json模块提供了非常完善的Python对象到JSON格式的转换

```python
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```

- dumps()方法返回一个str，内容就是标准的JSON，dump()方法可以直接把JSON写入一个file-like Object
- 要把JSON反序列化为Python对象，用loads()或者对应的load()方法，前者把JSON的字符串反序列化，后者从file-like Object中读取字符串并反序列化

```python
>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}
```

- 由于JSON标准规定JSON编码是UTF-8，所以我们总是能正确地在Python的str与JSON的字符串之间转换
- Python的dict对象可以直接序列化为JSON的{}，不过，很多时候，我们更喜欢用class表示对象，比如定义Student类，然后序列化

```python
import json

class Student(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score

s = Student('Bob', 20, 88)
print(json.dumps(s))
```

- 默认情况下，dumps()方法不知道如何将Student实例变为一个JSON的{}对象
- 可选参数default就是把任意一个对象变成一个可序列为JSON的对象，我们只需要为Student专门写一个转换函数

```python
def student2dict(std):
    return {
        'name': std.name,
        'age': std.age,
        'score': std.score
    }
```

- 这样，Student实例首先被student2dict()函数转换成dict，然后再被顺利序列化为JSON

```python
>>> print(json.dumps(s, default=student2dict))
{"age": 20, "name": "Bob", "score": 88}
```

- 把任意class的实例变为dict：
> print(json.dumps(s, default=lambda obj: obj.__dict__))
- 因为通常class的实例都有一个__dict__属性，它就是一个dict，用来存储实例变量
- 同样的道理，如果要把JSON反序列化为一个Student对象实例，loads()方法首先转换出一个dict对象，然后，我们传入的object_hook函数负责把dict转换为Student实例：

```python
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])


>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> print(json.loads(json_str, object_hook=dict2student))
<__main__.Student object at 0x10cd3c190>
# 打印出的是反序列化的Student实例对象
```

## 异常

- 当程序出现例外情况时就会发生异常（Exception）

### 处理异常

- 可以通过使用 try..except 来处理异常状况。一般来说我们会把通常的语句放在 try 代码块中，将我们的错误处理器代码放置在 except 代码块中

```python
try:
    text = input('Enter	something --> ')
except EOFError:
    print('Why did you do an EOF on me?')
except KeyboardInterrupt:
    print('You cancelled the operation.')
else:
    print('You entered {}'.format(text))
```

- 将所有可能引发异常或错误的语句放在 try 代码块中，并将相应的错误或异常的处理器（Handler）放在 except 子句或代码块中
- except 子句可以处理某种特定的错误或异常，或者是一个在括号中列出的错误或异常。如果没有提供错误或异常的名称，它将处理所有错误与异常。
- 如果没有任何错误或异常被处理，那么将调用 Python 默认处理器，它只会终端程序执行并打印出错误信息
- 还可以拥有一个 else 子句与 try..except 代码块相关联。else 子句将在没有发生异常的时候执行

### 抛出异常

-  可以通过 raise 语句来引发一次异常，具体方法是提供错误名或异常名以及要抛出（Thrown）异常的对象
- 能够引发的错误或异常必须是直接或间接从属于 Exception（异常）类的派生类

```python
#	encoding=UTF-8
class ShortInputException(Exception):
    '''一个由用户定义的异常类'''
    def __init__(self, length, atleast):
        Exception.__init__(self)
        self.length = length
        self.atleast = atleast
try:
    text = input('Enter something --> ')
    if len(text) < 3:
    raise ShortInputException(len(text), 3)
    # 其他工作能在此处继续正常运行
except EOFError:
    print('Why did you do an EOF on me?')
except ShortInputException as ex:
    print(('ShortInputException: The input was' + '{0} long, expected at least {1}').format(ex.length, ex.atleast))
else:
    print('No exception was raised.')
```

- 我们创建了我们自己的异常类型。这一新的异常类型叫作 ShortInputException 。它包含两个字段——获取给定输入文本长度的 length，程序期望的最小长度 atleast
- 在 except 子句中，我们提及了错误类，将该类存储 as（为）相应的错误名或异常名。这类似于函数调用中的形参与实参。在这个特殊的 except 子句中我们使用异常对象的 length 与 atlease 字段来向用户打印一条合适的信息

### Try ... Finally

- 可以通过 finally 块来确保文件对象被正确关闭

```python
import time
f = None
try:
    f = open("poem.txt")
    	while True:
            line = f.readline()
            if len(line) == 0:
                break
            print(line, end='')
except IOError:
    print("Could not find file poem.txt")
except KeyboardInterrupt:
    print("!! You cancelled the reading from the file.")
finally:
    if f:
        f.close()
    print("(Cleaning up: Closed the file)")
```

- 在程序退出之前，finally 子句得到执行，文件对象总会被关闭

### with 语句

- 在 try 块中获取资源，然后在 finally 块中释放资源是一种常见的模式
- 还有一个 with 语句使得这一过程可以以一种干净的姿态得以完成

```python
with open("poem.txt") as f:
    for line in f:
        print(line, end='')
```

- 程序输出的内容应与上一个案例所呈现的相同。本例的不同之处在于我们使用的是 open 函数与 with 语句——我们将关闭文件的操作交由 with open 来自动完成
- 在幕后发生的事情是有一项 with 语句所使用的协议（Protocol）。它会获取由 open 语句返回的对象，在本案例中就是“thefile”
- 它总会在代码块开始之前调用 thefile.__enter__ 函数，并且总会在代码块执行完毕之后调用 thefile.__exit__
- 因此，我们在 finally 代码块中编写的代码应该格外留心 __exit__ 方法的自动操作。这能够帮助我们避免重复显式使用 try..finally 语句

## 标准库

- Python 标准库（Python Standrad Library）中包含了大量有用的模块，同时也是每个标准的 Python 安装包中的一部分

### sys 模块

- sys 模块包括了一些针对特定系统的功能

### 日志模块

- 如果想将一些调试（Debugging）信息或一些重要的信息储存在某个地方，以便你可以检查程序是否如期望那般运行，这可以通过 logging 模块来实现

```python
import os
import platform
import logging
if platform.platform().startswith('Windows'):
    logging_file = os.path.join(os.getenv('HOMEDRIVE'), os.getenv('HOMEPATH'), 'test.log')
else:
    logging_file = os.path.join(os.getenv('HOME'), 'test.log')
print("Logging to", logging_file)
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s : %(levelname)s : %(message)s',
    filename=logging_file,
    filemode='w')

logging.debug("Start of the program")
logging.info("Doing something")
logging.warning("Dying now")
```

- 输出：

```bash
$ python stdlib_logging.py Logging to
/Users/swa/test.log

$ cat /Users/swa/test.log
2014-03-29 09:27:36,660 : DEBUG : Start of the program
2014-03-29 09:27:36,660 : INFO : Doing something
2014-03-29 09:27:36,660 : WARNING : Dying now
```

- 我们使用了三款标准库中的模块—— os 模块用以和操作系统交互 platform 模块用以获取平台——操作系统——的信息， logging 模块用来记录（Log）信息
- 首先，我们通过检查 platform.platform() 返回的字符串来确认我们正在使用的操作系统（有关更多信息，请参阅 import platform;help(platform)）。如果它是 Windows，我们将找出其主驱动器（Home Drive），主文件夹（Home Folder）以及我们希望存储信息的文件名。将这三个部分汇聚到一起，我们得到了有关文件的全部位置信息。对于其它平台而言，我们需要知道的只是用户的主文件夹位置，这样我们就可获得文件的全部位置信息
- 我们使用 os.path.join() 函数来将这三部分位置信息聚合到一起。使用这一特殊函数，而非仅仅将这几段字符串拼凑在一起的原因是这个函数会确保完整的位置路径符合当前操作系统的预期格式
- 然后我们配置 logging 模块，让它以特定的格式将所有信息写入我们指定的文件
- 最后，无论这些信息是用以调试，提醒，警告甚至是其它关键的消息，我们都可以将其聚合并记录。一旦程序开始运行，我们可以检查这一文件，从而我们便能知道程序运行过程中究竟发生了什么，哪怕在用户运行时什么信息都没有显示

## 进程和线程

### 多进程

- Unix/Linux 操作系统提供了一个 fork() 系统调用，fork() 调用一次，返回两次，因为操作系统自动把当前进程（称为父进程）复制了一份（称为子进程），然后，分别在父进程和子进程内返回
- 子进程永远返回0，而父进程返回子进程的ID。一个父进程可以fork出很多子进程，所以，父进程要记下每个子进程的ID，而子进程只需要调用getppid()就可以拿到父进程的ID
- Python 的 os 模块封装了常见的系统调用，其中就包括 fork，可以在Python程序中创建子进程

```python
import os

print('Process (%s) start...' % os.getpid())
# Only works on Unix/Linux/Mac:
pid = os.fork()
if pid == 0:
    print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid()))
else:
    print('I (%s) just created a child process (%s).' % (os.getpid(), pid))
# 运行结果
Process (876) start...
I (876) just created a child process (877).
I am child process (877) and my parent is 876.
```

- 由于 Windows 没有 fork 调用，上面的代码在 Windows 上无法运行

#### multiprocessing

- multiprocessing 模块是跨平台版本的多进程模块
- multiprocessing 模块提供了一个 Process 类来代表一个进程对象，下面的例子演示了启动一个子进程并等待其结束：

```python
from multiprocessing import Process
import os

# 子进程要执行的代码
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('Child process will start.')
    p.start()
    p.join()
    print('Child process end.')

# 执行结果：
Parent process 928.
Process will start.
Run child process test (929)...
Process end.
```

- 创建子进程时，只需要传入一个执行函数和函数的参数，创建一个 Process 实例，用 start() 方法启动
- join() 方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步

#### Pool

- 如果要启动大量的子进程，可以用进程池的方式批量创建子进程：

```python
from multiprocessing import Pool
import os, time, random

def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        p.apply_async(long_time_task, args=(i,))
    print('Waiting for all subprocesses done...')
    p.close()
    p.join()
    print('All subprocesses done.')

# 执行结果如下：
Parent process 669.
Waiting for all subprocesses done...
Run task 0 (671)...
Run task 1 (672)...
Run task 2 (673)...
Run task 3 (674)...
Task 2 runs 0.14 seconds.
Run task 4 (673)...
Task 1 runs 0.27 seconds.
Task 3 runs 0.86 seconds.
Task 0 runs 1.41 seconds.
Task 4 runs 1.91 seconds.
All subprocesses done.
```

- 对 Pool 对象调用 join() 方法会等待所有子进程执行完毕，调用 join() 之前必须先调用 close()，调用 close() 之后不能继续添加新的 
- task 0，1，2，3 是立刻执行的，而 task 4 要等待前面某个task完成后才执行，因为 Pool 的默认大小是 CPU 的核数，因此，最多同时执行4个进程。这是 Pool 有意设计的限制，并不是操作系统的限制
- 如果改成：
> p = Pool(5)
- 就可以同时跑5个进程

#### 子进程

- 很多时候，子进程并不是自身，而是一个外部进程。我们创建了子进程后，还需要控制子进程的输入和输出
- subprocess模块可以非常方便地启动一个子进程，然后控制其输入和输出

### 多线程

- 多任务可以由多进程完成，也可以由一个进程内的多线程完成
- Python 的线程是真正的 Posix Thread，而不是模拟出来的线程
- Python 的标准库提供了两个模块：_thread 和 threading，_thread 是低级模块，threading 是高级模块，对 _thread 进行了封装
- 启动一个线程就是把一个函数传入并创建 Thread 实例，然后调用 start() 开始执行：

```python
import time, threading

# 新线程执行的代码:
def loop():
    print('thread %s is running...' % threading.current_thread().name)
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s ended.' % threading.current_thread().name)

print('thread %s is running...' % threading.current_thread().name)
t = threading.Thread(target=loop, name='LoopThread')
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name)

# 执行结果如下：
thread MainThread is running...
thread LoopThread is running...
thread LoopThread >>> 1
thread LoopThread >>> 2
thread LoopThread >>> 3
thread LoopThread >>> 4
thread LoopThread >>> 5
thread LoopThread ended.
thread MainThread ended.
```

- 由于任何进程默认就会启动一个线程，我们把该线程称为主线程，主线程又可以启动新的线程，Python 的 threading 模块有个 current_thread() 函数，它永远返回当前线程的实例
- 主线程实例的名字叫 MainThread，子线程的名字在创建时指定，我们用 LoopThread 命名子线程
- 名字仅仅在打印时用来显示，完全没有其他意义，如果不起名字 Python 就自动给线程命名为 Thread-1，Thread-2……

#### Lock

- 多线程和多进程最大的不同在于，多进程中，同一个变量，各自有一份拷贝存在于每个进程中，互不影响，而多线程中，所有变量都由所有线程共享，所以，任何一个变量都可以被任何一个线程修改，因此，线程之间共享数据最大的危险在于多个线程同时改一个变量
- 创建一个锁就是通过 threading.Lock() 来实现：

```python
balance = 0
lock = threading.Lock()

def run_thread(n):
    for i in range(100000):
        # 先要获取锁:
        lock.acquire()
        try:
            # 放心地改吧:
            change_it(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()
```

- 当多个线程同时执行 lock.acquire() 时，只有一个线程能成功地获取锁，然后继续执行代码，其他线程就继续等待直到获得锁为止

## 网络编程

### TCP/IP简介

- TCP 是目前网络通信中使用的协议集合，在一次通信过程中，需要把信息打包处理后进行传送；在接收方重新组合成原始信息。
- 在这个过程中，TCP 需要识别远程机器和进行通信的程序，通过 IP 地址和服务器端口号完成。
- 为保证数据传输的可靠性，在发送到信息包中应包含校验码，一旦接收方发现校验码不正确，则丢弃该数据包；如果收到正确的信息包，需要发送反馈信息。发送方对于没有收到反馈信息的包，会重新发送
- IP地址对应的实际上是计算机的网络接口，通常是网卡，IP 协议负责把数据从一台计算机通过网络发送到另一台计算机。数据被分割成一小块一小块，然后通过 IP 包发送出去
- 路由器就负责决定如何把一个IP包转发出去。IP 包的特点是按块发送，途径多个路由，但不保证能到达，也不保证顺序到达
- TCP 协议则是建立在 IP 协议之上的。TCP 协议负责在两台计算机之间建立可靠连接，保证数据包按顺序到达。TCP 协议会通过握手建立连接，然后，对每个IP包编号，确保对方按顺序收到，如果包丢掉了，就自动重发
- 许多常用的更高级的协议都是建立在 TCP 协议基础上的，比如用于浏览器的 HTTP 协议、发送邮件的 SMTP 协议等

### socket 模块

- socket 套接字是一个通信端点，操作系统使用整数来标识套接字，而 Python 则使用 socket.socket 对象来表示套接字。该对象内部维护了操作系统标识套接字的整数（可以调用它的 fileno() 方法来查看）。每当调用 socket.socket 对象的方法请求使用该套接字的系统调用时，该对象都会自动使用内部维护的套接字整数标识符。
- 套接字 socket 包括两个：客户端 socket 和服务器端 socket。服务器端 socket 创建后，处于等待连接状态，并在其服务端口进行监听，一旦发现有客户端 socket 的连接请求，立即进行连接操作，连接成功后双方就可以通信了






### UDP（用户数据报协议）

- TCP是建立可靠连接，并且通信双方都可以以流的形式发送数据。相对TCP，UDP则是面向无连接的协议
- 使用UDP协议时，不需要建立连接，只需要知道对方的IP地址和端口号，就可以直接发数据包
- 虽然用UDP传输数据不可靠，但它的优点是和TCP比，速度快，对于不要求可靠到达的数据，就可以使用UDP协议

```python
# 使用自环接口的 UDP 服务器和客户端

import argparse, socket
from datetime import datetime

MAX_BYTES = 65535

def server(port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    sock.bind(('127.0.0.1', port))
    print('Listening at {}'.format(sock.getsockname()))
    while True:
        data, address = sock.recvfrom(MAX_BYTES)
        text = data.decode('ascii')
        print('The client at {} says {!r}'.format(address, text))
        text = 'Your data was {} bytes long'.format(len(data))
        data = text.encode('ascii')
        sock.sendto(data, address)

def client(port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    text = 'The time is {}'.format(datetime.now())
    data = text.encode('ascii')
    sock.sendto(data, ('127.0.0.1', port))
    print('The OS assigned me the address {}'.format(sock.getsockname()))
    data, address = sock.recvfrom(MAX_BYTES)  # Danger! See Chapter 2
    text = data.decode('ascii')
    print('The server {} replied {!r}'.format(address, text))

if __name__ == '__main__':
    choices = {'client': client, 'server': server}
    parser = argparse.ArgumentParser(description='Send and receive UDP locally')
    parser.add_argument('role', choices=choices, help='which role to play')
    parser.add_argument('-p', metavar='PORT', type=int, default=1060,
                        help='UDP port (default 1060)')
    args = parser.parse_args()
    function = choices[args.role]
    function(args.p)
```

- 服务器启动和运行的过程经历了三步：
1. 服务器使用 socket() 调用创建了一个空套接字。这个新创建的套接字没有与任何 IP 地址或端口号绑定，也没有进行任何连接。如果此时就尝试使用其进行任何通信操作，将会抛出一个异常。这个套接字还标记了所属的特定类别：协议族 AF_INET 以及数据报类型 SOCK_DGRAM。
    - SOCK_DGRAM 表示在 IP 网络上使用 UDP 协议。数据报（datagram）[而不是数据包(packet)]是用来表示应用层数据块传输的官方术语
1. 接着，服务器使用 bind() 命令请求绑定一个 UDP 网络地址，这个网络地址是一个 Python 二元组，包含了一个 IP 地址字符串（也可以使用主机名）和一个整型的 UDP 端口号（可以使用 socket 对象的 getsockname() 方法来获取包含该信息的二元组）
    - 一旦套接字成功绑定，服务器就准备好开始接收请求了。服务器会进入一个循环，不断运行 recvfrom()。recvfrom(MAX_BYTES) 表示可接收最长为 65535 字节的信息，这也是一个 UDP 数据报可以包含的最大长度。因此，服务器将接收每个数据报的完整内容。在没有收到客户端发送的请求信息前，recvfrom() 将永远保持等待
    - 一旦接收到一个数据报，recvfrom() 就会返回两个值。第一个是发送该数据报的客户端地址，第二个是以字节表示的数据报内容。使用 Python 提供的支持直接将字节转换为字符串，接着向客户端返回一个响应数据报




#### 连接 UDP 套接字

- 如果使用 sendto()，那么每次向服务器发送信息的时候都必须显式地给出服务器的 IP 地址和端口
- 而如果使用 connect() 调用，那么操作系统事先就已经知道数据包要发送到的远程地址，这样就可以简单地把要发送的数据作为参数传入 send() 调用，而无需重复给出服务器地址
- 如果要对响应数据包返回地址进行仔细检查，有两种方法：
    1. 使用 sendto() 指定每个数据包的目标地址，然后使用 recvfrom() 接收响应，并检查响应数据包的返回地址
    1. 在创建了套接字后使用 connect() 将其与目标地址连接，然后使用 send() 和 recv() 进行通信。操作系统会将不需要的数据包过滤掉。这种做法只支持同时与一台服务器交互的情况，因为在同一套接字上重复运行 connect() 不会增加目标地址，反而会将之前的地址覆盖
- 在使用 connect() 连接了一个 UDP 套接字后，可以使用 getpeername() 方法得到所连接的地址。如果在尚未连接的套接字上调用此方法会抛出一个 socket.error
- 使用 connect() 连接 UDP 套接字，没有在网络上传输任何信息，connect() 只是简单地将连接的地址写入操作系统的内存，以供之后调用 send() 和 recv() 的时候使用
- 无论什么时候 IP 网络栈都不会 UDP 端口看作是一个可以连接或正在使用的单独实体。相反，IP 网络栈关注的是 UDP “套接字名”。UDP 套接字名是 IP 接口和 UDP 端口号组成的二元组，必须保证其不会与正在监听的服务器产生冲突

#### UDP 分组

- UDP 数据包最大可以达到 64KB，而以太网或无限网卡只能处理 1500B 左右的数据包
- UDP 必须把较大的 UDP 数据报分为多个较小的数据报，这意味着，较大的数据包在传输过程中更易发生丢包

#### 套接字选项

- POSIX 的套接字接口


1）udp传输包大小
报文大小：最大为1.4K
2）允许端口复用，否则使用使用过的端口需要等待一段时间
self.__sock.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1) 
3）发送报文速度
发送报文速度上限与报文大小有一定关系，外网情况下1K的报文，服务端接收速度可以达到400个左右每秒，发送过快反而会导致接收速度下降
4) 接收方缓冲区大小需比报文大小大，最好大个5-10倍，否则容易出现缓冲区溢出问题
5）udp协议是不可靠传输协议，不能保证到达另一端以及到达顺序

## 电子邮件

### SMTP 发送邮件

- SMTP（Simple Mail Transfer Protocol）即简单邮件传输协议，它是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式
- Python 的 smtplib 提供了一种很方便的途径发送电子邮件。它对 SMTP 协议进行了简单的封装

```python
import smtplib

smtpObj = smtplib.SMTP()
```