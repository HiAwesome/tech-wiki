# JS

### [JavaScript 错误参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Errors)

### [JavaScript 标准内置对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

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
* 

































































































































