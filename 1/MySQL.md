# MySQL

## PingCAP TiDB

### [Can't connect using mysql client (using password: NO)](https://github.com/pingcap/tidb/issues/14021#issuecomment-591560953)

> Reproducible on MySQL 8 client bundled with Ubuntu with master version of TiDB.
>
> The problem is authentication plugin mismatch. While TiDB wants the client to authenticate using mysql_native_password, the client still replies using caching_sha2_password which TiDB doesn't support yet ([#9411](https://github.com/pingcap/tidb/issues/9411)).
>
> You can [force the MySQL client to use the native password plugin](https://dev.mysql.com/doc/refman/8.0/en/native-pluggable-authentication.html) with:
>
> ```sql
> mysql --default-auth=mysql_native_password -h ::1 -P 4000 -u user -pPassword ...
> ```

## null vs empty string

* [MySQL: NULL vs “”](https://stackoverflow.com/a/12706491)
* [https://dba.stackexchange.com/questions/59/when-to-use-null-and-when-to-use-an-empty-string](https://dba.stackexchange.com/a/69)

## Soft delete design

### [Deleting data: soft, hard or audit?](https://www.martyfriedel.com/blog/deleting-data-soft-hard-or-audit)

### [Physical vs. logical (hard vs. soft) delete of database record?](https://stackoverflow.com/a/378355)

### [Are soft deletes a good idea?](https://stackoverflow.com/a/2549843)

### [Soft delete best practices (PHP/MySQL)](https://stackoverflow.com/a/5020597)

## Primary Key

### [阿里巴巴 Java开发手册（嵩山版）: MySQL 数据库建表规约](https://github.com/alibaba/p3c)

> 9.【强制】表必备三字段：id, create_time, update_time。
> 
> 说明：其中 id 必为主键，类型为 bigint unsigned、单表时自增、步长为 1。create_time, update_time 的类型均为 datetime 类型，前者现在时表示主动式创建，后者过去分词表示被动式更新。

### [为什么总是需要无意义的 ID](https://draveness.me/whys-the-design-meaningless-identifier/)

> 需要通过唯一的标识符对数据或者事件进行识别或者去重；
> 
> 只有无意义的标识符才会绝对唯一的，任何携带其他信息的标识都可能重复；

相关优质 blog: [为什么这么设计系列文章](https://draveness.me/whys-the-design/)

### [为什么 MySQL 的自增主键不单调也不连续](https://draveness.me/whys-the-design-mysql-auto-increment/)

### [The great primary-key debate](https://www.techrepublic.com/article/the-great-primary-key-debate/)

> The single-most important issue facing the database developer is good design. If the foundation is weak, so is the building. To avoid future problems and subsequent (and perhaps convoluted) repairs, we recommend that you use surrogate keys. This simple design choice is one of the easiest ways to provide your application with a strong, stable, yet flexible foundation.

### [MySQL 数据库主键的思考](https://blog.csdn.net/zengqiang1/article/details/79312774)

> 小型系统或者系统架构初期，数据没有超过百万或者千万，可以使用自增主键；
> 
> 当数据成长到几百万或者超过千万时，并且增量很高时，这时就涉及到分库分表，这时就必须使用自动生成 id 方案。

### [MySQL原理与实践（六）：自增主键的使用](https://blog.csdn.net/qq_25827845/article/details/91477092)

> 表定义的自增值达到上限后的逻辑是：再申请下一个 id 时，得到的值保持不变。
> 
> 从这个角度看，我们还是应该在 InnoDB 表中主动创建自增主键。因为，表自增 id 到达上限后，再插入数据时报主键冲突错误，是更能被接受的。毕竟覆盖数据，就意味着数据丢失，影响的是数据可靠性；报主键冲突，是插入失败，影响的是可用性。而一般情况下，可靠性优先于可用性。

### [ALTER TABLE to add a composite primary key](https://stackoverflow.com/a/8859374)

### [Composite Primary Key performance drawback in MySQL](https://stackoverflow.com/a/1460544) 

### [What's the difference between composite primary index and two primary indexes?](https://stackoverflow.com/a/45292184)

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

#### [Reset MySQL Root Password in Mac OS.md](https://gist.github.com/zubaer-ahammed/c81c9a0e37adc1cb9a6cdc61c4190f52)

Update: On 8.0.15 (maybe already before that version) the PASSWORD() function does not work You have to do:

Make sure you have Stopped MySQL first (above).
Run the server in safe mode with privilege bypass: sudo mysqld_safe --skip-grant-tables

> mysql -u root
> UPDATE mysql.user SET authentication_string=null WHERE User='root';
> FLUSH PRIVILEGES;
> exit;

Then

> mysql -u root
> ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'yourpasswd';
