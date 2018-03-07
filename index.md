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
 存储过程列程是存储在数据库服务器的一组SQL语句,通过查询中调用一个指定的名称来执行这些sql。
 
 
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
#### .INOUT参数例子

创建:
```
DELIMITER //

CREATE PROCEDURE demo_inout_parameter(INOUT p_inout int)
BEGIN

SELECT p_inout;

SET p_inout = 2;

SELECT p_inout;

END;

//

DELIMITER ;

set @p_inout=1;

CALL demo_inout_parameter(@p_inout) ;

结果: 1

SELECT @p_inout;

结果:2
```

### 局部变量

#### 变量定义

局部变量声明一定要放在存储过程体的开始 

DECLAREvariable_name [,variable_name…] datatype [DEFAULT value]; 

其中，datatype为MySQL的数据类型，如:int, float, date,varchar(length) 

例如:
```
DECLARE l_int int unsigned default 4000000;

DECLARE l_numeric number(8,2) DEFAULT 9.95;

DECLARE l_date date DEFAULT '1999-12-31';

DECLARE l_datetime datetime DEFAULT '1999-12-31 23:59:59';

DECLARE l_varchar VARCHAR(255) DEFAULT 'hello,world'  
```

#### 变量赋值
SET 变量名 = 表达式值 [,variable_name = expression …]

#### 用户变量

在MySQL客户端使用用户变量
```
1. SELECT 'Hello World' into @x;

SELECT @x;

结果: "Hello World"

2.SET @y="Goodbye Cruel World";

SELECT @y;

结果:Goodbye Cruel World

3.SET @z=1+2+3;

SELECT @z;

结果: 6
```

#### 在存储过程中使用用户变量
```
CREATE PROCEDURE GreetWorld1() SELECT CONCAT(@greeting, ' World');

SET @greeting='Hello';

CALL GreetWorld1();

结果：Hello World 
```

#### 在存储过程间传递全局范围的用户变量
```
CREATE PROCEDURE p1() SET @last_procedure='p1';

CREATE PROCEDURE p2() SELECT CONCAT('Last procedure was ', @last_procedure);

CALL p1();

CALL p2();

结果：Last procedure was p1
```
*:
①用户变量名一般以@开头 

②滥用用户变量会导致程序难以理解及管理

### 注释

MySQL存储过程可使用两种风格的注释

双模杠：– 

该风格一般用于单行注释 

c风格： 一般用于多行注释 
```
DELIMITER // 

CREATE PROCEDURE proc3 -- name存储过程名

(IN parameter1 INTEGER)

BEGIN

DECLARE variable1 CHAR(10);

if parameter1=17 THEN

SET variable1 = 'birds';

ELSE

SET variable1 = 'beasts';

END IF;

INSERT INTO table1 VALUES(variable1);

END;

//

DELIMITER ;
```

### MySQL存储过程的调用

用call和你过程名以及一个括号，括号里面根据需要，加入参数，参数包括输入参数、输出参数、输入输出参数。具体的调用方法可以参看上面的例子。

### MySQL存储过程的查询

查看某个数据库下面的存储过程:
```
1.  SELECT name from mysql.proc where db="数据库名"
2.SELECT routine_name FROM information_schema.ROUTINES WHERE ROUTINE_SCHEMA = '数据库'
3.show PROCEDURE STATUS where db="数据库"
```
- 查看存储过程的详细
show CREATE PROCEDURE 数据库.存储过程

### MySQL存储过程的删除
DROP PROCEDURE 存储过程名称

### MySQL存储过程的控制语句

#### 变量作用域
内部的变量在其作用域范围内享有更高的优先权，当执行到end。变量时，内部变量消失，此时已经在其作用域外，变量不再可见了，

应为在存储过程外再也不能找到这个申明的变量，但是你可以通过out参数或者将其值指派 

给会话变量来保存其值。
```
DELIMITER //

CREATE PROCEDURE proc3()

BEGIN

DECLARE x1 VARCHAR(5) DEFAULT 'outer';

BEGIN

DECLARE x1 VARCHAR(5) DEFAULT 'inner';

SELECT x1;

END;

SELECT x1;

END;

//

DELIMITER ;
```

### 条件语句
#### if-then -else语句
