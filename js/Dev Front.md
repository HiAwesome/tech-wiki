# Dev Front

* 现代工具系统：从高层次来看，您可以将客户端工具放入以下三大类需要解决的问题中：
  1. 安全网络 — 在代码开发期间有用的工具。
  2. 转换 — 以某种方式转换代码的工具，例如将一种中间语言转换为浏览器可以理解的JavaScript。
  3. 开发后阶段 — 编写完代码后有用的工具，如测试和部署工具。
* Linters 是检查您的代码并告诉您存在任何错误的工具，它们是什么类型的错误，以及它们出现在哪些代码行上。通常，linters不仅可以被配置为报告错误，还可以报告任何违反您的团队可能正在使用的指定样式指南的行为(例如代码使用了错误的缩进空格数，或者使用了 [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) 而不是常规的字符串文本)。 [eslint](https://eslint.org/) 业界标准的JavaScript linter是一种高度可配置的工具，用于捕获潜在的语法错误，并在代码中鼓励“最佳实践”。一些公司和项目也是这样 [shared their eslint configs](https://www.npmjs.com/search?q=keywords:eslintconfig).
* 代码格式化程序与linters有些关联，除了它们不是指出代码中的错误，而是根据你的样式规则，确保你的代码被正确格式化，理想情况下自动修复它们发现的错误。 [Prettier](https://prettier.io/) 是一个非常流行的代码格式化程序示例，稍后我们将在模块中使用它。
* 打包工具：这些工具让你的代码准备生产,例如，通过tree-shaking来确保只有实际使用的代码库的部分被放到最终的生产代码中,或“缩减”删除所有空格在生产代码中,使其尽可能小之前上传到服务器。 [Parcel](https://parceljs.org/) 是一个特别好用的工具,都属于这个类别可以完成上面的任务,但也有助于资产包像HTML, CSS和图像文件方便的包,你可以继续部署,也为您自动添加依赖项当你试着使用它们。它甚至可以为您处理一些代码转换任务。 [Webpack](https://webpack.js.org/) 是一个非常流行的代码格式化程序示例，稍后我们将在模块中使用它。
* 转换：web应用程序生命周期的这个阶段通常允许您编写“未来代码”(比如最新的CSS或JavaScript特性，这些特性可能还没有得到浏览器的本地支持)，或者完全使用另一种语言编写代码，比如 TypeScript. 转换工具将为您生成与浏览器兼容的代码，以用于生产。 通常web开发被认为是三种语言: HTML, CSS, and JavaScript, 所有这些语言都有转换工具。转换提供了两个主要好处(还有其他好处)。 
  * 能够使用最新的语言特性编写代码，并将其转换为可在日常设备上使用的代码。例如，您可能希望使用尖端的新语言特性来编JavaScript，但是您的最终产品代码仍然可以在不支持这些特性的旧浏览器上工作。例如： 
    * [Babel](https://babeljs.io/): 一个JavaScript编译器，允许开发人员使用最前沿的JavaScript编写代码，然后Babel将其转换为老式的JavaScript，让更多的浏览器能够理解。开发人员也可以编写和发布 [plugins for Babel](https://babeljs.io/docs/en/plugins). 
    * [PostCSS](https://postcss.org/): 和Babel做同样的事情，但是有先进的CSS特性。如果没有相同的方法使用旧的CSS特性来做一些事情，PostCSS将安装一个JavaScript填充来模拟您想要的CSS效果。 
  * 选择用一种完全不同的语言编写代码，并将其转换为与web兼容的语言。例如： 
    * [Sass/SCSS](https://sass-lang.com/): 这个CSS扩展允许您使用变量、嵌套规则、混合、函数和许多其他特性，其中一些特性在本地CSS中是可用的(比如变量)，而另一些则不是。 
    * [TypeScript](https://www.typescriptlang.org/): TypeScript是JavaScript的一个超集，它提供了一堆额外的特性。TypeScript编译器在生成产品时将TypeScript代码转换为JavaScript。 
    * 框架例如 [React](https://reactjs.org/), [Ember](https://emberjs.com/), and [Vue](https://vuejs.org/): 框架提供了许多免费的功能，并允许您通过构建在普通JavaScript之上的自定义语法来使用它们。在后台，框架的JavaScript代码努力解释这个定制语法，并将其呈现为最终的web应用程序。
* 开发后阶段：开发后阶段工具可以确保您的软件能够访问web并继续运行。这包括部署流程、测试框架、审计工具等等。 在开发过程的这个阶段，您希望与之进行最少的主动交互，这样，一旦配置完毕，它基本上是自动运行的，只有在出现错误时才弹出窗口打招呼。
* 测试工具：它们通常采用一种工具的形式，该工具将自动对您的代码运行测试，以确保在进行进一步操作之前它是正确的(例如，当您试图将更改推送到GitHub repo时)。这可能包括linting，但也包括更复杂的过程，如单元测试，在这里运行部分代码，以确保它们按照应有的方式运行。 
  * 框架包括编写测试 [Jest](https://jestjs.io/), [Mocha](https://mochajs.org/), 和 [Jasmine](https://jasmine.github.io/). 
  * 自动测试运行和通知系统包括 [Travis CI](https://travis-ci.org/), [Jenkins](https://jenkins.io/), [Circle CI](https://circleci.com/), 和 [others](https://en.m.wikipedia.org/wiki/List_of_build_automation_software#Continuous_integration).
* 