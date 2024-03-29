## Lua程序设计（第4版）

 **罗伯拖·鲁萨利姆斯奇**


### 推荐序二

* Lua作为一门诞生已经超过20年的语言，在设计上是非常克制的，以Lua 5.1.4版本来说，这个版本是Lua发展了十几年之后稳定使用了很长时间的版本，其解释器加上周边的库函数等不过就是一万多行的代码。

* “上善若水，水善利万物而不争”，这大概是我能想到的最适合用于来描述Lua语言设计哲学的句子


### 译者序

* 这是我自己真正署名的第一本技术书籍，我会把本书微薄的版税收入全部用于Lua语言中文官方网站http://www.lua-lang.org.cn的日常服务器及带宽开支，并捐献给其他投入国内Lua语言推广的相关组织和活动。我想，我终于可以给这个世界留下点什么了。


### 前言

* Lua语言从一开始就被设计为能与C/C++及其他常用语言开发的软件集成在一起使用的语言，这种设计带来了非常多的好处。一方面，Lua语言不需要在性能、与三方软件交互等C语言已经非常完善的方面重复“造轮子”，可以直接依赖C语言实现上述特性，因而Lua语言非常精简；另一方面，通过引入安全的运行时环境、自动内存管理、良好的字符串处理能力和可变长的多种数据类型，Lua语言弥补了C语言在非面向硬件的高级抽象能力、动态数据结构、鲁棒性、调试能力等方面的不足。

* 一些读者可能对Lua语言的C语言API毫无兴趣，而其他一些读者可能觉得这一部分是本书中最有意义的部分。

* 由于Lua语言中两个连续的连字符（--）表示单行注释，因此在程序中使用记号-->不会有任何问题。


### 1.1 程序段

* 函数dofile在开发阶段也非常有用。我们可以同时打开两个窗口，一个窗口中使用文件编辑器编辑的代码（例如文件prog.lua），另一个窗口中使用交互模式运行Lua语言解释器。当修改完代码并保存后，只要在Lua语言交互模式的提示符下执行dofile（"prog.lua"）就可以加载新代码，然后就可以观察新代码的函数调用和执行结果了。


### 1.2 一些词法规范

* Lua语言中使用两个连续的连字符（--）表示单行注释的开始（从--之后直到此行结束都是注释），使用两个连续的连字符加两对连续左方括号表示长注释或多行注释的开始（直到两个连续的右括号为止，中间都是注释）


### 1.3 全局变量

* Lua语言不区分未初始化变量和被赋值为nil的变量。在上述赋值语句执行后，Lua语言会最终回收该变量占用的内存。


### 1.4 类型和值

* Lua语言中有8种基本类型：nil（空）、boolean（布尔）、number（数值）、string（字符串）、userdata（用户数据）、function（函数）、thread（线程）和table（表）。使用函数type可获取一个值对应的类型名称

* Boolean类型具有两个值，true和false，它们分别代表了传统布尔值。不过，在Lua语言中，Boolean值并非是用于条件测试的唯一方式，任何值都可以表示条件。在Lua语言中，条件测试（例如控制结构中的分支语句）将除Boolean值false和nil外的所有其他值视为真。特别的是，在条件检测中Lua语言把零和空字符串也都视为真。

* 逻辑运算符and的运算结果为：如果它的第一个操作数为“false”，则返回第一个操作数，否则返回第二个操作数。逻辑运算符or的运算结果为：如果它的第一个操作数不为“false”，则返回第一个操作数，否则返回第二个操作数

* 在Lua语言中，形如x=x or v的惯用写法非常有用


### 1.5 独立解释器

* 分号使得最后一行在语法上变成了无效的表达式，但可以被当作有效的命令执行。


### 3.1 数值常量

* 其他很多编程语言不同，Lua语言还支持十六进制的浮点数，这种十六进制浮点数由小数部分和以p或P开头的指数部分组成。


### 3.4 数学库

* 函数randomseed用于设置伪随机数发生器的种子，该函数的唯一参数就是数值类型的种子。在一个程序启动时，系统固定使用1为种子初始化伪随机数发生器。如果不设置其他的种子，那么每次程序运行时都会生成相同的伪随机数序列。从调试的角度看，这是一个不错的特性，然而，对于一个游戏来说却会导致相同的场景重复不断地出现。为了解决这个问题，通常调用math.randomseed（os.time（））来使用当前系统时间作为种子初始化随机数发生器


### 4 字符串

* 可以使用长度操作符（length operator）（#）获取字符串的长度

* 我们可以使用连接操作符..（两个点）来进行字符串连接。如果操作数中存在数值，那么Lua语言会先把数值转换成字符串


### 4.3 强制类型转换

* Lua语言在运行时提供了数值与字符串之间的自动转换（conversion）。针对字符串的所有算术操作会尝试将字符串转换为数值。Lua语言不仅仅在算术操作时进行这种强制类型转换（coercion），还会在任何需要数值的情况下进行，例如函数math.sin的参数。


* 如果需要显式地将一个字符串转换成数值，那么可以使用函数tonumber。当这个字符串的内容不能表示为有效数字时该函数返回nil；否则，该函数就按照Lua语法扫描器的规则返回对应的整型值或浮点类型值

* 为了避免出现不一致的结果，当比较操作符中混用了字符串和数值（比如2<"15"）时，Lua语言会抛出异常。


### 4.4 字符串标准库

* 在上例中，%.4f表示小数点后保留4位小数；%02d表示一个十进制数至少由两个数字组成，不足两个数字的用0补齐，而%2d则表示用空格来补齐。关于这些指示符的完整描述可以参阅C语言printf函数的相关文档，因为Lua语言是通过调用C语言标准库来完成实际工作的。


### 5 表

* 表（Table）是Lua语言中最主要（事实上也是唯一的）和强大的数据结构。使用表，Lua语言可以以一种简单、统一且高效的方式表示数组、集合、记录和其他很多数据结构。Lua语言也使用表来表示包（package）和其他对象。当调用函数math.sin时，我们可能认为是“调用了math库中函数sin”；而对于Lua语言来说，其实际含义是“以字符串"sin"为键检索表math”。



### 5.1 表索引

* 同一个表中存储的值可以具有不同的类型索引￼，并可以按需增长以容纳新的元素

* 当把表当作结构体使用时，可以把索引当作成员名称使用（a.name等价于a\["name"\]）。

* 初学者常常会混淆a.x和a\[x\]。实际上，a.x代表的是a\["x"\]，即由字符串"x"索引的表；而a\[x\]则是指由变量x对应的值索引的表

* 更准确地说，当被用作表索引时，任何能够被转换为整型的浮点数都会被转换成整型数。例如，当执行表达式a\[2.0\]=10时，键2.0会被转换为2。相反，不能被转换为整型数的浮点数则不会发生上述的类型转换。


### 5.2 表构造器

* 在同一个构造器中，可以混用记录式（record-style）和列表式

* 可以使用另一种更加通用的构造器，即通过方括号括起来的表达式显式地指定每一个索引


### 5.3 数组、列表和序列

* 如果想表示常见的数组（array）或列表（list），那么只需要使用整型作为索引的表即可。同时，也不需要预先声明表的大小，只需要直接初始化我们需要的元素即可

* Lua语言提供了获取序列长度的操作符#。正如我们之前所看到的，对于字符串而言，该操作符返回字符串的字节数；对于表而言，该操作符返回表对应序列的长度。

* 将长度操作符用于存在空洞的列表的行为是Lua语言中最具争议的内容之一。在过去几年中，很多人建议在操作存在空洞的列表时直接抛出异常，也有人建议扩展长度操作符的语义。然而，这些建议都是说起来容易做起来难。其根源在于列表实际上是一个表，而对于表来说，“长度”的概念在一定程度上是不容易理解的。

* 请注意，对于Lua语言而言，一个为nil的字段和一个不存在的元素没有区别。因此，上述列表与{10,20,30}是等价的——其长度是3，而不是5。


### 5.4 遍历表

* 受限于表在Lua语言中的底层实现机制，遍历过程中元素的出现顺序可能是随机的，相同的程序在每次运行时也可能产生不同的顺序。唯一可以确定的是，在遍历的过程中每个元素会且只会出现一次。


### 5.5 安全访问

* 对于这种情景，诸如C#的一些编程语言提供了一种安全访问操作符（safe navigation operator）。在C#中，这种安全访问操作符被记为“？.”。例如，对于表达式a？.b，当a为nil时，其结果是nil而不会产生异常。

* Lua语言并没有提供安全访问操作符，并且认为也不应该提供这种操作符。一方面，Lua语言在设计上力求简单；另一方面，这种操作符也是非常有争议的，很多人就无理由地认为该操作符容易导致无意的编程错误。不过，我们可以使用其他语句在Lua语言中模拟安全访问操作符。


### 6 函数

* 在Lua语言中，函数（Function）是对语句和表达式进行抽象的主要方式。函数既可以用于完成某种特定任务（有时在其他语言中也称为过程（procedure）或子例程（subroutine）），也可以只是进行一些计算然后返回计算结果。在前一种情况下，我们将一句函数调用视为一条语句；而在后一种情况下，我们则将函数调用视为表达式

* 调用函数时使用的参数个数可以与定义函数时使用的参数个数不一致。Lua语言会通过抛弃多余参数和将不足的参数设为nil的方式来调整参数的个数。


### 6.1 多返回值

* Lua语言中一种与众不同但又非常有用的特性是允许一个函数返回多个结果（Multiple Results）。

* Lua语言根据函数的被调用情况调整返回值的数量。当函数被作为一条单独语句调用时，其所有返回值都会被丢弃；当函数被作为表达式（例如，加法的操作数）调用时，将只保留函数的第一个返回值。只有当函数调用是一系列表达式中的最后一个表达式（或是唯一一个表达式）时，其所有的返回值才能被获取到。

* 这里所谓的“一系列表达式”在Lua中表现为4种情况：多重赋值、函数调用时传入的实参列表、表构造器和return语句。

* 应该意识到，return语句后面的内容是不需要加括号的，如果加了括号会导致程序出现额外的行为。因此，无论f究竟返回几个值，形如return（f（x））的语句只返回一个值。有时这可能是我们所希望出现的情况，但有时又可能不是。


### 6.2 可变长参数函数

* Lua语言中的函数可以是可变长参数函数（variadic），即可以支持数量可变的参数。例如，我们已经使用一个、两个或更多个参数调用过函数print。虽然函数print是在C语言中定义的，但也可以在Lua语言中定义可变长参数函数。

* 该函数是一个多值恒等式函数（multi-value identity function）。下列函数的行为则类似于直接调用函数foo，唯一不同之处是在调用函数foo之前会先打印出传递给函数foo的所有参数

* 要遍历可变长参数，函数可以使用表达式{...}将可变长参数放在一个表中，就像add示例中所做的那样。不过，在某些罕见的情况下，如果可变长参数中包含无效的nil，那么{...}获得的表可能不再是一个有效的序列。此时，就没有办法在表中判断原始参数究竟是不是以nil结尾的。对于这种情况，Lua语言提供了函数table.pack。￼该函数像表达式{...}一样保存所有的参数，然后将其放在一个表中返回，但是这个表还有一个保存了参数个数的额外字段"n"

* 另一种遍历函数的可变长参数的方法是使用函数select。函数select总是具有一个固定的参数selector，以及数量可变的参数。如果selector是数值n，那么函数select则返回第n个参数后的所有参数；否则，selector应该是字符串"#"，以便函数select返回额外参数的总数。


### 6.3 函数table.unpack

* 多重返回值还涉及一个特殊的函数table.unpack。该函数的参数是一个数组，返回值为数组内的所有元素：


### 6.4 正确的尾调用

* Lua语言中有关函数的另一个有趣的特性是，Lua语言是支持尾调用消除（tail-call elimination）的。这意味着Lua语言可以正确地（properly）尾递归（tail recursive），虽然尾调用消除的概念并没有直接涉及递归

* 当函数f调用完函数g之后，f不再需要进行其他的工作。这样，当被调用的函数执行结束后，程序就不再需要返回最初的调用者。因此，在尾调用之后，程序也就不需要在调用栈中保存有关调用函数的任何信息。当g返回时，程序的执行路径会直接返回到调用f的位置。在一些语言的实现中，例如Lua语言解释器，就利用了这个特点，使得在进行尾调用时不使用任何额外的栈空间。我们就将这种实现称为尾调用消除（tail-call elimination


### 6.5 练习

* 练习6.6：有时，具有正确尾调用（proper-tail call）的语句被称为正确的尾递归（properly tail recursive），争论在于这种正确性只与递归调用有关（如果没有递归调用，那么一个程序的最大调用深度是静态固定的）。
请证明上述争论的观点在像Lua语言一样的动态语言中不成立：不使用递归，编写一个能够实现支持无限调用链（unbounded call chain）的程序


### 7 输入输出

* 由于Lua语言强调可移植性和嵌入性，所以Lua语言本身并没有提供太多与外部交互的机制。在真实的Lua程序中，从图形、数据库到网络的访问等大多数I/O操作，要么由宿主程序实现，要么通过不包括在发行版中的外部库实现。单就Lua语言而言，只提供了ISO C语言标准支持的功能，即基本的文件操作等。


### 7.4 其他系统调用

* 如果要使用操作系统的其他扩展功能，最好的选择是使用第三方库，比如用于基本目录操作和文件属性操作的LuaFileSystem，或者提供了POSIX.1标准支持的luaposix库。



### 8.1 局部变量和代码块

* Lua语言中的变量在默认情况下是全局变量，所有的局部变量在使用前必须声明。与全局变量不同，局部变量的生效范围仅限于声明它的代码块。一个代码块（block）是一个控制结构的主体，或是一个函数的主体，或是一个代码段（即变量被声明时所在的文件或字符串）

* 请注意，上述示例在交互模式中不能正常运行。因为在交互模式中，每一行代码就是一个代码段（除非不是一条完整的命令）。一旦输入示例的第二行（local i=1），Lua语言解释器就会直接运行它并在下一行开始一个新的代码段。这样，局部（local）的声明就超出了原来的作用范围。解决这个问题的一种方式是显式地声明整个代码块，即将它放入一对do–end中。一旦输入了do，命令就只会在遇到匹配的end时才结束，这样Lua语言解释器就不会单独执行每一行的命令。

* 尽可能地使用局部变量是一种良好的编程风格。首先，局部变量可以避免由于不必要的命名而造成全局变量的混乱；其次，局部变量还能避免同一程序中不同代码部分中的命名冲突；再次，访问局部变量比访问全局变量更快；最后，局部变量会随着其作用域的结束而消失，从而使得垃圾收集器能够将其释放。

* Lua语言中有一种常见的用法：
￼
这段代码声明了一个局部变量foo，然后用全局变量foo对其赋初值（局部变量foo只有在声明之后才能被访问）。这个用法在需要提高对foo的访问速度时很有用。当其他函数改变了全局变量foo的值，而代码段又需要保留foo的原始值时，这个用法也很有用，尤其是在进行运行时动态替换（monkey patching，猴子补丁）时。


### 8.2 控制结构

* 由于Lua语言不支持switch语句，所以这种一连串的else-if语句比较常见。

* 和大多数其他编程语言不同，在Lua语言中，循环体内声明的局部变量的作用域包括测试条件

* 在这种循环中，var的值从exp1变化到exp2之前的每次循环会执行something，并在每次循环结束后将步长（step）exp3增加到var上。第三个表达式exp3是可选的，若不存在，Lua语言会默认步长值为1。


### 8.3 break、return和goto

* break和return语句用于从当前的循环结构中跳出，goto语句则允许跳转到函数中的几乎任何地方。

* goto语句用于将当前程序跳转到相应的标签处继续执行。goto语句一直以来备受争议，至今仍有很多人认为它们不利于程序开发并且应该在编程语言中禁止。不过尽管如此，仍有很多语言出于很多原因保留了goto语句。goto语句有很强大的功能，只要足够细心，我们就能够利用它来提高代码质量。


### 第2部分 编程实操

* “第一类值”意味着Lua语言中的函数与其他常见类型的值（例如数值和字符串）具有同等权限：一个程序可以将某个函数保存到变量中（全局变量和局部变量均可）或表中，也可以将某个函数作为参数传递给其他函数，还可以将某个函数作为其他函数的返回值返回。

### 9 闭包

* 请注意，在Lua语言中，所有的函数都是匿名的（anonymous）。像其他所有的值一样，函数并没有名字。当讨论函数名时，比如print，实际上指的是保存该函数的变量。虽然我们通常会把函数赋值给全局变量，从而看似给函数起了一个名字，但在很多场景下仍然会保留函数的匿名性 

* 像函数sort这样以另一个函数为参数的函数，我们称之为高阶函数（ higher-order function ）。高阶函数是一种强大的编程机制，而利用匿名函数作为参数正是其灵活性的主要来源。不过尽管如此，请记住高阶函数也并没有什么特殊的，它们只是Lua语言将函数作为第一类值处理所带来结果的直接体现。

* 当把一个函数存储到局部变量时，就得到了一个局部函数（ local function ），即一个被限定在指定作用域中使用的函数。局部函数对于包（package）而言尤其有用：由于Lua语言将每个程序段（chunk）作为一个函数处理，所以在一段程序中声明的函数就是局部函数，这些局部函数只在该程序段中可见。词法定界保证了程序段中的其他函数可以使用这些局部函数。

* 当编写一个被其他函数B包含的函数A时，被包含的函数A可以访问包含其的函数B的所有局部变量，我们将这种特性称为词法定界（ lexical scoping ） ￼ 。虽然这种可见性规则听上去很明确，但实际上并非如此。词法定界外加嵌套的第一类值函数可以为编程语言提供强大的功能，但很多编程语言并不支持将这两者组合使用。

* 在后一个示例中，有趣的一点就在于传给函数sort的匿名函数可以访问grades，而grades是包含匿名函数的外层函数sortbygrade的形参。在该匿名函数中，grades既不是全局变量也不是局部变量，而是我们所说的非局部变量（ non-local variable ）（由于历史原因，在Lua语言中非局部变量也被称为上值）。

* 由于创建变量的函数（newCounter）己经返回，因此当我们调用匿名函数时，变量count似乎已经超出了作用范围。但其实不然，由于闭包（ closure ）概念的存在，Lua语言能够正确地应对这种情况。简单地说，一个闭包就是一个函数外加能够使该函数正确访问非局部变量所需的其他机制。

* 包在另一种很不一样的场景下也非常有用。由于函数可以被保存在普通变量中，因此在Lua语言中可以轻松地重新定义函数，甚至是预定义函数。这种机制也正是Lua语言灵活的原因之一

* 我们可以使用同样的技巧来创建安全的运行时环境（secure environment），即所谓的沙盒（sandbox）。当执行一些诸如从远程服务器上下载到的未受信任代码（untrusted code）时，安全的运行时环境非常重要。


### 10 模式匹配

* 与其他几种脚本语言不同，Lua语言既没有使用POSIX正则表达式，也没有使用Perl正则表达式来进行模式匹配（pattern matching）。之所以这样做的主要原因在于大小问题：一个典型的POSIX正则表达式实现需要超过4000行代码，这比所有Lua语言标准库总大小的一半还大。相比之下，Lua语言模式匹配的实现代码只有不到600行。尽管Lua语言的模式匹配做不到完整POSIX实现的所有功能，但是Lua语言的模式匹配仍然非常强大，同时还具有一些与标准POSIX不同但又可与之媲美的功能。

* Lua语言的解决方案更加简单：Lua语言中的模式使用百分号（percent sign）作为转义符（C语言中的一些函数采用的也是同样的方式，如函数printf和函数strftime）。总体上，所有被转义的字母都具有某些特殊含义（例如'%a'匹配所有字母），而所有被转义的非字母则代表其本身（例如'%.'匹配一个点）。

* 通常，在Lua程序中使用模式匹配时的效率是足够高的：笔者的新机器可以在不到0.2秒的时间内计算出一个4.4MB大小（具有85万个单词）的文本中所有单词的数量。


### 13 位和字节

* Lua语言从5.3版本开始提供了针对数值类型的一组标准位运算符（bitwise operator）。与算术运算符不同的是，位运算符只能用于整型数。位运算符包括＆（按位与）、|（按位或）、～（按位异或）、>>（逻辑右移）、<<（逻辑左移）和一元运算符～（按位取反）。（请注意，在其他一些语言中，异或运算符为^，而在Lua语言中^代表幂运算。）



### 14 数据结构

* 使用表时，很多算法可以被简化。例如，由于表本身就支持任意数据类型的直接访问，因此我们很少在Lua语言中编写搜索算法。

* 不过，在Lua语言中一般以1作为数组的起始索引，Lua语言的标准库和长度运算符都遵循这个惯例。如果数组的索引不从1开始，那就不能使用这些机制。

* 许多有关数据结构的书籍都会深入地讨论如何实现这种稀疏矩阵而不必浪费800MB内存空间，但在Lua语言中却很少需要用到那些技巧。这是因为，我们使用表实现数组而表本来就是稀疏的。在第一种实现中（表的表），需要1万个表，每个表包含5个元素，总共5万个元素。在第二种实现中，只需要一个表，其中包含5万个元素。无论哪种实现，都只有非nil的元素才占用空间。

* 由于表是动态对象，所以在Lua语言中可以很容易地实现链表（linked list）。我们可以把每个节点用一个表来表示（也只能用表表示），链接则为一个包含指向其他表的引用的简单表字段。例如，让我们实现一个单链表（singly-linked list），其中每个节点具有两个字段value和next。

* 正如此前提到的，我们很少在Lua语言中进行搜索操作。相反，我们使用被称为索引表（index table）或反向表（reverse table）的数据结构。

* 这是为什么呢？为了搞清楚到底发生了什么，让我们想象一下读取循环中发生了什么。假设每行有20字节，当我们读取了大概2500行后，buff就会变成一个50KB大小的字符串。在Lua语言中进行字符串连接buff..line.."\n"时，会创建一个50020字节的新字符串，然后从buff中复制50000字节中到这个新字符串中。这样，对于后续的每一行，Lua语言都需要移动大概50KB且还在不断增长的内存。因此，该算法的时间复杂度是二次方的。在读取了100行（仅2KB）以后，Lua语言就已经移动了至少5MB内存。当Lua语言完成了350KB的读取后，它已经至少移动了50GB的数据。（这个问题不是Lua语言特有的：在其他语言中，只要字符串是不可变值（immutable value），就会出现类似的问题，其中最有名的例子就是Java。


### 16 编译、执行和错误

* 虽然我们把Lua语言称为解释型语言（interpreted language），但Lua语言总是在运行代码前先预编译（precompile）源码为中间代码（这没什么大不了的，很多解释型语言也这样做）。编译（compilation）阶段的存在听上去超出了解释型语言的范畴，但解释型语言的区分并不在于源码是否被编译，而在于是否有能力（且轻易地）执行动态生成的代码。可以认为，正是由于诸如dofile这样函数的存在，才使得Lua语言能够被称为解释型语言。

* 人人皆难免犯错误 ￼ 。因此，我们必须尽可能地处理错误。由于Lua语言是一种经常被嵌入在应用程序中的扩展语言，所以当错误发生时并不能简单地崩溃或退出。相反，只要错误发生，Lua语言就必须提供处理错误的方式。

* Lua语言会在遇到非预期的情况时引发错误。例如，当试图将两个非数值类型的值相加，对不是函数的值进行调用，对不是表类型的值进行索引等（我们会在后续学习中使用元表（ metatable ）来改变上述行为）。我们也可以显式地通过调用函数error并传入一个错误信息作为参数来引发一个错误。

* 由于“针对某些情况调用函数error”这样的代码结构太常见了，所以Lua语言提供了一个内建的函数assert来完成这类工作

* 当一个函数发现某种意外的情况发生时（即异常 exception ），在进行异常处理（exception handling）时可以采取两种基本方式：一种是返回错误代码（通常是nil或者 false ），另一种是通过调用函数error引发一个错误。如何在这两种方式之间进行选择并没有固定的规则，但笔者通常遵循如下的指导原则：容易避免的异常应该引发错误，否则应该返回错误码。


### 17 模块和包

* Lua语言从5.1版本开始为模块和包（package，模块的集合）定义了一系列的规则。这些规则不需要从语言中引入额外的功能，程序员可以使用目前为止我们学习到的机制实现这些规则。程序员也可以自由地使用不同的策略。当然，不同的实现可能会导致程序不能使用外部模块，或者模块不能被外部程序使用。

* 函数require用于搜索Lua文件的路径是变量package.path的当前值。当package模块被初始化后，它就把变量package.path设置成环境变量LUA_PATH_5_3的值。如果这个环境变量没有被定义，那么Lua语言则尝试另一个环境变量LUA_PATH。如果这两个环境变量都没有被定义，那么Lua语言则使用一个编译时定义的默认路径。 ￼ 在使用一个环境变量的值时，Lua语言会将其中所有的";;"替换成默认路径。例如，如果将LUA_PATH_5_3设为"mydir/？.lua;;"，那么最终路径就会是模板"mydir/？.lua"后跟默认路径

* C语言中的名称不能包含点，因此一个用C语言编写的子模块a.b无法导出函数luaopen_a.b。这时，函数require会将点转换为其他字符，即下画线。因此，一个名为a.b的C标准库应将其加载函数命名为luaopen_a_b。


### 18 迭代器和泛型for

* 泛型 for 在循环过程中在其内部保存了迭代函数。实际上，泛型 for 保存了三个值：一个迭代函数、一个不可变状态（ invariant state ）和一个控制变量（ control variable ）。

* 一个常见的困惑发生在开发人员想要对表中的元素进行排序时。由于一个表中的元素没有顺序，所以如果想对这些元素排序，就不得不先把键值对拷贝到一个数组中，然后再对数组进行排序。

* 有些人可能会困惑。毕竟，对于Lua语言来说，数组也没有顺序（毕竟它们是表）。但是我们知道如何数数！因此，当我们使用有序的索引访问数组时，就实现了有序。这正是应该总是使用ipairs而不是pairs来遍历数组的原因。第一个函数通过有序的键1、2等来实现有序，然而后者使用的则是天然的随机顺序（虽然大多数情况下顺序随机也无碍，但有时可能并非我们想要的）。


### 20 元表和元方法

* 元表可以修改一个值在面对一个未知操作时的行为。例如，假设a和b都是表，那么可以通过元表定义Lua语言如何计算表达式a+b。当Lua语言试图将两个表相加时，它会先检查两者之一是否有元表（ metatable ）且该元表中是否有__add字段。如果Lua语言找到了该字段，就调用该字段对应的值，即所谓的元方法（ metamethod ）（是一个函数），在本例中就是用于计算表的和的函数。
可以认为，元表是面向对象领域中的受限制类。像类一样，元表定义的是实例的行为。不过，由于元表只能给出预先定义的操作集合的行为，所以元表比类更受限；同时，元表也不支持继承

* 正如我们此前所看到的，当访问一个表中不存在的字段时会得到nil。这是正确的，但不是完整的真相。实际上，这些访问会引发解释器查找一个名为__index的元方法。如果没有这个元方法，那么像一般情况下一样，结果就是nil；否则，则由这个元方法来提供最终结果。

* 在Lua语言中，使用元方法__index来实现继承是很普遍的方法。虽然被叫作方法，但元方法__index不一定必须是一个函数，它还可以是一个表。当元方法是一个函数时，Lua语言会以表和不存在的键为参数调用该函数，正如我们刚刚所看到的。当元方法是一个表时，Lua语言就访问这个表。


### 21 面向对象（Object-Oriented）编程

* 使用参数 self 是所有面向对象语言的核心点。大多数面向对象语言都向程序员隐藏了这个机制，从而使得程序员不必显式地声明这个参数（虽然程序员仍然可以在方法内使用 self 或者 this ）。Lua语言同样可以使用冒号操作符（ colon operator ）隐藏该参数。

* 大多数面向对象语言提供了类的概念，类在对象的创建中扮演了模子（mold）的作用。在这些语言中，每个对象都是某个特定类的实例（instance）。Lua语言中没有类的概念；虽然元表的概念在某种程度上与类的概念相似，但是把元表当作类使用在后续会比较麻烦。相反，我们可以参考基于原型的语言（prototype-based language）中的一些做法来在Lua语言中模拟类，例如Self语言（JavaScript采用的也是这种方式）。在这些语言中，对象不属于类。相反，每个对象可以有一个原型（prototype）。原型也是一种普通的对象，当对象（类的实例）遇到一个未知操作时会首先在原型中查找。要在这种语言中表示一个类，我们只需要创建一个专门被用作其他对象（类的实例）的原型对象即可。类和原型都是一种组织多个对象间共享行为的方式。

* Lua语言中的对象有一个有趣的特性，就是无须为了指定一种新行为而创建一个新类。如果只有单个对象需要某种特殊的行为，那么我们可以直接在该对象中实现这个行为。例如，假设账户s表示一个特殊的客户，这个客户的透支额度总是其余额的10%，那么可以只修改这个账户

* 许多人认为，私有性（也被称为信息隐藏， information hiding ）是一门面向对象语言不可或缺的一部分：每个对象的状态都应该由它自己控制。在一些诸如C++和Java的面向对象语言中，我们可以控制一个字段（也被称为实例变量， instance variable ）或一个方法是否在对象之外可见。另一种非常流行的面向对象语言Smalltalk，则规定所有的变量都是私有的，而所有的方法都是公有的。第一种面向对象语言Simula，则不提供任何形式的私有性保护。
此前，我们所学习的Lua语言中标准的对象实现方式没有提供私有性机制。一方面，这是使用普通结构（表）来表示对象所带来的后果；另一方面，这也是Lua语言为了避免冗余和人为限制所采取的方法。如果读者不想访问一个对象内的内容，那就不要去访问就是了。一种常见的做法是把所有私有名称的最后加上一个下画线，这样就能立刻区分出全局名称了。
不过，尽管如此，Lua语言的另外一项设计目标是灵活性，它为程序员提供能够模拟许多不同机制的元机制（meta-mechanism）。虽然在Lua语言中，对象的基本设计没有提供私有性机制，但可以用其他方式来实现具有访问控制能力的对象。尽管程序员一般不会用到这种实现，但是了解这种实现还是有好处的，因为这种实现既探索了Lua语言中某些有趣的方面，又可以成为其他更具体问题的良好解决方案。
这种做法的基本思想是通过两个表来表示一个对象：一个表用来保存对象的状态，另一个表用于保存对象的操作（或接口）。我们通过第二个表来访问对象本身，即通过组成其接口的操作来访问。为了避免未授权的访问，表示对象状态的表不保存在其他表的字段中，而只保存在方法的闭包中。


### 22 环境（Environment）

* 全局变量在大多数编程语言中是让人爱恨交织又不可或缺的。一方面，使用全局变量会明显地使无关的代码部分纠缠在一起，容易导致代码复杂。另一方面，谨慎地使用全局变量又能更好地表达程序中真正的全局概念；此外，虽然全局常量看似无害，但像Lua语言这样的动态语言是无法区分常量和变量的。像Lua这样的嵌入式语言更复杂：虽然全局变量是在整个程序中均可见的变量，但由于Lua语言是由宿主应用调用代码段（chunk）的，因此“程序”的概念不明确。
Lua语言通过不使用全局变量的方法来解决这个难题，但又不遗余力地在Lua语言中对全局变量进行模拟。在第一种近似的模拟中，我们可以认为Lua语言把所有的全局变量保存在一个称为全局环境（ global environment ）的普通表中。在本章的后续内容中，我们可以看到Lua语言可以用几种环境来保存“全局”变量，但现在还是来关注第一种近似的模拟。

* Lua语言中的全局变量不需要声明就可以使用。虽然这种行为对于小型程序来说较为方便，但在大型程序中一个简单的手误 ￼ 就有可能造成难以发现的Bug。不过，如果我们乐意的话，也可以改变这种行为。由于Lua语言将全局变量存放在一个普通的表中，所以可以通过元表来发现访问不存在全局变量的情况。

* 在Lua语言中，全局变量并不一定非得是真正全局的。正如笔者此前所提到的，Lua语言甚至根本没有全局变量。由于我们在本书中不断地使用全局变量，所以一开始听上去这可能很诡异。正如笔者所说，Lua语言竭尽全力地让程序员有全局变量存在的幻觉。现在，让我们看看Lua语言是如何构建这种幻觉的。 

* 配置文件中的所有代码会运行在空的环境env中，类似于某种沙盒。特别地，所有的定义都会进入这个环境中。即使出错，配置文件也无法影响任何别的东西，甚至是恶意的代码也不能对其他东西造成任何破坏。除了通过消耗CPU时间和内存来制造拒绝服务（Denial of Service，DoS）攻击，恶意代码也做不了什么其他的事。


### 23 垃圾收集

* Lua语言使用自动内存管理。程序可以创建对象（表、闭包等），但却没有函数来删除对象。Lua语言通过垃圾收集（ garbage collection ）自动地删除成为垃圾的对象，从而将程序员从内存管理的绝大部分负担中解放出来。更重要的是，将程序员从与内存管理相关的大多数Bug中解放出来，例如无效指针（dangling pointer）和内存泄漏（memory leak）等问题。
在一个理想的环境中，垃圾收集器对程序员来说是不可见的，就像一个好的清洁工不会和其他工人打交道一样。不过，有时即使是最智能的垃圾收集器也会需要我们的辅助。在某些关键的性能阶段，我们可能需要将其停止，或者让其只在特定的时间运行。另外，一个垃圾收集器只能收集它确定是垃圾的内容，而不能猜测我们把什么当作垃圾。没有垃圾收集器能够做到让我们完全不用操心资源管理的问题，比如驻留内存（hoarding memory）和外部资源

* 不过，简单地清除引用可能还不够。在有些情况下，还需要程序和垃圾收集器之间的协作。一个典型的例子是，当我们要保存某些类型（例如，文件）的活跃对象的列表时。这个需求看上去很简单，我们只需要把每个新对象插入数组即可；但是，一旦一个对象成为了数组的一部分，它就再也无法被回收了！虽然已经没有其他任何地方在引用它，但数组依然在引用它。除非我们告诉Lua语言数组对该对象的引用不应该阻碍对此对象的回收，否则Lua语言本身是无从知晓的。

* 从程序员的角度看，字符串是值而不是对象。所以，字符串就像数值和布尔值一样，对于一个字符串类型的键来说，除非它对应的值被回收，否则是不会从弱引用表中被移除的。

* Lua语言通过瞬表 ￼ 的概念来解决上述问题。 ￼ 在Lua语言中，一个具有弱引用键和强引用值的表是一个瞬表。在一个瞬表中，一个键的可访问性控制着对应值的可访问性。更确切地说，考虑瞬表中的一个元素 （k,v） ，指向的 v 的引用只有当存在某些指向 k 的其他外部引用存在时才是强引用，否则，即使 v （直接或间接地）引用了 k ，垃圾收集器最终会收集 k 并把元素从表中移除。

* 一直到Lua 5.0，Lua语言使用的都是一个简单的标记-清除（mark-and-sweep）式垃圾收集器（Garbage Collector，GC）。这种收集器又被称为“stop-the-world（全局暂停）”式的收集器，意味着Lua语言会时不时地停止主程序的运行来执行一次完整的垃圾收集周期（garbagecollection cycle）。每一个垃圾收集周期由四个阶段组成：标记（ mark ）、清理（ cleaning ）、清除（ sweep ）和析构（ finalization ）。

* 使用真正的垃圾收集器意味着Lua语言能够处理对象引用之间的环。在使用环形数据结构时，我们不需要花费额外的精力，它们会像其他数据一样被回收。

* Lua 5.1使用了增量式垃圾收集器（ incremental collector ）。这种垃圾收集器像老版的垃圾收集器一样执行相同的步骤，但是不需要在垃圾收集期间停止主程序的运行。相反，它与解释器一起交替运行。每当解释器分配了一定数量的内存时，垃圾收集器也执行一小步（这意味着，在垃圾收集器工作期间，解释器可能会改变一个对象的可达性。为了保证垃圾收集器的正确性，垃圾收集器中的有些操作具有发现危险改动和纠正所涉及对象标记的内存屏障\[barrier\]）。
Lua 5.2引入了紧急垃圾收集（ emergency collection ）。当内存分配失败时，Lua语言会强制进行一次完整的垃圾收集，然后再次尝试分配。这些紧急情况可以发生在Lua语言进行内存分配的任意时刻，包括Lua语言处于不一致的代码执行状态时，因此，这些收集动作不能运行析构器

* 函数collectgarbage的另外一些参数用来在垃圾收集器运行时控制它的行为。同样，对于大多数程序员来说，默认值已经足够好了，但是对于一些特殊的应用，用手工控制可能更好，游戏就经常需要这种类型的控制。例如，如果我们不想让垃圾收集在某些阶段运行，那么可以通过调用函数collectgarbage（"stop"）停止垃圾收集器，然后再调用collectgarba ge（"restart"）重新启动垃圾收集器。在一些具有周期性休眠阶段的程序中，可以让垃圾收集器停止，然后在程序休眠期间调用collectgarbage（"step",n）。要设置在每一个休眠期间进行多少工作，要么为n实验性地选择一个恰当的值，要么把n设成零（意为最小的步长），然后在一个循环中调用函数collectgarbage直到休眠结束。



### 24 协程（Coroutine）

* 我们并不经常需要用到协程，但是当需要的时候，协程会起到一种不可比拟的作用。协程可以颠倒调用者和被调用者的关系，而且这种灵活性解决了软件架构中被笔者称为“谁是老大（who-is-the-boss）”或者“谁拥有主循环（who-has-the-main-loop）”的问题。这正是对诸如事件驱动编程、通过构造器构建迭代器和协作式多线程等几个看上去并不相关的问题的泛化，而协程以简单和高效的方式解决了这些问题。
从多线程（multithreading）的角度看，协程（ coroutine ）与线程（thread）类似：协程是一系列的可执行语句，拥有自己的栈、局部变量和指令指针，同时协程又与其他协程共享了全局变量和其他几乎一切资源。线程与协程的主要区別在于，一个多线程程序可以并行运行多个线程，而协程却需要彼此协作地运行，即在任意指定的时刻只能有一个协程运行，且只有当正在运行的协程显式地要求被挂起（suspend）时其执行才会暂停

* Lua语言中一个非常有用的机制是通过一对resume–yield来交换数据。第一个resume函数（没有对应等待它的yield）会把所有的额外参数传递给协程的主函数

* 虽然协程的概念很容易理解，但涉及的细节其实很多。因此，对于那些已经对协程有一定了解的读者来说，有必要在进行进一步学习前先理清一些细节。Lua语言提供的是所谓的非对称协程（ asymmetric coroutine ），也就是说需要两个函数来控制协程的执行，一个用于挂起协程的执行，另一个用于恢复协程的执行。而其他一些语言提供的是对称协程（ symmetric coroutine ），只提供一个函数用于在一个协程和另一个协程之间切换控制权。
一些人将非对称协程称为 semi-coroutines 。然而，其他人则用相同的术语半协程（ semicoroutine ）表示协程的一种受限制版实现。在这种实现中，一个协程只能在它没有调用其他函数时才可以挂起，即在调用栈中没有挂起的调用时。换句话说，只有这种半协程的主函数才能让出执行权（Python中的 generator 正是这种半协程的一个例子）。
与对称协程和非对称协程之间的区别不同，协程与generator（例如Python中的）之间的区别很大。generator比较简单，不足以实现某些最令人关心的代码结构，而这些代码结构可以使用完整功能的协程实现。Lua语言提供了完整的、非对称的协程。对于那些更喜欢对称协程的用户而言，可以基于非对称协程实现对称协程（参见练习24.6）。

* 如果读者在阅读了上例后想起了POSIX操作系统下的管道（pipe），那么这并非偶然。毕竟，协程是一种非抢占式（non-preemptive）多线程。使用管道时，每项任务运行在各自独立的进程中；而使用协程时，每项任务运行在各自独立的协程中。管道在写入者（生产者）和读取者（消费者）之间提供一个缓冲区，因此它们的相对运行速度可以存在一定差异。由于进程间切换的开销很高，所以这一点在使用管道的场景下非常重要。在使用协程时，任务切换的开销则小得多（基本与函数调用相同），因此生产者和消费者可以手拉手以相同的速度运行。

* 函数permutations使用了Lua语言中一种常见的模式，就是将唤醒对应协程的调用包装在一个函数中。由于这种模式比较常见，所以Lua语言专门提供了一个特殊的函数coroutine.wrap来完成这个功能。与函数create类似，函数wrap也用来创建一个新的协程。但不同的是，函数wrap返回的不是协程本身而是一个函数，当这个函数被调用时会唤醒协程。与原始的函数resume不同，该函数的第一个返回值不是错误代码，当遇到错误时该函数会抛出异常。我们可以使用函数wrap改写permutations


### 25 反射（Re flection）

* 反射是程序用来检查和修改其自身某些部分的能力。像Lua语言这样的动态语言支持几种反射机制：环境允许运行时观察全局变量；诸如type和pairs这样的函数允许运行时检查和遍历未知数据结构；诸如load和require这样的函数允许程序在自身中追加代码或更新代码。不过，还有很多方面仍然是缺失的：程序不能检查局部变量，开发人员不能跟踪代码的执行，函数也不知道是被谁调用的，等等。调试库（debug library）填补了上述的缺失。
调试库由两类函数组成：自省函数（ introspective function ）和钩子（ hook ）。自省函数允许我们检查一个正在运行中的程序的几个方面，例如活动函数的栈、当前正在执行的代码行、局部变量的名称和值。钩子则允许我们跟踪一个程序的执行。
虽然名字里带有“调试”的字眼，但调试库提供的并不是Lua语言的调试器（debugger）。不过，调试库提供了编写我们自己的调试器所需要的不同层次的所有底层机制。
调试库与其他库不同，必须被慎重地使用。首先，调试库中的某些功能的性能不高。其次，调试库会打破语言的一些固有规则，例如不能从一个局部变量的词法定界范围外访问这个局部变量。虽然调试库作为标准库直接可用，但笔者建议在使用调试库的代码段中显式地加载调试库。

* 调试库中主要的自省函数是getinfo，该函数的第一个参数可以是一个函数或一个栈层次。当为某个函数foo调用debug.getinfo（foo）时，该函数会返回一个包含与该函数有关的一些数据的表。

* 函数getinfo的效率不高。Lua语言以一种不影响程序执行的形式来保存调试信息，至于获取这些调试信息的效率则是次要的。为了实现更好的性能，函数getinfo有一个可选的第二参数，该参数用于指定希望获取哪些信息。通过这个参数，函数getinfo就不会浪费时间去收集用户不需要的数据。这个参数是一个字符串，其中每个字母代表选择一组字段，如下表所示

* 我们可以通过函数debug.getlocal来检查任意活跃函数的局部变量。该函数有两个参数，一个是要查询函数的栈层次，另一个是变量的索引。该函数返回两个值，变量名和变量的当前值。如果变量索引大于活跃变量的数量，那么函数getlocal返回nil。如果栈层次无效，则会抛出异常（我们可以使用函数debug.getinfo来检查栈层次是否有效）

* 调试库中的钩子机制允许用户注册一个钩子函数，这个钩子函数会在程序运行中某个特定事件发生时被调用。有四种事件能够触发一个钩子：
·每当调用一个函数时产生的 call 事件；
·每当函数返回时产生的 return 事件；
·每当开始执行一行新代码时产生的 line 事件；
·执行完指定数量的指令后产生的 count 事件。（这里的指令指的是内部操作码，在16.2节中对其有简单的描述。）

* 除了调试，反射的另外一个常见用法是用于调优，即程序使用资源的行为分析。对于时间相关的调优，最好使用C接口，因为每次钩子调用函数开销太大从而可能导致测试结果无效。不过，对于计数性质的调优，Lua代码就可以做得很好。

* 在22.6节中，我们已经看到过，利用函数load在受限的环境中运行Lua代码是非常简单的。由于Lua语言通过库函数完成所有与外部世界的通信，因此一旦移除了这些函数也就排除了一个脚本能够影响外部环境的可能。不过尽管如此，我们仍然可能会被消耗大量CPU时间或内存的脚本进行拒绝服务（DoS）攻击。反射，以调试钩子的形式，提供了一种避免这种攻击的有趣方式

* 由于通过少量指令就可以消耗很多内存，所以我们应该设置一个很低的限制或以很小的步进来调用钩子函数。更具体地说，一个程序用40行以内的指令就能把一个字符串的大小增加上千倍。因此，我们要么以比40条指令更高的频率调用钩子，要么把内存限制设为我们能够承受的最大值的一千分之一。笔者可能两种方式都会采用。

* 我们绝不考虑移除哪些函数，而是应该思考增加哪些函数。对于每一个要增加的函数，必须仔细考虑函数可能的弱点，这些弱点可能隐藏得很深。根据经验，所有数学标准库中的函数都是安全的。字符串库中的大部分也是安全的，只要小心涉及资源消耗的那些函数即可。调试库和模块库则不靠谱，它们中的几乎全部函数都是危险的。函数setmetatable和getmetatable同样很微妙：首先，它们可以访问别人访问不了的值；其次，它们允许创建带有析构器的表，在析构器中可以安装各种各样的“时间炸弹（time bomb）”（当表被垃圾回收时，代码可能在沙盒外被执行）。


### 27 C语言API总览

* 上述两种对Lua语言的定位（嵌入式语言和可扩展语言）分别对应C语言和Lua语言之间的两种交互形式。在第一种形式中，C语言拥有控制权，而Lua语言被用作库，这种交互形式中的C代码被称为应用代码（ application code ）。在第二种形式中，Lua语言拥有控制权，而C语言被用作库，此时的C代码被称为库代码（ library code ）。应用代码和库代码都使用相同的API与Lua语言通信，这些API被称为CAPI。

* 在C语言中，真实的错误处理可能会相当复杂，并且如何处理错误取决于应用的性质。Lua核不会直接向任何输出流写入数据，它只会通过返回错误信息来提示错误。每个应用可以用其所需的最恰当的方式来处理这些错误信息

* Lua和C之间通信的主要组件是无处不在的虚拟栈（ stack ），几乎所有的API调用都是在操作这个栈中的值，Lua与C之间所有的数据交换都是通过这个栈完成的。此外，还可以利用栈保存中间结果。
当我们想在Lua和C之间交换数据时，会面对两个问题：第一个问题是动态类型和静态类型体系之间不匹配；第二个问题是自动内存管理和手动内存管理之间不匹配。

* Lua严格地按照LIFO（Last In First Out，后进先出）的规则来操作栈。在调用Lua时，只有栈顶部的部分会发生改变；而C语言代码则有更大的自由度。更具体地说，C语言可以检视栈中的任何一个元素，甚至可以在栈的任意位置插入或删除元素。

* 即使指定的元素的类型不正确，调用这些函数也不会有问题。函数lua_toboolean适用于所有类型，它可以按照如下的规则将任意Lua值转换为C的布尔值：nil和false转换为0，所有其他的Lua值转换为1。对于类型不正确的值，函数lua_tolstring和lua_tothread返回NULL。不过，数值相关的函数都无法提示数值的类型错误，因此只能简单地返回0。

* Lua语言使用异常来提示错误，而没有在API的每个操作中使用错误码。与C++或Java不同，C语言没有提供异常处理机制。为了解决这个问题，Lua使用了C语言中的setjmp机制，setjmp营造了一个类似异常处理的机制。因此，大多数API函数都可以抛出异常（即调用函数longjmp）而不是直接返回。
在编写库代码时（被Lua语言调用的C函数），由于Lua会捕获所有异常，因此，对我们来说使用longjmp并不用进行额外的操作。不过，在编写应用程序代码（调用Lua的C代码）时，则必须提供一种捕获异常的方式

* Lua是一种安全（ safe ）的语言。这意味着不管用Lua写的是什么，也不管写出来的内容多么不正确，我们总是能用它自身的机制来理解程序的行为。此外，程序中的错误（error）也是通过Lua语言的机制来检测和解释的。与之相比，许多C语言代码中的错误只能从底层硬件的角度来解释（例如，把异常位置作为指令地址给出）。

* Lua语言核心对内存分配不进行任何假设，它既不会调用malloc也不会调用realloc来分配内存。相反，Lua语言核心只会通过一个分配函数（ allocation function ）来分配和释放内存，当用户创建Lua状态时必须提供该函数。

* 研究表明，内存碎片更多是由糟糕的分配策略导致的，而非程序的行为造成的；而优秀的分配函数不会造成太多内存碎片。


### 28 扩展应用

* 最后一个使用Lua的理由是，使用它以后，向程序中添加新的配置机制时会很方便。这种便利性可以让人形成一种态度，这种态度让程序变得更加灵活。


### 29 在Lua中调用C语言

* 我们说Lua可以调用C语言函数，但这并不意味着Lua可以调用所有的C函数 ￼ 。正如我们在第28章所看到的，当C语言调用Lua函数时，该函数必须遵循一个简单的规则来传递参数和获取结果。同样，当Lua调用C函数时，这个C函数也必须遵循某种规则来获取参数和返回结果。此外，当Lua调用C函数时，我们必须注册该函数，即必须以一种恰当的方式为Lua提供该C函数的地址。
Lua调用C函数时，也使用了一个与C语言调用Lua函数时相同类型的栈，C函数从栈中获取参数，并将结果压入栈中。
此处的重点在于，这个栈不是一个全局结构；每个函数都有其私有的局部栈（private local stack）。当Lua调用一个C函数时，第一个参数总是位于这个局部栈中索引为1的位置。即使一个C函数调用了Lua代码，而且Lua代码又再次调用了同一个（或其他）的C函数，这些调用每一次都只会看到本次调用自己的私有栈，其中索引为1的位置上就是第一个参数。


### 30 编写C函数的技巧

* 函数lua_call做的是不受保护的调用，该函数类似于lua_pcall，但在发生错误时lua_call会传播错误而不是返回错误码。在一个应用中编写主函数时，不应使用lua_call，因为我们需要捕获所有的错误。不过，编写一个函数时，一般情况下使用lua_call是个不错的主意；如果发生错误，就留给关心错误的人去处理吧。


* 当C函数接收到一个Lua字符串为参数时，必须遵守两条规则：在使用字符串期间不能从栈中将其弹出，而且不应该修改字符串。

* 通常情况下，C函数需要保存一些非局部数据，即生存时间超出C函数执行时间的数据。在C语言中，我们通常使用全局变量（extern）或静态变量来满足这种需求。然而，当我们为Lua编写库函数时 ￼ ，这并不是一个好办法。首先，我们无法在一个C语言变量中保存普通的Lua值。其次，使用这类变量的库无法用于多个Lua状态。
更好的办法是从Lua语言中寻求帮助。Lua函数有两个地方可用于存储非局部数据，即全局变量和非局部变量，而CAPI也提供了两个类似的地方来存储非局部数据，即注册表（registry）和上值（upvalue）。


### 31 C语言中的用户自定义类型

* 这个示例实现了一种很简单的类型，即布尔数组。选用这个示例的主要动机在于它不涉及复杂的算法，便于我们专注于API的问题。不过尽管如此，这个示例本身还是很有用的。当然，我们可以在Lua中用表来实现布尔数组。但是，在C语言实现中，可以将每个布尔值存储在一个比特中，所使用的内存量不到使用表方法的3%。

* BITS_PER_WORD表示一个无符号整型数的位数，宏I_WORD用于根据指定的索引来计算存放相应比特位的字，I_BIT用于计算访问这个字中相应比特位要用的掩码。

* 轻量级用户数据为这种映射提供了一种好的解决方案。我们可以保存一张表，其中键是带有流地址的轻量级用户数据，值是Lua中表示流的完全用户数据。在回调函数中，一旦有了流地址，就可以将其作为轻量级用户数据，把它当作这张表的索引来获取对应的Lua对象（这张表很可能得是弱引用的；否则，这些完全用户数据可能永远不会被作为垃圾回收）。


### 32 管理资源

* 现在，让我们看一下如何在Lua中使用这个库。第一种方法是一种直接的方法，即简单地把所有函数导出给Lua。另一个更好的方法是让这些函数适配Lua。例如，因为Lua语言不是强类型的，所以不需要为每一种回调函数设置不同的函数。我们可以做得更好，甚至免去所有注册回调函数的函数。我们要做的只是在创建解析器时提供一个包含所有事件处理函数的回调函数表，其中每一个键值对是与相应事件对应的键和事件处理函数。


### 33 线程和状态

* Lua语言不支持真正的多线程，即不支持共享内存的抢占式线程。原因有两个，其一是ISO C没有提供这样的功能，因此也没有可移植的方法能在Lua中实现这种机制；其二，也是更重要的原因，在于我们认为在Lua中引入多线程不是一个好主意

* 多线程一般用于底层编程。像信号量（semaphore）和监视器（monitor）这样的同步机制一般都是操作系统上下文（以及老练的程序员）提供的，而非应用程序提供。要查找和纠正多线程相关的Bug是很困难的，其中有些Bug还会导致安全隐患。此外，程序中的一些需要同步的临界区（例如内存分配函数）还可能由于同步而导致性能问题。

* 多线程的这些问题源于线程抢占（preemption）和共享内存，因此如果使用非抢先式的线程或者不使用共享内存就可以避免这些问题。Lua语言同时支持这两种方案。Lua语言的线程（也就是所谓的协程）是协作式的，因此可以避免因不可预知的线程切换而带来的问题。另一方面，Lua状态之间不共享内存，因此也为Lua语言中实现并行化提供了良好基础。

* 每次调用luaL_newstate（或lua_newstate）都会创建一个新的Lua状态。不同的Lua状态之间是完全独立的，它们根本不共享数据。也就是说，无论在一个Lua状态中发生了什么，都不会影响其他Lua状态。这也意味着Lua状态之间不能直接通信，因而必须借助一些C语言代码的帮助。

* 在支持多线程的系统中，一种有趣的设计是为每个线程创建一个独立的Lua状态。这种设计使得线程类似于POSIX进程，它实现了非共享内存的并发（concurrency）。在本节中，我们会根据这种方法开发一个多线程的原型实现。在这个实现中，将会使用POSIX线程（pthread）。因为这些代码只使用了一些基础功能，所以将它们移植到其他线程系统中并不难。

