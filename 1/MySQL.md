# MySQL

## Functions and Operators

### [Infomation Functions](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html)

* [LAST_INSERT_ID()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id)

## mysql primary key int vs varchar

### [Is there a REAL performance difference between INT and VARCHAR primary keys?](https://stackoverflow.com/a/332363)

You make a good point that you can avoid some number of joined queries by using what's called a [natural key](https://en.wikipedia.org/wiki/Natural_key) instead of a [surrogate key](https://en.wikipedia.org/wiki/Surrogate_key). Only you can assess if the benefit of this is significant in your application.

That is, you can measure the queries in your application that are the most important to be speedy, because they work with large volumes of data or they are executed very frequently. If these queries benefit from eliminating a join, and do not suffer by using a varchar primary key, then do it.

Don't use either strategy for all tables in your database. It's likely that in some cases, a natural key is better, but in other cases a surrogate key is better.

Other folks make a good point that it's rare in practice for a natural key to never change or have duplicates, so surrogate keys are usually worthwhile.

## Character Sets, Collations, Unicode

### [10.3.3 Database Character Set and Collation](https://dev.mysql.com/doc/refman/8.0/en/charset-database.html)

### [What is the best collation to use for MySQL with PHP?](https://stackoverflow.com/a/367725)

The main difference is sorting accuracy (when comparing characters in the language) and performance. The only special one is utf8_bin which is for comparing characters in binary format.

`utf8_general_ci` is somewhat faster than `utf8_unicode_ci`, but less accurate (for sorting). The specific language utf8 encoding (such as `utf8_swedish_ci`) contain additional language rules that make them the most accurate to sort for those languages. Most of the time I use `utf8_unicode_ci` (I prefer accuracy to small performance improvements), unless I have a good reason to prefer a specific language.

You can read more on specific unicode character sets on the MySQL manual - [10.10.1 Unicode Character Sets](https://dev.mysql.com/doc/refman/8.0/en/charset-unicode-sets.html).

## SQL Statements

### [13.1.15 CREATE INDEX Statement](https://dev.mysql.com/doc/refman/8.0/en/create-index.html)

## Partitioning

### [24.6.1 Partitioning Keys, Primary Keys, and Unique Keys](https://dev.mysql.com/doc/refman/8.0/en/partitioning-limitations-partitioning-keys-unique-keys.html)

## Data Types

### Date and Time Data Types

* [11.2.5 Automatic Initialization and Updating for TIMESTAMP and DATETIME](https://dev.mysql.com/doc/refman/8.0/en/timestamp-initialization.html)

### enum type

* [11.3.5 The ENUM Type](https://dev.mysql.com/doc/refman/8.0/en/enum.html)

### bigint VS int

* [11.1.2 Integer Types](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)

#### [BIGINT mysql performance compared to INT](https://stackoverflow.com/a/9377107)

To answer your question: yes it'll get less performant. Obviously, the bigger the type, the bigger the table, the slower the queries (more I/O, bigger indexes, longer access time, result less likely to fit in the various caches, and so on). So as a rule of thumb: always use the **smallest type** that fits you need.

That being said, **performance doesn't matter**. Why? Because when you reach a point where you overflow an INT, then BIGINT is the only solution and you'll have to live with it. Also at that point (considering you're using an auto increment PK, you'll be over 4 **billions** rows), you'll have bigger performance issues, and the overhead of a BIGINT compared to a INT will be the least of your concerns.

So, consider the following points:

* Use UNSIGNED if you don't need negative values, that'll double the limit.
* UNSIGNED INT max value is 4.294.967.295. If you're using an auto increment PK, and **you only have 300.000 entries, you really don't need to worry**. You could even use a MEDIUMINT at the moment, unless you're planning for really really fast growth.
* The number in parenthesis after the type doesn't impact the max value of the type. INT(7) is the same as INT(8) or INT(32). It's used to indicate the display width if you specify ZEROFILL.

#### [MySQL: bigint Vs int](https://stackoverflow.com/a/4769440)

The difference is purely in the maximum value which can be stored (18,446,744,073,709,551,615 for the bigint(20) and 4,294,967,295 for the int(10), I believe), as per the details on the MySQL [Numeric Types](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html) manual page.

Incidentally, the use of (20) and (10) is largely irrelevant unless you're using ZEROFILL. (i.e.: It doesn't actually change the size of the number stored - that's all down to the type.)

However, in practical terms it should be noted that you're not likely to hit either of these limits any time soon, unless you're a **really** active blogger.

#### [When should I use UNSIGNED and SIGNED INT in MySQL?](https://stackoverflow.com/a/11515613)

`UNSIGNED` only stores positive numbers (or zero). On the other hand, signed can store negative numbers (i.e., may have a negative sign).

Here's a table of the ranges of values each `INTEGER` type can store:

![MySQL INTEGER types and lengths](https://i.stack.imgur.com/lrXCt.png)

[Source](http://dev.mysql.com/doc/refman/5.6/en/integer-types.html)

`UNSIGNED` ranges from `0` to `n`, while signed ranges from about `-n/2` to `n/2`.

In this case, you have an `AUTO_INCREMENT` ID column, so you would not have negatives. Thus, use `UNSIGNED`. If you do not use `UNSIGNED` for the `AUTO_INCREMENT` column, your maximum possible value will be half as high (and the negative half of the value range would go unused).

## Other

### [How can I simulate an array variable in MySQL?](https://stackoverflow.com/a/13996761)

### [MySQL 获取表的 comment 字段](https://blog.csdn.net/u011341352/article/details/48272963)

查看获取表内字段注释：

```mysql-sql
show full columns from tablename;
```

查看表注释:

```mysql-sql
show table status;
```

### [MySQL 5.7.18 中文注释乱码](https://www.pianshen.com/article/26111468501/)

查看mysql字符集编码：

```mysql-sql
show variables where Variable_name like '%char%';
```

修改字符集编码：

```mysql-sql
set character_set_client=utf8;
set character_set_connection=utf8;
set character_set_database=utf8;
set character_set_results=utf8;
set character_set_server=utf8;
```

### [9.3 Keywords and Reserved Words](https://dev.mysql.com/doc/refman/8.0/en/keywords.html)

### [MySQL command line formatting with UTF8](https://stackoverflow.com/a/6788223)

**Short answer:**

```shell
mysql --default-character-set=utf8

```
