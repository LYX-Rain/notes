# EDA 技术——VHDL语言

## EDA

- EDA：电子设计自动化，在 EDA 软件平台上，用硬件描述语言 HDL 完成设计，然后由计算机自动地完成逻辑化简、逻辑分割、逻辑综合、结构综合（布局布线），以及逻辑优化和仿真测试，直至实现既定性能的电子线路系统功能。
- 利用 EDA 技术进行电子系统设计的最后目标，是完成专用集成电路 ASIC 或印制电路板 PCB 的设计和实现
- SOC：单片子系统

### 自顶向下的设计技术

- 在 EDA 使用自顶向下的设计技术，在整个设计流程中各设计环节逐步求精


- ASIC 半定制实现方法：
  1. 门阵列法
  2. 标准单元法
  3. 可编程逻辑器件法

### IP 核

- 软 IP：用 Verilog/VHDL 等硬件描述语言的功能模块，不涉及用什么具体电路元件实现这些功能
- 固 IP：完成了综合的功能块。有较大的设计深度，以网表文件的形式提交用户使用
- 硬 IP：提供设计的最终阶段产品：掩模
- 设计深度越高，灵活性就越小

### 逻辑综合

- 将划分的各个子模块用文本（网表或硬件描述语言）、原理图等进行具体逻辑描述
- 对于硬件描述语言描述的设计模块需要用综合器进行综合，以获得具体的电路网表文件，对于原理图等描述方式描述的设计模块经简单编译后得到逻辑网表文件

## VHDL

- 基本组成：库、包、实体、结构体、配置
- 库：存放包集合、实体、结构体和配置的定义

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

### 实体

- 实体描述的是电路器件的端口构成（有哪些端口），端口类型（信号的流动方向和方式）和端口上流动的信号的属性

### 结构体

- 在结构体中给出相应的电路功能描述语句，可以是并行、顺序或它们的混合

### 进程语句和顺序语句

- 功能描述语句中，由 PROCESS 引导的语句称为进程语句。所有顺序描述语句都必须放在进程语句中，并行语句要放在进程语句外
- PROCESS 旁的(clk) 称为进程的敏感信号，PROCESS 语句的执行依赖于敏感信号的变化
- 层次化设计：先定义各个模块的功能，使多个设计者并行工作，可对每个模块单独进行仿真

### 数据类型

- VHDL 是强类型语言，必须先定义好要使用的数据类型
- BIT、INTEGER（整型）、BOOLEAN（布尔型）、STD_LOGIC（标准逻辑类型）
- STD_LOGIC 数据类型在 STD_LOGIC_1164 程序包中；STD_LOGIC 定义了9种数据：U、X、O、1、Z、W、L、H、-

### 端口模式

- 电路端口模式由四种：
  1. IN：输入端口，定义的通道为单向只读模式
  2. OUT：输出端口，定义的通道为单向输出模式
  3. INOUT：双向端口
  4. BUFFER：缓冲端口，当要输入数据时，只允许内部回读输出信号，即允许反馈；回读的信号不是由外部输入的，而是由内部产生、向外输出的信号

### 变量与信号

- 信号是逻辑电路中的连接线，可以用于元件间和元件内部电路各单元间的连接；使用”<=“符号赋值； 在顺序描述语句中，信号的赋值不是即时更新的。只有在相应的进程、函数或过程完成之后，信号的值才会进行更新
- 变量只用于局部电路的描述，只能在 process、function 和 procedure 内部使用；使用”:=“符号赋值；变量的赋值是立即生效的，可以在下一行代码中立即使用新的变量值

## 代码

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