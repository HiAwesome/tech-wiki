# JS

### [JavaScript 错误参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Errors)

### [JS Object 对象包装器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)

### [JavaScript 标准内置对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

### [JS Class 类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)

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
* 

































































































































