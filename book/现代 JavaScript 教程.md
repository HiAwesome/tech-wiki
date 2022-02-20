# [现代 JavaScript 教程](https://zh.javascript.info/)

## 散装笔记

* 多行输入：通常，当我们向控制台输入一行代码后，按 Enter，这行代码就会立即执行。如果想要插入多行代码，请按 Shift+Enter 来进行换行。这样就可以输入长片段的 JavaScript 代码了。
* 即使语句被换行符分隔了，我们依然建议在它们之间加分号。这个规则被社区广泛采用。我们再次强调一下 —— 大部分时候可以省略分号，但是最好不要省略分号，尤其对新手来说。
* "use strict": 这个指令看上去像一个字符串 "use strict" 或者 'use strict'。当它处于脚本文件的顶部时，则整个脚本文件都将以“现代”模式进行工作。
* 当你使用 开发者控制台 运行代码时，请注意它默认是不启动 use strict 的。 有时，当 use strict 会对代码产生一些影响时，你会得到错误的结果。 那么，怎么在控制台中启用 use strict 呢？ 首先，你可以尝试搭配使用 Shift+Enter 按键去输入多行代码，然后将 use strict 放在代码最顶部，就像这样:
  ```text
  'use strict'; <Shift+Enter 换行>
  //  ...你的代码
  <按下 Enter 以运行>
  ```
* T

## 兼容性表

JavaScript 是一门还在发展中的语言，定期会添加一些新的功能。

要查看它们在基于浏览器的引擎及其他引擎中的支持情况，请看：

- <http://caniuse.com> —— 每个功能的支持表，例如，查看哪个引擎支持现代加密（cryptography）函数：<http://caniuse.com/#feat=cryptography>。
- <https://kangax.github.io/compat-table> —— 一份列有语言功能以及引擎是否支持这些功能的表格。

所有这些资源在实际开发中都有用武之地，因为它们包含了有关语言细节，以及它们被支持的程度等非常有价值的信息。

