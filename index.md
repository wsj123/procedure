## mysql存储过程

### MYSQL存储过程语法：
```
Syntax highlighted code block

DELIMITER //声明语句结束符，用于区分；

CREATE PROCEDURE demo_in_parameter(IN p_in int) //声明存储过程

BEGIN...END //存储过程开始和结束符号

SET @p_in=1 //变量赋值

DECLARE l_int int unsigned default 4000000; // 定义变量
```
### mysql存储过程列程
 存储过程列程是存储在数据库服务器的一组SQL语句,通过查询中调用一个指定的名称来执行这些sql.
 
 
 #### 为什么要使用mysql存储过程？ 
我们都知道应用程序分为两种，一种是基于web，一种是基于桌面，他们都和数据库进行交互来完成数据的存取工作。假设现在有一种应用程序包含了这两 种，现在要修改其中的一个查询sql语句，那么我们可能要同时修改他们中对应的查询sql语句，当我们的应用程序很庞大很复杂的时候问题就出现这，不易维 护！另外把sql查询语句放在我们的web程序或桌面中很容易遭到sql注入的破坏。而存储例程正好可以帮我们解决这些问题。 
存储过程(stored procedure)、存储例程(store routine)、存储函数区别 

Mysql存储例程实际包含了存储过程和存储函数，它们被统称为存储例程。 

其中存储过程主要完成在获取记录或插入记录或更新记录或删除记录，即完成select insert delete update等的工作。

而存储函数只完成查询的工作，可接受输入参数并返回一个结果。

### 创建mysql存储过程、存储函数

create procudure 存储过程名(参数)

### 存储过程体

create function 存储函数名(参数)

### 存储过程的列子
```
DELIMITER //
CREATE PROCEDURE proc1(OUT s int)
BEGIN
SELECT COUNT(*) INTO s FROM shop_user;
END
//
DELIMITER ;
```
* （1）这里需要注意的是DELIMITER//和DELIMITER;两句，DELIMITER是分割符的意思，因为MySQL默认以”;”为分隔 符，如果我们没有声明分割符，那么编译器会把存储过程当成SQL语句进行处理，则存储过程的编译过程会报错，所以要事先用DELIMITER关键字申明当 前段分隔符，这样MySQL才会将”;”当做存储过程中的代码，不会执行这些代码，用完了之后要把分隔符还原。

（2）存储过程根据需要可能会有输入、输出、输入输出参数，这里有一个输出参数s，类型是int型，如果有多个参数用”,”分割开。 

（3）过程体的开始与结束使用BEGIN与END进行标识。


### 参数

MySQL存储过程的参数用在存储过程的定义，共有三种参数类型,IN,OUT,INOUT,形式如：

CREATEPROCEDURE 存储过程名([[IN |OUT |INOUT ] 参数名 数据类形…]) 

IN 输入参数:表示该参数的值必须在调用存储过程时指定，在存储过程中修改该参数的值不能被返回，为默认值 

OUT 输出参数:该值可在存储过程内部被改变，并可返回 

INOUT 输入输出参数:调用时指定，并且可被改变和返回

#### .in参数例子
```
DELIMITER //

CREATE PROCEDURE demo_in_parameter(IN p_in int)

BEGIN

SELECT p_IN;

SET p_in = 2;

SELECT p_in;

END;

//

DELIMITER;
执行结果:
SET @p_in=1;

CALL demo_in_parameter(@p_in);

SELECT @p_in;

结果:P_IN
1
```

#### .OUT参数例子
```
DELIMITER //

CREATE PROCEDURE demo_out_parameter(OUT p_out int)

BEGIN

SELECT p_out;

SET p_out = 2;

SELECT p_out;

END;

//

DELIMITER;
执行结果:
SET @p_out = 1;

CALL demo_out_parameter(@p_out);

结果： NULL

SELECT @p_out;

结果: 2
```
