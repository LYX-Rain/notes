# EDA 技术——VHDL语言

## EDA

- EDA：电子设计自动化，在 EDA 软件平台上，用硬件描述语言 HDL 完成设计，然后由计算机自动地完成逻辑化简、逻辑分割、逻辑综合、结构综合（布局布线），以及逻辑优化和仿真测试，直至实现既定性能的电子线路系统功能。
- 利用 EDA 技术进行电子系统设计的最后目标，是完成专用集成电路 ASIC 或印制电路板 PCB 的设计和实现

### 名词简称

- 专用集成电路 ASIC （Application Specific Integrated Circuit)
- 单片电子系统 SOC（System on a Chip）
- 电子设计自动化 EDA（Electronics Design Automation）
- 硬件描述语言HDL（Hardware Description Language）
- 现场可编程门阵列FPGA（Field－Programmable Gate Array）
- 复杂可编程逻辑器件CPLD(Complex Programmable Logic Device)
- 超高速集成电路硬件描述语言VHDL（Very-High-Speed Integrated Circuit HardwareDescription Language）
- 知识产权核IP(Intellectual property)

### 自顶向下的设计技术

- 在 EDA 技术应用中，自顶向下的设计技术，就是在整个设计流程中各设计环节逐步求精


- ASIC 半定制实现方法：
  1. 门阵列法
  2. 标准单元法
  3. 可编程逻辑器件法

### IP 核

- 软 IP：用 Verilog/VHDL 等硬件描述语言的功能模块，不涉及用什么具体电路元件实现这些功能
- 固 IP：完成了综合的功能块。有较大的设计深度，以网表文件的形式提交用户使用
- 硬 IP：提供设计的最终阶段产品：掩模
- 设计深度越高，灵活性就越小
- IP 核具有规范的接口协议，良好的可移植性，为系统开发提供了可靠的保证

### 综合

- 在电子设计领域中综合：将用行为和功能层次表达的电子系统转换为低层次的便于具体实现的模块组合装配的过程

#### 逻辑综合

- 从 RTL 级（寄存器传输级）表述转换到逻辑门（包括触发器）的表述
- 将划分的各个子模块用文本（网表或硬件描述语言）、原理图等进行具体逻辑描述
- 对于硬件描述语言描述的设计模块需要用综合器进行综合，以获得具体的电路网表文件，对于原理图等描述方式描述的设计模块经简单编译后得到逻辑网表文件

## VHDL

- 基本组成：库、包、实体、结构体、配置

```VHDL
LIBRARY <设计库名>
USE <设计库名>.<程序包名>.ALL
ENTITY <实体名> IS
    PORT (clk:  IN  BIT;
          a:    OUT BIT);
END ENTITY <实体名>
ARCHITECTURE <结构体名> OF <实体名> IS
    [说明语句]
    BEGIN
        (功能描述语句)
        PROCESS(clk)
        BEGIN
        END PROCESS
END ARCHITECTURE <结构体名>
```

### 库

- 库是经编译后的数据集合，库中存放的是各种程序包、实体定义、结构体描述等
- 设计人员在用 VHDL语言设计系统时，库中内容有的可作为标准，有的可作为资源被引用
- 库的作用就在于使设计者可以共享已经编译过的设计文件及有用数据

### 包

- 程序包是 VHDL 程序的公共存储区，在程序包内说明的数据对实体是透明的。程序包由程序包说明和程序包体组成。

### 实体

- 实体可以表示小到一个与门，也可以大到一个数字系统，这个系统可以像微处理器一样的复杂
- 在实体的说明部分主要完成设计对象的输入输出端口名称、传输方向、数据类型的定义，即端口的定义

### 结构体

- 结构体是设计实体的具体描述，如果把设计实体抽象为一个功能方块图，结构体则描述这个功能方块图内部的具体逻辑实现细节
- 一个设计实体的内部实现细节通过结构体的具体描述表现出来，结构体中的电路功能描述语句，可以是并行、顺序或它们的混合

### 配置

- 配置是用于描述设计不同层次之间的关系和实体与结构体之间的连接关系
- 在实体与结构体之间的连接关系配置说明中，设计者可以利用配置语句为实体提供不同的结构体与之相匹配
- 在仿真设计中，可以利用不同配置方式选择不同结构体，分别对不同结构体进行仿真测试

### VHDL描述方式

- RTL描述
- 行为描述
- 数据流描述
- 结构描述

### 进程语句和顺序语句

- 功能描述语句中，由 PROCESS 引导的语句称为进程语句。所有顺序描述语句都必须放在进程语句中，并行语句要放在进程语句外
- PROCESS 旁的(clk) 称为进程的敏感信号，PROCESS 语句的执行依赖于敏感信号的变化
- 层次化设计：先定义各个模块的功能，使多个设计者并行工作，可对每个模块单独进行仿真

### 数据对象

- 在 VHDL 中，数据对象有三类：变量（VARIABLE）、常量（Constant）、信号（SIGNAL）

#### 常量

- 常量是一个恒定不变的值，一旦作了数据类型和赋值定义后，在程序中就不能再改变。常量在哪定义就只能作用在定义的范围内
> CONSTANT 常量名 : 数据类型 := 表达式;

#### 变量

- 变量是一个局部量，只能在进程和子进程中使用。变量不能将信息带出对它做出定义的当前结构
- 变量的赋值是一种理想化的数据传输，是立即发生的，不存在任何延时行为
- 变量的主要作用是在进程中作为临时的数据存储单元
- 定义：
> VARIABLE 变量名 : 数据类型 := 初始值;
- 赋值：
> 目标变量名 := 表达式;

#### 信号

- 信号是描述硬件系统的基本数据对象，其性质类似于连接线；可作为设计实体中并行语句模块间的信息交流通道。信号不但可以容纳当前值，也可以保持历史值
- 进程只能将信号作为敏感信号量，而不能使用变量；不同进程中不允许同时对同一信号赋值，因为结构体中的所有进程都是并行的。信号的赋值只在进程结束时发生，有延时
- 定义：
> SIGNAL 信号名 : 数据类型 := 初始值;
- 赋值：
> 目标信号名 <= 表达式;


### 数据类型

- VHDL 是强类型语言，必须先定义好要使用的数据类型
- BIT、INTEGER（整型）、BOOLEAN（布尔型）、STD_LOGIC（标准逻辑类型）
- STD_LOGIC 数据类型在 STD_LOGIC_1164 程序包中；STD_LOGIC 定义了9种数据：U、X、O、1、Z、W、L、H、-

### 端口模式

- 电路端口模式由四种：
  1. IN：输入端口，定义的通道为单向只读模式
  2. OUT：输出端口，定义的通道为单向输出模式
  3. INOUT：具有三态控制的双向传送端口
  4. BUFFER：缓冲端口、具有输出反馈的单向传送出口，当要输入数据时，只允许内部回读输出信号，即允许反馈；回读的信号不是由外部输入的，而是由内部产生、向外输出的信号。

## 代码

### 4 选 1 多路选择器

```VHDL
LIBRARY IEEE;
USE IEEE.STD_LOGIC.1164.ALL;
ENTITY mux4_1 IS
PORT(a, b, c, d : IN STD_LOGIC;
    s0, s1 : IN STD_LOGIC;
    out : OUT STD_LOGIC);
END ENTITY mux4_1;
ARCHITECTURE bhv OF mux4_1 IS
    SIGNAL S : STD_LOGIC_VECTOR(1 DOWNTO 0);
BEGIN
    S <= s1 & s0;
    -------------------------------------IF_THEN 法
    PROCESS(S, a, b, c, d)
    BEGIN
        IF S = '00' THEN out <= a;
        ELSEIF S = '01' THEN out <= b;
        ELSEIF S = '10' THEN out <= c;
        ELSEIF S = '11' THEN out <= d;
        END IF;
    END PROCESS
    -------------------------------------CASE_WHEN 法
    PROCESS(S, a, b, c, d)
    BEGIN
        CASE S IS
            WHEN '00' => out <= a;
            WHEN '01' => out <= b;
            WHEN '10' => out <= c;
            WHEN '11' => out <= d;
    END PROCESS
END ARCHITECTURE bhv;
```

### 计数器

```VHDL
LIBRARY IEEE;
USE IEEE.STD_LOGIC_1164.ALL;
USE IEEE.STD_LOGIC_UNSIGNED.ALL;
ENTITY CNT4 IS
    PROT(CLK: IN STD_LOGIC;
          Q : OUT STD_LOGIC_VECTOR(3 DOWNTO 0));
END;
ARCHITECTURE bhv OF CNT4 IS
    SIGNAL Q1 : STD_LOGIC_VECTOR(3 DOWNTO 0);
BEGIN
    PROCESS(CLK)
    BEGIN
        IF CLK'EVENT AND CLK='1' THEN
            Q1<=Q1+1;
        END IF;
    END PROCESS;
    Q<=Q1;
END bhv;
```

- 因为 Q1<=Q1+1 中数据赋值传输符 <= 右边加号的两个操作数分属不同的数据类型，因此必须调用运算符重载函数，这就要打开 IEEE 库中的程序包 STD_LOGIC_UNSIGNED

### 状态机

- 说明部分使用 TYPE 语句定义新的数据类型

```VHDL
ARCHITECTURE .... IS
    TYPE FSM_ST IS (s0, s1, s2);
    SIGNAL state, next_state : FSM_ST;
BEGIN
```

```VHDL
LIBRARY IEEE;
USE IEEE.STD_LOGIC_1164.ALL;
ENTITY FSM_EXP IS
PORT(clk, reset : IN STD_LOGIC;
    state_input : IN STD_LOGIC_VECTOR(0 TO 1);
    comb_output : OUT INTEGER RANGE 0 TO 15);
END FSM_EXP;
ARCHITECTURE behv OF FSM_EXP IS
    TYPE FSM_ST IS (s0, s1, s2, s3, s4);
    SIGNAL c_st, next_state : FSM_ST;
BEGIN
    PROCESS(reset, clk)
    BEGIN
        IF reset='0' THEN c_st<=s0;
        ELSIF clk='1' AND clk'EVENT THEN c_st <= next_state;
        END IF;
    END PROCESS;
    PROCESS(c_st, state_input)
    BEGIN
        CASE c_st IS
            WHEN s0 => comb_output <= 5;
                IF state_input='00' THEN next_state <= s0;
                ELSE next_state <= s1;
                END IF;
            WHEN s1 => comb_output <= 8;
                IF state_input='01' THEN next_state <= s1;
                ELSE next_state <= s2;
                END IF;
            WHEN s2 => comb_output <= 12;
                IF state_input='10' THEN next_state <= s0;
                ELSE next_state <= s3;
                END IF;
            WHEN s3 => comb_output <= 14;
                IF state_input='11' THEN next_state <= s3;
                ELSE next_state <= s4;
                END IF;
            WHEN s4 => comb_output <= 9;
                next_state <= s0;
            WHEN OTHERS => next_state <= s0;
        END CASE;
    END PROCESS
END behv;
```

- Moore型状态机：输出信号仅为状态的函数，与输入信号无关
- Mealy型状态机：输出信号为状态和输入信号的函数