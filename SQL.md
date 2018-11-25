# SQL 数据库

## RDBMS 术语

- 数据库(Database,DB): 数据库是一些关联表的集合。
- 数据库管理系统 (Database Management System,DBMS)
- 数据库系统 (Database System,DBS)
- 数据表: 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
- 列（字段）: 一列(数据元素) 包含了相同的数据, 例如邮政编码的数据。
- 行：一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
- 冗余：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
- 主键：主键是唯一的标识符。一个数据表中只能包含一个主键。使用主键来查询数据性能更高。
- 外键：外键用于关联两个表。
- 复合键：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
- 索引：使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。

## 数据库系统的模式结构

### 外模式--用户模式

- 是模式的子集或变形，是与某一应用有关的数据的逻辑表示
- 不同用户需求不同，看待数据的方式也可以不同，对数据保密的要求也可以不同，使用的程序设计语言也可以不同，因此不同用户的外模式的描述可以是不同的。

### 模式--逻辑模式

- 是数据库中全体数据的逻辑结构和特性的描述，是所有用户的公共数据视图
- DBMS提供数据定义语言DDL来描述逻辑模式，严格定义数据的名称、特征、相互关系、约束等

### 内模式--存储模式

- 是数据物理结构和存储方式的描述
- 是数据在数据库内部的表示方式
  - 记录的存储方式（顺序存储，按照B树结构存储，按hash方法存储）
  - 索引的组织方式
  - 数据是否压缩存储
  - 数据是否加密
  - 数据存储记录结构的规定

## SQL 中的存储过程

### 存储过程

- 是存储在服务器上的 Transact-SQL 语句的命名集合
- 是封装重复性任务的方法
- 支持用户声明变量、条件执行以及其他强有力的编程特性

### 创建存储过程

```sql
CREATE PROC dbo.OverdueOrders
AS
  SELECT *
    FROM dbo.Orders
    WHERE RequiredDate < GETDATE() AND ShippedDate IS Null
GO
```

- 只能在当前数据库内创建存储过程，除了临时存储过程。临时存储过程总是创建在 tempdb 数据库中
- 存储过程可以引用表、视图、用户定义函数、其他存储过程以及临时表
- 若存储过程创建了局部临时表，则当存储过程执行结束后临时表消失

### 执行存储过程

#### 不带参数的情况

> [[EXEC]UTE]   存储过程名 [ WITH RECOMPILE]
- 存储过程在执行后都会返回一个整型值。如果执行成功，则返回0；否则返回-1～-99之间的数值。也可以使用RETURN语句来指定一个返回值。

### 修改和删除存储过程

#### 修改

```sql
ALTER PROC dbo.OverdueOrders
AS
SELECT  CONVERT(char(8), OrderDate, 1) OrderDate,OrderID, CustomerID, EmployeeID
FROM Orders
WHERE RequiredDate < GETDATE() AND ShippedDate IS Null
ORDER BY RequiredDate
GO
```

- 用 ALTER PROCEDURE 中的定义取代现有存储过程原先的定义，但保留权限分配

### 在存储过程中使用参数

#### 使用输入参数

- 输入参数允许传递信息到存储过程内
- 在 CREATE PROCEDURE 中指定：@参数名 数据类型 [=默认值]

```sql
CREATE  PROC dbo.OverdueOrders2
@Employee_ID int ,
@Order_date datetime
AS
SELECT  OrderDate, OrderID, CustomerID, EmployeeID
FROM Orders
WHERE EmployeeID = @Employee_ID and OrderDate < @Order_date
GO
```

#### 删除

- 语法：DROP PROCEDURE {存储过程名} [,...n]
  - 用 DROP PROCEDURE 语句从当前数据库中移除用户定义存储过程
- 在删除存储过程之前，执行系统存储过程 sp_depends 检查是否有对象依赖于此存储过程

## 安装

### Linux/UNIX上安装Mysql

#### Linux平台上推荐使用RPM包来安装Mysql

- MySQL - MySQL服务器。你需要该选项，除非你只想连接运行在另一台机器上的MySQL服务器。
- MySQL-client - MySQL 客户端程序，用于连接并操作Mysql服务器。
- MySQL-devel - 库和包含文件，如果你想要编译其它MySQL客户端，例如Perl模块，则需要安装该RPM包。
- MySQL-shared - 该软件包包含某些语言和应用程序需要动态装载的共享库(libmysqlclient.so*)，使用MySQL。
- MySQL-bench - MySQL数据库服务器的基准和性能测试工具。

#### centOS

1. 检测系统是否自带安装 mysql:
  > rpm -qa | grep mysql
1. 如果你系统有安装，那可以选择进行卸载:

  ```bash
  rpm -e mysql            # 普通删除模式
  rpm -e --nodeps mysql   # 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除
  ```

1. 安装 mysql：

  ```bash
  yum install mysql
  ```

1. 启动 mysql：

  ```bash
  service mysqld start
  ```

### 验证Mysql安装

- 在成功安装Mysql后，一些基础表会表初始化，在服务器启动后，你可以通过简单的测试来验证Mysql是否工作正常。
- 使用 mysqladmin 工具来获取服务器状态：
> mysqladmin --version

## SQL语句

### SQL标准写法

- 关键字大写
- 库、表、字段需要加上 ``（反单引号）

### 增-INSERT

```sql
INSERT INTO 表（字段列表） VALUE(值列表)
INSERT INTO `user_table` (`ID`, `username`, `password`) VALUE(0, 'Rain', '572739554');
```

### 删-DELETE

### 改-UPDATE

### 查-SELECT

```sql
SELECT 什么 FROM 表
SELECT * FROM `user_table`;
```