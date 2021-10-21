# JS

### [JavaScript 错误参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Errors)

## JS 基础

* API 是已经建立好的一套代码组件，可以让开发者实现原本很难甚至无法实现的程序。就像现成的家具套件之于家居建设，用一些已经切好的木板组装一个书柜，显然比自己设计，寻找合适的木材，裁切至合适的尺寸和形状，找到正确尺寸的螺钉，再组装成书柜要简单得多。 API 通常分为两类：浏览器 API 与第三方 API.
* 先稳住！你看到的只是冰山一角。你不可能学一天 JavaScript 就能构建下一个Facebook, 谷歌地图, 或者 Instagram。敬请「牢记初心，砥砺前行」。
* JavaScript 是轻量级解释型语言。浏览器接受到JavaScript代码，并以代码自身的文本格式运行它。技术上，几乎所有 JavaScript 转换器都运用了一种叫做即时编译（just-in-time compiling）的技术；当 JavaScript 源代码被执行时，它会被编译成二进制的格式，使代码运行速度更快。尽管如此，JavaScript 仍然是一门解释型语言，因为编译过程发生在代码运行中，而非之前。
* 而服务器端代码在服务器上运行，接着运行结果才由浏览器下载并展示出来。流行的服务器端 web 语言包括：PHP、Python、Ruby、ASP.NET 以及...... JavaScript！JavaScript 也可用作服务器端语言，比如现在流行的 Node.js 环境，你可以在我们的 [动态网页 - 服务器端编程](https://developer.mozilla.org/zh-CN/docs/learn/Server-side) 主题中找到更多关于服务器端 JavaScript 的知识。
* 脚本调用策略小结：
  * 如果脚本无需等待页面解析，且无依赖独立运行，那么应使用 async。
  * 如果脚本需要等待页面解析，且依赖于其它脚本，调用这些脚本时应使用 defer，将关联的脚本按所需顺序置于 HTML 中。
* 就像 HTML 和 CSS，JavaScript 代码中也可以添加注释，浏览器会忽略它们，注释只是为你的同事（还有你，如果半年后再看自己写的代码你会说，这是什么垃圾玩意。）提供关于代码如何工作的指引。注释非常有用，而且应该经常使用，尤其在大型应用中。
* 像程序员一样思考
  * 学习编程，语法本身并不难，真正困难的是如何应用它来解决现实世界的问题。 你要开始像程序员那样思考。一般来讲，这种思考包括了解你程序运行的目的，为达到该目的应选定的代码类型，以及如何使这些代码协同运行。
  * 为达成这一点，我们需要努力编程，获取语法经验，注重实践，再加一点创造力，几项缺一不可。代码写的越多，就会完成的越优秀。虽然我们不能保证你在5分钟内拥有“程序员大脑”，但是整个课程中你将得到大量机会来训练程序员思维。
  * 请牢记这一点，然后开始观察本文的示例，体会一下将其分解为可操作任务的大体过程。
* 事件就是浏览器中发生的事儿，比如点击按钮、加载页面、播放视频，等等，我们可以通过调用代码来响应事件。 侦听事件发生的结构称为事件监听器（Event Listener），响应事件触发而运行的代码块被称为事件处理器（Event Handler）。
* 这一行通过 [focus()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus) 方法让光标在页面加载完毕时自动放置于 [\<input\>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input)  输入框内，这意味着玩家可以马上开始第一次猜测，而无需点击输入框。 这只是一个小的改进，却提高了可用性——为使用户能投入游戏提供一个良好的视觉线索。 深入分析一下。JavaScript 中一切都是对象。对象是存储在单个分组中的相关功能的集合。可以创建自己的对象，但这是较高阶的知识，我们今后才会谈及。现在，仅需简要讨论浏览器内置的对象，它们已经能够做许多有用的事情。
* 一般来说，代码错误主要分为两种：
  * 语法错误：代码中存在拼写错误，将导致程序完全或部分不能运行，通常你会收到一些出错信息。只要熟悉语言并了解出错信息的含义，你就能够顺利修复它们。
  * 逻辑错误：有些代码语法虽正确，但执行结果和预期相悖，这里便存在着逻辑错误。这意味着程序虽能运行，但会给出错误的结果。由于一般你不会收到来自这些错误的提示，它们通常比语法错误更难修复。
  * 事情远没有你想的那么简单，随着探究的深入，会有更多差异因素浮出水面。但在编程生涯的初级阶段上述分类方法已足矣。
* 注：更多信息请参考 [类型错误：“x”不是一个函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Errors/Not_a_function).
* 这里我们需要一个类选择器，它应以一个点开头（.），但被传递到第 48 行的 querySelector() 方法中的选择器没有点。这可能是问题所在！尝试将第 48 行中的 lowOrHi 改成 .lowOrHi。
* 注：此错误的更多详细信息请参阅：类型错误：[“x”（不）是“y”](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Errors/Unexpected_type).
* 我们说，变量是用来存储数值的，那么有一个重要的概念需要区分。变量不是数值本身，它们仅仅是一个用于存储数值的容器。你可以把变量想象成一个个用来装东西的纸箱子。
* 提示: 在JavaScript中，所有代码指令都会以分号结尾 (;) — 如果忘记加分号，你的单行代码可能执行正常，但是在多行代码在一起的时候就可能出错。所以，最好是养成主动以分号作为代码结尾的习惯。
* 提示: 千万不要把两个概念弄混淆了，“一个变量存在，但是没有数值”和“一个变量并不存在” — 他们完全是两回事 — 在前面你看到的盒子的类比中，不存在意味着没有可以存放变量的“盒子”。没有定义的值意味着有一个“盒子”，但是它里面没有任何值。
* 出于这些以及其他原因，我们建议您在代码中尽可能多地使用 let，而不是 var。因为没有理由使用 var，除非您需要用代码支持旧版本的 Internet Explorer (它直到第 11 版才支持 let ，现代的 Windows Edge 浏览器支持的很好)。
* Note: 你能从 [词汇语法—关键字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#keywords) 找到一个相当完整的保留关键字列表来避免使用关键字当作变量。
* Note: 有时你可能会看到使用较旧的 [Math.pow()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/pow) 方法表达的指数，该方法的工作方式非常相似。 例如，在 Math.pow(7, 3)  中，7 是基数，3 是指数，因此表达式的结果是 343。 Math.pow(7, 3) 相当于 7 ** 3。
* Note: 您可能会看到有些人在他们的代码中使用==和!=来判断相等和不相等，这些都是JavaScript中的有效运算符，但它们与===/!==不同，前者测试值是否相同， 但是数据类型可能不同，而后者的严格版本测试值和数据类型是否相同。 严格的版本往往导致更少的错误，所以我们建议您使用这些严格的版本。
* 要修复我们之前的问题代码行，我们需要避免引号的问题。转义字符意味着我们对它们做一些事情，以确保它们被识别成文本，而不是代码的一部分。在JavaScript中，我们通过在字符之前放一个反斜杠来实现这一点。
* 如果可以的话， [Number](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number) 对象将把传递给它的任何东西转换成一个数字。另一方面，每个数字都有一个名为 [toString()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toString) 的方法，它将把它转换成等价的字符串。
* 检索特定字符串字符：在相关的注释中，您可以使用方括号表示法返回字符串中的任何字符 - 这意味着您可以在变量名的末尾包含方括号（\[\]）。 在方括号内，您可以包含要返回的字符的编号，例如，您要检索第一个字母，可以这样做：`browserType[0];`
* 当你知道字符串中的子字符串开始的位置，以及想要结束的字符时，[slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/slice) 可以用来提取 它。 尝试以下：`browserType.slice(0,3);`
* 注意，在实际程序中，想要真正更新 browserType 变量的值，您需要设置变量的值等于刚才的操作结果；它不会自动更新子串的值。所以事实上你需要这样写：`browserType = browserType.replace('moz','van');`
* 您可以将任何类型的元素存储在数组中 - 字符串，数字，对象，另一个变量，甚至另一个数组。 您也可以混合和匹配项目类型 - 它们并不都是数字，字符串等。
* 首先，要在数组末尾添加或删除一个项目，我们可以使用 [push()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 和 [pop()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/pop).
* [unshift()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift) 和 [shift()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/shift) 从功能上与 [push()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 和 [pop()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/pop) 完全相同，只是它们分别作用于数组的开始，而不是结尾。
* 我们想特别提到测试布尔值（true / false），和一个通用模式，你会频繁遇到它，任何不是 false, undefined, null, 0, NaN 的值，或一个空字符串（''）在作为条件语句进行测试时实际返回true，因此您可以简单地使用变量名称来测试它是否为真，甚至是否存在（即它不是未定义的）。
* [switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) 语句在这里是您的朋友 - 他们以单个表达式/值作为输入，然后查看多个选项，直到找到与该值相匹配的选项，执行与之相关的代码。
  ```text
  var select = document.querySelector('select');
  var para = document.querySelector('p');
  
  select.addEventListener('change', setWeather);
  
  
  function setWeather() {
    var choice = select.value;
  
    switch (choice) {
      case 'sunny':
        para.textContent = 'It is nice and sunny outside today. Wear shorts! Go to the beach, or the park, and get an ice cream.';
        break;
      case 'rainy':
        para.textContent = 'Rain is falling outside; take a rain coat and a brolly, and don\'t stay out for too long.';
        break;
      case 'snowing':
        para.textContent = 'The snow is coming down — it is freezing! Best to stay in with a cup of hot chocolate, or go build a snowman.';
        break;
      case 'overcast':
        para.textContent = 'It isn\'t raining, but the sky is grey and gloomy; it could turn any minute, so take a rain coat just in case.';
        break;
      default:
        para.textContent = '';
    }
  }
  ```
* [三元或条件运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) 是一个语法的小点，用于测试一个条件，并返回一个值/表达，如果它是true，另一个是false-这种情况下是有用的，并且可以占用比if...else块较少的代码块。
* 

































































































































