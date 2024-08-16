### 聚合函数:返回汇总值。
AVG(表达式) 返回表达式中所有的平均值。仅用于数字列并自动忽略NULL值。

COUNT(表达式) 返回表达式中非NULL值的数量。可用于数字和字符列。

COUNT(*) 返回表中的行数(包括有NULL值的列)。

MAX(表达式) 返回表达式中的最大值,忽略NULL值。可用于数字、字符和日期时间列。

MIN(表达式) 返回表达式中的最小值,忽略NULL值。可用于数字、字符和日期时间列。

SUM(表达式) 返回表达式中所有的总和,忽略NULL值。仅用于数字列。

### 转型函数:将一种数据类型转换为另外一种。

CONVERT(data_type[(length)], expression [, style])
例: Select convert(varchar(10) ,stuno) as stuno,stuname from student

CAST( expression AS data_type )（简单的类型转换-字符串转换为整数）
例: Select cast(stuno as varchar(10)) as stuno,stuname from student

### 日期函数:处理日期和时间。（不重要）
### 数学函数:执行算术运算。（不重要）
### 字符串函数:对字符串、二进制数据或表达式执行操作。（不重要）
### 系统函数:从数据库返回在SQLSERVER中的值、对象或设置的特殊信息。（不重要）
### 文本和图像函数:对文本和图像数据执行操作。（不重要）



