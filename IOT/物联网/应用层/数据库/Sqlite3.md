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

