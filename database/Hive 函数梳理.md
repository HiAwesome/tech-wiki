* [总览](#总览)
* [内置运算符](#内置运算符)
   * [运算符优先级](#运算符优先级)
   * [关系运算符](#关系运算符)
   * [算术运算符](#算术运算符)
   * [逻辑运算符](#逻辑运算符)
   * [复杂类型构造函数](#复杂类型构造函数)
   * [复杂类型上的运算符](#复杂类型上的运算符)
* [内置函数](#内置函数)
   * [数学函数](#数学函数)
   * [集合的函数](#集合的函数)
   * [类型转换函数](#类型转换函数)
   * [日期函数](#日期函数)
   * [条件函数](#条件函数)
   * [字符串函数](#字符串函数)
   * [数据隐藏函数](#数据隐藏函数)
   * [杂项函数](#杂项函数)
      * [Xpath UDF](#xpath-udf)
* [内置聚合函数 UDAF](#内置聚合函数-udaf)
* [内置表生成函数 UDTF](#内置表生成函数-udtf)
   * [使用范例](#使用范例)
      * [explode (array)](#explode-array)
      * [explode (map)](#explode-map)
      * [posexplode (array)](#posexplode-array)
      * [inline (array of structs)](#inline-array-of-structs)
      * [stack (values)](#stack-values)
   * [explode 爆炸](#explode-爆炸)
      * [Array](#array)
      * [Map](#map)
   * [posexplode 爆炸(带位置)](#posexplode-爆炸带位置)
   * [json_tuple](#json_tuple)
   * [parse_url_tuple](#parse_url_tuple)
* [对f（column）进行分组和排序](#对fcolumn进行分组和排序)
* [实用功能](#实用功能)
* [个人使用经验](#个人使用经验)
   * [固定范围随机数](#固定范围随机数)

# 总览

对 Hive 定义的不常用的运算符和函数进行梳理。

* 官网：[Hive UDF 官网](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF)
* 注意：不区分大小写：所有 Hive 关键字都不区分大小写，包括 Hive 运算符和函数的名称。
* 命令解释：使用以下命令显示最新文档：

```sql
-- 查看所有运算符函数
show functions;
-- 简单解释某个运算符或者函数
describe function <function_name>;
-- 完整解释某个运算符或者函数
describe function extended <function_name>;
```

# 内置运算符

## 运算符优先级

双竖线表示字符串连接，concat(A, B) 的简写，从[Hive 2.2.0](https://issues.apache.org/jira/browse/HIVE-14580)开始受支持：

```sql
-- A || B =>	字符串连接（||）=>	字符串连接
select 'name' || ' zhangsan';
-- name zhangsan
select concat('name', ' zhangsan');
-- name zhangsan
```

## 关系运算符

以下运算符比较传递的操作数，并根据操作数之间的比较是否成立来生成 TRUE 或 FALSE 值。

```sql
-- A <=> B => 所有原始类型
-- 对于非空操作数，使用EQUAL（=）运算符返回相同的结果；
-- 如果两个均为NULL，则返回TRUE；
-- 如果其中之一为NULL，则返回FALSE。（从0.9.0版开始。）
select 10 <=> 10;
-- true
select 10 <=> 2;
-- false
select 10 <=> null;
-- false
select null <=> null;
-- true

-- A <> B => 所有原始类型
-- 如果A或B为NULL，则为NULL；
-- 如果表达式A不等于表达式B，则为TRUE，否则为FALSE。
select 10 <> null;
-- null
select 10 <> 10;
-- false
select 10 <> 2;
-- true
```

## 算术运算符

以下运算符支持对操作数的各种常见算术运算。所有返回号码类型；如果任何操作数为NULL，则结果也为NULL。

```sql
-- A / B => 所有数字类型
-- 给出将A除以B的结果。在大多数情况下，该结果为双精度类型。
-- 当A和B都是整数时，结果是双精度类型，除非将hive.compat配置参数设置为"0.13"或"latest"，
-- 在这种情况下，结果是十进制类型。
select 10 / 3;
-- 3.3333333333333335
select 9 / 3;
-- 3.0

-- A DIV B => 整数类型	
-- 给出将 A 除以 B 最大的整数部分：例如 17 div 3 = 5
select 17 div 3;
-- 5
```

## 逻辑运算符

以下运算符为创建逻辑表达式提供支持。它们都根据操作数的布尔值返回布尔值TRUE，FALSE或NULL。NULL表现为“未知”标志，因此，如果结果取决于未知状态，则结果本身是未知的。

```sql
-- A IN（val1，val2，...） => 布尔值
-- 如果A等于任何值，则为TRUE。从Hive 0.13开始，IN语句支持子查询。
select 10 in (10,20);
-- true
select 10 in (20);
-- false
```

## 复杂类型构造函数

以下函数构造复杂类型的实例。

* 构造函数：表示：描述
* map：(key1, value1, key2, value2, ...)：使用给定的键/值对创建一个映射。
* struct：(val1, val2, val3, ...)：用给定的字段值创建一个结构。结构字段名称将为col1，col2，...。
* named_struct：(name1, val1, name2, val2, ...)：用给定的字段名称和值创建一个结构，从Hive [0.8.0开始](https://issues.apache.org/jira/browse/HIVE-1360)。
* array：(val1, val2, ...)：用给定的元素创建一个数组。
* create_union：(tag, val1, val2, ...)：使用tag参数指向的值创建联合类型。

```sql
select map(1,1,2,2,3,3);
-- {1:1,2:2,3:3}

select struct(1,2,3);
-- {"col1":1,"col2":2,"col3":3}

select named_struct('name', 'zhangsan', 'age', 10);
-- {"name":"zhangsan","age":10}

select array(1,2,4,5,3);
-- [1,2,4,5,3]

select create_union(1,2,3,4);
-- {1:3}
create table union_test(foo uniontype<int,double,array<string>,struct<a:int,b:string>>);
insert into table union_test select create_union(1, cast('100' as int), cast('1.234' as double), array('tom'), named_struct('a', 1, 'b', 'lisa'));
select * from union_test;
-- {1:1.234}
```

## 复杂类型上的运算符

以下运算符提供了访问复杂类型中的元素的机制。

* 操作符：操作类型：描述
* A[n]：A是一个数组，n是一个整数：返回数组A中的第n个元素。第一个元素的索引为0。例如，如果A是包含['foo'，'bar']的数组，则 A[0] 返回'foo'，而 A[1] 返回'bar'。
* M[key]：M是Map <K，V>并且键的类型为K：返回与映射中的键对应的值。例如，如果M是包含 {'f'->'foo'，'b'->'bar'，'all'->'foobar'} 的映射，则 M['all'] 返回'foobar'。
* S.x：S是一个Struct：返回S的x字段。例如，对于结构foobar {int foo，int bar}，foobar.foo返回存储在结构的foo字段中的整数。

```sql
select array(0,1,2,3)[2];
-- 2

select map(1,-1,2,-2,3,-8)[3];
-- -8

select named_struct('name', 'zhansgan', 'age', 18).age;
-- 18
```

# 内置函数

## 数学函数

Hive支持以下内置数学函数；当参数为NULL时，大多数返回NULL：

* bin（BIGINT a） => 以二进制格式返回数字（请参见[mysql-string-function-bin](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function_bin)）。

```sql
select bin(3449);
-- 110101111001

-- e() => 返回 e 的值
select e();
-- 2.718281828459045

-- pi() => 返回 pi 的值
select pi();
-- 3.141592653589793
```

## 集合的函数

Hive支持以下内置的集合的函数：

* 返回值：名称：描述
* int：size(Map<K.V>)：返回 Map 中的元素数。
* int：size(Array\<T\>)：返回 Array 中的元素数。
* array\<K\>：map_keys(Map<K.V>)：返回包含 Map K 的无序数组。
* array\<V>：map_values(Map<K.V>)：返回包含 Map V 的无序数组。
* boolean：array_contains(Array\<T\>, value)：如果数组包含值，则返回TRUE。
* array\<t\>：sort_array(Array\<T\>)：根据数组元素的自然顺序对输入数组进行升序排序并返回（从0.9.0版本开始）。

```sql
select size(map(1,1,2,2,3,3));
-- 3

select size(array(292));
-- 1

select map_keys(map(1,-1,2,-3));
-- [1,2]

select map_values(map(1,-1,2,-3));
-- [-1,-3]

select array_contains(array(1,2,3), 2);
-- true

select array_contains(array(1,2,3), 8);
-- false

select sort_array(array(1,2,4,5,3));
-- [1,2,3,4,5]
```

## 类型转换函数

Hive支持以下类型转换函数：

```sql
-- binary()：将参数转换为二进制。
select binary('tom smith')
-- tom smith

-- cast(expr as <type>): 将表达式expr的结果转换为<type>。
-- 例如，cast（'1'as BIGINT）会将字符串'1'转换为其整数表示。
-- 如果转换不成功，则返回null。
-- 如果cast（expr为boolean），则Hive对于非空字符串返回true。
select cast('1' as bigint);
-- 1
select cast('abc' as bigint);
-- NULL
select cast('abc' as boolean);
-- true
select cast('' as boolean);
-- false
select cast(null as boolean);
-- NULL
```

## 日期函数

Hive支持以下内置日期函数：

* 返回类型：方法名：描述
* string：from_unixtime(bigint unixtime[, string format])：将unix纪元 (1970-01-01 00:00:00 UTC) 的秒数转换为一个字符串，该字符串表示当前系统时区中该时刻的时间戳，格式为"1970-01-01 00:00:00"。
* bigint：unix_timestamp()：以秒为单位获取当前的Unix时间戳。此函数不是确定性的，其值在查询执行范围内也不是固定的，因此会阻止对查询的适当优化-自2.0版以来已弃用此函数，而推荐使用CURRENT_TIMESTAMP常量。
* bigint：unix_timestamp(string date)：yyyy-MM-dd HH:mm:ss使用默认时区和默认语言环境将时间字符串格式转换为Unix时间戳（以秒为单位），如果失败，则返回0。
* bigint：unix_timestamp(string date, string pattern)：将具有给定模式的时间字符串(请参阅[SimpleDateFormat]( http://docs.oracle.com/javase/tutorial/i18n/format/simpleDateFormat.html ))转换为Unix时间戳（以秒为单位），如果失败，则返回0。

```sql
select from_unixtime(unix_timestamp(current_timestamp), 'yyyy-MM-dd HH:mm:ss');
-- 2019-12-29 09:01:24
select from_unixtime(unix_timestamp(current_timestamp), 'yyyy-MM-dd');
-- 2019-12-29
select from_unixtime(unix_timestamp(current_timestamp), 'HH:mm:ss');
-- 09:02:33

select unix_timestamp('2019-08-08 12:33:34');
-- 1565238814

select unix_timestamp('2019-08-08 12:33:34', 'yyyy-MM-dd');
-- 1565193600
```

* pre 2.1.0: string；2.1.0 on: date：to_date(string timestamp)：返回时间戳字符串（Hive 2.1.0之前）的日期部分：to_date（“ 1970-01-01 00:00:00”）=“ 1970-01-01”。从Hive 2.1.0开始，返回日期对象。在Hive 2.1.0（[HIVE-13248](https://issues.apache.org/jira/browse/HIVE-13248)）之前，返回类型为String，因为创建方法时不存在Date类型。
* int：year(string date)：返回年份。
* int：quarter(date/timestamp/string)：返回季度。
* int：month(string date)：返回月份。
* int：day(string date) / dayofmonth(date)：返回月中天数。
* int：hour(string date)：返回小时数。
* int：minute(string date)：返回分钟数。
* int：second(string date)：返回秒数。
* int： weekofyear(string date)：返回年中星期数。
* int：extract(field FROM source)：抽取固定字段的值，参阅下面多种示例。从 source 中检索字段，例如天或小时（从Hive 2.2.0开始）。source 必须是日期，时间戳，时间间隔或可以转换为日期或时间戳的字符串。支持的字段包括：日，星期几，小时，分钟，月，季度，秒，周和年。
  * select extract(month from "2016-10-20") results in 10.
  * select extract(hour from "2016-10-20 05:06:07") results in 5.
  * select extract(dayofweek from "2016-10-20 05:06:07") results in 5.
  * select extract(month from interval '1-3' year to month) results in 3.
  * select extract(minute from interval '3 12:20:30' day to second) results in 20.
* int：datediff(string enddate, string startdate)：返回两段日期的差值，注意结束日期作为参数放在前面。

```sql
select to_date('1998-02-03');
-- 1998-02-03

select year('2019-08-08 12:33:34');
-- 2019

select quarter('2019-08-08 12:33:34');
-- 3

select month('2019-08-08 12:33:34');
-- 8

select day('2019-08-08 12:33:34');
-- 8

select dayofmonth('2019-08-08 12:33:34');
-- 8

select hour('2019-08-08 12:33:34');
-- 12

select minute('2019-08-08 12:33:34');
-- 33

select second('2019-08-08 12:33:34');
-- 34

select weekofyear('2019-08-08 12:33:34');
-- 32

select extract(month from '2019-08-08 12:33:34');
-- 8

select extract(hour from '2019-08-08 12:33:34');
-- 12

select extract(month from interval '1-2' year to month);
-- 2

select extract(minute from interval '5 11:11:11' day to second);
-- 11

select datediff('2019-08-08', '2019-12-09');
-- -123
```

* pre 2.1.0: string；2.1.0 on: date：date_add(date/timestamp/string startdate, tinyint/smallint/int days)：添加开始日期的天数：date_add('2008-12-31', 1) = '2009-01-01'。在Hive 2.1.0([HIVE-13248](https://issues.apache.org/jira/browse/HIVE-13248))之前，返回类型为String，因为创建方法时不存在Date类型。

* pre 2.1.0: string；2.1.0 on: date：date_sub(date/timestamp/string startdate, tinyint/smallint/int days)：减去开始日期的天数：date_sub('2008-12-31', 1) = '2008-12-30'。在Hive 2.1.0([HIVE-13248](https://issues.apache.org/jira/browse/HIVE-13248))之前，返回类型为String，因为创建方法时不存在Date类型。

* timestamp：from_utc_timestamp({any primitive type} ts, string timezone)：将UTC中的timestamp *转换为给定的时区（从Hive [0.8.0开始](https://issues.apache.org/jira/browse/HIVE-2272)）。时间戳是一种原始类型，包括时间戳/日期，tinyint / smallint / int / bigint，float / double和十进制。 小数部分被视为秒。整数值以毫秒为单位。例如：from_utc_timestamp(2592000.0,'PST'),  from_utc_timestamp(2592000000,'PST') and from_utc_timestamp(timestamp '1970-01-30 16:00:00','PST') 都会返回 timestamp 1970-01-30 08:00:00.

* timestamp：to_utc_timestamp({any primitive type} ts, string timezone)：将UTC中的timestamp *转换为给定的时区（从Hive [0.8.0开始](https://issues.apache.org/jira/browse/HIVE-2272)）。时间戳是一种原始类型，包括时间戳/日期，tinyint / smallint / int / bigint，float / double和十进制。 小数部分被视为秒。整数值以毫秒为单位。例如：to_utc_timestamp(2592000.0,'PST'), to_utc_timestamp(2592000000,'PST') and to_utc_timestamp(timestamp '1970-01-30 16:00:00','PST') 都会返回 timestamp 1970-01-31 00:00:00.

* date：current_date：返回查询评估开始时的当前日期（从Hive [1.2.0开始](https://issues.apache.org/jira/browse/HIVE-5472)）。同一查询中对 current_date 的所有调用均返回相同的值。

* timestamp：current_timestamp：返回查询评估开始时的当前时间戳（从Hive [1.2.0开始](https://issues.apache.org/jira/browse/HIVE-5472)）。同一查询中对 current_timestamp 的所有调用均返回相同的值。

* string：add_months(string start_date, int num_months, output_date_format)：返回起始日期之后num_months的日期（从Hive [1.1.0开始](https://issues.apache.org/jira/browse/HIVE-9357)）。start_date是字符串，日期或时间戳。num_months是一个整数。如果start_date是该月的最后一天，或者如果结果月份的天数少于start_date的天部分，则结果是结果月份的最后一天。否则，结果与start_date具有相同的日组成部分。默认输出格式为“ yyyy-MM-dd”。

  在Hive 4.0.0之前，日期的时间部分将被忽略。从Hive [4.0.0开始](https://issues.apache.org/jira/browse/HIVE-19370)，add_months支持可选参数output_date_format，该参数接受一个String，该String表示输出的有效日期格式。这样可以在输出中保留时间格式。例如 ：add_months（'2009-08-31'，1）返回'2009-09-30'。 add_months（'2017-12-31 14:15:16'，2，'YYYY-MM-dd HH：mm：ss'）返回'2018-02-28 14:15:16'。

* string：last_day(string date)：返回日期所属月份的最后一天（从Hive [1.1.0开始](https://issues.apache.org/jira/browse/HIVE-9358)）。date是格式为“ yyyy-MM-dd HH：mm：ss”或“ yyyy-MM-dd”的字符串。日期的时间部分将被忽略。

* string：next_day(string start_date, string day_of_week)：返回第一个日期，该日期晚于start_date，并命名为day_of_week （从Hive [1.2.0开始](https://issues.apache.org/jira/browse/HIVE-9520)）。start_date是字符串/日期/时间戳。day_of_week是2个字母，3个字母或一周中某天的全名（例如Mo，tue，FRIDAY）。start_date的时间部分将被忽略。例如：next_day（'2015-01-14'，'TU'）= 2015-01-20。

* string：trunc(string date, string format)：返回截断为格式指定单位的日期（从Hive [1.2.0开始](https://issues.apache.org/jira/browse/HIVE-9480)）。支持的格式：MONTH / MON / MM，YEAR / YYYY / YY。示例：trunc（'2015-03-17'，'MM'）= 2015-03-01。

* double：months_between(date1, date2)：返回日期date1和date2之间的月份数（从Hive [1.2.0开始](https://issues.apache.org/jira/browse/HIVE-9518)）。如果date1晚于date2，则结果为正。如果date1早于date2，则结果为负。如果date1和date2是月份的同一天或月份的最后几天，则结果始终是整数。否则，UDF将基于31天的月份来计算结果的分数部分，并考虑时间分量date1和date2的差异。date1和date2类型可以是日期，时间戳或字符串，格式为“ yyyy-MM-dd”或“ yyyy-MM-dd HH：mm：ss”。结果四舍五入到小数点后8位。例如：months_between（'1997-02-28 10:30:00'，'1996-10-30'）= 3.94959677 

* string：date_format(date/timestamp/string ts, string fmt)：将日期/时间戳记/字符串转换为日期格式fmt指定的格式的字符串值（从Hive [1.2.0开始](https://issues.apache.org/jira/browse/HIVE-10276)）。支持的格式是[SimpleDateFormat](https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html)。第二个参数fmt应该是常量。示例：date_format（'2015-04-08'，'y'）='2015'。 date_format可用于实现其他UDF，例如：dayname(date) 是 date_format(date，'EEEE'); dayofyear(date)是date_format(date，'D').

```sql
select date_add('2018-12-12', 2);
-- 2018-12-14

select date_sub('2018-12-12', 2);
-- 2018-12-10

select from_utc_timestamp(2592000000,'PST');
-- 1970-01-31 00:00:00

select to_utc_timestamp(2592000000,'PST');
-- 1970-01-31 16:00:00

select current_date;
-- 2019-12-29

select current_timestamp;
-- 2019-12-29 09:58:36.009

select add_months('2019-08-31', 1);
-- 2019-09-30

select last_day('2019-08-03');
-- 2019-08-31

select next_day('2019-12-29', 'Mo');
-- 2019-12-30

select trunc('2015-03-17', 'MM');
-- 2015-03-01

select trunc('2015-03-17', 'YYYY');
-- 2015-01-01

select months_between('2019-01-01', '2019-08-08');
-- -7.22580645

select date_format(current_date, 'MM-dd');
-- 12-29
```

## 条件函数

* 返回值类型：名称：解释
* T：if(boolean testCondition, T valueTrue, T valueFalseOrNull)：当testCondition为true时返回valueTrue，否则返回valueFalseOrNull。
* T：COALESCE(T v1, T v2, ...)：返回第一个不为NULL的v，如果所有v均为NULL，则返回NULL。
* T：CASE a WHEN b THEN c [WHEN d THEN e]* [ELSE f] END：当a = b时，返回c; 当a = d时，返回e; 否则返回f。
* T：CASE WHEN a THEN b [WHEN c THEN d]* [ELSE e] END：当a = true时，返回b; 当c = true时，返回d; 否则返回e。
* T：nullif( a, b )：如果a = b，则返回NULL。否则返回a （从Hive [2.3.0开始](https://issues.apache.org/jira/browse/HIVE-13555)）。简写：CASE WHEN a = b则为NULL否则a。

```sql
select if(10 > 1, 100, 0);
-- 100

select if(10 < 1, 100, 0);
-- 0

select coalesce(null, 10, null);
-- 10

select coalesce(null, null, null);
-- NULL

select case 10 when 10 then 10 else -1 end;
-- 10

select case 10 when 1000 then 10 else -1 end;
-- -1

select case when 10 > 1 then 100 else -1 end;
-- 100

select case when 10 < 1 then 100 else -1 end;
-- -1

select nullif(10, 10);
-- NULL

select nullif(10, 9);
-- 10
```

## 字符串函数

由于字符串函数较多，小部分需要着重说明的放在正文，其他放在代码块中。Hive支持以下内置的String函数：

* 返回值类型：函数名称：说明
* array<struct<string,double>>：context_ngrams(array<array\<string\>>, array\<string\>, int K, int pf)：给定字符串“ context”，从一组标记化语句返回前k个上下文N-gram。有关更多信息，请参见[StatisticsAndDataMining](https://cwiki.apache.org/confluence/display/Hive/StatisticsAndDataMining)。
* string：elt(N int,str1 string,str2 string,str3 string,...)：返回索引号处的字符串。例如elt(2，'hello'，'world')返回'world'。如果N小于1或大于参数个数，则返回NULL。(请参阅[mysql-function-elt](https://dev.mysql.com/doc/refman/5.7/zh-CN/string-functions.html#function_elt))。
* int：field(val T,val1 T,val2 T,val3 T,...)：返回val1，val2，val3，...列表中val的索引；如果未找到，则返回0。例如，字段（“ world”，“ say”，“ hello”，“ world”）返回3。支持所有原始类型，使用str.equals（x）比较参数。如果val为NULL，则返回值为0。(请参阅[mysql-function-field](https://dev.mysql.com/doc/refman/5.7/en/string-functions.html#function_field))。
* string：format_number(number x, int d)：将数字X格式化为 '#,##,##.#' 之类的格式，四舍五入到D小数位，然后将结果作为字符串返回。如果D为0，则结果没有小数点或小数部分。(在 [Hive 0.10.0](https://issues.apache.org/jira/browse/HIVE-2694) 开始支持 ;在 [Hive 0.14.0](https://issues.apache.org/jira/browse/HIVE-7257) 发现 float 类型有 bug ，引入 decimal 类型在 [Hive 0.14.0](https://issues.apache.org/jira/browse/HIVE-7279))。


```sql
-- 返回str的第一个字符的数值。
select ascii('name');
-- 110

-- 将参数从二进制转换为基本64字符串（从Hive 0.12.0开始）。
select base64(binary('name'));
-- bmFtZQ==

-- 返回str中包含的UTF-8字符数（从Hive 2.2.0开始）。函数char_length是该函数的简写。
select character_length('name');
-- 4

-- 返回具有与A等效的二进制值的ASCII字符（从Hive 1.3.0和2.1.0开始）。
-- 如果A大于256，则结果等于chr（A％256）。示例：选择chr（88）; 返回“ X”。
select chr(100);
-- d

-- 返回按顺序串联作为参数传入的字符串或字节所得到的字符串或字节。
-- 例如，concat（'foo'，'bar'）的结果为'foobar'。
-- 请注意，此函数可以接受任意数量的输入字符串。
select concat('a', 'b');
-- ab

-- 与上面的concat（）类似，但具有自定义分隔符SEP。
select concat_ws('zzz', 'a', 'b', 'c');
-- azzzbzzzc

-- 就像上面的concat_ws（）一样，但是采用字符串数组。（从Hive 0.9.0开始）
select concat_ws('tom', array('1', '2', '888'));
-- 1tom2tom888

-- 使用提供的字符集('US-ASCII', 'ISO-8859-1', 'UTF-8', 'UTF-16BE', 'UTF-16LE', 'UTF-16')。
-- 如果任一参数为null，则结果也将为null。（从Hive 0.12.0开始。）
select decode(binary('name'), 'UTF-8');
-- name

select elt(2, 'tom', 'smith');
-- smith

-- 使用提供的字符集('US-ASCII', 'ISO-8859-1', 'UTF-8', 'UTF-16BE', 'UTF-16LE', 'UTF-16')。
-- 如果任一参数为null，则结果也将为null。（从Hive 0.12.0开始。）
select encode('tom', 'UTF-8');
-- tom

select field('word', 'hello', 'world', 'word');
-- 3

-- 返回str在strList中的第一次出现，其中strList是一个逗号分隔的字符串。
-- 如果任一参数为null，则返回null。
-- 如果第一个参数包含逗号，则返回0。
-- 例如，find_in_set('ab', 'abc,b,ab,c,def') 返回3。
select find_in_set('ab', 'abc,b,ab,c,def');
-- 3

select format_number(100.234, 5);
-- 100.23400
select format_number(100.234, 0);
-- 100

-- 根据指定的json路径从json字符串中提取json对象，并返回提取的json对象的json字符串。
-- 如果输入的json字符串无效，它将返回null。
-- 注意：json路径只能包含字符[0-9a-z_]，即不能包含大写或特殊字符。
-- 另外，键不能以数字开头，这是由于对Hive列名的限制。
select get_json_object('{"name":"zhangsan", "age":18}', '$.name');
-- zhangsan
select get_json_object('{"name":"zhangsan", "age":18}', '$.age');
-- 18
select get_json_object('{"time": [1,2,3]}', '$.time');
-- [1,2,3]
select get_json_object('{"time": [1,2,3]}', '$.time[1]');
-- 2
select get_json_object('{"account": {"weibo": "zhangsan_de_weibo"}}', '$.account');
-- {"weibo":"zhangsan_de_weibo"}
select get_json_object('{"account": {"weibo": "zhangsan_de_weibo"}}', '$.account.weibo');
-- zhangsan_de_weibo

-- 返回substr中第一次出现在str的位置n。返回null的如果任一参数是null，返回0如果str不包含substr。
-- 请注意，这不是基于零的。中的第一个字符的str索引为1。
select instr('hello world', 'world');
-- 7

-- 返回字符串的长度。
select length('time');
4

-- 返回在位置pos之后的str中第一次出现substr的位置，pos省略则为1。
select locate('time', 'aaatime');
-- 4
select locate('time', 'aaatimetime', 5);
-- 8

-- 小写
select lower('Tom Simth');
-- tom simth
```

* string：lpad(string str, int len, string pad)：返回str，在其左边填充pad，长度为len。如果str大于len，则返回值缩短为len个字符。如果填充字符串为空，则返回值为null。
* array<struct<string,double>>：ngrams(array<array\<strin\g>>, int N, int K, int pf)：从一组标记化的句子中返回前k个N-gram，例如 sentences() UDAF 返回的句子。有关更多信息，请参见[StatisticsAndDataMining](https://cwiki.apache.org/confluence/display/Hive/StatisticsAndDataMining)。
* int：octet_length(string str)：返回以UTF-8编码保存字符串str所需的八位字节数（从Hive [2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15979)）。请注意，octet_length（str）可以大于character_length（str）。
* string：parse_url(string urlString, string partToExtract [, string keyToExtract])：从URL返回指定的部分。partToExtract的有效值包括HOST，PATH，QUERY，REF，PROTOCOL，AUTHORITY，FILE和USERINFO。例如，parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'HOST') 返回 'facebook.com'。通过将键作为第三个参数，也可以提取QUERY中特定键的值，例如 parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'QUERY', 'k1')  返回'v1'。
* string：regexp_extract(string subject, string pattern, int index)：返回使用模式提取的字符串。例如regexp_extract('foothebar', 'foo(.*?)(bar)', 2) 返回'bar'。请注意，使用预定义的字符类时必须格外小心：使用'\s'作为第二个参数将与字母s匹配；'\\\s'是匹配空格等所必需的。'index'参数是Java regex Matcher group（）方法的索引。有关'index'或Java regex group() 方法的更多信息，请参见[Matcher.html](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html)。
* string：regexp_replace(string INITIAL_STRING, string PATTERN, string REPLACEMENT)：返回将替换INITIAL_STRING中所有与PATTERN中定义的Java正则表达式语法匹配的子字符串替换为REPLACEMENT的实例所产生的字符串。例如，regexp_replace("foobar", "oo|ar", "") 返回'fb'。请注意，使用预定义的字符类时必须格外小心：使用'\s'作为第二个参数将与字母s匹配；'\\\s'是匹配空格等所必需的。


```sql
select lpad('abc', 10, 'tom');
-- tomtomtabc
select lpad('abc', 2, 'tom');
-- ab
select lpad('abc', 10, null);
--NULL

-- 返回从A的开头（左侧）起修剪空格所得的字符串。例如，ltrim( 'foobar') 的结果为'foobar'。
select ltrim('              tom');
-- tom

select octet_length('tom smith wow');
-- 13

select parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1','HOST');
-- facebook.com
select parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'QUERY', 'k1');
-- v1
select parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'PATH');
-- /path1/p.php
select parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'REF');
-- Ref1
select parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'PROTOCOL');
-- http
select parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'AUTHORITY');
-- facebook.com
select parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'FILE');
-- /path1/p.php?k1=v1&k2=v2
select parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'USERINFO');
-- NULL

-- 返回根据do printf样式格式字符串格式化的输入（从Hive 0.9.0开始）。
select printf('%.2f', pi());
-- 3.14
select printf('%.10f', pi());
-- 3.1415926536

select regexp_extract('foothebar', 'foo(.*?)(bar)', 2);
-- bar

select regexp_replace("foobar", "oo|ar", "");
-- fb

-- 重复str n次。
select repeat('time ', 10);
-- time time time time time time time time time time
```

* string：replace(string A, string OLD, string NEW)：返回字符串A，其中所有不重叠的OLD都替换为NEW（从[Hive 1.3.0和2.1.0开始](https://issues.apache.org/jira/browse/HIVE-13063)）。Example: select replace("ababab", "abab", "Z"); returns "Zab".
* array<array\<string\>>：sentences(string str, string lang, string locale)：将一串自然语言文本标记为单词和句子，其中每个句子在适当的句子边界处断开并作为单词数组返回。“ lang”和“ locale”是可选参数。For example, sentences('Hello there! How are you?') returns ( ("Hello", "there"), ("How", "are", "you") ).
* array\<string\>：split(string str, string pat)：在pat周围拆分str（pat是一个正则表达式）。
* map<string,string>：str_to_map(text[, delimiter1, delimiter2])：使用两个定界符将文本拆分为键/值对。Delimiter1将文本分成KV对，Delimiter2将每个KV对分开。默认的定界符是','代表定界符1, ':'代表定界符2。
* string：substr(string|binary A, int start) substring(string|binary A, int start)：返回A的字节数组的子字符串或切片，从字符串的起始位置开始到字符串A的结尾。For example, substr('foobar', 4) results in 'bar' (see [mysql-function-substr](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function_substr)).
* string：substr(string|binary A, int start, int len) substring(string|binary A, int start, int len)：从长度为len的起始位置返回A的字节数组的子字符串或切片。For example, substr('foobar', 4, 1) results in 'b'  (see [mysql-function-substr](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function_substr)).
* string：substring_index(string A, string delim, int count)：在计数出现定界符delim之前，从字符串A返回子字符串（从Hive [1.3.0开始](https://issues.apache.org/jira/browse/HIVE-686)）。如果count为正，则返回最后定界符左侧的所有内容（从左侧开始计数）。如果count为负，则返回最后定界符右边的所有内容（从右边开始计数）。搜索delim时，Substring_index执行区分大小写的匹配。Example: substring_index('www.apache.org', '.', 2) = 'www.apache'。

```sql
select replace("ababab", "abab", "Z");
-- Zab
select replace("abababgggggggabab", "abab", "Z");
-- ZabgggggggZ

-- 反转字符串
select reverse('time');
-- emit

-- 返回str，右用pad填充到len的长度。
-- 如果str大于len，则返回值缩短为len个字符。
-- 如果填充字符串为空，则返回值为null。
select rpad('abc', 10, 'tom');
-- abctomtomt
select rpad('abc', 2, 'tom');
-- ab
select rpad('abc', 10, null);
--NULL

-- 返回从A的结尾（右侧）起修剪空格所得的字符串。例如，ltrim( 'foobar') 的结果为'foobar'。
select rtrim('simth      ');
-- smith

select sentences('Hello there! How are you?');
-- [["Hello","there"],["How","are","you"]]
select sentences('你好，今天去哪里玩', 'zh', 'cn');
-- [["你好","今天去哪里玩"]]

-- spece(int n) => 返回 n 个空格的字符串
select concat('tom', space(10), 'smith');
-- tom          smith

select split('1,2,3,4,5' , ',');
-- ["1","2","3","4","5"]

select str_to_map('name:zhangsan,age:18');
-- {"name":"zhangsan","age":"18"}

select substr('facebook', 5);
-- book

select substr('facebook', 5, 2);
-- bo

select substring_index('www.apache.org', '.', 2);
-- www.apache
select substring_index('www.apache.org', '.', 3);
-- www.apache.org
select substring_index('www.apache.org', '.', 1);
-- www
select substring_index('www.apache.org', '.', 4);
-- www.apache.org
```

* string：translate(string|char|varchar input, string|char|varchar from, string|char|varchar to)：通过将字符串中存在的字符替换为 from 字符串中的相应字符来翻译输入 to 字符串。这类似于[PostgreSQL中](http://www.postgresql.org/docs/9.1/interactive/functions-string.html)的 translate 功能。如果此UDF的任何参数为NULL，则结果也为NULL。（自Hive [0.10.0起](https://issues.apache.org/jira/browse/HIVE-2418)，适用于字符串类型）从[Hive 0.14.0开始](https://issues.apache.org/jira/browse/HIVE-6622)添加了对Char / varchar的支持。

```sql
select translate('12345', '143', 'tom');
-- t2mo5
select translate('12345', '143', 'to');
-- t2o5
select translate('12345', '143', 'op');
-- o2p5
select translate('12345', '1435', 'op');
-- o2p

-- 返回由A两端的空格修剪产生的字符串。例如，trim（' foobar '）结果为'foobar'
select trim('   fac   ');
-- fac

-- 将参数从基数为64的字符串转换为BINARY。（从Hive 0.12.0开始。）
select unbase64('bmFtZQ==');
-- name

-- 大写
select upper('tom');
-- TOM

-- 单词首字母大写
select initcap('tom smith');
-- Tom Smith

-- 返回两个字符串之间的Levenshtein距离（从Hive 1.2.0开始）。
-- 例如，levenshtein（'kitten'，'sitting'）得出3。
select levenshtein('tom', 'smith');
-- 5

-- 返回字符串的soundex代码（从Hive 1.2.0开始）。例如，soundex（'Miller'）生成M460。
select soundex('tom');
-- T500
```

## 数据隐藏函数

Hive支持以下内置数据隐藏函数：

* 返回值类型：函数名：描述
* string：mask(string str[, string upper[, string lower[, string number]]])：返回str的掩码版本（从Hive 2.1.0开始）。默认情况下，大写字母转换为“ X”，小写字母转换为“ x”，数字转换为“ n”。例如mask（“ abcd-EFGH-8765-4321”）的结果为xxxx-XXXX-nnnn-nnnn。您可以通过提供其他参数来覆盖掩码中使用的字符：第二个参数控制大写字母的掩码字符，第三个参数控制小写字母的字符，第四个参数控制数字的字符。For example, mask("abcd-EFGH-8765-4321", "U", "l", "#") results in llll-UUUU-####-####.

* string：mask_first_n(string str[, int n])：返回str的掩码版本，显示未掩码的最后n个字符（从Hive [2.1.0开始](https://issues.apache.org/jira/browse/HIVE-13568)）。大写字母转换为“ X”，小写字母转换为“ x”，数字转换为“ n”。For example, mask_first_n("1234-5678-8765-4321", 4) results in nnnn-5678-8765-4321.
* string：mask_last_n(string str[, int n])：返回带有掩码的最后一个n值的str的掩码版本（从Hive [2.1.0开始](https://issues.apache.org/jira/browse/HIVE-13568)）。大写字母转换为“ X”，小写字母转换为“ x”，数字转换为“ n”。 For example, mask_last_n("1234-5678-8765-4321", 4) results in 1234-5678-8765-nnnn.
* string：mask_show_first_n(string str[, int n])：返回带掩码的str版本，显示未掩码的前n个字符（从Hive [2.1.0开始](https://issues.apache.org/jira/browse/HIVE-13568)）。大写字母转换为“ X”，小写字母转换为“ x”，数字转换为“ n”。 For example, mask_show_first_n("1234-5678-8765-4321", 4) results in 1234-nnnn-nnnn-nnnn.
* string：mask_show_last_n(string str[, int n])：返回str的掩码版本，显示未掩码的最后n个字符（从Hive [2.1.0开始](https://issues.apache.org/jira/browse/HIVE-13568)）。大写字母转换为“ X”，小写字母转换为“ x”，数字转换为“ n”。For example, mask_show_last_n("1234-5678-8765-4321", 4) results in nnnn-nnnn-nnnn-4321.
* string：mask_hash(string|char|varchar str)：返回基于str的哈希值（从Hive [2.1.0开始](https://issues.apache.org/jira/browse/HIVE-13568)）。哈希是一致的，可用于将跨表的掩码值连接在一起。对于非字符串类型，此函数返回null。

```sql
select mask("abcd-EFGH-8765-4321", "U", "l", "#");
-- llll-UUUU-####-####

select mask_first_n("1234-5678-8765-4321", 4);
-- nnnn-5678-8765-4321

select mask_last_n("1234-5678-8765-4321", 8);
-- 1234-5678-8nnn-nnnn

select mask_last_n("1234-5678-8765-4321", 9);
-- 1234-5678-nnnn-nnnn

select mask_show_first_n("1234-5678-8765-4321", 4);
-- 1234-nnnn-nnnn-nnnn

select mask_show_last_n("1234-5678-8765-4321", 4) ;
-- nnnn-nnnn-nnnn-4321

select mask_hash('1234-5678-8765-4321');
-- 6b3f97b404e2baafb64d7978373fcf0f
```

## 杂项函数

* 返回值类型：函数名：描述
* varies：java_method(class, method[, arg1[, arg2..]])： reflect 的同义词. (As of Hive 0.9.0.)
* varies：reflect(class, method[, arg1[, arg2..]])：通过使用反射匹配参数签名来调用Java方法。（从Hive [0.7.0开始](https://issues.apache.org/jira/browse/HIVE-471)。）有关示例，请参见[反射（通用）UDF](https://cwiki.apache.org/confluence/display/Hive/ReflectUDF)。
* int：hash(a1[, a2...])：返回参数的哈希值。（从Hive 0.4开始。）
* string：current_user()：从配置的身份验证器管理器（从Hive [1.2.0开始](https://issues.apache.org/jira/browse/HIVE-9143)）返回当前用户名。可以与连接时提供的用户相同，但是与某些身份验证管理器（例如HadoopDefaultAuthenticator）不同。
* string：logged_in_user()：从会话状态返回当前的用户名（从Hive [2.2.0开始](https://issues.apache.org/jira/browse/HIVE-14100)）。这是连接到Hive时提供的用户名。
* string：current_database()：返回当前数据库名称（从Hive [0.13.0开始](https://issues.apache.org/jira/browse/HIVE-4144)）。
* string：md5(string/binary)：计算字符串或二进制文件的MD5 128位校验和（从Hive [1.3.0开始](https://issues.apache.org/jira/browse/HIVE-10485)）。该值以32个十六进制数字的字符串形式返回，如果参数为NULL，则返回NULL。 Example: md5('ABC') = '902fbdd2b1df0c4f70b4a5d23525e932'.
* string：sha1(string/binary) or sha(string/binary)：计算字符串或二进制文件的SHA-1摘要，并以十六进制字符串形式返回值(从Hive [1.3.0开始](https://issues.apache.org/jira/browse/HIVE-10639))。 sha1('ABC') = '3c01bdbb26f358bab27f267924aa2c9a03fcfdb8'.
* bigint：crc32(string/binary)：计算字符串或二进制参数的循环冗余校验值，并返回bigint值（从Hive [1.3.0开始](https://issues.apache.org/jira/browse/HIVE-10641)）。Example: crc32('ABC') = 2743272264.
* string：sha2(string/binary, int)：计算SHA-2系列哈希函数（SHA-224，SHA-256，SHA-384和SHA-512）（从Hive [1.3.0开始](https://issues.apache.org/jira/browse/HIVE-10644)）。第一个参数是要哈希的字符串或二进制。第二个参数表示结果的所需位长度，该位长度必须具有224、256、384、512或0（等于256）的值。从Java 8开始支持SHA-224。如果任一参数为NULL或哈希长度不是允许的值之一，则返回值为NULL。Example: sha2('ABC', 256) = 'b5d4045c3f466fa91fe2cc6abe79232a1a57cdf104f7a26e716e0a1e2789df78'.
* binary：aes_encrypt(input string/binary, key string/binary)：使用AES加密输入（自Hive [1.3.0起](https://issues.apache.org/jira/browse/HIVE-11593)）。可以使用128、192或256位的密钥长度。如果安装了Java密码学扩展（JCE）无限强度管辖权策略文件，则可以使用192位和256位密钥。如果任一参数为NULL或密钥长度不是允许的值之一，则返回值为NULL。Example: base64(aes_encrypt('ABC', '1234567890123456')) = 'y6Ss+zCYObpCbgfWfyNWTw=='.
* binary：aes_decrypt(input binary, key string/binary)：使用AES解密输入（自Hive [1.3.0起](https://issues.apache.org/jira/browse/HIVE-11593)）。可以使用128、192或256位的密钥长度。如果安装了Java密码学扩展（JCE）无限强度管辖权策略文件，则可以使用192位和256位密钥。如果任一参数为NULL或密钥长度不是允许的值之一，则返回值为NULL。Example: aes_decrypt(unbase64('y6Ss+zCYObpCbgfWfyNWTw=='), '1234567890123456') = 'ABC'.
* string：version()：返回Hive版本（从Hive [2.1.0开始](https://issues.apache.org/jira/browse/HIVE-12983)）。该字符串包含2个字段，第一个是内部版本号，第二个是内部散列。Example: "select version();" might return "2.1.0.2.5.0.0-1245 r027527b9c5ce1a3d7d0b6d2e6de2378fb0c39232". 实际结果将取决于您的构建。
* bigint：surrogate_key([write_id_bits, task_id_bits])：在向表中输入数据时自动为行生成数字ID。只能用作 ACID 的表或仅插入表的默认值，参考[Generate surrogate keys](https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.4/using-hiveql/content/hive_surrogate_keys.html) 和 [Apache Hive RowID Generation](https://medium.com/datafibers-blog/apache-hive-rowid-generation-8033dce24aeb)。

```sql
select reflect('java.lang.Math', 'max', 100, 200);
-- 200

select hash('tom');
-- 115026

select current_user();
-- moqi

select logged_in_user();
-- NULL

select current_database();
-- default

select md5('tom');
-- 34b7da764b21d298ef307d04d8152dc5

select sha1('tom');
-- 96835dd8bfa718bd6447ccc87af89ae1675daeca

select crc32('tom');
-- 2111795987

select sha2('tom', 256);
-- e1608f75c5d7813f3d4031cb30bfb786507d98137538ff8e128a6ff74e84e643

-- 必须搭配 base64
select base64(aes_encrypt('ABC', '1234567890123456'));
-- y6Ss+zCYObpCbgfWfyNWTw==

-- 必须搭配 unbase64
select aes_decrypt(unbase64('y6Ss+zCYObpCbgfWfyNWTw=='), '1234567890123456');
-- ABC

select version();
-- 2.3.4 r56acdd2120b9ce6790185c679223b8b5e884aaf2
```

### Xpath UDF

[LanguageManual XPathUDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+XPathUDF) 中描述了以下功能：xpath，xpath_short，xpath_int，xpath_long，xpath_float，xpath_double，xpath_number，xpath_string。

# 内置聚合函数 UDAF

```sql
-- 需要先创建表，填充数据，最后提供给 udaf 函数调用
drop table udaf_test;
create table if not exists udaf_test(num int);
insert into table udaf_test values(1), (1), (2), (3), (100), (200), (300), (null);
select * from udaf_test;
-- 1
-- 1
-- 2
-- 3
-- 100
-- 200
-- 300
-- NULL

-- 清空表数据可以使用：truncate table udaf_test;
```

* 返回值类型：名称：描述
* BIGINT：count(*), count(expr), count(DISTINCT expr[, expr...])：*
  * count(*) - 返回检索到的行总数，包括包含NULL值的行。
  * count(expr) - 返回为其提供的表达式为非NULL的行数。
  * count(DISTINCT expr[, expr]) - 返回为其提供的表达式唯一且非NULL的行数。可以使用[hive.optimize.distinct.rewrite](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties#ConfigurationProperties-hive.optimize.distinct.rewrite)优化执行。

```sql
select count(*) from udaf_test;
-- 8

select count(0) from udaf_test;
-- 8

select count(num) from udaf_test;
-- 7

select count(distinct num) from udaf_test;
-- 6
```

* double：sum(col), sum(DISTINCT col)：返回组中元素的总和或组中列的不同值的总和。
* double：avg(col), avg(DISTINCT col)：返回组中元素的平均值或组中列的不同值的平均值。
* double：min(col)：返回组中列的最小值。
* double：max(col)：返回组中列的最大值。
* double：variance(col), var_pop(col)：返回组中数字列的方差。
* double：var_samp(col)：返回组中数字列的无偏样本方差。
* double：stddev_pop(col)：返回组中数字列的标准偏差。
* double：stddev_samp(col)：返回组中数字列的无偏样本标准差。
* double：covar_pop(col1, col2)：返回组中一对数字列的总体协方差。
* double：covar_samp(col1, col2)：返回组中一对数字列的样本协方差。
* double：corr(col1, col2)：返回组中一对数字列的皮尔逊相关系数。

```sql
select sum(num) from udaf_test;
-- 607

select avg(num) from udaf_test;
-- 86.71428571428571

select min(num) from udaf_test;
-- 1

select max(num) from udaf_test;
-- 300

select var_pop(num) from udaf_test;
-- 12482.775510204083

select var_samp(num) from udaf_test;
-- 14563.238095238097

select stddev_pop(num) from udaf_test;
-- 111.72634206042943

select stddev_samp(num) from udaf_test;
-- 120.67824201254382

select covar_pop(num, num) from udaf_test;
-- 12482.775510204083

select covar_samp(num, num) from udaf_test;
-- 14563.238095238097

select corr(num, num) from udaf_test;
-- 1.0000000000000002
```

* double：percentile(BIGINT col, p)：返回组中列的精确第p 个百分位数（不适用于浮点类型）。p必须在0到1之间。注意：只能为整数值计算真实百分位数。如果您输入的内容不是整数，请使用PERCENTILE_APPROX。
* array\<double\>：percentile(BIGINT col, array(p1 [, p2]...))：返回组中列的精确百分位数p 1，p 2，...（不适用于浮点类型）。p i必须在0到1之间。注意：只能为整数值计算真实百分位数。如果您输入的内容不是整数，请使用PERCENTILE_APPROX。
* double：percentile_approx(double col, p [, B])：返回组中数字列（包括浮点类型）的大约p 个百分位数。B参数控制近似精度，但要以存储为代价。值越高，近似值越好，默认值为10,000。当col中的不同值的数量小于B时，这将给出一个精确的百分位值。
* array\<double\>：percentile_approx(double col, array(p1 [, p2]...) [, B])：与上面相同，但是接受并返回一个百分位值数组，而不是单个值。
* double：regr_avgx(independent, dependent)：等效于 avg(dependent). 从[Hive 2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15978)。
* double：regr_avgy(independent, dependent)：等效于 avg(independent). 从[Hive 2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15978)。
* double：regr_count(independent, dependent)：返回用于拟合线性回归线的非空对的数量。从[Hive 2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15978)。
* double：regr_intercept(independent, dependent)：返回[线性回归线](https://en.wikipedia.org/wiki/Linear_regression)的y截距，即等式中的b值=独立* a *独立+ b。从[Hive 2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15978)。
* double：regr_r2(independent, dependent)：返回回归[的确定系数](https://en.wikipedia.org/wiki/Coefficient_of_determination)。从[Hive 2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15978)。
* double：regr_slope(independent, dependent)：返回[线性回归线](https://en.wikipedia.org/wiki/Linear_regression)的斜率，即等式= a *独立+ b中a的值。从[Hive 2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15978)。
* double：regr_sxx(independent, dependent)：等效于regr_count（独立，从属）* var_pop（独立）。从[Hive 2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15978)。
* double：regr_sxy(independent, dependent)：等效于regr_count（独立，从属）* covar_pop（独立，从属）。从[Hive 2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15978)。
* double：regr_syy(independent, dependent)：等效于regr_count（独立，从属）* var_pop（独立）。从[Hive 2.2.0开始](https://issues.apache.org/jira/browse/HIVE-15978)。
* array<struct {'x','y'}>：histogram_numeric(col, b)：使用b个非均匀间隔的bin计算组中数字列的直方图。输出是大小为b的双值（x，y）坐标数组，这些坐标表示箱的中心和高度。
* array：collect_set(col)：返回消除了重复元素的一组对象。
* array：collect_list(col)：返回具有重复项的对象列表。（从Hive [0.13.0开始](https://issues.apache.org/jira/browse/HIVE-5294)。）
* INTEGER：ntile(INTEGER x)：将有序分区划分为`x`多个桶，并为分区中的每一行分配一个桶号。这样可以轻松计算三分位数，四分位数，十分位数，百分位数和其他常见的汇总统计信息。（从Hive [0.11.0开始](https://issues.apache.org/jira/browse/HIVE-896)。）

```sql
select percentile(num, 0.5) from udaf_test;
-- 3.0

select percentile(num, array(0.1, 0.2)) from udaf_test;
-- [1.0,1.2000000000000002]

select percentile_approx(num, 0.1) from udaf_test;
-- 1.0

select percentile_approx(num, array(0.8, 0.9)) from udaf_test;
-- [160.00000000000006,229.99999999999997]

select regr_avgx(num, num) from udaf_test;
-- 86.71428571428571

select regr_avgy(num, num) from udaf_test;
-- 86.71428571428571

select regr_count(num, num) from udaf_test;
-- 7

select regr_intercept(num, num) from udaf_test;
-- 0.0

select regr_r2(num, num) from udaf_test;
-- 1.0

select regr_slope(num, num) from udaf_test;
-- 1.0

select regr_sxx(num, num) from udaf_test;
-- 87379.42857142858

select regr_sxy(num, num) from udaf_test;
-- 87379.42857142858

select regr_syy(num, num) from udaf_test;
-- 87379.42857142858

select histogram_numeric(num, 10) from udaf_test;
-- [{"x":1.0,"y":2.0},{"x":2.0,"y":1.0},{"x":3.0,"y":1.0},{"x":100.0,"y":1.0},{"x":200.0,"y":1.0},{"x":300.0,"y":1.0}]

select collect_set(num) from udaf_test;
-- [1,2,3,100,200,300]

select collect_list(num) from udaf_test;
-- [1,1,2,3,100,200,300]
```

# 内置表生成函数 UDTF

普通的用户定义函数(例如 concat())接受单个输入行并输出单个输出行。相反，表生成函数将单个输入行转换为多个输出行。

* Row-set 类型：名称：描述
* T：explode(ARRAY\<T\> a)：将数组分解为多行。返回带有单列 (col) 的行集，该数组代表数组中每个元素的一行。
* Tkey,Tvalu：explode(MAP<Tkey,Tvalue> m)：将 Map 分解为多行。返回一个行集合与两列（K，V），一个行从输入图中的每个键-值对。（从Hive [0.8.0开始](https://issues.apache.org/jira/browse/HIVE-1735)。）。
* int,T：posexplode(ARRAY\<T\> a)：使用附加的*int*类型位置列将数组分解为多行（原始数组中项的位置，从0开始）。返回具有两列（pos，val）的行集，该数组中的每个元素一行。
* T1,...,Tn：inline(ARRAY\<STRUCT\<f1:T1,...,fn:Tn\>\> a)：将结构数组分解为多行。返回具有N列的行集（N =结构中顶级元素的数量），数组中每个结构一行一行。（从Hive[ 0.10开始](https://issues.apache.org/jira/browse/HIVE-3238)。）
* T1,...,Tn/r：stack(int r,T1 V1,...,Tn/r Vn)：将*n个*值V 1，...，V n分解为*r*行。每行将有*n / r*列。*r*必须是常数。
* string1,...,stringn：json_tuple(string jsonStr,string k1,...,string kn)：接收JSON字符串和一组*n个*键，并返回*n个*值的元组。这是get_json_object UDF的一种更有效的版本，因为它只需一次调用就可以获取多个Key的值。
* string 1,...,stringn：parse_url_tuple(string urlStr,string p1,...,string pn)：接受URL字符串和一组*n个*URL部分，并返回*n个*值的元组。这类似于parse_url()UDF，但可以一次从URL中提取多个部分。有效的部件名称是：HOST, PATH, QUERY, REF, PROTOCOL, AUTHORITY, FILE, USERINFO, QUERY:\<KEY\>.

## 使用范例

### explode (array)

```sql
select explode(array('A','B','C'));
-- col
-- A
-- B
-- C

select explode(array('A','B','C')) as alphabet;
-- alphabet
-- A
-- B
-- C

select tf.* from (select 0) t lateral view explode(array('A','B','C')) tf;
-- tf.col
-- A
-- B
-- C

select tf.* from (select 0) t lateral view explode(array('A','B','C')) tf as zzz;
-- tf.zzz
-- A
-- B
-- C
```

### explode (map)

```sql
select explode(map('A',10,'B',20,'C',30));
-- key	value
-- A	10
-- B	20
-- C	30

select explode(map('A',10,'B',20,'C',30)) as (name, age);
-- name	age
-- A	10
-- B	20
-- C	30

select tf.* from (select 0) t lateral view explode(map('A',10,'B',20,'C',30)) tf;
-- tf.key	tf.value
-- A	10
-- B	20
-- C	30

select tf.* from (select 0) t lateral view explode(map('A',10,'B',20,'C',30)) tf as name, age;
-- tf.name	tf.age
-- A	10
-- B	20
-- C	30
```

### posexplode (array)

```sql
select posexplode(array('A','B','C'));
-- pos	val
-- 0	A
-- 1	B
-- 2	C

select posexplode(array('A','B','C')) as (index, value);
-- index	value
-- 0	A
-- 1	B
-- 2	C

select tf.* from (select 0) t lateral view posexplode(array('A','B','C')) tf;
-- tf.pos	tf.val
-- 0	A
-- 1	B
-- 2	C

select tf.* from (select 0) t lateral view posexplode(array('A','B','C')) tf as index, value;
-- tf.index	tf.value
-- 0	A
-- 1	B
-- 2	C
```

### inline (array of structs)

```sql
select inline(array(struct('A',10,date '2015-01-01'),struct('B',20,date '2016-02-02')));
-- col1	col2	col3
-- A	10	2015-01-01
-- B	20	2016-02-02

select inline(array(struct('A',10,date '2015-01-01'),struct('B',20,date '2016-02-02'))) as (name, age, time);
-- name	age	time
-- A	10	2015-01-01
-- B	20	2016-02-02

select tf.* from (select 0) t lateral view inline(array(struct('A',10,date '2015-01-01'),struct('B',20,date '2016-02-02'))) tf;
-- tf.col1	tf.col2	tf.col3
-- A	10	2015-01-01
-- B	20	2016-02-02

select tf.* from (select 0) t lateral view inline(array(struct('A',10,date '2015-01-01'),struct('B',20,date '2016-02-02'))) tf as name, age, time;
-- tf.name	tf.age	tf.time
-- A	10	2015-01-01
-- B	20	2016-02-02
```

### stack (values)

```sql
select stack(2,'A',10,date '2015-01-01','B',20,date '2016-01-01');
-- col0	col1	col2
-- A	10	2015-01-01
-- B	20	2016-01-01

select stack(2,'A',10,date '2015-01-01','B',20,date '2016-01-01') as (name, age, time);
-- name	age	time
-- A	10	2015-01-01
-- B	20	2016-01-01

select tf.* from (select 0) t lateral view stack(2,'A',10,date '2015-01-01','B',20,date '2016-01-01') tf;
-- tf.col0	tf.col1	tf.col2
-- A	10	2015-01-01
-- B	20	2016-01-01

select tf.* from (select 0) t lateral view stack(2,'A',10,date '2015-01-01','B',20,date '2016-01-01') tf as name, age, time;
-- tf.name	tf.age	tf.time
-- A	10	2015-01-01
-- B	20	2016-01-01
```

使用语法“ SELECT udtf（col）AS colAlias ...”有一些限制：

- SELECT中不允许其他表达式
  - 不支持SELECT pageid，explode（adid_list）AS myCol ...
- UDTF不能嵌套
  - 不支持SELECT explode（explode（adid_list））AS myCol ...
- 不支持GROUP BY / CLUSTER BY / DISTRIBUTE BY / SORT BY
  - SELECT explode（adid_list）AS myCol ...不支持GROUP BY myCol

请参阅[LanguageManual LateralView](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView)，以获取没有这些限制的替代语法。

如果要创建自定义UDTF，也请参见[编写](https://cwiki.apache.org/confluence/display/Hive/DeveloperGuide+UDTF) UDTF。

## explode 爆炸

explode()接受数组（或Map）作为输入，并将数组（Map）的元素作为单独的行输出。UDTF可以在SELECT表达式列表中使用，也可以作为LATERAL VIEW的一部分使用。

### Array

```sql
-- 需要先创建表，填充数据，最后提供给 explode 函数调用
drop table explode_array_test;
create table if not exists explode_array_test(array_int_col array<int>);
insert into table explode_array_test select array(100, 200, 300) union select array(400, 500, 600);
select * from explode_array_test;
-- explode_array_test.array_int_col
-- [100,200,300]
-- [400,500,600]

-- 清空表数据可以使用：truncate table explode_array_test;

SELECT explode(array_int_col) AS int_col FROM explode_array_test;
-- int_col
-- 100
-- 200
-- 300
-- 400
-- 500
-- 600
```

### Map

```sql
-- 需要先创建表，填充数据，最后提供给 explode 函数调用
drop table explode_map_test;
create table if not exists explode_map_test(map_col map<int, int>);
insert into table explode_map_test select map(10, 100) union select map(20, 200);
select * from explode_map_test;
-- explode_map_test.map_col
-- {10:100}
-- {20:200}

-- 清空表数据可以使用：truncate table explode_map_test;

SELECT explode(map_col) AS (key_col, value_col) FROM explode_map_test;
-- key_col	value_col
-- 10	100
-- 20	200
```

## posexplode 爆炸(带位置)

posexplode()类似于explode但不只是返回数组的元素，它还返回元素及其在原始数组中的位置(自Hive 0.13.0起可用。参见[HIVE-4943](https://issues.apache.org/jira/browse/HIVE-4943))。

```sql
-- 需要先创建表，填充数据，最后提供给 posexplode 函数调用
drop table posexplode_array_test;
create table if not exists posexplode_array_test(array_int_col array<int>);
insert into table posexplode_array_test select array(100, 200, 300) union select array(400, 500, 600);
select * from posexplode_array_test;
-- posexplode_array_test.array_int_col
-- [100,200,300]
-- [400,500,600]

-- 清空表数据可以使用：truncate table posexplode_array_test;

SELECT posexplode(array_int_col) AS (pos, int_col) FROM posexplode_array_test;
-- pos	int_col
-- 0	100
-- 1	200
-- 2	300
-- 0	400
-- 1	500
-- 2	600
```

## json_tuple

Hive 0.7中引入了新的json_tuple（）UDTF。它使用一组名称（键）和一个JSON字符串，并使用一个函数返回值的元组。这比调用 get_json_object 从单个JSON字符串中检索多个密钥要有效得多。在任何情况下，单个JSON字符串都会被解析多次，如果您解析一次JSON_TUPLE，查询将更加高效。由于JSON_TUPLE是UDTF，因此您需要使用[LATERAL VIEW](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView)语法才能实现相同的目标。

```sql
-- 分开使用 get_json_object
select get_json_object('{"name":"zhangsan", "age":18}', '$.name') name, get_json_object('{"name":"zhangsan", "age":18}', '$.age') age;
-- name	age
-- zhangsan	18

-- 使用 json_tuple 只调用一次，而且不需要写 $. 表示 JSON 主体
select name, age from (select 0) t lateral view json_tuple('{"name":"zhangsan", "age":18}', 'name', 'age') b as name, age;
-- name	age
-- zhangsan	18
```

## parse_url_tuple

parse_url_tuple（）UDTF与parse_url（）相似，但是可以提取给定URL的多个部分，以元组形式返回数据。可以通过在冒号和partToExtract参数后附加键来提取QUERY中特定键的值，例如parse_url_tuple（'http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1' ，'QUERY：k1'，'QUERY：k2'）返回值为'v1'，'v2'的元组。这比多次调用parse_url（）更有效。所有输入参数和输出列类型都是字符串。

```sql
-- 分开使用 parse_url
select parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'PROTOCOL') protocol, parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1','HOST') host, parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'PATH') path;
-- protocol	host	path
-- http	facebook.com	/path1/p.php

-- 使用 parse_url_tuple 只调用一次
select protocol, host, path from (select 0) t lateral view parse_url_tuple('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'PROTOCOL', 'HOST', 'PATH') b as protocol, host, path;
-- protocol	host	path
-- http	facebook.com	/path1/p.php
```

# 对f（column）进行分组和排序

```sql
-- 需要先创建表，填充数据，最后提供给 group_sort_test 调用
drop table group_sort_test;
create table if not exists group_sort_test(ts timestamp);
insert into table group_sort_test select current_timestamp union select date_sub(current_timestamp, 1);
select * from group_sort_test;
-- group_sort_test.ts
-- 2019-12-28 00:00:00
-- 2019-12-29 20:55:26.64

-- 清空表数据可以使用：truncate table group_sort_test;
```

一个典型的OLAP模式是您有一个timestamp列，并且您希望按每日或其他粒度较小的日期窗口（而不是按秒）进行分组。因此，您可能要选择concat（year（dt），month（dt）），然后在该concat（）上分组。但是，如果您尝试在应用了函数和别名的列上使用GROUP BY或SORT BY，如下所示：

```sql
select day(ts) as day, count(*) from group_sort_test group by day;
```

你会得到一个错误：

```sql
FAILED: SemanticException [Error 10004]: Line 1:62 Invalid table alias or column reference 'day': (possible column names are: ts)
```

因为您不能对应用了功能的列别名进行GROUP BY或SORT BY。有两种解决方法。首先，您可以使用子查询来重新构造此查询，这有点复杂：

```sql
select 
    sq.day, 
    count(*) c 
from(
    select 
        day(ts) as day 
    from 
        group_sort_test
) sq
group by 
    sq.day;
-- sq.day	c
-- 28	1
-- 29	1
```

或者您可以确保不使用更简单的列别名：

```sql
select day(ts) as day, count(*) from group_sort_test group by day(ts);
-- day	_c1
-- 28	1
-- 29	1
```

# 实用功能

* 返回值类型：名称：描述
* String：version：提供Hive版本详细信息（软件包内置版本）
* String：buildversion：版本功能的扩展，其中包括校验和

```sql
select version();
-- 2.3.4 r56acdd2120b9ce6790185c679223b8b5e884aaf2

-- 个人猜测这个方法是后面加上来的
select buildversion();
-- FAILED: SemanticException [Error 10011]: Invalid function buildversion
```

# 个人使用经验

## 固定范围随机数

select cast(rand() * n as int)：其中 n 表示范围的右边界，左边界为0，具体范围为 [0, n]

```sql
select cast(rand() * 100 as int);
-- 10
-- 5
-- 23
```
