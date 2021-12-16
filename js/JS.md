# JS

### Update

* [How to check if array is empty or does not exist?](https://stackoverflow.com/a/24403771), `if (!Array.isArray(array) || !array.length) {}`.

### [JavaScript 错误参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Errors)

### [JS Object 对象包装器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)

### [JavaScript 标准内置对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

### [JS Class 类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)

### [异步 JavaScript: 选择正确的方法](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Asynchronous/Choosing_the_right_approach)

### [JS Learn Promises](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Asynchronous/Promises)

### [JS Learn Async/Await](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Asynchronous/Async_await)

### [Web API 接口参考](https://developer.mozilla.org/zh-CN/docs/Web/API)

### [JS 正则表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)

* [JavaScript regular expression exception (Invalid group)](https://stackoverflow.com/a/4200196), `(?<= )` is a positive lookbehind. JavaScript's flavor of RegEx does not support lookbehinds (but it does support lookaheads).

### JS Books

* [Secrets of the JavaScript Ninja](https://www.amazon.com/gp/product/193398869X/), 第6章 - 由John Resig和Bear Bibeault撰写的关于高级JavaScript概念和技术的好书。第6章很好地介绍了原型和继承的相关方面；您可以很容易地找到打印版本或在线副本。
* [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS/tree/1ed-zh-CN): this & Object Prototypes - 凯尔·辛普森（Kyle Simpson）的一系列优秀的JavaScript手册，第5章对原型的解释比我们在这里做的更详细。我们在本系列针对初学者的文章中提出了简化的观点，而凯尔深入学习，并提供了更为复杂但更准确的图景。

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
* 重要: 使用 for 与所有循环一样，您必须确保初始化程序被迭代，以便最终达到退出条件。 如果没有，循环将永不停止，浏览器将强制它停止，否则会崩溃。 这被称为无限循环。
* 注：还有其他循环类型/特性，这些特性在 高级/专门 的情况下是有用的，超出了本文的范围。如果您想进一步了解循环学习，请阅读我们的 [高级循环和迭代指南](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Loops_and_iteration).
* 这个函数叫做匿名函数 — 它没有函数名! 它也不会自己做任何事情。 你通常将匿名函数与事件处理程序一起使用, 例如，如果单击相关按钮，以下操作将在函数内运行代码：
  ```text
  var myButton = document.querySelector('button');
  
  myButton.onclick = function() {
    alert('hello');
  }
  ```
* 匿名函数也称为函数表达式。函数表达式与函数声明有一些区别。函数声明会进行声明提升（declaration hoisting），而函数表达式不会。
* 通常，返回值是用在函数在计算某种中间步骤。你想得到最终结果，其中包含一些值。那些值需要通过一个函数计算得到，然后返回结果可用于计算的下一个阶段。
* 函数的 [默认参数值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Default_parameters).
* 每个可用的事件都会有一个事件处理器，也就是事件触发时会运行的代码块。当我们定义了一个用来回应事件被激发的代码块的时候，我们说我们注册了一个事件处理器。注意事件处理器有时候被叫做事件监听器——从我们的用意来看这两个名字是相同的，尽管严格地来说这块代码既监听也处理事件。监听器留意事件是否发生，然后处理器就是对事件发生做出的回应。
* 注： 网络事件不是 JavaScript 语言的核心——它们被定义成内置于浏览器的 JavaScript APIs。
* 值得注意的是并不是只有 JavaScript 使用事件——大多的编程语言都有这种机制，并且它们的工作方式不同于 JavaScript。实际上，JavaScript 网页上的事件机制不同于在其他环境中的事件机制。 比如, [Node.js](https://developer.mozilla.org/zh-CN/docs/Learn/Server-side/Express_Nodejs) 是一种非常流行的允许开发者使用 JavaScript 来建造网络和服务器端应用的运行环境。 [Node.js event model](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/) 依赖定期监听事件的监听器和定期处理事件的处理器——虽然听起来好像差不多，但是实现两者的代码是非常不同的，Node.js 使用像 on() 这样的函数来注册一个事件监听器，使用 once() 这样函数来注册一个在运行一次之后注销的监听器。
* 一些事件非常通用，几乎在任何地方都可以用（比如 onclick 几乎可以用在几乎每一个元素上），然而另一些元素就只能在特定场景下使用，比如我们只能在 video 元素上使用 [onplay](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onplay).
* 行内事件处理器 - 请勿使用: 你会发现HTML属性等价于对许多事件处理程序的属性；但是，你不应该使用这些 —— 他们被认为是不好的做法。使用一个事件处理属性似乎看起来很简单，如果你只是在做一些非常快的事情，但很快就变得难以管理和效率低下。 一开始，您不应该混用 HTML 和 JavaScript，因为这样文档很难解析——最好的办法是只在一块地方写 JavaScript 代码。 即使在单一文件中，内置事件处理器也不是一个好主意。一个按钮看起来还好，但是如果有一百个按钮呢？您得在文件中加上100个属性。这很快就会成为维护人员的噩梦。使用 Java Script，您可以给网页中的 button 都加上事件处理器。 注释: 将您的编程逻辑与内容分离也会让您的站点对搜索引擎更加友好。
* 我该使用哪种机制？ 在三种机制中,您绝对不应该使用HTML事件处理程序属性 - 这些属性已经过时了，而且也是不好的做法，如上所述. 另外两种是相对可互换的，至少对于简单的用途:
  * 事件处理程序属性功能和选项会更少，但是具有更好的跨浏览器兼容性(在Internet Explorer 8的支持下)，您应该从这些开始学起。
  DOM Level 2 Events (addEventListener(), etc.) 更强大，但也可以变得更加复杂，并且支持不足（只支持到Internet Explorer 9）。 但是您也应该尝试这个方法，并尽可能地使用它们。 
  * 第三种机制（DOM Level 2 Events (addEventListener(), etc.)）的主要优点是，如果需要的话，可以使用removeEventListener()删除事件处理程序代码，而且如果有需要，您可以向同一类型的元素添加多个监听器。例如，您可以在一个元素上多次调用addEventListener('click', function() { ... })，并可在第二个参数中指定不同的函数。
  * 注解:如果您在工作中被要求支持比Internet Explorer 8更老的浏览器，那么您可能会遇到困难，因为这些古老的浏览器会使用与现代浏览器不同的事件处理模型。但是不要害怕，大多数 JavaScript 库(例如 jQuery )都内置了能够跨浏览器差异的函数。在你学习 JavaScript 旅程里的这个阶段，不要太担心这个问题。
* 事件对象: 有时候在事件处理函数内部，您可能会看到一个固定指定名称的参数，例如event，evt或简单的e。 这被称为事件对象，它被自动传递给事件处理函数，以提供额外的功能和信息。 事件对象 e 的target属性始终是事件刚刚发生的元素的引用。Note: 您可以使用任何您喜欢的名称作为事件对象 - 您只需要选择一个名称，然后可以在事件处理函数中引用它。 开发人员最常使用 e / evt / event，因为它们很简单易记。 坚持标准总是很好。当您要在多个元素上设置相同的事件处理程序时，e.target非常有用，并且在发生事件时对所有元素执行某些操作.  例如，你可能有一组16块方格，当它们被点击时就会消失。用e.target总是能准确选择当前操作的东西（方格）并执行操作让它消失，而不是必须以更困难的方式选择它。
* 你遇到的大多数事件处理器的事件对象都有可用的标准属性和函数（方法）（请参阅完整列表 Event 对象引用 ）。然而，一些更高级的处理程序会添加一些专业属性，这些属性包含它们需要运行的额外数据。例如，媒体记录器API有一个dataavailable事件，它会在录制一些音频或视频时触发，并且可以用来做一些事情(例如保存它，或者回放)。对应的ondataavailable处理程序的事件对象有一个可用的数据属性。
* 事件冒泡及捕获: 最后即将介绍的这个主题你常常不会深究，但如果你不理解这个主题，就会十分痛苦。事件冒泡和捕捉是两种机制，主要描述当在一个元素上有两个相同类型的事件处理器被激活会发生什么。
* 注解: 为什么我们要弄清楚捕捉和冒泡呢？那是因为，在过去糟糕的日子里，浏览器的兼容性比现在要小得多，Netscape（网景）只使用事件捕获，而Internet Explorer只使用事件冒泡。当W3C决定尝试规范这些行为并达成共识时，他们最终得到了包括这两种情况（捕捉和冒泡）的系统，最终被应用在现在浏览器里。
* 注解: 如上所述，默认情况下，所有事件处理程序都是在冒泡阶段注册的，这在大多数情况下更有意义。如果您真的想在捕获阶段注册一个事件，那么您可以通过使用addEventListener()注册您的处理程序，并将可选的第三个属性设置为true。
* 冒泡还允许我们利用事件委托——这个概念依赖于这样一个事实,如果你想要在大量子元素中单击任何一个都可以运行一段代码，您可以将事件监听器设置在其父节点上，并让子节点上发生的事件冒泡到父节点上，而不是每个子节点单独设置事件监听器。 一个很好的例子是一系列列表项，如果你想让每个列表项被点击时弹出一条信息，您可以将click单击事件监听器设置在父元素 \<ul\> 上，这样事件就会从列表项冒泡到其父元素 \<ul\ >上。
* 对象成员的值可以是任意的，在我们的person对象里有字符串(string)，数字(number)，两个数组(array)，两个函数(function)。前4个成员是资料项目，被称为对象的属性(property)，后两个成员是函数，允许对象对资料做一些操作，被称为对象的方法(method) 一个如上所示的对象被称之为对象的字面量(literal)——手动的写出对象的内容来创建一个对象。不同于从类实例化一个对象，我们会在后面学习这种方式。 当你想要传输一些有结构和关联的资料时常见的方式是使用字面量来创建一个对象，举例来说，发起一个请求到服务器以存储一些数据到数据库，发送一个对象要比分别发送这些数据更有效率，而且比起数组更为易用，因为你使用名字(name)来标识这些资料。
* 括号表示法一个有用的地方是它不仅可以动态的去设置对象成员的值，还可以动态的去设置成员的名字。这是使用点表示法无法做到的，点表示法只能接受字面量的成员的名字，不接受变量作为名字。
  ```text
  var myDataName = 'height'
  var myDataValue = '1.75m'
  person[myDataName] = myDataValue
  
  person.height
  ```
* 你也许想知道"this"是什么，关键字"this"指向了当前代码运行时的对象( 原文：the current object the code is being written inside )——这里即指person对象，为什么不直接写person呢？当你学到下一篇 [Object-oriented JavaScript for beginners](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_JS) 文章时，我们开始使用构造器(constructor)时，"this"是非常有用的——它保证了当代码的上下文(context)改变时变量的值的正确性（比如：不同的person对象拥有不同的name这个属性，很明显greeting这个方法需要使用的是它们自己的name）。
* 请注意内建的对象或API不会总是自动地创建对象的实例，举例来说，这个 [Notifications API](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API)——允许浏览器发起系统通知，需要你为每一个你想发起的通知都使用构造器进行实例化。 Note: 这样来理解对象之间通过消息传递来通信是很有用的——当一个对象想要另一个执行某种动作时，它通常会通过那个对象的方法给其发送一些信息，并且等待回应，即我们所知的返回值。
* 在一些面向对象的语言中，我们用类（class）的概念去描述一个对象（您在下面就能看到JavaScript使用了一个完全不同的术语）-类并不完全是一个对象，它更像是一个定义对象特质的模板。当一个对象需要从类中创建出来时，类的构造函数就会运行来创建这个实例。这种创建对象的过程我们称之为实例化-实例对象被类实例化。
* 注：多态——这个高大上的词正是用来描述多个对象拥有实现共同方法的能力。
* 有些人认为 JavaScript 不是真正的面向对象的语言，比如它没有像许多面向对象的语言一样有用于创建class类的声明。JavaScript 用一种称为构建函数的特殊函数来定义对象和它们的特征。构建函数非常有用，因为很多情况下您不知道实际需要多少个对象（实例）。构建函数提供了创建您所需对象（实例）的有效方法，将对象的数据和特征函数按需联结至相应对象。 不像“经典”的面向对象的语言，从构建函数创建的新实例的特征并非全盘复制，而是通过一个叫做原形链的参考链链接过去的。（参见 [Object prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes) ），所以这并非真正的实例，严格的讲， JavaScript 在对象间使用和其它语言的共享机制不同。 注： “经典”的面向对象的语言并非好事，就像上面提到的，OOP 可能很快就变得非常复杂，JavaScript 找到了在不变的特别复杂的情况下利用面向对象的优点的方法。
* 这个构建函数是 JavaScript 版本的类。您会发现，它只定义了对象的属性和方法，除了没有明确创建一个对象和返回任何值和之外，它有了您期待的函数所拥有的全部功能。这里使用了this关键词，即无论是该对象的哪个实例被这个构建函数创建，它的 name 属性就是传递到构建函数形参name的值，它的 greeting() 方法中也将使用相同的传递到构建函数形参name的值。注： 一个构建函数通常是大写字母开头，这样便于区分构建函数和普通函数。值得注意的是每次当我们调用构造函数时，我们都会重新定义一遍 greeting()，这不是个理想的方法。为了避免这样，我们可以在原型里定义函数，接下来我们会讲到。
  ```text
  // before:
  function createNewPerson(name) {
    var obj = {};
    obj.name = name;
    obj.greeting = function () {
      alert('Hi! I\'m ' + this.name + '.');
    }
    return obj;
  }
  
  var salva = createNewPerson('salva');
  salva.name;
  salva.greeting();
  
  // after:
  function Person(name) {
    this.name = name;
    this.greeting = function() {
      alert('Hi! I\'m ' + this.name + '.');
    };
  }

  var person1 = new Person('Bob');
  var person2 = new Person('Sarah');
  ```
* JavaScript 常被描述为一种基于原型的语言 (prototype-based language)——每个对象拥有一个原型对象，对象以其原型为模板、从原型继承方法和属性。原型对象也可能拥有原型，并从中继承方法和属性，一层一层、以此类推。这种关系常被称为原型链 (prototype chain)，它解释了为何一个对象会拥有定义在其他对象中的属性和方法。 准确地说，这些属性和方法定义在Object的构造器函数(constructor functions)之上的prototype属性上，而非对象实例本身。 在传统的 OOP 中，首先定义“类”，此后创建对象实例时，类中定义的所有属性和方法都被复制到实例中。在 JavaScript 中并不如此复制——而是在对象实例和它的构造器之间建立一个链接（它是__proto__属性，是从构造函数的prototype属性派生的），之后通过上溯原型链，在构造器中找到这些属性和方法。注意: 理解对象的原型（可以通过Object.getPrototypeOf(obj)或者已被弃用的__proto__属性获得）与构造函数的prototype属性之间的区别是很重要的。前者是每个实例上都有的属性，后者是构造函数的属性。也就是说，Object.getPrototypeOf(new Foobar())和Foobar.prototype指向着同一个对象。
* 注意：必须重申，原型链中的方法和属性没有被复制到其他对象——它们被访问需要通过前面所说的“原型链”的方式。
* 注意：没有官方的方法用于直接访问一个对象的原型对象——原型链中的“连接”被定义在一个内部属性中，在 JavaScript 语言标准中用 \[\[prototype\]\] 表示（参见 [ECMAScript](https://developer.mozilla.org/zh-CN/docs/Glossary/ECMAScript) ）。然而，大多数现代浏览器还是提供了一个名为 \_\_proto__ （前后各有2个下划线）的属性，其包含了对象的原型。你可以尝试输入 person1.\_\_proto__ 和 person1.\_\_proto__.\_\_proto__，看看代码中的原型链是什么样的！
* 注意：这看起来很奇怪——构造器本身就是函数，你怎么可能在构造器这个函数中定义一个方法呢？其实函数也是一个对象类型，你可以查阅 [Function()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function) 构造器的参考文档以确认这一点。
* 重要：prototype 属性大概是 JavaScript 中最容易混淆的名称之一。你可能会认为，this 关键字指向当前对象的原型对象，其实不是（还记得么？原型对象是一个内部对象，应当使用 __proto__ 访问）。prototype 属性包含（指向）一个对象，你在这个对象中定义需要被继承的成员。
* 每个实例对象都从原型中继承了一个constructor属性，该属性指向了用于构造此实例对象的构造函数。一个小技巧是，你可以在 constructor 属性的末尾添加一对圆括号（括号中包含所需的参数），从而用这个构造器创建另一个对象实例。毕竟构造器是一个函数，故可以通过圆括号调用；只需在前面添加 new 关键字，便能将此函数作为构造器使用。例如：`var person3 = new person1.constructor('Karen', 'Stephenson', 26, 'female', ['playing drums', 'mountain climbing']);` 正常工作。通常你不会去用这种方法创建新的实例；但如果你刚好因为某些原因没有原始构造器的引用，那么这种方法就很有用了。
* 我们的代码中定义了构造器，然后用这个构造器创建了一个对象实例，此后向构造器的 prototype 添加了一个新的方法。这很有用。但更关键的是，整条继承链动态地更新了，任何由此构造器创建的对象实例都自动获得了这个方法。但是 farewell() 方法仍然可用于 person1 对象实例——旧有对象实例的可用功能被自动更新了。这证明了先前描述的原型链模型。这种继承模型下，上游对象的方法不会复制到下游的对象实例中；下游对象本身虽然没有定义这些方法，但浏览器会通过上溯原型链、从上游对象中找到它们。这种继承模型提供了一个强大而可扩展的功能系统。
* 译者注：关于 this 关键字指代（引用）什么范围/哪个对象，这个问题超出了本文讨论范围。事实上，这个问题有点复杂，如果现在你没能理解，也不用担心。
* 事实上，一种极其常见的对象定义模式是，在构造器（函数体）中定义属性、在 prototype 属性上定义方法。如此，构造器只包含属性定义，而方法则分装在不同的代码块，代码更具可读性。如下:
  ```text
  // 构造器及其属性定义
  
  function Test(a,b,c,d) {
    // 属性定义
  };
  
  // 定义第一个方法
  
  Test.prototype.x = function () { ... }
  
  // 定义第二个方法
  
  Test.prototype.y = function () { ... }
  
  // 等等……
  ```
* 正如前面课程所提到的，有些人认为JavaScript并不是真正的面向对象语言，在经典的面向对象语言中，您可能倾向于定义类对象,然后您可以简单地定义哪些类继承哪些类（参考 [C++ inheritance](https://www.tutorialspoint.com/cplusplus/cpp_inheritance.htm) 里的一些简单的例子），JavaScript使用了另一套实现方式，继承的对象函数并不是通过复制而来，而是通过原型链继承（通常被称为 原型式继承 —— prototypal inheritance）。
* 注：在这个例子里我们在创建一个新的对象实例时同时指派了继承的所有属性，但是注意您需要在构造器里将它们作为参数来指派，即使实例不要求它们被作为参数指派（比如也许您在创建对象的时候已经得到了一个设置为任意值的属性）
* ECMAScript 2015 introduces [class syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) to JavaScript as a way to write reusable classes using easier, cleaner syntax, which is more similar to classes in C++ or Java. In this section we'll convert the Person and Teacher examples from prototypal inheritance to classes, to show you how it's done. Note: This modern way of writing classes is supported in all modern browsers, but it is still worth knowing about the underlying prototypal inheritance in case you work on a project that requires supporting a browser that doesn't support this syntax (most notably Internet Explorer).
* 何时在 JavaScript 中使用继承？
  * 特别是在读完这段文章内容之后，您也许会想 "天啊，这实在是太复杂了". 是的，您是对的，原型和继承代表了JavaScript这门语言里最复杂的一些方面，但是JavaScript的强大和灵活性正是来自于它的对象体系和继承方式，这很值得花时间去好好理解下它是如何工作的。 
  * 在某种程度上来说，您一直都在使用继承 - 无论您是使用WebAPI的不同特性还是调用字符串、数组等浏览器内置对象的方法和属性的时候，您都在隐式地使用继承。 
  * 就在自己代码中使用继承而言，您可能不会使用的非常频繁，特别是在小型项目中或者刚开始学习时 - 因为当您不需要对象和继承的时候，仅仅为了使用而使用它们只是在浪费时间而已。但是随着您的代码量的增大，您会越来越发现它的必要性。如果您开始创建一系列拥有相似特性的对象时，那么创建一个包含所有共有功能的通用对象，然后在更特殊的对象类型中继承这些特性，将会变得更加方便有用。 
  * 注: 考虑到JavaScript的工作方式，由于原型链等特性的存在，在不同对象之间功能的共享通常被叫做 委托 - 特殊的对象将功能委托给通用的对象类型完成。这也许比将其称之为继承更为贴切，因为“被继承”了的功能并没有被拷贝到正在“进行继承”的对象中，相反它仍存在于通用的对象中。 
  * 在使用继承时，建议您不要使用过多层次的继承，并仔细追踪定义方法和属性的位置。很有可能您的代码会临时修改了浏览器内置对象的原型，但您不应该这么做，除非您有足够充分的理由。过多的继承会在调试代码时给您带来无尽的混乱和痛苦。 
  * 总之，对象是另一种形式的代码重用，就像函数和循环一样，有他们特定的角色和优点。如果您发现自己创建了一堆相关的变量和函数，还想一起追踪它们并将其灵活打包的话，对象是个不错的主意。对象在您打算把一个数据集合从一个地方传递到另一个地方的时候非常有用。这些都可以在不使用构造器和继承的情况下完成。如果您只是需要一个单一的对象实例，也许使用对象常量会好些，您当然不需要使用继承。
* 但是有时候我们没有那么幸运，我们接收到一些 字符串作为 JSON 数据，然后我们想要将它转换为对象。当我们想要发送 JSON 数据作为信息，我们将需要转换它为字符串，我们经常需要正确的转换数据，幸运的是，这两个问题在web环境中是那么普遍以至于浏览器拥有一个内建的 JSON，包含以下两个方法。
  * parse(): 以文本字符串形式接受JSON对象作为参数，并返回相应的对象。
  * stringify(): 接收一个对象作为参数，返回一个对应的JSON字符串。
* JavaScript 传统上是单线程的。即使有多个内核，也只能在单一线程上运行多个任务，此线程称为主线程（main thread）。经过一段时间，JavaScript获得了一些工具来帮助解决这种问题。通过 [Web workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API) 可以把一些任务交给一个名为worker的单独的线程，这样就可以同时运行多个JavaScript代码块。一般来说，用一个worker来运行一个耗时的任务，主线程就可以处理用户的交互（避免了阻塞）.
* web workers相当有用，但是他们确实也有局限。主要的一个问题是他们不能访问 [DOM](https://developer.mozilla.org/zh-CN/docs/Glossary/DOM) — 不能让一个worker直接更新UI。我们不能在worker里面渲染1百万个蓝色圆圈，它基本上只能做算数的苦活。为了解决这些问题，浏览器允许我们异步运行某些操作。像 [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 这样的功能就允许让一些操作运行 (比如：从服务器上获取图片)，然后等待直到结果返回，再运行其他的操作。由于操作发生在其他地方，因此在处理异步操作的时候，主线程不会被阻塞。
* Note: 这很重要请记住，[alert()](https://developer.mozilla.org/en-US/docs/Web/API/Window/alert) 在演示阻塞效果的时候非常有用，但是在正式代码里面，它就是一个噩梦。
* 在JavaScript代码中，你经常会遇到两种异步编程风格：老派 callbacks，新派 promise。
* 异步callbacks 其实就是函数，只不过是作为参数传递给那些在后台执行的其他函数. 当那些后台运行的代码结束，就调用callbacks函数，通知你工作已经完成，或者其他有趣的事情发生了。使用callbacks 有一点老套，在一些老派但经常使用的API里面，你会经常看到这种风格。请注意，不是所有的回调函数都是异步的 — 有一些是同步的。一个例子就是使用 [Array.prototype.forEach()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) 来遍历数组。
* promise 是表示异步操作完成或失败的对象。可以说，它代表了一种中间状态。 本质上，这是浏览器说“我保证尽快给您答复”的方式，因此得名“promise”。 这个概念需要练习来适应;它感觉有点像运行中的 [薛定谔的猫](https://zh.wikipedia.org/wiki/%E8%96%9B%E5%AE%9A%E8%B0%94%E7%8C%AB).这两种可能的结果都还没有发生，因此fetch操作目前正在等待浏览器试图在将来某个时候完成该操作的结果。
* promises与旧式callbacks有一些相似之处。它们本质上是一个返回的对象，您可以将回调函数附加到该对象上，而不必将回调作为参数传递给另一个函数。 然而，Promise是专门为异步操作而设计的，与旧式回调相比具有许多优点:
  * 您可以使用多个then()操作将多个异步操作链接在一起，并将其中一个操作的结果作为输入传递给下一个操作。这种链接方式对回调来说要难得多，会使回调以混乱的“末日金字塔”告终。 (也称为 [回调地狱](http://callbackhell.com/) )。 
  * Promise总是严格按照它们放置在事件队列中的顺序调用。 
  * 错误处理要好得多——所有的错误都由块末尾的一个.catch()块处理，而不是在“金字塔”的每一层单独处理。
* 在最基本的形式中，JavaScript是一种同步的、阻塞的、单线程的语言，在这种语言中，一次只能执行一个操作。但web浏览器定义了函数和API，允许我们当某些事件发生时不是按照同步方式，而是异步地调用函数(比如，时间的推移，用户通过鼠标的交互，或者获取网络数据)。这意味着您的代码可以同时做几件事情，而不需要停止或阻塞主线程。 异步还是同步执行代码，取决于我们要做什么。 有些时候，我们希望事情能够立即加载并发生。例如，当将一些用户定义的样式应用到一个页面时，您希望这些样式能够尽快被应用。 但是，如果我们正在运行一个需要时间的操作，比如查询数据库并使用结果填充模板，那么最好将该操作从主线程中移开使用异步完成任务。随着时间的推移，您将了解何时选择异步技术比选择同步技术更有意义。
* Note: 指定的时间（或延迟）不能保证在指定的确切时间之后执行，而是最短的延迟执行时间。在主线程上的堆栈为空之前，传递给这些函数的回调将无法运行。 结果，像 setTimeout(fn, 0) 这样的代码将在堆栈为空时立即执行，而不是立即执行。如果执行类似 setTimeout(fn, 0) 之类的代码，之后立即运行从 1 到 100亿 的循环之后，回调将在几秒后执行。 
* 我们希望传递给setTimeout()中运行的函数的任何参数，都必须作为列表末尾的附加参数传递给它。 例如，我们可以重构之前的函数，这样无论传递给它的人的名字是什么，它都会向它打招呼：`function sayHi(who) { alert('Hello ' + who + '!'); }`, 人名可以通过第三个参数传进 setTimeout(): `let myGreeting = setTimeout(sayHi, 2000, 'Mr. Universe');`.
* 最后，如果创建了 timeout，您可以通过调用clearTimeout()，将setTimeout()调用的标识符作为参数传递给它，从而在超时运行之前取消。要取消上面的超时，你需要这样做：`clearTimeout(myGreeting);`.
* 这就是setInterval()的作用所在。这与setTimeout()的工作方式非常相似，只是作为第一个参数传递给它的函数，重复执行的时间不少于第二个参数给出的毫秒数，而不是一次执行。您还可以将正在执行的函数所需的任何参数作为 setInterval() 调用的后续参数传递。setInterval()永远保持运行任务,除非我们做点什么——我们可能会想阻止这样的任务,否则当浏览器无法完成任何进一步的任务时我们可能得到错误, 或者动画处理已经完成了。我们可以用与停止超时相同的方法来实现这一点——通过将setInterval()调用返回的标识符传递给clearInterval()函数.
* 还有另一种方法可以使用setTimeout()：我们可以递归调用它来重复运行相同的代码，而不是使用setInterval()。
  ```text
  // recursive setTimeout()
  let i = 1;

  setTimeout(function run() {
    console.log(i);
    i++;
    setTimeout(run, 100);
  }, 100);
  
  // setInterval()
  let i = 1;

  setInterval(function run() {
    console.log(i);
    i++
  }, 100);
  ```
* 递归setTimeout()和setInterval()有何不同？ 上述代码的两个版本之间的差异是微妙的。
  * 递归 setTimeout() 保证执行之间的延迟相同，例如在上述情况下为100ms。 代码将运行，然后在它再次运行之前等待100ms，因此无论代码运行多长时间，间隔都是相同的。
  * 使用 setInterval() 的示例有些不同。 我们选择的间隔包括执行我们想要运行的代码所花费的时间。假设代码需要40毫秒才能运行 - 然后间隔最终只有60毫秒。
  * 当递归使用 setTimeout() 时，每次迭代都可以在运行下一次迭代之前计算不同的延迟。 换句话说，第二个参数的值可以指定在再次运行代码之前等待的不同时间（以毫秒为单位）。
  * **当你的代码有可能比你分配的时间间隔，花费更长时间运行时，最好使用递归的 setTimeout() - 这将使执行之间的时间间隔保持不变，无论代码执行多长时间，你不会得到错误。**
* [requestAnimationFrame()](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) 是一个专门的循环函数，旨在浏览器中高效运行动画。它基本上是现代版本的setInterval() —— 它在浏览器重新加载显示内容之前执行指定的代码块，从而允许动画以适当的帧速率运行，不管其运行的环境如何。 它是针对setInterval() 遇到的问题创建的，比如 setInterval()并不是针对设备优化的帧率运行，有时会丢帧。还有即使该选项卡不是活动的选项卡或动画滚出页面等问题。
* 注意: 如果要执行某种简单的常规DOM动画, [CSS 动画](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations) 可能更快，因为它们是由浏览器的内部代码计算而不是JavaScript直接计算的。但是，如果您正在做一些更复杂的事情，并且涉及到在DOM中不能直接访问的对象(such as [2D Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) or [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API) objects), requestAnimationFrame() 在大多数情况下是更好的选择。
* 如前所述，我们没有为requestAnimationFrame();指定时间间隔；它只是在当前条件下尽可能快速平稳地运行它。如果动画由于某些原因而处于屏幕外浏览器也不会浪费时间运行它。另一方面setInterval()需要指定间隔。我们通过公式1000毫秒/60Hz得出17的最终值，然后将其四舍五入。四舍五入是一个好主意，浏览器可能会尝试运行动画的速度超过60fps，它不会对动画的平滑度有任何影响。如前所述，60Hz是标准刷新率。传递给 requestAnimationFrame() 函数的实际回调也可以被赋予一个参数（一个时间戳值），表示自 requestAnimationFrame() 开始运行以来的时间。这是很有用的，因为它允许您在特定的时间以恒定的速度运行，而不管您的设备有多快或多慢。
* 使用箭头函数，你可以进一步简化代码：
```text
chooseToppings()
.then(toppings =>
  placeOrder(toppings)
)
.then(order =>
  collectOrder(order)
)
.then(pizza =>
  eatPizza(pizza)
)
.catch(failureCallback);
```
* 甚至这样：
```text
chooseToppings()
.then(toppings => placeOrder(toppings))
.then(order => collectOrder(order))
.then(pizza => eatPizza(pizza))
.catch(failureCallback);
```
* 这是有效的，因为使用箭头函数 () => x 是 ()=> {return x;}  的有效简写; 。
* 你甚至可以这样做，因为函数只是直接传递它们的参数，所以不需要额外的函数层：`chooseToppings().then(placeOrder).then(collectOrder).then(eatPizza).catch(failureCallback);`
* 最基本的，promise与事件监听器类似，但有一些差异：
  * 一个promise只能成功或失败一次。它不能成功或失败两次，并且一旦操作完成，它就无法从成功切换到失败，反之亦然。
  * 如果promise成功或失败并且你稍后添加成功/失败回调，则将调用正确的回调，即使事件发生在较早的时间。
* Promises 很重要，因为大多数现代Web API都将它们用于执行潜在冗长任务的函数。要使用现代Web技术，你需要使用promises。
* 注意: .then()块的工作方式类似于使用AddEventListener()向对象添加事件侦听器时的方式。它不会在事件发生之前运行（当promise履行时）。最显着的区别是.then()每次使用时只运行一次，而事件监听器可以多次调用。
* 如果其中一个promise失败（rejects，in promise-speak），目前没有什么可以明确地处理错误。我们可以通过运行前一个promise的 .catch() 方法来添加错误处理。如果你根本不操心包括的 .catch() 块，这并没有做太多的事情，但考虑一下（指.catch()块） ––这会使我们可以完全控制错误处理方式。在真实的应用程序中，你的.catch()块可以重试获取图像，或显示默认图像，或提示用户提供不同的图像URL等等。
* 请记住，履行的promise所返回的值将成为传递给下一个 .then() 块的executor函数的参数。
* 注意: promise中的.then()/catch()块基本上是同步代码中try...catch块的异步等价物。请记住，同步try ... catch在异步代码中不起作用。
* 创建promise时，它既不是成功也不是失败状态。这个状态叫作pending（待定）。 
* 当promise返回时，称为 resolved（已解决）
  * 一个成功resolved的promise称为fullfilled（实现）。它返回一个值，可以通过将.then()块链接到promise链的末尾来访问该值。 .then()块中的执行程序函数将包含promise的返回值。 
  * 一个不成功resolved的promise被称为rejected（拒绝）了。它返回一个原因（reason），一条错误消息，说明为什么拒绝promise。可以通过将.catch()块链接到promise链的末尾来访问此原因。
* 首先，链接进程一个接一个地发生都很好，但是如果你想在一大堆Promises全部完成之后运行一些代码呢？ 你可以使用巧妙命名的Promise.all()静态方法完成此操作。这将一个promises数组作为输入参数，并返回一个新的Promise对象，只有当数组中的所有promise都满足时才会满足。它看起来像这样：`Promise.all([a, b, c]).then(values => { ... });`, 如果它们都实现，那么数组中的结果将作为参数传递给.then()块中的执行器函数。如果传递给Promise.all()的任何一个 promise 拒绝，整个块将拒绝。
* 在promise完成后，你可能希望运行最后一段代码，无论它是否已实现（fullfilled）或被拒绝（rejected）。在现代浏览器中，[.finally()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally) 方法可用，它可以链接到常规promise链的末尾，允许你减少代码重复并更优雅地执行操作。
* 注意:finally()允许你在异步代码中编写异步等价物try/ catch / finally。
* 可以使用 [Promise()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 构造函数构建自己的promise。当你需要使用现有的旧项目代码、库或框架以及基于现代promise的代码时，这会派上用场。比如，当你遇到没有使用promise的旧式异步API的代码时，你可以用promise来重构这段异步代码。
* 当我们不知道函数的返回值或返回需要多长时间时，Promises是构建异步应用程序的好方法。它们使得在没有深度嵌套回调的情况下更容易表达和推理异步操作序列，并且它们支持类似于同步try ... catch语句的错误处理方式。 Promise适用于所有现代浏览器的最新版本;promise有兼容问题的唯一情况是Opera Mini和IE11及更早版本。 本文中，我们没有涉及的所有promise的功能，只是最有趣和最有用的功能。当你开始了解有关promise的更多信息时，你会遇到更多功能和技巧。 大多数现代Web API都是基于promise的，因此你需要了解promise才能充分利用它们。这些API包括 [WebRTC](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API), [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API), [Media Capture and Streams](https://developer.mozilla.org/en-US/docs/Web/API/Media_Streams_API) 等等。随着时间的推移，Promises将变得越来越重要，因此学习使用和理解它们是学习现代JavaScript的重要一步。
* [async functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) 和 [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) 关键字是最近添加到JavaScript语言里面的。它们是ECMAScript 2017 JavaScript版的一部分。简单来说，它们是基于promises的语法糖，使异步代码更易于编写和阅读。通过使用它们，异步代码看起来更像是老式同步代码，因此它们非常值得学习。
* 将 async 关键字加到函数申明中，可以告诉它们返回的是 promise，而不是直接返回值。此外，它避免了同步函数为支持使用 await 带来的任何潜在开销。在函数声明为 async 时，JavaScript引擎会添加必要的处理，以优化你的程序。爽！
* 当 [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) 关键字与异步函数一起使用时，它的真正优势就变得明显了 —— 事实上， await 只在异步函数里面才起作用。它可以放在任何异步的，基于 promise 的函数之前。它会暂停代码在该行上，直到 promise 完成，然后返回结果值。在暂停的同时，其他正在等待执行的代码就有机会执行了。 您可以在调用任何返回Promise的函数时使用 await，包括Web API函数。
* 使用 async/await 重写 promise 代码：
  ```text
  // 之前
  fetch('coffee.jpg')
  .then(response => response.blob())
  .then(myBlob => {
    let objectURL = URL.createObjectURL(myBlob);
    let image = document.createElement('img');
    image.src = objectURL;
    document.body.appendChild(image);
  })
  .catch(e => {
    console.log('There has been a problem with your fetch operation: ' + e.message);
  });
  
  // 之后
  async function myFetch() {
  let response = await fetch('coffee.jpg');
  let myBlob = await response.blob();

  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
  }
  
  myFetch()
  .catch(e => {
    console.log('There has been a problem with your fetch operation: ' + e.message);
  });
  ```
* 它使代码简单多了，更容易理解 —— 去除了到处都是 .then() 代码块！ 由于 async 关键字将函数转换为 promise，您可以重构以上代码 —— 使用 promise 和 await 的混合方式，将函数的后半部分抽取到新代码块中。这样做可以更灵活：
  ```text
  async function myFetch() {
    let response = await fetch('coffee.jpg');
    return await response.blob();
  }
  
  myFetch().then((blob) => {
    let objectURL = URL.createObjectURL(blob);
    let image = document.createElement('img');
    image.src = objectURL;
    document.body.appendChild(image);
  });
  ```
* 如果你想添加错误处理，你有几个选择。您可以将同步的 [try...catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) 结构和 async/await 一起使用 。如果你想使用我们上面展示的第二个（重构）代码版本，你最好继续混合方式并将 .catch() 块链接到 .then() 调用的末尾。
* async / await 建立在 [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 之上，因此它与promises提供的所有功能兼容。这包括[Promise.all()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) –– 你完全可以通过调用 await Promise.all() 将所有结果返回到变量中，就像同步代码一样。在这里，通过使用await，我们能够在三个promise的结果都可用的时候，放入values数组中。这看起来非常像同步代码。我们需要将所有代码封装在一个新的异步函数displayContent() 中，尽管没有减少很多代码，但能够将大部分代码从 .then() 代码块移出，使代码得到了简化，更易读。 为了错误处理，我们在 displayContent() 调用中包含了一个 .catch() 代码块;这将处理两个函数中出现的错误。注意: 也可以在异步函数中使用同步 [finally](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/try...catch#finally%E5%9D%97) 代码块代替 [.finally()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally) 异步代码块，以显示操作如何进行的最终报告。
* Async/await 让你的代码看起来是同步的，在某种程度上，也使得它的行为更加地同步。 await 关键字会阻塞其后的代码，直到promise完成，就像执行同步操作一样。它确实可以允许其他任务在此期间继续运行，但您自己的代码被阻塞。 这意味着您的代码可能会因为大量await的promises相继发生而变慢。每个await都会等待前一个完成，而你实际想要的是所有的这些promises同时开始处理（就像我们没有使用async/await时那样）。 有一种模式可以缓解这个问题——通过将 Promise 对象存储在变量中来同时开始它们，然后等待它们全部执行完毕。
* 决定是否使用 async/await 时的一个考虑因素是支持旧浏览器。它们适用于大多数浏览器的现代版本，与promise相同; 主要的支持问题存在于Internet Explorer和Opera Mini。 如果你想使用async/await但是担心旧的浏览器支持，你可以考虑使用 [BabelJS](https://babeljs.io/) 库 —— 这允许你使用最新的JavaScript编写应用程序，让Babel找出用户浏览器需要的更改。在遇到不支持async/await 的浏览器时，Babel的 polyfill 可以自动提供适用于旧版浏览器的实现。
* async/await提供了一种很好的，简化的方法来编写更易于阅读和维护的异步代码。即使浏览器支持在撰写本文时比其他异步代码机制更受限制，但无论是现在还是将来，都值得学习和考虑使用。
* 尽管有局限性，Web API仍然允许我们访问许多的功能，使我们用web页做很多事情。有几个在代码中经常引用的非常明显的部位：
  * window是载入浏览器的标签，在JavaScript中用 [Window](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 对象来表示，使用这个对象的可用方法，你可以返回窗口的大小（参见 [Window.innerWidth](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/innerWidth) 和 [Window.innerHeight](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/innerHeight) ），操作载入窗口的文档，存储客户端上文档的特殊数据（例如使用本地数据库或其他存储设备），为当前窗口绑定 [event handler](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#a_series_of_fortunate_events), 等等。 
  * navigator表示浏览器存在于web上的状态和标识（即用户代理）。在JavaScript中，用 [Navigator](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator) 来表示。你可以用这个对象获取一些信息，比如来自用户摄像头的地理信息、用户偏爱的语言、多媒体流等等。 
  * document（在浏览器中用DOM表示）是载入窗口的实际页面，在JavaScript中用 [Document](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 对象表示，你可以用这个对象来返回和操作文档中HTML和CSS上的信息。例如获取DOM中一个元素的引用，修改其文本内容，并应用新的样式，创建新的元素并添加为当前元素的子元素，甚至把他们一起删除。
* 注意，和JavaScript中的许多事情一样，有很多方法可以选择一个元素，并在一个变量中存储一个引用。 [Document.querySelector()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelector) 是推荐的主流方法，它允许你使用CSS选择器选择元素，使用很方便。对于获取元素引用，还有一些更旧的方法，如：[Document.getElementById()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementById), 选择一个id属性值已知的元素；[Document.getElementsByTagName()](https://developer.mozilla.org/zh-CN/docs/Wehttps://developer.mozilla.org/zh-CN/docs/Web/APIb/API/Document/getElementsByTagName), 返回页面中包含的所有已知类型元素的数组。
* 注意: CSS样式的JavaSript属性版本以小驼峰式命名法书写，而CSS版本带连接符号（backgroundColor 对 background-color）。确保你不会混淆，否则就不能工作。
* 使用JavaScript创建静态内容是毫无意义的 — 最好将其写入HTML，而不使用JavaScript。用JavaScript创建内容也有其他问题（如不能被搜索引擎读取），比HTML复杂得多。
* 注意：在早期，这种通用技术被称为Asynchronous JavaScript and XML（Ajax）， 因为它倾向于使用 [XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 来请求XML数据。 但通常不是这种情况 (你更有可能使用 XMLHttpRequest 或 Fetch 来请求JSON), 但结果仍然是一样的，术语“Ajax”仍然常用于描述这种技术。
* XMLHttpRequest （通常缩写为XHR）现在是一个相当古老的技术 - 它是在20世纪90年代后期由微软发明的，并且已经在相当长的时间内跨浏览器进行了标准化。
* Fetch API基本上是XHR的一个现代替代品——它是最近在浏览器中引入的，它使异步HTTP请求在JavaScript中更容易实现，对于开发人员和在Fetch之上构建的其他API来说都是如此。
* 关于promises: 当你第一次见到它们的时候，promises会让你有点困惑，但现在不要太担心这个。在一段时间之后，您将会习惯它们，特别是当您了解更多关于现代JavaScript api的时候——大多数现代的JavaScript api都是基于promises的。
* 值得注意的是你可以直接将promise块 (.then()块, 但也有其他类型) 链接到另一个的尾部, 顺着链条将每个块的结果传到下一个块。 这使得promises非常强大。很多开发者更喜欢这种样式, 因为它更扁平并且按理说对于更长的promise链它更容易读 — 每一个promise(承诺）接续上一个promise，而不是在上一个promise的里面(会使得整个代码笨重起来，难以理解）。以上两种写法还有一个不同的地方是我们在response.text()  语句之前得包含一个 return  语句, 用来把这一部分的结果传向promise链的下一段。
* Note: 一些api处理对其功能的访问略有不同，相反，它们要求开发人员向特定的URL模式发出HTTP请求(参见从服务器获取数据)，以检索特定的数据。这些被称为 RESTful API.
* 当你能够使用 CSS 和 JavaScript 让 SVG 矢量图动起来时，位图却依然没有相应的支持。同时 SVG 动画的可用工具也少得可怜。有效地生成动画、游戏画面、3D场景和其他的需求依然没有满足，而这些在诸如 C++ 或者 Java 等底层语言中却司空见惯。 当浏览器开始支持 HTML 画布元素 [\<canvas\>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/canvas) 和相关的 [Canvas API](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API) （由苹果公司在 2004 年前后发明，后来其他的浏览器开始跟进）时，形势开始改善。下面你会看到，canvas 提供了许多有用的工具，特别是当捆绑了由网络平台提供的一些其他的 API 时。它们用来生成 2D 动画、游戏画面和数据分析图，以及其他类型的 app。大约在 2006 - 2007 年，Mozilla 开始测试 3D 画布。后来演化为 [WebGL](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL_API), 它获得了各大浏览器厂商的认可，于是大约在 2009 - 2010 年间得到了标准化。WebGL 可以让你在 web 浏览器中生成真正的 3D 图形。下面的例子显示了一个简单的旋转的 WebGL 立方体。
* WebGL 基于 [OpenGL](https://developer.mozilla.org/en-US/docs/Glossary/OpenGL) 图形编程语言实现，可直接与 [GPU](https://developer.mozilla.org/en-US/docs/Glossary/GPU) 通信，基于此，编写纯 WebGL 代码与常规的 JavaScript 不尽相同，更像 C++ 那样的底层语言，更加复杂，但无比强大。
* 作为HTML5规范的一部分，[HTMLMediaElement](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement) API提供允许你以编程方式来控制视频和音频播放的功能—例如 [HTMLMediaElement.play()](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement/play), [HTMLMediaElement.pause()](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement/pause), 等。该接口对 [\<audio\>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio) 和 [\<video\>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video) 两个元素都是可用的，因为在这两个元素中要实现的功能几乎是相同的。
* 注意：使用客户端存储 API 可以存储的数据量是有限的（可能是每个API单独的和累积的总量）;具体的数量限制取决于浏览器，也可能基于用户设置。有关更多信息，获取更多信息，请参考 [浏览器存储限制和清理标准](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API/Browser_storage_limits_and_eviction_criteria).
* cookie的唯一优势是它们得到了非常旧的浏览器的支持，所以如果您的项目需要支持已经过时的浏览器（比如 Internet Explorer 8 或更早的浏览器），cookie可能仍然有用，但是对于大多数项目（很明显不包括本站）来说，您不需要再使用它们了。其实cookie也没什么好说的，[document.cookie](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie) 一把梭就完事了。
* 现代浏览器有比使用 cookies 更简单、更有效的存储客户端数据的 API。
  * [Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) 提供了一种非常简单的语法，用于存储和检索较小的、由名称和相应值组成的数据项。当您只需要存储一些简单的数据时，比如用户的名字，用户是否登录，屏幕背景使用了什么颜色等等，这是非常有用的。
  * [IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) 为浏览器提供了一个完整的数据库系统来存储复杂的数据。这可以用于存储从完整的用户记录到甚至是复杂的数据类型，如音频或视频文件。
* 未来：Cache API, 一些现代浏览器支持新的 [Cache API](https://developer.mozilla.org/zh-CN/docs/Web/API/Cache). 这个API是为存储特定HTTP请求的响应文件而设计的，它对于像存储离线网站文件这样的事情非常有用，这样网站就可以在没有网络连接的情况下使用。缓存通常与 [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) 组合使用，尽管不一定非要这么做。 Cache 和 Service Workers 的使用是一个高级主题。
* [Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) 非常容易使用 — 你只需存储简单的 键名/键值 对数据 (限制为字符串、数字等类型) 并在需要的时候检索其值。
* 你所有的 web storage 数据都包含在浏览器内两个类似于对象的结构中： [sessionStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage) 和 [localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage). 第一种方法，只要浏览器开着，数据就会一直保存 (关闭浏览器时数据会丢失) ，而第二种会一直保存数据，甚至到浏览器关闭又开启后也是这样。我们将在本文中使用第二种方法，因为它通常更有用。
* 为每个域名分离储存：每个域都有一个单独的数据存储区(每个单独的网址都在浏览器中加载). 你 会看到，如果你加载两个网站（例如google.com和amazon.com）并尝试将某个项目存储在一个网站上，该数据项将无法从另一个网站获取。 这是有道理的 - 你可以想象如果网站能够查看彼此的数据，就会出现安全问题！
* [IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) （有时简称 IDB ）是可以在浏览器中访问的一个完整的数据库系统，在这里，你可以存储复杂的关系数据。其种类不限于像字符串和数字这样的简单值。你可以在一个IndexedDB中存储视频，图像和许多其他的内容。 但是，这确实是有代价的：使用IndexedDB要比Web Storage API复杂得多。
* JavaScript frameworks were created to make this kind of work a little easier — they exist to provide a better developer experience. They don't bring brand-new powers to JavaScript; they give you easier access to JavaScript's powers so you can build for today's web.
* the advantages of frameworks are achievable in vanilla JavaScript, but using a framework takes away all of the cognitive load of having to solve these problems yourself.
* 框架提供给我们的其他功能: [Tooling](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Introduction#tooling), [Compartmentalization](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Introduction#compartmentalization), [Routing](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Introduction#routing).
* Modern web applications typically do not fetch and render new HTML files — they load a single HTML shell, and continually update the DOM inside it (referred to as single page apps, or SPAs) without navigating users to new addresses on the web. Each new pseudo-webpage is usually called a view, and by default, no routing is done.
* 使用框架的注意事项: [Familiarity with the tool](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Introduction#familiarity_with_the_tool), [Overengineering](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Introduction#overengineering), [Larger code base and abstraction](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Introduction#larger_code_base_and_abstraction).
* [Larger code base and abstraction](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Introduction#larger_code_base_and_abstraction)
  * Frameworks allow you to write more declarative code – and sometimes less code overall – by dealing with the DOM interactions for you, behind the scenes. This abstraction is great for your experience as a developer, but it isn't free. In order to translate what you write into DOM changes, frameworks have to run their own code, which in turn makes your final piece of software larger and more computationally expensive to operate.
  * Some extra code is inevitable, and a framework that supports tree-shaking (removal of any code that isn't actually used in the app during the build process) will allow you to keep your applications small, but this is still a factor you need to keep in mind when considering your app's performance, especially on more network/storage-constrained devices, like mobile phones.
  * The abstraction of frameworks affects not only your JavaScript, but your relationship with the very nature of the web. No matter how you build for the web, the end result, the layer that your users ultimately interact with, is HTML. Writing your whole application in JavaScript can make you lose sight of HTML and the purpose of its various tags, and lead you to produce an HTML document that is un-semantic and inaccessible. In fact, it's possible to write a fragile application that depends entirely on JavaScript and will not function without it.
  * Frameworks are not the source of our problems. With the wrong priorities, it's possible for any application to be fragile, bloated, and inaccessible. Frameworks do, however, amplify our priorities as developers. If your priority is to make a complex web app, it's easy to do that. However, if your priorities don't carefully guard performance and accessibility, frameworks will amplify your fragility, your bloat, and your inaccessibility. Modern developer priorities, amplified by frameworks, have inverted the structure of the web in many places. Instead of a robust, content-first network of documents, the web now often puts JavaScript first and user experience last.
* 如何选择一个框架:
  * What browsers does the framework support?
  * What domain-specific languages does the framework utilize?
  * Does the framework have a strong community and good docs (and other support) available?
* Does the framework have a strong community? This is perhaps the hardest metric to measure, because community size does not correlate directly to easy-to-access numbers. You can check a project's number of GitHub stars or weekly npm downloads to get an idea of its popularity, but sometimes the best thing to do is search a few forums or talk to other developers. It is not just about the community's size, but also how welcoming and inclusive it is, and how good available documentation is.
* The Wikimedia Foundation recently chose to use Vue for its front-end, and posted a [request for comments (RFC) on framework adoption](https://phabricator.wikimedia.org/T241180).
* 一个更简洁的框架——Vue:
  * Vue是一个现代JavaScript框架提供了有用的设施渐进增强——不像许多其他框架,您可以使用Vue增强现有的HTML。这使您可以使用Vue作为 [jQuery](https://developer.mozilla.org/zh-CN/docs/Glossary/jQuery) 等库的临时替代品。
  * 也就是说,您还可以使用Vue编写整个单页应用程序(SPAs)。这允许您创建标记完全由Vue管理,可以提高开发人员的经验和性能在处理复杂的应用程序。当你需要的时候它还允许您利用其他库对客户端路由和状态进行管理。此外,Vue需要“中间地带”的方法工具客户端路由和状态管理。虽然Vue核心团队维护了建议的函数库，但他们并没有直接捆绑到 Vue 里。这样你就可以选择一个其他路由/状态管理库，来更好地适应您的应用程序。
  * 除了允许您逐步将Vue集成到您的应用程序中,Vue还提供了一种渐进的方式编写标记。像大多数框架,Vue通过组件允许您创建可重用块标记。大多数时候,Vue组件是使用一个特殊的HTML模板的语法写的。当您需要比HTML语法允许的更多的控制时，您可以编写JSX或纯JavaScript函数来定义组件。
* 
































































































































