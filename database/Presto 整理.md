* [总览](#总览)
* [函数和运算符](#函数和运算符)
   * [逻辑运算符](#逻辑运算符)
   * [比较函数和运算符](#比较函数和运算符)
      * [BETWEEN](#between)
      * [IS NULL](#is-null)
      * [IS DISTINCT FROM](#is-distinct-from)
      * [GREATEST and LEAST](#greatest-and-least)
      * [量化比较谓词](#量化比较谓词)
   * [条件表达式](#条件表达式)
      * [TRY](#try)
   * [Lambda 表达式](#lambda-表达式)
   * [转换函数](#转换函数)
   * [数学函数和运算符](#数学函数和运算符)
   * [按位函数](#按位函数)
   * [十进制函数和运算符](#十进制函数和运算符)
   * [字符串函数和运算符](#字符串函数和运算符)
   * [正则表达式函数](#正则表达式函数)
   * [二元函数和运算符](#二元函数和运算符)
   * [JSON函数和运算符](#json函数和运算符)

# 总览

Facebook 在开源 Hive 后，发现 MapReduce 模型计算效率慢且对异构数据库支持较差。为解决这些问题，Facebook 开发并开源了 Presto，本文对 Presto 的应用做基本的整理。整理文档时 Persto 官网版本为 0.229。

[Presto 官方文档地址](https://prestodb.io/docs/current/)

# 函数和运算符

## 逻辑运算符

[官网地址]((https://prestodb.io/docs/current/functions/logical.html))

在 SQL 中 `AND` 和 `OR` 执行的情况，将 `NULL` 变量参与进来。

```sql
-- AND with NULL
SELECT CAST(null AS boolean) AND true;
-- null
SELECT CAST(null AS boolean) AND false; 
-- false
SELECT CAST(null AS boolean) AND CAST(null AS boolean); 
-- null

-- OR with NULL
SELECT CAST(null AS boolean) OR CAST(null AS boolean); 
-- null
SELECT CAST(null AS boolean) OR false; 
-- null
SELECT CAST(null AS boolean) OR true; 
-- true
```

## 比较函数和运算符

[官网地址]((https://prestodb.io/docs/current/functions/comparison.html))

大于、小于、等于、不等于、`BETWEEN` 等。值得注意的是：不等于的标准写法是 `<>` 而不是 `!=`，官网给第二种方法的注释是：非标准但流行的语法。

### BETWEEN

```sql
-- BETWEEN 和普通 SQL 的转换
SELECT 3 BETWEEN 2 AND 6;
SELECT 3 >= 2 AND 3 <= 6;
-- true
SELECT 3 NOT BETWEEN 2 AND 6;
SELECT 3 < 2 OR 3 > 6;
-- false

-- BETWEEN with NULL
SELECT NULL BETWEEN 2 AND 4; 
-- null
SELECT 2 BETWEEN NULL AND 6;
-- null

-- BETWEEN 可以用于字符串，因为普通 SQL 也支持，调用字典顺序进行比较。
select 'b' between 'a' and 'c';
-- true
select 'a' between 'b' and 'c';
-- false
select 'a' < 'b';
-- true
```

### IS NULL

```sql
select NULL IS NULL;
-- true
SELECT 3.0 IS NULL; 
-- false
SELECT 3 IS NOT NULL; 
-- true
```

### IS DISTINCT FROM

在SQL中，`NULL`值表示未知值，因此任何涉及的比较`NULL`都会产生`NULL`。此函数可以返回确定的布尔结果而规避`NULL`。

```sql
SELECT NULL IS DISTINCT FROM NULL; 
-- false
SELECT NULL IS NOT DISTINCT FROM NULL; 
-- true
```

### GREATEST and LEAST

获取一组参数的最大最小值，如果有`NULL`存在则直接返回`NULL`。

```sql
select greatest('a', 'b');
-- b
select greatest('a', 'b', NULL);
-- null
select least('a', 'b', NULL, 'c');
-- null
```

### 量化比较谓词

这三个量词 `ALL`, `ANY`, `SOME`可与比较运算符以下列方式使用：

```sql
expression operator quantifier (subquery)
```

举例子：

```sql
SELECT 'hello' = ANY (VALUES 'hello', 'world'); 
-- true
SELECT 21 < ALL (VALUES 19, 20, 21); 
-- false
SELECT 42 >= SOME (SELECT 41 UNION ALL SELECT 42 UNION ALL SELECT 43); 
-- true
```

`ANY `和`SOME`意义相同，都表示在子查询中某一个或者多个满足要求，可以互换使用，而`ALL`必须全部满足。

## 条件表达式

[官网地址]((https://prestodb.io/docs/current/functions/conditional.html))

case when、if、nullif、coalesce 可以参考整理过的 Hive 函数。

### TRY

`try`(*expression*)：评估表达式并通过返回 `NULL`来处理某些类型的错误。

如果在发生查询异常时将异常值转化为 `NULL`而不是直接查询失败，则`try`命令非常有用。要指定默认值则配合`COALESCE`函数一并使用即可。

 `TRY`可以处理以下错误:

- Division by zero（除法中分母为 0）
- Invalid cast or function argument（无效的类型强制转换或参数）
- Numeric value out of range（数值超出范围）

```sql
-- 目前本机测试 Presto 访问 Hive 的数据库表
-- 需要先在 Hive 中创建表，填充数据，最后提供给 try 函数调用
drop table try_test;
create table if not exists try_test(zip_code string);
insert into table try_test values('1'), ('3'), ('a100');
select * from try_test;
-- 1
-- 3
-- a100

-- 清空表数据可以使用：truncate table try_test;
```

开始测试 TRY 函数：

```sql
-- 不带 try 函数：直接失败
SELECT CAST(zip_code AS BIGINT) FROM try_test;
-- Query failed: Cannot cast 'a100' to BIGINT

-- 带 try 函数：异常值显示为 NULL
SELECT TRY(CAST(zip_code AS BIGINT)) FROM try_test;
-- 1
-- 3
-- NULL

-- 带 try 函数和 coalesce 组合：给异常值默认值
SELECT COALESCE(TRY(CAST(zip_code AS BIGINT)), 0) FROM try_test;
-- 1
-- 3
-- 0
```

## Lambda 表达式

[官网地址](https://prestodb.io/docs/current/functions/lambda.html)

除了少数例外，大多数 SQL 表达式都可以在 lambda 主体中使用：

- 不支持子查询。 `x -> 2 + (SELECT 3)`
- 不支持聚合。 `x -> max(y)`

在 0.229 版本官网的文档找不到范例，找到了 0.172 版本的[历史镜像](https://prestodb.io/docs/0.172/functions/lambda.html)。

- `filter`(*array*, *function, *boolean>*) → ARRAY\<T\>：根据`array`为其`function`返回true的那些元素构造一个数组。

```sql
SELECT filter(ARRAY [], x -> true); 
-- []
SELECT filter(ARRAY [5, -6, NULL, 7], x -> x > 0); 
-- [5, 7]
SELECT filter(ARRAY [5, NULL, 7, NULL], x -> x IS NOT NULL); 
-- [5, 7]
```

- `map_filter`(*map, *V>*, *function, *V*, *boolean>*) → MAP<K,V>：`map`根据`function`返回true的条目构造一个映射。

```sql
SELECT map_filter(MAP(ARRAY[], ARRAY[]), (k, v) -> true); 
-- {}
SELECT map_filter(MAP(ARRAY[10, 20, 30], ARRAY['a', NULL, 'c']), (k, v) -> v IS NOT NULL); 
-- {10 -> a, 30 -> c}
SELECT map_filter(MAP(ARRAY['k1', 'k2', 'k3'], ARRAY[20, 3, 15]), (k, v) -> v > 10); 
-- {k1 -> 20, k3 -> 15}
```

- `reduce`(*array*, *initialState S*, *inputFunction, *T*, *S>*, *outputFunction, *R>*) → R：返回从减去的单个值`array`。`inputFunction`将按`array`顺序为每个元素调用。除了采用元素之外，还`inputFunction`采用当前状态Initially `initialState`并返回新状态。`outputFunction`将被调用以将最终状态转换为结果值。它可能是身份函数（）。例如：`i -> i`。

```sql
SELECT reduce(ARRAY [], 0, (s, x) -> s + x, s -> s); 
-- 0
SELECT reduce(ARRAY [5, 20, 50], 0, (s, x) -> s + x, s -> s); 
-- 75
SELECT reduce(ARRAY [5, 20, NULL, 50], 0, (s, x) -> s + x, s -> s); 
-- NULL
SELECT reduce(ARRAY [5, 20, NULL, 50], 0, (s, x) -> s + COALESCE(x, 0), s -> s); 
-- 75
SELECT reduce(ARRAY [5, 20, NULL, 50], 0, (s, x) -> IF(x IS NULL, s, s + x), s -> s); 
-- 75

-- 这里 SQL reduce 的第二个参数必须转换为 bigint 提升类型，如果直接是 0 则报错：
-- integer addition overflow: 2147483647 + 1
SELECT reduce(ARRAY [2147483647, 1], CAST (0 AS BIGINT), (s, x) -> s + x, s -> s); 
-- 2147483648 

-- 计算算术平均值
SELECT reduce(ARRAY [5, 6, 10, 20],
              CAST(ROW(0.0, 0) AS ROW(sum DOUBLE, count INTEGER)),
              (s, x) -> CAST(ROW(x + s.sum, s.count + 1) AS ROW(sum DOUBLE, count INTEGER)),
              s -> IF(s.count = 0, NULL, s.sum / s.count));
-- 10.25
```

- `transform`(*array*, *function, *U>*) → ARRAY\<U\>：返回适用的阵列`function`到的每个元素`array`。

```sql
SELECT transform(ARRAY [], x -> x + 1); 
-- []
SELECT transform(ARRAY [5, 6], x -> x + 1); 
-- [6, 7]
SELECT transform(ARRAY [5, NULL, 6], x -> COALESCE(x, 0) + 1); 
-- [6, 1, 7]

-- 这里 || 表示单纯的字符串连接
SELECT transform(ARRAY ['x', 'abc', 'z'], x -> x || '0'); 
-- ['x0', 'abc0', 'z0']

-- 这里对子字符串使用了 filter 嵌套调用
SELECT transform(ARRAY [ARRAY [1, NULL, 2], ARRAY[3, NULL]], a -> filter(a, x -> x IS NOT NULL)); 
-- [[1, 2], [3]]
```

- `transform_keys`(*map, *V>*, *function, *V*, *K2>*) → MAP<K2,V>：返回适用`function`于的每个条目的映射`map`并转换 key。

```sql
SELECT transform_keys(MAP(ARRAY[], ARRAY[]), (k, v) -> k + 1); 
-- {}
SELECT transform_keys(MAP(ARRAY [1, 2, 3], ARRAY ['a', 'b', 'c']), (k, v) -> k + 1);
-- {2 -> a, 3 -> b, 4 -> c}
SELECT transform_keys(MAP(ARRAY ['a', 'b', 'c'], ARRAY [1, 2, 3]), (k, v) -> v * v); 
-- {1 -> 1, 4 -> 2, 9 -> 3}
SELECT transform_keys(MAP(ARRAY ['a', 'b'], ARRAY [1, 2]), (k, v) -> k || CAST(v as VARCHAR)); 
-- {a1 -> 1, b2 -> 2}
SELECT transform_keys(MAP(ARRAY [1, 2], ARRAY [1.0, 1.4]),                       
                      (k, v) -> MAP(ARRAY[1, 2], ARRAY['one', 'two'])[k]);
-- {one -> 1.0, two -> 1.4}
```

- `transform_values`(*map, *V1>*, *function, *V1*, *V2>*) → MAP<K, V2>：返回适用`function`于的每个条目的映射`map`并转换 value。

```sql
SELECT transform_values(MAP(ARRAY[], ARRAY[]), (k, v) -> v + 1); 
-- {}
SELECT transform_values(MAP(ARRAY [1, 2, 3], ARRAY [10, 20, 30]), (k, v) -> v + 1); 
-- {1 -> 11, 2 -> 22, 3 -> 33}
SELECT transform_values(MAP(ARRAY [1, 2, 3], ARRAY ['a', 'b', 'c']), (k, v) -> k * k); 
-- {1 -> 1, 2 -> 4, 3 -> 9}
SELECT transform_values(MAP(ARRAY ['a', 'b'], ARRAY [1, 2]), (k, v) -> k || CAST(v as VARCHAR)); 
-- {a -> a1, b -> b2}
SELECT transform_values(MAP(ARRAY [1, 2], ARRAY [1.0, 1.4]), 
                        (k, v) -> MAP(ARRAY[1, 2], ARRAY['one', 'two'])[k] || '_' || CAST(v AS VARCHAR));
-- {1 -> one_1.0, 2 -> two_1.4}
```

- `zip_with`(*array*, *array*, *function, *U*, *R>*) → array\<R\>：使用将两个给定的数组按元素合并到单个数组中`function`。两个数组的长度必须相同。

```sql
SELECT zip_with(ARRAY[1, 3, 5], ARRAY['a', 'b', 'c'], (x, y) -> (y, x)); 
--  [{field0=a, field1=1}, {field0=b, field1=3}, {field0=c, field1=5}]
SELECT zip_with(ARRAY[1, 2], ARRAY[3, 4], (x, y) -> x + y); 
-- [4, 6]
SELECT zip_with(ARRAY['a', 'b', 'c'], ARRAY['d', 'e', 'f'], (x, y) -> concat(x, y)); 
-- ['ad', 'be', 'cf']
```

## 转换函数

[官网地址](https://prestodb.io/docs/current/functions/conversion.html)

如果有可能，Presto会将数字和字符值隐式转换为正确的类型。Presto不会在字符和数字类型之间转换。例如，要求使用varchar的查询不会自动将bigint值转换为等效的varchar。

必要时，可以将值显式转换为特定类型。

- `cast`(*value AS type*) → type：明确地将值转换为类型。这可以用于将varchar转换为数值类型，反之亦然。

- `try_cast`(*value AS type*) → type：与 cast() 相似，但如果强制转换失败，则返回null。
- `typeof`(*expr*) → varchar：返回提供的表达式类型的名称。

```sql
-- cast
select cast(1 as varchar);
-- 1
select cast('abc' as int);
-- Query failed: Cannot cast 'abc' to INT

-- try_cast
select try_cast('abc' as int);
-- null
select COALESCE(try_cast('abc' as int), 0);
-- 0

-- typeof
select typeof('abc');
-- varchar(3)
SELECT typeof(123);
-- integer
SELECT typeof(123.8);
-- decimal(4,1)
```

## 数学函数和运算符

[官网地址](https://prestodb.io/docs/current/functions/math.html)

加减乘除模运算，加上 Java Math 的方法，具体参考官网。

```sql
select rand();
-- 0.4786029407518667
select rand(100);
-- 44
select rand(100);
-- 58
```

## 按位函数

[官网地址](https://prestodb.io/docs/current/functions/bitwise.html)

补码与算术左移右移运算，具体见官网。

```sql
SELECT bit_count(9, 64); 
-- 2
SELECT bit_count(9, 8);
-- 2
SELECT bit_count(-7, 64); 
-- 62
SELECT bit_count(-7, 8); 
-- 6
```

## 十进制函数和运算符

[官网地址](https://prestodb.io/docs/current/functions/decimal.html)

使用语法定义DECIMAL类型的文字。`DECIMAL 'xxxxxxx.yyyyyyy'`

文字的DECIMAL类型的精度将等于文字中的位数（包括尾随和前导零）。小数位数将等于小数部分的位数（包括尾随零）。

```sql
-- DECIMAL '0' -> DECIMAL(1)
-- DECIMAL '12345' -> DECIMAL(5)
-- DECIMAL '0000012345.1234500000' -> DECIMAL(20, 10)
```

## 字符串函数和运算符

[官网地址](https://prestodb.io/docs/current/functions/string.html)

Presto 和 Hive 在字符串函数这里最大的区别是左边下标从 1 开始，而 Hive 是从 0 开始。

- `split`(*string*, *delimiter) -> array(varchar*)：分割`string`上`delimiter`并返回一个数组。
- `split`(*string*, *delimiter*, *limit) -> array(varchar*)：分割`string`上`delimiter`并返回至多大小的阵列 `limit`。数组中的最后一个元素始终包含剩余的所有内容`string`。`limit`必须为正数。
- `split_part`(*string*, *delimiter*, *index*) → varchar：拆分`string`上`delimiter`，并返回现场`index`。字段索引以开头`1`。如果索引大于字段数，则返回null。
- `split_to_map`(*string*, *entryDelimiter*, *keyValueDelimiter*) → map<varchar, varchar>：分割`string`为`entryDelimiter`和，`keyValueDelimiter`然后返回地图。 `entryDelimiter`分为`string`键值对。`keyValueDelimiter`将每对分为键和值。请注意，`entryDelimiter`和`keyValueDelimiter`会按字面意义进行解释，即作为完整字符串匹配。

```sql
select split('a,b,c,f,g,h', ',');
-- [a, b, c, f, g, h]

-- 这个函数好用，一定情况下可以满足单一分隔符组装多种信息的需要。
-- 不加第三个参数的情况下也可以，只是后半部分成为了字符串数组需要再合并起来。
select split('a,b,c,f,g,h', ',', 3);
-- [a, b, c,f,g,h]

select split_part('a,b,c,,,,e,f,g,h', ',', 1);
-- a
```

- `split_to_map`(*string*, *entryDelimiter*, *keyValueDelimiter*, *function(k*, *v1*, *v2*, *res)*) → map<varchar, varchar>：分割`string`为`entryDelimiter`和，`keyValueDelimiter`然后返回地图。 `entryDelimiter`分为`string`键值对。`keyValueDelimiter`将每对分为键和值。请注意，`entryDelimiter`和`keyValueDelimiter`会按字面意义进行解释，即作为完整字符串匹配。 在键重复的情况下调用，`function(k, v1, v2, res)`用以解析应该在映射中的值。

```sql
SELECT split_to_map('a:1,b:2,c:3', ',', ':');
-- {a=1, b=2, c=3}

SELECT split_to_map('a:1,b:2,a:3', ',', ':');
-- Query failed: Duplicate keys (a) are not allowed. 
-- Specifying a lambda to resolve conflicts can avoid this error

SELECT split_to_map('a:1,b:2,a:3', ',', ':', (k, v1, v2) -> v1);
-- {a=1, b=2}

SELECT split_to_map('a:1,b:2,a:3', ',', ':', (k, v1, v2) -> v2);
-- {a=3, b=2}
```

- `split_to_multimap`(*string*, *entryDelimiter*, *keyValueDelimiter) -> map(varchar*, *array(varchar)*)：分割`string`为`entryDelimiter`和，`keyValueDelimiter`然后返回一个映射，其中包含每个唯一键的值数组。`entryDelimiter`分为`string` 键值对。`keyValueDelimiter`将每对分为键和值。每个键的值将与出现的顺序相同`string`。请注意，`entryDelimiter`和`keyValueDelimiter`会按字面意义进行解释，即作为完整字符串匹配。

```sql
SELECT split_to_multimap('a:1,b:2,c:3', ',', ':');
-- {a=[1], b=[2], c=[3]}

SELECT split_to_multimap('a:1,b:2,a:3', ',', ':');
-- {a=[1, 3], b=[2]}
```

## 正则表达式函数

[官网地址](https://prestodb.io/docs/current/functions/regexp.html)

所有正则表达式函数都使用[Java模式](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)语法，但有一些值得注意的例外：

- 使用多行模式（通过`(?m)`标志启用）时，仅`\n`被识别为行终止符。此外，该`(?d)`标志不受支持，因此不能使用。

- 不区分大小写的匹配（通过`(?i)`标志启用）始终以Unicode感知的方式执行。但是，不支持上下文相关和本地敏感匹配。此外，该 `(?u)`标志不受支持，因此不能使用。

- 不支持代理对。例如，`\uD800\uDC00`不被视为`U+10000`，必须将其指定为`\x{10000}`。

- `\b`没有基本字符的非间距标记的边界（）处理不正确。

- `\Q`并且`\E`在字符类（例如`[A-Z123]`）中不受支持，而是被视为文字。

- 支持Unicode字符类（`\p{prop}`），但有以下差异：

  - 名称中的所有下划线都必须删除。例如，使用 `OldItalic`代替`Old_Italic`。
  - 脚本必须直接指定，如果没有 `Is`，`script=`或`sc=`前缀。例：`\p{Hiragana}`
  - 必须使用`In`前缀指定块。将`block=`与`blk=`前缀不被支持。例：`\p{Mongolian}`
- 分类必须直接指定，如果没有`Is`， `general_category=`或`gc=`前缀。例：`\p{L}`
  - 二进制属性必须直接指定，不带`Is`。例：`\p{NoncharacterCodePoint}`

正则表达式函数展示：

- `regexp_extract_all`(*string*, *pattern) -> array(varchar*)：由正则表达式匹配的返回子（一个或多个）`pattern` 中`string`。

  Returns the substring(s) matched by the regular expression `pattern` in `string`:`SELECT regexp_extract_all('1a 2b 14m', '\d+'); -- [1, 2, 14] `

- `regexp_extract_all`(*string*, *pattern*, *group) -> array(varchar*)：发现正则表达式的所有出现`pattern`在`string` 与返回所述[捕获组号码](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#gnumber) `group`。

  Finds all occurrences of the regular expression `pattern` in `string` and returns the [capturing group number](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#gnumber) `group`:`SELECT regexp_extract_all('1a 2b 14m', '(\d+)([a-z]+)', 2); -- ['a', 'b', 'm'] `

- `regexp_extract`(*string*, *pattern*) → varchar：返回由正则表达式匹配的第一个字符串`pattern` 中`string`。

  Returns the first substring matched by the regular expression `pattern` in `string`:`SELECT regexp_extract('1a 2b 14m', '\d+'); -- 1 `

- `regexp_extract`(*string*, *pattern*, *group*) → varchar：查找中出现的第一个正则表达式`pattern`， `string`并返回[捕获组号](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#gnumber) `group`。

- `regexp_like`(*string*, *pattern*) → boolean：计算正则表达式`pattern`并确定它是否包含在中`string`。

  此功能类似于`LIKE`运算符，除了模式仅需要包含在其中`string`，而不需要匹配所有`string`。换句话说，这执行 *包含*操作而不是*匹配*操作。您可以通过使用`^`和锚定模式来匹配整个字符串`$`。

- `regexp_replace`(*string*, *pattern*) → varchar：`pattern`从中删除与正则表达式匹配的子字符串的每个实例 `string`。

- `regexp_replace`(*string*, *pattern*, *replacement*) → varchar：通过替换正则表达式匹配的子串的每个实例 `pattern`中`string`使用`replacement`。可以在使用编号组或 命名组时引用[捕获](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#cg)组。A dollar sign (`$`) may be included in the replacement by escaping it with a backslash (`\$`):

- `regexp_replace`(*string*, *pattern*, *function*) → varchar：通过替换正则表达式匹配的子串的每个实例 `pattern`中`string`使用`function`。所述[lambda表达式](https://prestodb.io/docs/current/functions/lambda.html) `function`被调用为每个匹配与[捕获基团](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#cg)作为数组传递。捕获组号从1开始；整个匹配没有分组（如果需要，请用括号将整个表达式括起来）。

- `regexp_split`(*string*, *pattern) -> array(varchar*)：`string`使用正则表达式拆分`pattern`并返回一个数组。尾随的空字符串被保留。

```sql
SELECT regexp_extract_all('1a 2b 14m', '\d+'); 
-- [1, 2, 14]
SELECT regexp_extract_all('1a 2b 14m', '(\d+)([a-z]+)', 2); 
-- [a, b, m]

SELECT regexp_extract('1a 2b 14m', '\d+'); 
-- 1
SELECT regexp_extract('1a 2b 14m', '(\d+)([a-z]+)', 2); 
-- a

SELECT regexp_like('1a 2b 14m', '\d+b'); 
-- true

SELECT regexp_replace('1a 2b 14m', '\d+[ab]'); 
-- 14m
SELECT regexp_replace('1a 2b 14m', '(\d+)([ab])', '3c$2'); 
-- 3ca 3cb 14m
SELECT regexp_replace('new york', '(\w)(\w*)', x -> upper(x[1]) || lower(x[2])); 
--'New York'

SELECT regexp_split('1a 2b 14m', '\s*[a-z]+\s*'); 
-- [1, 2, 14, ]
```

## 二元函数和运算符

[官网地址](https://prestodb.io/docs/current/functions/binary.html)

大多数二元函数与字符串传函数一致。

```sql
select md5(to_utf8('abc'));
--  90 01 50 98 3c d2 4f b0 d6 96 3f 7d 28 e1 7f 72
```

## JSON函数和运算符

[官网地址](https://prestodb.io/docs/current/functions/json.html)

其他格式转换为 JSON：

```sql
SELECT CAST(NULL AS JSON); 
-- NULL
SELECT CAST(1 AS JSON); 
-- JSON '1'
SELECT CAST(9223372036854775807 AS JSON); 
-- JSON '9223372036854775807'
SELECT CAST('abc' AS JSON); 
-- JSON '"abc"'
SELECT CAST(true AS JSON); 
-- JSON 'true'
SELECT CAST(1.234 AS JSON); 
-- JSON '1.234'
SELECT CAST(ARRAY[1, 23, 456] AS JSON); 
-- JSON '[1,23,456]'
SELECT CAST(ARRAY[1, NULL, 456] AS JSON); 
-- JSON '[1,null,456]'
SELECT CAST(ARRAY[ARRAY[1, 23], ARRAY[456]] AS JSON); 
-- JSON '[[1,23],[456]]'
SELECT CAST(MAP_FROM_ENTRIES(ARRAY[('k1', 1), ('k2', 23), ('k3', 456)]) AS JSON); 
-- JSON '{"k1":1,"k2":23,"k3":456}'
SELECT CAST(CAST(ROW(123, 'abc', true) AS ROW(v1 BIGINT, v2 VARCHAR, v3 BOOLEAN)) AS JSON); -- JSON '[123,"abc",true]'
```

 JSON转换为其他格式：

```sql
SELECT CAST(JSON 'null' AS VARCHAR); 
-- NULL
SELECT CAST(JSON '1' AS INTEGER); 
-- 1
SELECT CAST(JSON '9223372036854775807' AS BIGINT); 
-- 9223372036854775807
SELECT CAST(JSON '"abc"' AS VARCHAR); 
-- abc
SELECT CAST(JSON 'true' AS BOOLEAN); 
-- true
SELECT CAST(JSON '1.234' AS DOUBLE); 
-- 1.234
SELECT CAST(JSON '[1,23,456]' AS ARRAY(INTEGER)); 
-- [1, 23, 456]
SELECT CAST(JSON '[1,null,456]' AS ARRAY(INTEGER)); 
-- [1, NULL, 456]
SELECT CAST(JSON '[[1,23],[456]]' AS ARRAY(ARRAY(INTEGER))); 
-- [[1, 23], [456]]
SELECT CAST(JSON '{"k1":1,"k2":23,"k3":456}' AS MAP(VARCHAR, INTEGER)); 
-- {k1=1, k2=23, k3=456}
SELECT CAST(JSON '{"v1":123,"v2":"abc","v3":true}' AS ROW(v1 BIGINT, v2 VARCHAR, v3 BOOLEAN)); 
-- {v1=123, v2=abc, v3=true}
SELECT CAST(JSON '[123,"abc",true]' AS ROW(v1 BIGINT, v2 VARCHAR, v3 BOOLEAN)); 
-- {value1=123, value2=abc, value3=true}
```

JSON数组可以具有混合的元素类型，而JSON映射可以具有混合的值类型。这使得在某些情况下无法将它们强制转换为SQL数组和映射。为了解决这个问题，Presto支持部分转换数组和映射：

```sql
SELECT CAST(JSON '[[1, 23], 456]' AS ARRAY(JSON)); 
-- [JSON '[1,23]', JSON '456']
SELECT CAST(JSON '{"k1": [1, 23], "k2": 456}' AS MAP(VARCHAR, JSON)); 
-- {k1 = JSON '[1,23]', k2 = JSON '456'}
SELECT CAST(JSON '[null]' AS ARRAY(JSON)); 
-- [JSON 'null']
```

相关的函数：

* `is_json_scalar`(*json*) → boolean：确定是否`json`为标量（即JSON数字，JSON字符串`true`，`false`或`null`）。

```sql
SELECT is_json_scalar('1'); 
-- true
SELECT is_json_scalar('[1, 2, 3]'); 
-- false
SELECT is_json_scalar('"[1, 2, 3]"');
-- true
```

* `json_array_contains`(*json*, *value*) → boolean：确定是否`value`存在于`json`（包含JSON数组的字符串）中。

```sql
select json_array_contains('[1, 2, 3]', 2);
-- true
select json_array_contains('[1, 2, 3]', 8);
-- false

-- 这里是因为类型不匹配
select json_array_contains('[1, 2, 3]', '2');
-- false
select json_array_contains('["1", "2", "3"]', '2');
-- true
```

* `json_array_length`(*json*) → bigint：返回的数组长度`json`（包含JSON数组的字符串）。
* `json_extract`(*json*, *json_path*) → json：求值的[JSONPath](http://goessner.net/articles/JsonPath/)样表达`json_path`上`json` （含有JSON的字符串），并返回其结果作为JSON字符串。
* `json_extract_scalar`(*json*, *json_path*) → varchar：与相似[`json_extract()`](https://prestodb.io/docs/current/functions/json.html#json_extract)，但以字符串形式返回结果值（与编码为JSON相对）。引用的值`json_path`必须是标量（布尔值，数字或字符串）。

```sql
SELECT json_array_length('[1, 2, 3]');
-- 3

SELECT json_extract('{"store":{"book": "go_in_action"}}', '$.store.book');
-- "go_in_action"

-- 注意这里下标从 0 开始
SELECT json_extract_scalar('[1, 2, 3]', '$[2]');
-- 3
SELECT json_extract_scalar('{"store":{"book": "go_in_action"}}', '$.store.book');
-- go_in_action

SELECT json_format(JSON '[1, 2, 3]'); 
-- '[1,2,3]'
SELECT json_format(JSON '"a"'); 
-- '"a"'
```

* `json_format`(*json*) → varchar：返回从输入JSON值序列化的JSON文本。这是反函数[`json_parse()`](https://prestodb.io/docs/current/functions/json.html#json_parse)。

> 注意
>
> [`json_format()`](https://prestodb.io/docs/current/functions/json.html#json_format)并具有完全不同的语义。`CAST(json AS VARCHAR)`
>
> [`json_format()`](https://prestodb.io/docs/current/functions/json.html#json_format) 将输入的JSON值序列化为符合以下条件的JSON文本 [**RFC 7159**](https://tools.ietf.org/html/rfc7159.html)。的JSON值可以是JSON对象，JSON阵列，JSON串，JSON号码，`true`，`false`或`null`：
>
> `CAST(json AS VARCHAR)`将JSON值转换为相应的SQL VARCHAR值。对于JSON字符串，JSON号码，`true`，`false`或`null`，铸造的行为是相同的相应的SQL类型。JSON对象和JSON数组不能强制转换为VARCHAR：

```sql
SELECT json_format(JSON '{"a": 1, "b": 2}'); 
-- '{"a":1,"b":2}'
SELECT json_format(JSON '[1, 2, 3]'); 
-- '[1,2,3]'
SELECT json_format(JSON '"abc"'); 
-- '"abc"'
SELECT json_format(JSON '42'); 
-- '42'
SELECT json_format(JSON 'true'); 
-- 'true'
SELECT json_format(JSON 'null'); 
-- 'null'

SELECT CAST(JSON '{"a": 1, "b": 2}' AS VARCHAR); 
-- ERROR!
SELECT CAST(JSON '[1, 2, 3]' AS VARCHAR); 
-- ERROR!
SELECT CAST(JSON '"abc"' AS VARCHAR); 
-- 'abc' 可以看到这里去掉了双引号
SELECT CAST(JSON '42' AS VARCHAR); 
-- '42'
SELECT CAST(JSON 'true' AS VARCHAR); 
-- 'true'
SELECT CAST(JSON 'null' AS VARCHAR); 
-- NULL
```

* `json_parse`(*string*) → json：返回从输入JSON文本反序列化的JSON值。这是反函数[`json_format()`](https://prestodb.io/docs/current/functions/json.html#json_format)。

> 注意
>
> [`json_parse()`](https://prestodb.io/docs/current/functions/json.html#json_parse)并具有完全不同的语义。`CAST(string AS JSON)`
>
> [`json_parse()`](https://prestodb.io/docs/current/functions/json.html#json_parse) 期望符合的JSON文本 [**RFC 7159**](https://tools.ietf.org/html/rfc7159.html)，并返回从JSON文本反序列化的JSON值。的JSON值可以是JSON对象，JSON阵列，JSON串，JSON号码， `true`，`false`或`null`：
>
> `CAST(string AS JSON)`接受任何VARCHAR值作为输入，并返回一个JSON字符串，其值设置为输入字符串：

```sql
SELECT json_parse('not_json'); 
-- ERROR!
SELECT json_parse('["a": 1, "b": 2]'); 
-- JSON '["a": 1, "b": 2]'
SELECT json_parse('[1, 2, 3]'); 
-- JSON '[1,2,3]'
SELECT json_parse('"abc"'); 
-- JSON '"abc"'
SELECT json_parse('42'); 
-- JSON '42'
SELECT json_parse('true'); 
-- JSON 'true'
SELECT json_parse('null'); 
-- JSON 'null'

SELECT CAST('not_json' AS JSON); 
-- JSON '"not_json"'
SELECT CAST('["a": 1, "b": 2]' AS JSON); 
-- JSON '"[\"a\": 1, \"b\": 2]"'
SELECT CAST('[1, 2, 3]' AS JSON); 
-- JSON '"[1, 2, 3]"'
SELECT CAST('"abc"' AS JSON); 
-- JSON '"\"abc\""'
SELECT CAST('42' AS JSON); 
-- JSON '"42"'
SELECT CAST('true' AS JSON); 
-- JSON '"true"'
SELECT CAST('null' AS JSON); 
-- JSON '"null"'
```

* `json_size`(*json*, *json_path*) → bigint：与相似[`json_extract()`](https://prestodb.io/docs/current/functions/json.html#json_extract)，但返回值的大小。对于对象或数组，大小为成员数，标量值的大小为零。

```sql
SELECT json_size('{"x": {"a": 1, "b": 2}}', '$.x'); 
-- 2
SELECT json_size('{"x": [1, 2, 3]}', '$.x'); 
-- 3
SELECT json_size('{"x": {"a": 1, "b": 2}}', '$.x.a'); 
-- 0
```

## 日期和时间功能及运算符

[官网地址](https://prestodb.io/docs/current/functions/datetime.html)

