# Java

## 体系

1. 基础核心 JavaSE
    - 面向对象
    - API
    - JVM
1. 企业版 JavaEE
    - 大型企业级应用开发
    - JSP
    - EJB
    - 服务
1. 嵌入式开发 JavaME
    - 移动设备
    - 游戏
    - 通信

## 核心机制

- JVM（Java Virtual Machine）Java虚拟机
- Code Security 代码安全性检测
- Garbage Collection 垃圾收集机制

## SDK & JDK & JRE & IDE

- SDK : Software Development Kit：软件开发工具包
- JDK : Java Development Kit：Java开发工具包，是一种 SDK
- JRE : Java Runtime Environment：Java运行时环境，JDK 中包含 JRE
- IDE : Integrated Development Environment：集成开发环境

## 配置环境变量

1. 下载 [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html)、安装
1. 在 Path 中添加 Java 安装目录下的 bin 文件夹
1. 在 cmd 中测试 java、javac 命令

### JDK 安装后的文件夹

- bin 该目录存放工具文件
- jre 该目录存放与 java 运行环境相关的文件
- lib 存放程序库
- include 存放与 C 相关的头文件
- db 数据库相关

## 编译与运行

- 源文件 xxx.java
- 编译 javac
- 字节码 xxx.class
- 运行 java

## Constants

- 常量：程序执行过程中，值（value）保持不变的量
- 声明+初始化

```java
final datatype constantName = value;

final int Size = 3;
```

- 整型常量：33, +342, -454
- 实型常量：0.12, .12, 12., 12.0, 0.4e8D
- 字符型常量：'A','@',\t (制表符),\b (退格符),\n (换行符),\r (回车符), \0 (空字符)
- tip: 在 Linux 中 \n 表示换行+回车；MAC 中 \r 表示换行+回车；Windows 中 \n\r 表示换行+回车
- 字符串常量："Hello World!"

## DataType

### 数值型基本数据类型

| Name | Range | storage size |
| ---- | ----- | ------------ |
| byte | -128 to 127 | 1 byte |
| short | -32768 to 32767 | 2 byte |
| int | -2147483648 to 2147483647 | 4 byte |
| long | -9223372036854775808 to 9223372036854775807 | 8 byte |
| float | Negative: -3.4028235E+38 to -1.4E-45 <br> Positive: 1.4E-45 to 3.4028235E+38 | 4 byte |
| double | Negative :      -1.7976931348623157E+308 to -4.9E-324 <br> Positive :        4.9E-324 to 1.7976931348623157E+308 | 8 byte |

### 字符型基本数据类型

#### char

- Unicode 字符：\u0000 ~ \uffff

### 布尔型基本数据类型

#### boolean

- true or false

### Example

```java
int x = 1;
long x = 64L; (or 64l)
float d = 1.4F; (or 1.4f)
double d = 1.4; (or 1.4D 1.4d)
char a = 'A';
boolean flag = true;
```

- 整数型字面量（例如64）会被 JVM 默认为 int 类型数据，将 int 类型数据赋值给 float double long会自动转换 （因为int类型数据长度比他们小）
- 浮点型字面量（例如1.4）会被 JVM 默认为 double 类型数据，转换比它小的数据类型时候要显式转换，否则要声明字面量类型（例如1.4f）
- Double -> float，8字节向4字节，精度可能丢失

## Operator（操作符）

### Arithmetic Operators（算数操作符）

1. 基本算数符：+，-，*，/，%
1. 算数运算：[API](http://docs.oracle.com/javase/8/docs/api/) -> java.lang.Math
- 常用的：

| 方法标识符 | 返回值 |
| --------- | ------ |
| abs(x) | x的绝对值 |
| pow(x,y) | x的y次方 |
| ceil(x) | 小于x的最大整数 |
| floor(x) | 大于x的最小整数 |
| rint(x) | 最接近x的整数 |
| round(x) | 四舍五入 |
| max(x,y) | x,y中最大的值 |
| min(x,y) | x,y中最小的值 |
| sin(x) | x的sin值 |

```java
import java.lang.Math;

class Test {
    public static void main(String args[]) {
        System.out.println(Math.abs(2.5));
        System.out.println(Math.pow(5.2, 2.0));
        System.out.println(Math.max(3, 10));
    }
}

```

### Relational Operators（关系运算符）

- >, <, =, !=
- P.S. (>、>=、<、<=) > (==、!=) <br> 关系运算符低于算术运算符

### Logical Operators（逻辑运算符）

| 逻辑与 | 逻辑或 | 逻辑非 |
|:-----:|:------:|:-----:|
| && | \|\| | ! |

- P.S. ( ! ) > ( && ) > ( || ) <br> ( ! )>算术运算符>关系运算符>( && ) > ( || )

### Bitwise Operators（位运算符）

| 按位与 | 按位或 | 按位异或 | 按位取反 |
|:-----:|:------:|:-------:|:-------:|
| & | \| | ^ | ~ |

- P.S. : 操作数为整数 <br> (~) > (&) > (^) > (|)

#### Tip

1. 判断 int 型变量 x 是奇数还是偶数
    > x & 1 == 0 则 x 是偶数
    > x & 1 == 1 则 x 是奇数
1. 将一个 int 变量 x 的某几位，置为1
    > x | 7 -> 将 x 的最后3位 置为1
1. 将两个 int 型变量 x,y 数值交换
    > x = x^y;
    > y = y^x;
    > x = x^y;
1. 计算两个 int 变量 x,y 的平均值
    > (x&y)+((x^y)>>1)

### Shift Operators（移位运算符）

| 左移 | 右移 | 无符号右移 |
|:---:|:----:|:---------:|
| opt1 << opt2 | opt1 >> opt2 | opt1 >>> opt2 |
| 将op1的二进制位向左移op2(正整数)位，低位补零 | 将op1的二进制位向右移op2(正整数)位 <br> 高位补0(原为正数)、高位补1(原为负数) | 将op1的二进制位向右移op2(正整数)位高位都补0 |

### Conditional Operators（条件运算符）

- op1 ? op2 : op3
- 若op1为真，则运算结果为op2，否则为op3

### Priority of Operators（优先级）

| 类别 | 操作符 | 关联性 |
| ---- | ------| ------ |
| 后缀 | () [] . (点操作符) | 左到右 |
| 一元 | + + - ! ~ | 右到左 |
| 乘性 | * /％ | 左到右 |
| 加性 | + - | 左到右 |
| 移位 | >> >>>  <<  | 左到右 |
| 关系 | >> = << =  | 左到右 |
| 相等 | ==  != | 左到右 |
| 按位与 | ＆ | 左到右 |
| 按位异或 | ^ | 左到右 |
| 按位或 | \| | 左到右 |
| 逻辑与 | && | 左到右 |
| 逻辑或 | \|\| | 左到右 |
| 条件 | ?: | 右到左 |
| 赋值 | = + = - = * = / =％= >> = << =＆= ^ = \| = | 从右到左 |
| 逗号 | , | 左到右 |

## Type Casting（类型转换）

- 将一种类型的数据转换为另一种类型的数据
- 表达形式：(数据类型)操作数
- 应用场景：
    1. 二元运算符的两个操作数类型不同
    2. 表达式值的类型与变量得到类型不同

```java
int x=5;byte y=10;
double result;

result = x+y;

1. y->int
2. x+y->15(int)
3. 15(int)->15(double)
```

### 转换方法：

#### 隐型类型转换：自动类型转换（系统完成）

```java
// widening conversion(宽化转换)精度不丢失
short a -> int a (auto)
int a=20: 00000000 00000000 00000000 00010100
short a=20:                 00000000 00010100
```

- 隐型类型转换表

| 源类型 | 目的类型 |
|:-----:|:--------:|
| byte | short,char,int,long,float,double |
| short | int,long,float,double |
| char | int,long,float,double |
| int | long,float,double |
| long | float,double |
| float | double |

#### 显性类型转换：强制类型转换

```java
// narrowing conversion（窄化转换）

double a=1.5;
float b=a;
// 编译：“possible loss of precision”
// 数据精度丢失 -> 数据丢失

double a=1.5;
float b=(float)a;   // b=1.5(在值域允许范围内转换)

int a=257;
byte b=(byte)a;
int a=257:  00000000 00000000 00000001 00000001
byte a=257:                            00000001 //b=1
```

```java
char c1 = 'A',  c2;                 // A的ASCII值为65
int i=(int)c1+1;                    // i=66
c2=(char)i;                         // c2='B'
System.out.println(c1 + c2);        // 131（隐式转换成 int 型）
System.out.println(c1 + ", " +c2);  //A,B
```

```java
lnteger.parselnt()      //将字符串转成int
```

## Standard I/O

### Standard Input（标准输入）

- 从标准输入流(Stream)读入数据

```java
System.in.read();
//从输入流中读取下一个字节，返回下一个字节 int 型

System.in.read(byte[] b)
//从输入流中读取一定数量的字节，存入字节数组b中，返回读取的字节数量

System.in.read(byte[] b,int off,int len)
//从输入流中读取len个字节，写入字节数组b的off位置，返回读取的字节数量
```

- 以字节读取的标准输入，读入的数据需要进行自己转换（ASCII码转换）
- 并且无法获取超过10的数字（会被认为是“1”和“0”）

```java
char x = (char)System.in.read();    //输入：a，输入流中：97
System.out.println(x);              //字符x：a

int x = System.in.read();           //输入：82，输入流中：56 50
x = x-48;;                          //x-8=8
System.out.println(x);
```

### Scanner

```java
import java.unit.Scanner;
Scanner s = new Scanner(System.in);
```

- 可以使用 s.next() 输入一个不含空格的字符串
- s.nextInt(): 输入一个整数
- s.nextDouble(): 输入一个 double
- s.nextByte(): 输入一个字符

### Standard Output（标准输出）

> System.out.println();

## Array（数组）

### Declaring Array Variable（声明数组变量）

- 数组声明后不能被访问，数组元素未分配内存空间

```java
datatype[] arrayName;
double[] list;

datatype arrayName[];
double list[];
```

### Creating Array Variable（创建数组变量）

```java
arrayName = new datatype[arraySize];
list = new double[10];
```

### Declaring and Creating Together（声明同时创建数组）

```java
datatype[] arrayName = new datatype[arraySize];
double[] list = new double[10];

datatype arrayName[] = new datatype[arraySize];
double list[] = new double[10];
```

#### 初值

- 整型->0
- 实型->0.0
- 布尔型->false
- 字符型->\u0000

### Initializers（初始化）

```java
//声明和创建数组后对数组初始化
int a[] = new int[5];
for (int i = 0; i < 5; i++) {
    a[i] = i+1;
}

//声明数组的同时对数组初始化
int a[] = {1,2,3,4,5};
```

### Array assignment（数组赋值）

1. 数组声明 -> 生成引用变量
1. 数组创建 -> 分配数组空间
1. 数组初始化 -> 给数组初始值
1. 数组赋值

#### 一：数组的整体赋值

```java
int a[] = {2,4,6,8};
int b[];
b=a;    // b 和 a 指向同一块内存空间，修改 b 实际上也是修改 a
```

#### 二：数组遍历赋值

```java
int[] sourceArray = {2, 3, 1, 5, 10};
int[] targetArray = new int[sourceArray.length];

for (int i = 0; i < sourceArrays.length; i++)
    targetArray[i] = sourceArray[i];
```

- 这时 targetArray 和 sourceArray 指向不同的内存空间，二者互不影响

#### 三：用java.lang.System类的方法进行数组赋值

- java.lang.System类的方法 arraycopy
> arraycopy(Object src, int srcIndex, Object dest, int destIndex, int length)
- src：原数组
- secIndex：原数组起始位置
- dest：目标数组
- destIndex：目标数组的起始位置
- length：要 copy 的数组的长度

```java
int a[] = {2, 4, 6, 8};
int[] c = {1, 3, 5, 7, 9};
System.arraycopy(a, 1, c, 0, 3);
// 从数组 a 的第1位开始复制3位到数组 c 的第0位
```

### Multi-Dimensional Array

```java
// 一次性直接分配全部空间
int a[][] = new int[2][3];

// 逐一为每一维分配空间
int c[][] = new int[2][];
c[0] = new int[4];
c[1] = new int[3];

```

- 对于二维数组 array.length 代表数组行数
- 不能在第一维为空的情况下指定第二维大小

### ArrayList

- Java提供的一个实现数组功能的类————良好的封装性，丰富的功能
- 相对于 array 效率较低，但可动态增长，可扩展

## 类（class）

### 类

- 属性：变量（字段 field）
- 行为：函数（方法 method）

```java
public class Person{
    //属性
    int age;
    String name;

    //构造函数
    public Person(){

    }
    //方法
    void say(){....}
}
```

#### 写法

[类修饰符] class 类名 [extends 父类名] [implements 接口名] {
    [访问权限修饰符] 类型   成员变量;
    [访问权限修饰符] 类型   方法;
}

1. 类修饰符
    - public ：无任何限制，可以被所有类访问
    - 无修饰(default)：仅仅只能被同一个包中的其他类访问
    - abstract：该类不能被实例化，称为抽象类，出现在继承关系中
    - 抽象类中的方法不必在抽象类中实现，因为没有意义，一般是交给子类实现
    - final：声明该类不能有子类（不能被继承）
1. 访问权限修饰符
    - 需要实例化
    - public：公有变量
    - private：私有变量/方法，仅能在定义该私有成员的类中被访问
    - protected：保护变量/方法，容许类本身、同一个包中所有类及子类访问（先构造对象再访问）
    - 无修饰(default)：友好变量/方法
    - final变量：变量值不可修改
    - final方法：不能被重写（overriding）
    - 静态（无需实例化）
    - static：静态变量/方法，类成员独立于类的对象。可直接根据类名调用

### 类与对象的关系

- 类是对象的抽象（模板）
- 对象是类的实例，在内存中

```java
Person p = new Person()
```

## 封装

- 模块化：将属性和行为封装在类中，程序定义很多类
- 信息隐蔽：将类的细节部分隐藏起来，用户只通过受保护的接口访问某个类
- 继承性：父类和子类之间共享数据和方法，可以更好的进行抽象和分类、增强代码的重用率、提高可维护性
- 将类的某些信息隐藏在类的内部，不允许外部 直接访问，但允许使用该类提供的方法实现对隐藏信息的访问
- 减少代码耦合性

```java
class Student extends Person{
    String school;
    double score;
}
```

## extends（继承）

- 子类可调用父类的方法和变量
- 子类可增加父类中没有的方法和变量
- 子类可调用父类的构造函数
- 子类可重新定义父类的变量/方法

### 静态变量/方法

- 静态变量：static int num
- 静态方法：static void print()

### 实例变量/方法

## polymorphism（多态性）

- 不同的对象收到 同一个消息（调用方法）可产生完全不同的效果
- 实现的细节则由接收对象自行决定

### overriding（重写）

- 同名、同参数、同返回类型的函数

```java
class s
```

## 常识

- jdk：Java开发工具包
- jre：Java运行环境（一般jdk中自带）
- jar：Java归档文件（多个“.class”的打包文件）
- .class：文件的集合
- project：开发项目的名称
- package：用于对类进行分组管理的命名空间--包

1. new project > 指定项目名称、指定jre > 指定项目位置
1. 创建包（package）
1. 创建类

- class 是主体
- public 类名与文件同名，且只能有一个 public 类
- main() 的写法是固定的

```java
public class HelloWorld{
    public static void main (String arg[]){
        System.out.printIn("Hello World!");
    }
}
// public 是公用的 static 是静态的 void 说明没有返回值
```

- import 导入其他类的类库

## 工具

### 主要工具

- javac 编译
- java 运行
- javaw 运行图形界面程序
- appletViewer 运行 applet 程序

### 常用的工具

- jar 打包工具
- javadoc 生成文档
- javap 查看类信息及反汇编

## 使用 jar 打包

1. 编译 javac A.java
1. 打包 jar cvfm A.java A.man A.class
    - c表示创建（create）v 表示显示详情（verbose）f 表示指定文件名，m 表示清单文件
1. 运行 java -jar A.jar

- 其中 A.man 是清单文件（manifest）
    > Manifest-Version: 1.0;

    > Class-Path: .

    > Main-Class: A
- 清单文件可以任意命名，常见的是用 MANIFEST.MF
- jar 文件其实是一个压缩文件 可以用解压缩软件解压

## 数据类型

### String

- String 不是基本数据类型，是引用数据类型

#### 常用方法

```java
String.length()         //返回长度
String.charAt()         //反回某个位置的字符
String.conncat(s)       //将 s 拼接到另一个字符串后
String.toUpperCase()    //转大写
String.toLowerCase()    //转小写
String.trim()           //去首尾空格

equals(s1) 判断某个字符串和 s1 是否相等
equalsIgnoreCase() 忽略大小写的比较
```

## 输入与输出

- 应用程序（Java Application）的输入输出可以是文本界面，也可以是图形界面
- 小程序（Java Applet）只能是图形界面

### 文本界面：使用 Scanner 类

- next() 得到一个字符
- nextInt() 得到一个整型的数
- nextDouble() 得到一个浮点数

```java
import java.util.Scanner;

class ScannerTest{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        int a = scanner.nextInt();
        System.out.printf("%d的平方是%d\n",a,a*a);
    }
}
```

### 图形界面输入输出

- Java Application 需要先创建自己的图形界面（AppGraphInOut.java）
- 通过创建一个 Frame 创建自己的用户界面，在构建 AppFrame 时，设定 Frame 的大小，并用 setVisible(true) 方法显示出来

- AppGraphInOut.java
- add(xxx) 加入对象
- btn.addActionListener 处理事件
- actionPerformed() 函数具体处理事件

```java
setLayout(new FlowLayOut());
add(in);
add(btn);
add(out);
btn.addActionListener(new BtnActionAdapter());

class BtnActionAdapter implements ActionListener{
    public void actionPerformed(ActionEvent e){
        String s = in.getText();
        double d = Double.parseDouble
    }
}
```

## tip

- 判断一个 int 型数字是奇数还是偶数
> a&1 == 0 偶; a&1 == 1 奇

- 将两个 int 型数值交换

```java
x=x^y;
y=y^x;
x=y^x;
```

- 判断是否为闰年

```java
(year%4==0 && year%100!=0) || year%400==0
```