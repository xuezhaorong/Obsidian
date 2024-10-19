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
