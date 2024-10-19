## 数据类型
### SQLite 存储类
SQLite 数据类型是一个用来指定任何对象的数据类型的属性。SQLite 中的每一列，每个变量和表达式都有相关的数据类型。

| 存储类     | 描述                                              |
| ------- | ----------------------------------------------- |
| NULL    | 值是一个 NULL 值。                                    |
| INTEGER | 值是一个带符号的整数，根据值的大小存储在 1、2、3、4、6 或 8 字节中。         |
| REAL    | 值是一个浮点值，存储为 8 字节的 IEEE 浮点数字。                    |
| TEXT    | 值是一个文本字符串，使用数据库编码（UTF-8、UTF-16BE 或 UTF-16LE）存储。 |
| BLOB    | 值是一个 blob 数据，完全根据它的输入存储。                        |

### SQLite 亲和(Affinity)类型
SQLite支持列的亲和类型概念。任何列仍然可以存储任何类型的数据，当数据插入时，该字段的数据将会优先采用亲缘类型作为该值的存储方式。
下表列出了当创建 SQLite3 表时可使用的各种数据类型名称，同时也显示了相应的亲和类型：

|数据类型|亲和类型|
|---|---|
|- INT<br>    <br>- INTEGER<br>    <br>- TINYINT<br>    <br>- SMALLINT<br>    <br>- MEDIUMINT<br>    <br>- BIGINT<br>    <br>- UNSIGNED BIG INT<br>    <br>- INT2<br>    <br>- INT8|INTEGER|
|- CHARACTER(20)<br>    <br>- VARCHAR(255)<br>    <br>- VARYING CHARACTER(255)<br>    <br>- NCHAR(55)<br>    <br>- NATIVE CHARACTER(70)<br>    <br>- NVARCHAR(100)<br>    <br>- TEXT<br>    <br>- CLOB|TEXT|
|- BLOB<br>    <br>- 未指定类型|BLOB|
|- REAL<br>    <br>- DOUBLE<br>    <br>- DOUBLE PRECISION<br>    <br>- FLOAT|REAL|
|- NUMERIC<br>    <br>- DECIMAL(10,5)<br>    <br>- BOOLEAN<br>    <br>- DATE<br>    <br>- DATETIME|NUMERIC|

### Boolean 数据类型
SQLite 没有单独的 Boolean 存储类。相反，布尔值被存储为整数 0（false）和 1（true）。

### Date 与 Time 数据类型

SQLite 没有一个单独的用于存储日期和/或时间的存储类，但 SQLite 能够把日期和时间存储为 TEXT、REAL 或 INTEGER 值。

|存储类|日期格式|
|---|---|
|TEXT|格式为 "YYYY-MM-DD HH:MM:SS.SSS" 的日期。|
|REAL|从公元前 4714 年 11 月 24 日格林尼治时间的正午开始算起的天数。|
|INTEGER|从 1970-01-01 00:00:00 UTC 算起的秒数。|

可以以任何上述格式来存储日期和时间，并且可以使用内置的日期和时间函数来自由转换不同格式。

## 创建数据库
sqlite3 命令的基本语法如下：
```bash
sqlite3 DatabaseName.db
```

通常情况下，数据库名称在 RDBMS 内应该是唯一的。

另外我们也可以使用 .open 来建立新的数据库文件：

```bash
sqlite>.open test.db
```

上面的命令创建了数据库文件 test.db，位于 sqlite3 命令同一目录下。

打开已存在数据库也是用 .open 命令，以上命令如果 **test.db** 存在则直接会打开，不存在就创建它。

## 表操作
### 创建表
SQLite 的 **CREATE TABLE** 语句用于在任何给定的数据库创建一个新表。创建基本表，涉及到命名表、定义列及每一列的数据类型。
CREATE TABLE 语句的基本语法如下：
```sql
CREATE TABLE database_name.table_name(
   column1 datatype  PRIMARY KEY(one or more columns),
   column2 datatype,
   column3 datatype,
   .....
   columnN datatype,
);
```

CREATE TABLE 是告诉数据库系统创建一个新表的关键字。CREATE TABLE 语句后跟着表的唯一的名称或标识。您也可以选择指定带有 _table_name_ 的 _database_name_。

下面是一个实例，它创建了一个 COMPANY 表，ID 作为主键，NOT NULL 的约束表示在表中创建纪录时这些字段不能为 NULL：

```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);

```

### 删除表
SQLite 的 **DROP TABLE** 语句用来删除表定义及其所有相关数据、索引、触发器、约束和该表的权限规范。
DROP TABLE 语句的基本语法如下。您可以选择指定带有表名的数据库名称，如下所示：
```sql
DROP TABLE database_name.table_name;
```

如：
```sql
DROP TABLE COMPANY;
```

### 插入数据
SQLite 的 **INSERT INTO** 语句用于向数据库的某个表中添加新的数据行。
INSERT INTO 语句有两种基本语法，如下所示：
```sql
INSERT INTO TABLE_NAME [(column1, column2, column3,...columnN)]  
VALUES (value1, value2, value3,...valueN);

```

在这里，column1, column2,...columnN 是要插入数据的表中的列的名称。

如果要为表中的所有列添加值，您也可以不需要在 SQLite 查询中指定列名称。但要确保值的顺序与列在表中的顺序一致。SQLite 的 INSERT INTO 语法如下：
```sql
INSERT INTO TABLE_NAME VALUES (value1,value2,value3,...valueN);

```

假如已经在 testDB.db 中创建了 COMPANY表，如下所示：
```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```

现在，下面的语句将在 COMPANY 表中创建六个记录：
```sql
INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Paul', 32, 'California', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (2, 'Allen', 25, 'Texas', 15000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (3, 'Teddy', 23, 'Norway', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (5, 'David', 27, 'Texas', 85000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (6, 'Kim', 22, 'South-Hall', 45000.00 );

```

也可以使用第二种语法在 COMPANY 表中创建一个记录，如下所示：
```sql
INSERT INTO COMPANY VALUES (7, 'James', 24, 'Houston', 10000.00 );

```

### 删除数据
SQLite 的 **DELETE** 查询用于删除表中已有的记录。可以使用带有 WHERE 子句的 DELETE 查询来删除选定行，否则所有的记录都会被删除。
带有 WHERE 子句的 DELETE 查询的基本语法如下：
```sql
DELETE FROM table_name
WHERE [condition];

```
可以使用 AND 或 OR 运算符来结合 N 个数量的条件。

假设 COMPANY 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0

```

下面是一个实例，它会删除 ID 为 7 的客户：
```sql
DELETE FROM COMPANY WHERE ID = 7;
```
\
现在，COMPANY 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0

```

如果您想要从 COMPANY 表中删除所有记录，则不需要使用 WHERE 子句，DELETE 查询如下：
```sql
DELETE FROM COMPANY;
```

现在，COMPANY 表中没有任何的记录，因为所有的记录已经通过 DELETE 语句删除。


### 查询数据
SQLite 的 **SELECT** 语句用于从 SQLite 数据库表中获取数据，以结果表的形式返回数据。这些结果表也被称为结果集。
```sql
SELECT column1, column2, columnN FROM table_name;
```

在这里，column1, column2...是表的字段，他们的值即是您要获取的。如果您想获取所有可用的字段，那么可以使用下面的语法：
```sql
SELECT * FROM table_name;
```

假设 `COMPANY` 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0

```

使用 SELECT 语句获取并显示所有这些记录。
```sql
SELECT * FROM COMPANY;
```

最后，将得到以下的结果：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0

```

如果只想获取 COMPANY 表中指定的字段，则使用下面的查询：
```sql
SELECT ID, NAME, SALARY FROM COMPANY;
```

上面的查询会产生以下结果：
```sql
ID          NAME        SALARY
----------  ----------  ----------
1           Paul        20000.0
2           Allen       15000.0
3           Teddy       20000.0
4           Mark        65000.0
5           David       85000.0
6           Kim         45000.0
7           James       10000.0

```

### 修改数据
SQLite 的 **UPDATE** 查询用于修改表中已有的记录。可以使用带有 WHERE 子句的 UPDATE 查询来更新选定行，否则所有的行都会被更新。

带有 WHERE 子句的 UPDATE 查询的基本语法如下：
```sql
UPDATE table_name
SET column1 = value1, column2 = value2...., columnN = valueN
WHERE [condition];

```

可以使用 AND 或 OR 运算符来结合 N 个数量的条件。

假设 COMPANY 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0

```

下面是一个实例，它会更新 ID 为 6 的客户地址：
```sql
UPDATE COMPANY SET ADDRESS = 'Texas' WHERE ID = 6;
```

现在，COMPANY 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          Texas       45000.0
7           James       24          Houston     10000.0

```

如果您想修改 COMPANY 表中 ADDRESS 和 SALARY 列的所有值，则不需要使用 WHERE 子句，UPDATE 查询如下：
```sql
UPDATE COMPANY SET ADDRESS = 'Texas', SALARY = 20000.00;
```

现在，COMPANY 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          Texas       20000.0
2           Allen       25          Texas       20000.0
3           Teddy       23          Texas       20000.0
4           Mark        25          Texas       20000.0
5           David       27          Texas       20000.0
6           Kim         22          Texas       20000.0
7           James       24          Texas       20000.0

```


## Sql子句
### Where子句



SQLite的 **WHERE** 子句用于指定从一个表或多个表中获取数据的条件。

如果满足给定的条件，即为真（true）时，则从表中返回特定的值。您可以使用 WHERE 子句来过滤记录，只获取需要的记录。

WHERE 子句不仅可用在 SELECT 语句中，它也可用在 UPDATE、DELETE 语句中：
SQLite 的带有 WHERE 子句的 SELECT 语句的基本语法如下：
```sql
SELECT column1, column2, columnN 
FROM table_name
WHERE [condition]

```

还可以使用[比较或逻辑运算符](https://www.runoob.com/sqlite/sqlite-operators.html)指定条件，比如 >、<、=、LIKE、NOT，等等。假设 COMPANY 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0

```

下面的实例演示了 SQLite 逻辑运算符的用法。下面的 SELECT 语句列出了 AGE 大于等于 25 **且**工资大于等于 65000.00 的所有记录：
```sql
SELECT * FROM COMPANY WHERE AGE >= 25 AND SALARY >= 65000;
```

```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
```

下面的 SELECT 语句列出了 AGE 大于等于 25 **或**工资大于等于 65000.00 的所有记录：
```sql
SELECT * FROM COMPANY WHERE AGE >= 25 OR SALARY >= 65000;
```

```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
```

下面的 SELECT 语句列出了 AGE 不为 NULL 的所有记录，结果显示所有的记录，意味着没有一个记录的 AGE 等于 NULL：
```sql
SELECT * FROM COMPANY WHERE AGE IS NOT NULL;
```

```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0
```

下面的 SELECT 语句列出了 NAME 以 'Ki' 开始的所有记录，'Ki' 之后的字符不做限制：
```sql
SELECT * FROM COMPANY WHERE NAME LIKE 'Ki%';
```

```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
6           Kim         22          South-Hall  45000.0
```

下面的 SELECT 语句列出了 NAME 以 'Ki' 开始的所有记录，'Ki' 之后的字符不做限制：
```sql
SELECT * FROM COMPANY WHERE NAME GLOB 'Ki*';
```

```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
6           Kim         22          South-Hall  45000.0
```

下面的 SELECT 语句列出了 AGE 的值为 25 或 27 的所有记录：
```sql
SELECT * FROM COMPANY WHERE AGE IN ( 25, 27 );
```

```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
2           Allen       25          Texas       15000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
```

下面的 SELECT 语句列出了 AGE 的值既不是 25 也不是 27 的所有记录：
```sql
SELECT * FROM COMPANY WHERE AGE NOT IN ( 25, 27 );
```

```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
3           Teddy       23          Norway      20000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0
```

下面的 SELECT 语句列出了 AGE 的值在 25 与 27 之间的所有记录：
```sql
SELECT * FROM COMPANY WHERE AGE BETWEEN 25 AND 27;
```

```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
2           Allen       25          Texas       15000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
```



#### 运算符
##### 算术运算符
假设变量 a=10，变量 b=20，则：

|运算符|描述|实例|
|---|---|---|
|+|加法 - 把运算符两边的值相加|a + b 将得到 30|
|-|减法 - 左操作数减去右操作数|a - b 将得到 -10|
|*|乘法 - 把运算符两边的值相乘|a * b 将得到 200|
|/|除法 - 左操作数除以右操作数|b / a 将得到 2|
|%|取模 - 左操作数除以右操作数后得到的余数|b % a 将得到 0|

##### 比较运算符
假设变量 a=10，变量 b=20，则：

| 运算符 | 描述                             | 实例            |
| --- | ------------------------------ | ------------- |
| ==  | 检查两个操作数的值是否相等，如果相等则条件为真。       | (a == b) 不为真。 |
| =   | 检查两个操作数的值是否相等，如果相等则条件为真。       | (a = b) 不为真。  |
| !=  | 检查两个操作数的值是否相等，如果不相等则条件为真。      | (a != b) 为真。  |
| <>  | 检查两个操作数的值是否相等，如果不相等则条件为真。      | (a <> b) 为真。  |
| >   | 检查左操作数的值是否大于右操作数的值，如果是则条件为真。   | (a > b) 不为真。  |
| <   | 检查左操作数的值是否小于右操作数的值，如果是则条件为真。   | (a < b) 为真。   |
| >=  | 检查左操作数的值是否大于等于右操作数的值，如果是则条件为真。 | (a >= b) 不为真。 |
| <=  | 检查左操作数的值是否小于等于右操作数的值，如果是则条件为真。 | (a <= b) 为真。  |
| !<  | 检查左操作数的值是否不小于右操作数的值，如果是则条件为真。  | (a !< b) 为假。  |
| !>  | 检查左操作数的值是否不大于右操作数的值，如果是则条件为真。  | (a !> b) 为真。  |

##### 逻辑运算符
下面是 SQLite 中所有的逻辑运算符列表。

| 运算符     | 描述                                                                    |
| ------- | --------------------------------------------------------------------- |
| AND     | AND 运算符允许在一个 SQL 语句的 WHERE 子句中的多个条件的存在。                               |
| BETWEEN | BETWEEN 运算符用于在给定最小值和最大值范围内的一系列值中搜索值。                                  |
| EXISTS  | EXISTS 运算符用于在满足一定条件的指定表中搜索行的存在。                                       |
| IN      | IN 运算符用于把某个值与一系列指定列表的值进行比较。                                           |
| NOT IN  | IN 运算符的对立面，用于把某个值与不在一系列指定列表的值进行比较。                                    |
| LIKE    | LIKE 运算符用于把某个值与使用通配符运算符的相似值进行比较。                                      |
| GLOB    | GLOB 运算符用于把某个值与使用通配符运算符的相似值进行比较。GLOB 与 LIKE 不同之处在于，它是大小写敏感的。          |
| NOT     | NOT 运算符是所用的逻辑运算符的对立面。比如 NOT EXISTS、NOT BETWEEN、NOT IN，等等。**它是否定运算符。** |
| OR      | OR 运算符用于结合一个 SQL 语句的 WHERE 子句中的多个条件。                                  |
| IS NULL | NULL 运算符用于把某个值与 NULL 值进行比较。                                           |
| IS      | IS 运算符与 = 相似。                                                         |
| IS NOT  | IS NOT 运算符与 != 相似。                                                    |
| \|      | 连接两个不同的字符串，得到一个新的字符串。                                                 |
| UNIQUE  | UNIQUE 运算符搜索指定表中的每一行，确保唯一性（无重复）。                                      |

### Like子句
SQLite 的 **LIKE** 运算符是用来匹配通配符指定模式的文本值。如果搜索表达式与模式表达式匹配，LIKE 运算符将返回真（true），也就是 1。这里有两个通配符与 LIKE 运算符一起使用：
- 百分号 （%）
- 下划线 （\_）

百分号（%）代表零个、一个或多个数字或字符。下划线（_）代表一个单一的数字或字符。这些符号可以被组合使用。

% 和 _ 的基本语法如下：
```sql
SELECT column_list 
FROM table_name
WHERE column LIKE 'XXXX%'



SELECT column_list 
FROM table_name
WHERE column LIKE '%XXXX%'



SELECT column_list 
FROM table_name
WHERE column LIKE 'XXXX_'



SELECT column_list 
FROM table_name
WHERE column LIKE '_XXXX'



SELECT column_list 
FROM table_name
WHERE column LIKE '_XXXX_'

```

下面一些实例演示了 带有 '%' 和 '_' 运算符的 LIKE 子句不同的地方：

|语句|描述|
|---|---|
|WHERE SALARY LIKE '200%'|查找以 200 开头的任意值|
|WHERE SALARY LIKE '%200%'|查找任意位置包含 200 的任意值|
|WHERE SALARY LIKE '_00%'|查找第二位和第三位为 00 的任意值|
|WHERE SALARY LIKE '2_%_%'|查找以 2 开头，且长度至少为 3 个字符的任意值|
|WHERE SALARY LIKE '%2'|查找以 2 结尾的任意值|
|WHERE SALARY LIKE '_2%3'|查找第二位为 2，且以 3 结尾的任意值|
|WHERE SALARY LIKE '2___3'|查找长度为 5 位数，且以 2 开头以 3 结尾的任意值|

### Distinct 关键字
SQLite 的 **DISTINCT** 关键字与 SELECT 语句一起使用，来消除所有重复的记录，并只获取唯一一次记录。

有可能出现一种情况，在一个表中有多个重复的记录。当提取这样的记录时，DISTINCT 关键字就显得特别有意义，它只获取唯一一次记录，而不是获取重复记录。

用于消除重复记录的 DISTINCT 关键字的基本语法如下：
```sql
SELECT DISTINCT column1, column2,.....columnN 
FROM table_name
WHERE [condition]

```

假设 COMPANY 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0
8           Paul        24          Houston     20000.0
9           James       44          Norway      5000.0
10          James       45          Texas       5000.0

```

首先，让我们来看看下面的 SELECT 查询，它将返回重复的工资记录：
```sql
SELECT name FROM COMPANY;
```

这将产生以下结果：
```sql
NAME
----------
Paul
Allen
Teddy
Mark
David
Kim
James
Paul
James
James

```

现在，让我们在上述的 SELECT 查询中使用 **DISTINCT** 关键字：
```sql
SELECT DISTINCT name FROM COMPANY;
```

这将产生以下结果，没有任何重复的条目：
```sql
NAME
----------
Paul
Allen
Teddy
Mark
David
Kim
James

```

### Order By子句
SQLite 的 **ORDER BY** 子句是用来基于一个或多个列按升序或降序顺序排列数据。

ORDER BY 子句的基本语法如下：
```sql
SELECT column-list 
FROM table_name 
[WHERE condition] 
[ORDER BY column1, column2, .. columnN] [ASC | DESC];

```

- **ASC** 默认值，从小到大，升序排列
- **DESC** 从大到小，降序排列

您可以在 ORDER BY 子句中使用多个列，确保您使用的排序列在列清单中：
```sql
SELECT
   select_list
FROM
   table
ORDER BY
    column_1 ASC,
    column_2 DESC;
```

column_1 与 column_2 如果后面不指定排序规则，默认为 ASC 升序，以上语句按 column_1 升序，column_2 降序读取，等价如下语句：
```sql
SELECT
   select_list
FROM
   table
ORDER BY
    column_1,
    column_2 DESC;
```

column_1 与 column_2 如果后面不指定排序规则，默认为 ASC 升序，以上语句按 column_1 升序，column_2 降序读取，等价如下语句：
```sql
SELECT
   select_list
FROM
   table
ORDER BY
    column_1,
    column_2 DESC;
```

假设 COMPANY 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0

```

下面是一个实例，它会将结果按 SALARY 升序排序：
```sql
SELECT * FROM COMPANY ORDER BY SALARY ASC;
```

这将产生以下结果：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
7           James       24          Houston     10000.0
2           Allen       25          Texas       15000.0
1           Paul        32          California  20000.0
3           Teddy       23          Norway      20000.0
6           Kim         22          South-Hall  45000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0

```

下面是一个实例，它会将结果按 NAME 和 SALARY 升序排序：
```sql
SELECT * FROM COMPANY ORDER BY NAME, SALARY ASC;
```

这将产生以下结果：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
2           Allen       25          Texas       15000.0
5           David       27          Texas       85000.0
7           James       24          Houston     10000.0
6           Kim         22          South-Hall  45000.0
4           Mark        25          Rich-Mond   65000.0
1           Paul        32          California  20000.0
3           Teddy       23          Norway      20000.0

```

下面是一个实例，它会将结果按 NAME 降序排序：
```sql
SELECT * FROM COMPANY ORDER BY NAME DESC;
```

这将产生以下结果：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
3           Teddy       23          Norway      20000.0
1           Paul        32          California  20000.0
4           Mark        25          Rich-Mond   65000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0
5           David       27          Texas       85000.0
2           Allen       25          Texas       15000.0

```

### Group By子句
SQLite 的 **GROUP BY** 子句用于与 SELECT 语句一起使用，来对相同的数据进行分组。

在 SELECT 语句中，GROUP BY 子句放在 WHERE 子句之后，放在 ORDER BY 子句之前。

下面给出了 GROUP BY 子句的基本语法。GROUP BY 子句必须放在 WHERE 子句中的条件之后，必须放在 ORDER BY 子句
之前。
```sql
SELECT column-list
FROM table_name
WHERE [ conditions ]
GROUP BY column1, column2....columnN
ORDER BY column1, column2....columnN

```

您可以在 GROUP BY 子句中使用多个列。确保您使用的分组列在列清单中。

假设 `COMPANY` 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0

```

如果您想了解每个客户的工资总额，则可使用 GROUP BY 查询，如下所示：
```sql
SELECT NAME, SUM(SALARY) FROM COMPANY GROUP BY NAME;
```

这将产生以下结果：
```sql
NAME        SUM(SALARY)
----------  -----------
Allen       15000.0
David       85000.0
James       10000.0
Kim         45000.0
Mark        65000.0
Paul        20000.0
Teddy       20000.0

```

现在，让我们使用下面的 INSERT 语句在 COMPANY 表中另外创建三个记录：
```sql
INSERT INTO COMPANY VALUES (8, 'Paul', 24, 'Houston', 20000.00 );
INSERT INTO COMPANY VALUES (9, 'James', 44, 'Norway', 5000.00 );
INSERT INTO COMPANY VALUES (10, 'James', 45, 'Texas', 5000.00 );
```

现在，我们的表具有重复名称的记录，如下所示：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0
8           Paul        24          Houston     20000.0
9           James       44          Norway      5000.0
10          James       45          Texas       5000.0

```

让我们用同样的 GROUP BY 语句来对所有记录按 NAME 列进行分组，如下所示：
```sql
SELECT NAME, SUM(SALARY) FROM COMPANY GROUP BY NAME ORDER BY NAME;
```

这将产生以下结果：
```sql
NAME        SUM(SALARY)
----------  -----------
Allen       15000
David       85000
James       20000
Kim         45000
Mark        65000
Paul        40000
Teddy       20000

```

把 ORDER BY 子句与 GROUP BY 子句一起使用，如下所示：
```sql
SELECT NAME, SUM(SALARY) 
         FROM COMPANY GROUP BY NAME ORDER BY NAME DESC;
```
这将产生以下结果：
```sql
NAME        SUM(SALARY)
----------  -----------
Teddy       20000
Paul        40000
Mark        65000
Kim         45000
James       20000
David       85000
Allen       15000

```

### Having子句
HAVING 子句允许指定条件来过滤将出现在最终结果中的分组结果。

WHERE 子句在所选列上设置条件，而 HAVING 子句则在由 GROUP BY 子句创建的分组上设置条件。
下面是 HAVING 子句在 SELECT 查询中的位置：
```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY

```

在一个查询中，HAVING 子句必须放在 GROUP BY 子句之后，必须放在 ORDER BY 子句之前。下面是包含 HAVING 子句的 SELECT 语句的语法：
```sql
SELECT column1, column2
FROM table1, table2
WHERE [ conditions ]
GROUP BY column1, column2
HAVING [ conditions ]
ORDER BY column1, column2

```

假设 `COMPANY` 表有以下记录：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0
8           Paul        24          Houston     20000.0
9           James       44          Norway      5000.0
10          James       45          Texas       5000.0

```

下面是一个实例，它将显示名称计数小于 2 的所有记录：
```sql
SELECT * FROM COMPANY GROUP BY name HAVING count(name) < 2;
```

这将产生以下结果：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
2           Allen       25          Texas       15000
5           David       27          Texas       85000
6           Kim         22          South-Hall  45000
4           Mark        25          Rich-Mond   65000
3           Teddy       23          Norway      20000

```

### Unions 子句
SQLite的 **UNION** 子句/运算符用于合并两个或多个 SELECT 语句的结果，不返回任何重复的行。

为了使用 UNION，每个 SELECT 被选择的列数必须是相同的，相同数目的列表达式，相同的数据类型，并确保它们有相同的顺序，但它们不必具有相同的长度。

**UNION** 的基本语法如下：
```sql
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]

UNION

SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]

```

这里给定的条件根据需要可以是任何表达式。

假设有下面两个表，（1）COMPANY 表如下所示：
```sql
sqlite> select * from COMPANY;
ID          NAME                  AGE         ADDRESS     SALARY
----------  --------------------  ----------  ----------  ----------
1           Paul                  32          California  20000.0
2           Allen                 25          Texas       15000.0
3           Teddy                 23          Norway      20000.0
4           Mark                  25          Rich-Mond   65000.0
5           David                 27          Texas       85000.0
6           Kim                   22          South-Hall  45000.0
7           James                 24          Houston     10000.0

```

（2）另一个表是 DEPARTMENT，如下所示：
```sql
ID          DEPT                  EMP_ID
----------  --------------------  ----------
1           IT Billing            1
2           Engineering           2
3           Finance               7
4           Engineering           3
5           Finance               4
6           Engineering           5
7           Finance               6

```

现在，让我们使用 SELECT 语句及 UNION 子句来连接两个表，如下所示：
```sql
SELECT EMP_ID, NAME, DEPT FROM COMPANY INNER JOIN DEPARTMENT
        ON COMPANY.ID = DEPARTMENT.EMP_ID
   UNION
     SELECT EMP_ID, NAME, DEPT FROM COMPANY LEFT OUTER JOIN DEPARTMENT
        ON COMPANY.ID = DEPARTMENT.EMP_ID;
```

这将产生以下结果：
```sql
EMP_ID      NAME                  DEPT
----------  --------------------  ----------
1           Paul                  IT Billing
2           Allen                 Engineerin
3           Teddy                 Engineerin
4           Mark                  Finance
5           David                 Engineerin
6           Kim                   Finance
7           James                 Finance

```

### Join语句
SQLite 的 **Join** 子句用于结合两个或多个数据库中表的记录。JOIN 是一种通过共同值来结合两个表中字段的手段。

SQL 定义了三种主要类型的连接：
- 交叉连接 - CROSS JOIN
 
- 内连接 - INNER JOIN
 
- 外连接 - OUTER JOIN

在我们继续之前，让我们假设有两个表 COMPANY 和 DEPARTMENT。我们已经看到了用来填充 COMPANY 表的 INSERT 语句。现在让我们假设 COMPANY 表的记录列表如下：
```sql
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0

```

另一个表是 DEPARTMENT，定义如下：
```sql
CREATE TABLE DEPARTMENT(
   ID INT PRIMARY KEY      NOT NULL,
   DEPT           CHAR(50) NOT NULL,
   EMP_ID         INT      NOT NULL
);

```

下面是填充 DEPARTMENT 表的 INSERT 语句：
```sql
INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID)
VALUES (1, 'IT Billing', 1 );

INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID)
VALUES (2, 'Engineering', 2 );

INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID)
VALUES (3, 'Finance', 7 );

```

最后，我们在 DEPARTMENT 表中有下列的记录列表：
```sql
ID          DEPT        EMP_ID
----------  ----------  ----------
1           IT Billing  1
2           Engineerin  2
3           Finance     7

```

*  **交叉连接 - CROSS JOIN**
交叉连接（CROSS JOIN）把第一个表的每一行与第二个表的每一行进行匹配。如果两个输入表分别有 x 和 y 行，则结果表有 x\*y 行。由于交叉连接（CROSS JOIN）有可能产生非常大的表，使用时必须谨慎，只在适当的时候使用它们。
> 交叉连接的操作，它们都返回被连接的两个表所有数据行的笛卡尔积，返回到的数据行数等于第一个表中符合查询条件的数据行数乘以第二个表中符合查询条件的数据行数。

下面是交叉连接（CROSS JOIN）的语法：
```sql
SELECT ... FROM table1 CROSS JOIN table2 ...

```

基于上面的表，我们可以写一个交叉连接（CROSS JOIN），如下所示：
```sql
SELECT EMP_ID, NAME, DEPT FROM COMPANY CROSS JOIN DEPARTMENT;
```

上面的查询会产生以下结果：
```sql
EMP_ID      NAME        DEPT
----------  ----------  ----------
1           Paul        IT Billing
2           Paul        Engineerin
7           Paul        Finance
1           Allen       IT Billing
2           Allen       Engineerin
7           Allen       Finance
1           Teddy       IT Billing
2           Teddy       Engineerin
7           Teddy       Finance
1           Mark        IT Billing
2           Mark        Engineerin
7           Mark        Finance
1           David       IT Billing
2           David       Engineerin
7           David       Finance
1           Kim         IT Billing
2           Kim         Engineerin
7           Kim         Finance
1           James       IT Billing
2           James       Engineerin
7           James       Finance

```

* 内连接 - INNER JOIN
内连接（INNER JOIN）根据连接谓词结合两个表（table1 和 table2）的列值来创建一个新的结果表。查询会把 table1 中的每一行与 table2 中的每一行进行比较，找到所有满足连接谓词的行的匹配对。当满足连接谓词时，A 和 B 行的每个匹配对的列值会合并成一个结果行。

内连接（INNER JOIN）是最常见的连接类型，是默认的连接类型。INNER 关键字是可选的。

下面是内连接（INNER JOIN）的语法：
```sql
SELECT ... FROM table1 [INNER] JOIN table2 ON conditional_expression ...

```
为了避免冗余，并保持较短的措辞，可以使用 **USING** 表达式声明内连接（INNER JOIN）条件。这个表达式指定一个或多个列的列表：
```sql
SELECT ... FROM table1 JOIN table2 USING ( column1 ,... ) ...

```

自然连接（NATURAL JOIN）类似于 **JOIN...USING**，只是它会自动测试存在两个表中的每一列的值之间相等值：
```sql
SELECT ... FROM table1 NATURAL JOIN table2...
```
基于上面的表，我们可以写一个内连接（INNER JOIN），如下所示：
```sql
SELECT EMP_ID, NAME, DEPT FROM COMPANY INNER JOIN DEPARTMENT
        ON COMPANY.ID = DEPARTMENT.EMP_ID;
```

上面的查询会产生以下结果：
```sql
EMP_ID      NAME        DEPT
----------  ----------  ----------
1           Paul        IT Billing
2           Allen       Engineerin
7           James       Finance
```

* 外连接 - OUTER JOIN
外连接（OUTER JOIN）是内连接（INNER JOIN）的扩展。虽然 SQL 标准定义了三种类型的外连接：LEFT、RIGHT、FULL，但 SQLite 只支持 **左外连接（LEFT OUTER JOIN）**。

外连接（OUTER JOIN）声明条件的方法与内连接（INNER JOIN）是相同的，使用 ON、USING 或 NATURAL 关键字来表达。最初的结果表以相同的方式进行计算。一旦主连接计算完成，外连接（OUTER JOIN）将从一个或两个表中任何未连接的行合并进来，外连接的列使用 NULL 值，将它们附加到结果表中。

下面是左外连接（LEFT OUTER JOIN）的语法：
```sql
SELECT ... FROM table1 LEFT OUTER JOIN table2 ON conditional_expression ...

```

为了避免冗余，并保持较短的措辞，可以使用 **USING** 表达式声明外连接（OUTER JOIN）条件。这个表达式指定一个或多个列的列表：
```sql
SELECT ... FROM table1 LEFT OUTER JOIN table2 USING ( column1 ,... ) ...

```

基于上面的表，我们可以写一个外连接（OUTER JOIN），如下所示：
```sql
SELECT EMP_ID, NAME, DEPT FROM COMPANY LEFT OUTER JOIN DEPARTMENT
        ON COMPANY.ID = DEPARTMENT.EMP_ID;
```

上面的查询会产生以下结果：
```sql
EMP_ID      NAME        DEPT
----------  ----------  ----------
1           Paul        IT Billing
2           Allen       Engineerin
            Teddy
            Mark
            David
            Kim
7           James       Finance

```

## 约束
约束是在表的数据列上强制执行的规则，这些是用来限制可以插入到表中的数据类型，这确保了数据库中数据的准确性和可靠性。

约束可以是列级或表级。列级约束仅适用于列，表级约束被应用到整个表。

以下是在 SQLite 中常用的约束。

- **NOT NULL 约束**：确保某列不能有 NULL 值。
 
- **DEFAULT 约束**：当某列没有指定值时，为该列提供默认值。
 
- **UNIQUE 约束**：确保某列中的所有值是不同的。
 
- **PRIMARY KEY 约束**：唯一标识数据库表中的各行/记录。

- **CHECK 约束**：CHECK 约束确保某列中的所有值满足一定条件。

* **FOREIGN KEY 约束**：外键约束用来强制 两个表之间”存在”的关系。

### NOT NULL 约束
默认情况下，列可以保存 NULL 值。如果您不想某列有 NULL 值，那么需要在该列上定义此约束，指定在该列上不允许 NULL 值。

NULL 与没有数据是不一样的，它代表着未知的数据。
例如，下面的 SQLite 语句创建一个新的表 COMPANY，并增加了五列，其中 ID、NAME 和 AGE 三列指定不接受 NULL 值：
```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);

```

### DEFAULT 约束
DEFAULT 约束在 INSERT INTO 语句没有提供一个特定的值时，为列提供一个默认值。

例如，下面的 SQLite 语句创建一个新的表 COMPANY，并增加了五列。在这里，SALARY 列默认设置为 5000.00。所以当 INSERT INTO 语句没有为该列提供值时，该列将被设置为 5000.00。
```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL    DEFAULT 50000.00
);

```

### UNIQUE 约束
UNIQUE 约束防止在一个特定的列存在两个记录具有相同的值。在 COMPANY 表中，例如，您可能要防止两个或两个以上的人具有相同的年龄。

例如，下面的 SQLite 语句创建一个新的表 COMPANY，并增加了五列。在这里，AGE 列设置为 UNIQUE，所以不能有两个相同年龄的记录：
```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL UNIQUE,
   ADDRESS        CHAR(50),
   SALARY         REAL    DEFAULT 50000.00
);

```

### PRIMARY KEY 约束
PRIMARY KEY 约束唯一标识数据库表中的每个记录。在一个表中可以有多个 UNIQUE 列，但只能有一个主键。在设计数据库表时，主键是很重要的。主键是唯一的 ID。

我们使用主键来引用表中的行。可通过把主键设置为其他表的外键，来创建表之间的关系。由于"长期存在编码监督"，在 SQLite 中，主键可以是 NULL，这是与其他数据库不同的地方。

主键是表中的一个字段，唯一标识数据库表中的各行/记录。主键必须包含唯一值。主键列不能有 NULL 值。

一个表只能有一个主键，它可以由一个或多个字段组成。当多个字段作为主键，它们被称为**复合键**。

如果一个表在任何字段上定义了一个主键，那么在这些字段上不能有两个记录具有相同的值。

已经看到了我们创建以 ID 作为主键的 COMAPNY 表的各种实例：
```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);

```

### CHECK 约束
CHECK 约束启用输入一条记录要检查值的条件。如果条件值为 false，则记录违反了约束，且不能输入到表。

CHECK 约束启用输入一条记录要检查值的条件。如果条件值为 false，则记录违反了约束，且不能输入到表。
```sql
CREATE TABLE COMPANY3(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL    CHECK(SALARY > 0)
);

```

### FOREIGN KEY 约束
确定表与表之间的联系方式，一般情况下通过主表的标识列进行确定。

```sql
create table persons(
id p integer not null,
lastname varchar(20),
firstname varchar(20),
address varchar(100),
city varchar(100),
primary key(id p)
);
create table orders(
id_o integer not null,
orderno not null,
id_p integer,
primary key(id o),
foreign key(id p)references persons(id p)
)
```