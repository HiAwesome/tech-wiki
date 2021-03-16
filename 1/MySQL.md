# MySQL

## SQL Statements

### [13.1.15 CREATE INDEX Statement](https://dev.mysql.com/doc/refman/8.0/en/create-index.html)

## Partitioning

### [24.6.1 Partitioning Keys, Primary Keys, and Unique Keys](https://dev.mysql.com/doc/refman/8.0/en/partitioning-limitations-partitioning-keys-unique-keys.html)

## Data Types

### enum type

* [11.3.5 The ENUM Type](https://dev.mysql.com/doc/refman/8.0/en/enum.html)

### bigint VS int

* [11.1.2 Integer Types](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)
* [BIGINT mysql performance compared to INT](https://stackoverflow.com/a/9377107)
* [MySQL: bigint Vs int](https://stackoverflow.com/questions/4769416/mysql-bigint-vs-int)
* [When should I use UNSIGNED and SIGNED INT in MySQL?](https://stackoverflow.com/a/11515613)

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
