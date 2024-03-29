# SQL查询：从入门到实践（第４版）

 **约翰·L.维斯卡斯**


## 划线部分


### 第一部分 关系型数据库和SQL

* 知识是从无知中整理和分类出来的很小一部分。


### 1.2 关系模型简史

* 作为数学家，他深信自己完全能够利用数学分支来解决诸如数据冗余、数据完整性不佳以及数据库结构过度依赖于物理实现等问题。

* 一个普遍的误解是，“关系模型”这个名称源自关系型数据库中的表彼此相关这一事实，但实际上，这里的“关系”指的是关系型数据库系统中的表。知道真相后，你今晚就能睡个安稳觉了！

* IBM提出了数据仓库的概念，旨在让组织能够访问存储在大量非关系型数据库中的数据。


### 1.3 关系型数据库剖析

* 键还有助于避免出现致命的“孤行”。

* 一对一关系不那么常见，通常仅在出于保密的目的而将一个表一分为二时才会出现。

* 在两个表中，如果任何一个表中的一行都可能关联到另一个表中的多行，则这两个表之间存在多对多关系。为了正确地建立这种关系，必须创建链接表（linking table）。


### 2.3 微调列

* ❖说明 SQL标准定义了常规标识符（regular identifier）和分隔式标识符（delimited identifier）。常规标识符指的是这样的名称，即必须以字母打头，且只能包含字母、数字和下划线（不能包含空格）；分隔式标识符指的是用双引号括起的名称，必须以字母打头，且只能包含字母、数字、下划线、空格以及其他几种特殊字符。给列命名时，建议你只使用常规标识符命名规则，因为很多SQL实现只支持常规标识符命名规则。

* 列名为何必须是单数型的呢？因为一个列表示它所属表表示的客体的一个特征。相反，表名必须是复数型的，因为一个表包含一系列类似的物件或事件。使用这种命名约定后，可轻松地区分表名和列名。



### 2.4 微调表

* ❖说明 对数据库进行逻辑设计时，使用复数形式的表名是一条非常不错的指导原则。这让你能够非常轻松地将表名和列名区分开来，将它们显示到投影屏幕或写在会议室的白板上时尤其如此。
然而，别忘了，当你（或负责实现数据库的数据库开发人员）着手在特定的RDBMS应用程序中实现数据库时，可能需要修改表名。届时，表名必须遵循开发人员在该RDBMS中都遵循的命名约定。

* 应尽可能定义简单主键，因为建立表关系时，简单主键的效率更高，且使用起来容易得多。仅当必要时才使用复合主键，如定义和创建链接表时。

* 不确定的情况下，解决方案是创建一个人造主键。你可以随便定义这样的列，将其添加到表中的唯一目的是用作主键。添加这种列的优点是，可确保它符合前述所有标准。在表中添加这样的列后，将其指定为主键，这就大功告成了！就这么简单。


### 2.5 建立合理的关系

* ❖说明 要实施参与程度约束，必须在数据库定义中定义一个或多个触发器或约束（如果数据库系统支持这些功能的话）。


### 3.4 ANSI/ISO标准的发展历程

* ODBC:1989年，一组数据库厂商建立了SQL Access集团，旨在解决数据库互操作性问题。虽然这些厂商最初所做的努力不太成功，但随后它们扩大了目标范围，力图制定一种将SQL数据库绑定到用户界面语言的标准。1992年，这种努力有了结晶，那就是调用级接口（Call-Level Interface, CLI）规范。在同一年，微软发布了基于CLI标准的开放数据库连接（Open Database Connectivity, ODBC）规范。从那时起，ODBC就成了访问SQL数据库以及在SQL数据库之间共享数据的事实标准。


### 3.6 展望未来

* 1986年，这两个标准委员会的脚步远远落后于商用实现，但现在完全可以说SQL标准在功能方面早已跟上既有数据库系统的步伐，在很多方面还走在了它们前面。


### 第二部分 SQL基础

* “像智者一样思考，但用通俗的语言交流。”
——威廉·巴特勒·叶芝


### 4.1 SELECT简介

* 在SQL中，SELECT操作可分解为3个更小的操作，我将这些操作称为SELECT语句、SELECT表达式和SELECT查询（通过以这种方式分解SELECT操作，你能轻松地明白其复杂性）。


### 4.3 说点题外话：数据和信息

* 对数据库进行查询前，有一点必须明白：数据和信息之间存在明显的差别。大体而言，数据是存储在数据库中的内容，而信息是从数据库检索得到的内容。你必须明白这种差别，因为这有助于看清本质。别忘了，设计数据库旨在向组织中的人员提供有意义的信息，但要提供这样的信息，必须满足两个条件：数据库包含合适的数据；数据库本身的结构为提供这种信息提供了支持。


### 4.4 将请求转换为SQL

* 在SELECT子句中，可使用星号来替代包含所有列的列表。

* 仅当需要快速创建查询以查看特定表中所有的信息时，才使用星号；在其他情况下，都应在查询中显式地指定所需的列。这样查询将返回所需的信息（不多也不少），且更容易理解。


### 4.6 对信息进行排序

* ORDER BY子句让你能够按一列或多列对SELECT语句的结果集进行排序，还可指定按各列升序还是降序排列。在ORDER BY子句中，只能指定在SELECT子句中指定了的列（虽然SQL标准做了这样的规定，但有些厂商实现允许你完全无视这样的规定，在ORDER BY子句中指定FROM子句指定的表中的任何列）。在ORDER BY子句中指定多列时，必须用逗号将各列隔开。排序完毕后，SELECT查询将立即返回最终的结果集。

* ❖说明 如果有多个供应商的邮政编码相同，而你又没有在ORDER BY子句中指定如何排列这些供应商，数据库系统将决定如何排列它们。

* 在这两个数据库系统中，还可以以百分比的方式指定要返回的行数。例如，要找出价格排名在前10%的商品，可像下面这样做：
￼         SELECT TOP 10 PERCENT ProductName, RetailPrice￼             FROM Products￼             ORDER BY RetailPrice DESC

* 实际上，如果你在视图中指定ORDER BY, SQL Server要求必须包含关键字TOP。因此，如果要返回所有的行，必须指定TOP 100 PERCENT。


### 4.7 保存所做的工作

* 不同于其他大多数数据库系统，在视图中使用ORDER BY方面，SQL Server有个小怪癖。虽然你可将包含ORDER BY子句的查询保存为视图，但如果你从程序、其他视图或命令行调用这种视图，SQL Server将忽略其中的ORDER BY子句。因此，在SQL Server中保存的包含ORDER BY的查询不会以指定的顺序返回结果。要按指定的顺序返回结果，要么在设计工具中打开并执行查询，要么在调用查询时添加相应的ORDER BY子句，如SELECT ＊ FROM <视图名> ORDER BY <原始排序规范>。


### 第5章 获取除简单列外的其他信息

* “事实是不容改变的。”
——Tobias Smollett, Gil Blas De Santillane


### 5.2 你要表示哪些类型的数据

* 如果字符串的长度超过了系统规定的最大长度（通常为255或1024个字符），就必须使用数据类型CHARACTER LARGE OBJECT、CHAR LARGE OBJECT或CLOB。在很多系统中，CLOB的别名为TEXT或MEMO。


### 5.3 修改数据类型：CAST函数

* 请注意，将字符列转换为数值或日期时间时，数据库系统将忽略开头和末尾的所有空格。


### 5.4 指定显式值

* 字符串字面量是用单引号括起的字符序列。

* SQL标准还规定在日期字面量前必须加上关键字DATE，但几乎所有的商用实现都允许只指定用分界符（通常是单引号）括起的字面量。

* SQL标准还规定在时间字面量前必须加上关键字TIME，但几乎所有的商用实现允许只指定用分界符（通常是单引号）括起的字面量。

* SQL标准还规定在时间戳字面量前必须加上关键字TIMESTAMP，但几乎所有支持数据类型TIMESTAMP的商用实现允许只指定用分界符（通常是单引号）括起的字面量。


### 5.5 表达式类型

* SQL标准指出，使用CAST函数进行转换时如何取整由数据库系统决定，但大多数数据库系统在转换为整数时都截除小数部分。

* SQL标准明确规定，对于这些运算，只能使用指定的数据类型，但很多数据库系统会自动转换列的数据类型。

* 基于上面的假设，下面是一些合法的时间表达式：


### 5.6 在SELECT子句中使用表达式

* SQL标准规定，可在各种语句中使用值表达式，但不管在哪里使用值表达式，你都将以刚才介绍的方式定义它们。


### 5.7 空值：Null

* Null的主要缺点在于它给数学运算带来的负面影响。任何包含Null的运算的结果都为Null，这合乎逻辑（如果一个值未知，那么对其执行运算的结果必然也是未知的）。

* 通过确保在数学表达式中使用的列都不包含Null值，可从根本上避免这种问题。



### 6.1 使用WHERE提炼信息

* ❖说明 对于最新的SQL标准中定义的其他13个谓词（Similar、Regex、Quantified、Exists、Unique、Normalized、Match、Overlaps、Distinct、Member、Submultiset、Set和Type），请不要过多地考虑。我发现，这些谓词中有10个没有任何商用实现。MySQL和PostgreSQL支持Regex。第11章将介绍另外两个谓词——Quantified和Exists。

* 编写合适的查找条件并将其加入到WHERE子句中是一件很简单的事情，难的是定义查找条件。


### 6.2 定义查找条件

* 两个长度不同的字符串进行比较前，数据库系统必须在较短的字符串右边添加默认的填充字符，直到其长度与较长的字符串相同（在大多数数据库系统中，默认的填充字符为空格），再根据排序序列判断这两个字符串是否相等。

* ❖说明 SQL标准将符号<>用作不等运算符。多个RDBMS程序都提供了不同的表示法，如！=（Microsoft SQL Server和Sybase）和¬=（IBM DB2）。务必查看你使用的数据库系统的文档，确定它使用哪种方式来表示不等运算符。

* 字符串：这种比较判断第一个值表达式的值是在第二个表达式的值的前面（<）还是后面（>）。

* 日期/时间：这种比较判断第一个值表达式的值是否早于（<）或晚于（>）第二个值表达式的值。

* 编写与字符串数据相关的范围时，请仔细想想你要包含的值。

* ❖说明 Microsoft Office Access是最流行的数据库系统之一，它使用星号（*）和问号（?），而不是百分比符号（%）和下划线（_）。Access还支持使用井号（#）在特定位置查找数字字符。如果你使用的是Microsoft Access，请在LIKE谓词的模式字符串中替换这些字符。

* 选项ESCAPE让你能够指定只包含一个字符的字符串字面量（这被称为转义字符），从而告诉数据库系统该如何解读模式字符串中的百分比符号和下划线。要指定转义字符，可将其放在关键字ESCAPE的后面，并像其他字符串字面量一样使用单引号将其括起。在模式字符串中，如果通配字符前面是转义字符，数据库系统将把它解读为实际字符。

* 说明 要在值表达式中查找Null值，必须使用Null条件。诸如<值表达式>=Null这样的条件是非法的，因为根据定义，不能将值表达式的值同未知值进行比较。事实上，在任何比较谓词中，使用Null的结果都是未知的，而未知被视为假，因此比较的结果是不满足条件。


### 6.3 使用多个条件

* 为避免这种二义性并让查找条件更清晰，可使用括号来合并某些条件并提高其优先级。

* ❖说明 几乎所有商业数据库系统都包含查询优化器，它查看整个请求，力图找出返回答案的最快方式。在决定优化器选择如何做方面，数据库管理员在表列上定义的索引影响最大，但通过将最严格的查找条件放在最前面，可进一步影响数据库系统的优化器，因此养成这样的习惯没有害处。

* 所有开始日期都有一个什么共同之处呢？它们都小于等于指定日期跨度的终点！同样，它们的结束日期都大于等于指定日期跨度的起点。


### 7.2 集合运算

* 鉴于几乎没有商业实现支持SQL标准定义的INTERSECT，我将介绍如何使用JOIN来解决原本需要使用INTERSECT的问题。

* 另外，由于只有几种商业实现支持SQL标准定义的EXCEPT（表示差集运算的关键字），我将介绍如何使用OUTER JOIN来解决原本需要使用EXCEPT的问题


### 7.3 SQL集合运算

* 从单个表中获得“或”型结果时，使用UNION可能比较麻烦。UNION最大的用武之地是使用多个结构类似的表来生成列表


### 8.2 内连接

* SQL标准定义了多种执行连接的方式，其中最常用的是内连接（INNER JOIN）。

* 连接有意义，编写出的查询也可能需要很长时间才能执行完毕。例如，如果数据库管理员没有为用于建立连接的列定义索引，数据库系统可能需要做大量额外的工作。另外，如果基于表达式（如拼接两个表中名和姓的表达式）来建立连接，数据库系统将不仅需要在所有行中根据表达式来生成一列，还可能必须多次扫描两个表中的所有数据，以便返回正确的结果。

* 如果没有明确地指定连接的类型，将默认为内连接。建议总是明确地指定连接的类型，让请求更清晰。

* 换而言之，通过利用灵巧（优化）的方案，数据库系统只取回匹配的行。数据库表只包含几百行时，这看起来不重要，但需要处理数十万行时，将有很大的不同！

* 给表指定关联名后，就可在其他所有子句中使用关联名来替代原来的表名，这包括SELECT子句、ON和WHERE子句的查找条件以及ORDER BY子句。

* 另外，有些数据库系统的优化器对连接定义的顺序很敏感，如果你发现使用了大量连接的查询在大型数据库中的执行时间很长，可调整SQL语句中连接的顺序，这样或许能够缩短查询的执行时间。


### 第9章 外连接

* 问题和解决方案的唯一差别在于后者是人们能够看懂的。


### 9.2 左/右外连接

* 通常使用外连接来获取一个表或结果集中的所有行以及另一个表或结果集中匹配的行，为此，可使用左外连接（LEFT OUTER JOIN）或右外连接（RIGHT OUTER JOIN）。

* 虽然可编写非常复杂的查找条件，但通常使用简单的相等比较测试


### 第六部分 解决棘手问题

* 每个复杂的问题都有简单、显而易见但错误的答案。


## 个人笔记部分


### 导言

* 语言从本质上说是一种交流工具，它表达的不是具体的事情，而是大家的共识。  （个人笔记: 好。）

