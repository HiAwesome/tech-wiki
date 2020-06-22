[[_TOC_]]

# 倾斜表梳理

[官网地址](https://cwiki.apache.org/confluence/display/Hive/ListBucketing).
打开 stored as directories 属性时，需要打开子目录支持和动态分区，确保查询有效性，可以参考[这里](http://svn.apache.org/repos/asf/hive/trunk/ql/src/test/queries/clientpositive/list_bucket_dml_6.q).

通过指定一个或者多个列经常出现的值（严重偏斜），Hive 会自动将涉及到这些值的数据拆分为单独的文件。在查询时，如果涉及到倾斜值，它就直接从独立文件中获取数据，而不是扫描所有文件，这使得性能得到提升。

## not skewed table

```sql
drop table not_skewed_t;
create table not_skewed_t(c1 string, c2 string);
insert into table not_skewed_t values('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),
('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),
('x2', 'x2'),('x3', 'x3'),('x4', 'x4'),('x15', 'x15'),('x8', 'x8');
select * from not_skewed_t;
```

## skewed table not stored

```sql
drop table skewed_t_not_stored;
create table skewed_t_not_stored(c1 string, c2 string) skewed by (c1) on ('x1');
insert into table skewed_t_not_stored values('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),
('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),
('x2', 'x2'),('x3', 'x3'),('x4', 'x4'),('x15', 'x15'),('x8', 'x8');
select * from skewed_t_not_stored;
```

## skewed table and stored

```sql
set hive.mapred.supports.subdirectories=true;
set hive.exec.dynamic.partition=true;
set hive.exec.dynamic.partition.mode=nonstrict;
set hive.input.format=org.apache.hadoop.hive.ql.io.BucketizedHiveInputFormat;
set hive.merge.smallfiles.avgsize=200;
set mapred.input.dir.recursive=true;
set hive.merge.mapfiles=false;	
set hive.merge.mapredfiles=false;

drop table skewed_t;
create table skewed_t(c1 string, c2 string) skewed by (c1) on ('x1') stored as directories;
insert into table skewed_t values('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),
('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),('x1', 'x1'),
('x2', 'x2'),('x3', 'x3'),('x4', 'x4'),('x15', 'x15'),('x8', 'x8');
select * from skewed_t;
```


