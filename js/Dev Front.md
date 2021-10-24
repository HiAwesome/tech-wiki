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
* 配置工具: 配置系统允许您发布网站，可用于静态和动态站点，通常与测试系统一起工作。例如，典型的工具链会等待您将更改推送到远程回购，运行一些测试以查看更改是否正确，然后如果测试通过，则自动将您的应用程序部署到生产站点。 [Netlify](https://netlify.com/) 是目前最流行的部署工具之一，[但其他包括Vercel](https://vercel.com/) 和 [Github Pages](https://pages.github.com/).
* 其他的: 在开发后期阶段，还有许多其他可用的工具类型，包括 [Code Climate](https://codeclimate.com/) 对于收集代码质量度量， [webhint browser extension](https://webhint.io/docs/user-guide/extensions/extension-browser/) 用于执行跨浏览器兼容性的运行时分析和其他检查, [Github bots](https://probot.github.io/) 提供更强大的GitHub功能, [Updown](https://updown.io/) 提供应用程序运行时间监控等等。
* 工具种类的想法
  在开发生命周期中应用不同的工具类型当然是有顺序的，但请放心，您不必在发布一个网站时将所有这些工具都准备就绪。事实上，你不需要这些。然而，在您的过程中包括这些工具将改善您自己的开发体验，并可能提高代码的总体质量。 新的开发人员工具通常需要一段时间才能适应其复杂性。**Webpack是最著名的工具之一，它以使用起来过于复杂而著称，但是在最新的主要版本中，它大力简化了常用的用法，因此所需的配置被减少到绝对最小。** 绝对没有银弹可以保证工具的成功，但是随着你经验的增加，你会发现适合你或者你的团队和他们的项目的工作流程。一旦过程中所有的扭结被平展，你的工具链应该是你可以忘记的东西，它应该只是工作。
* 如何选择并寻求特殊工具的帮助: 大多数工具往往是独立编写和发布的，因此，尽管几乎可以肯定有可用的帮助，但它们的位置或格式永远不会相同。因此，很难找到使用工具的帮助，甚至很难选择使用什么工具。关于哪些是最好的工具的知识是有点部落式的，这意味着如果您还没有进入web社区，就很难确定到底应该使用哪些工具!这是我们编写本系列文章的原因之一，希望能够提供其他方法难以找到的第一步。 您可能需要以下内容的组合:
  * 有经验的老师、导师、同学或有一定经验的同事以前解决过这类问题，并能提出建议。 
  * 一个有用的特定地方搜索。对前端开发人员工具的一般web搜索通常是无用的，除非您已经知道您正在搜索的工具的名称。
    * 例如，如果您正在使用npm包管理器来管理依赖项，那么转到 [npm homepage](https://www.npmjs.com/) 并搜索您正在寻找的工具的类型，例如，如果您想要日期格式化实用程序，请尝试搜索“date”，如果您想要搜索通用代码格式化程序，则尝试搜索“formatter”。请注意流行度、质量和维护分数，以及软件包最近更新的时间。还可以点击工具页面，了解每个包每月有多少下载，以及它是否有好的文档，可以用来了解它是否完成了您需要它做的事情。以这些标准为基础[date-fns library](https://www.npmjs.com/package/date-fns) 看起来是一个很好的日期格式化工具。在本模块的第3章中，您将看到这个工具的实际应用，并了解更多关于包管理器的信息。 
    * 如果您正在寻找将工具功能集成到代码编辑器中的插件，请查看代码编辑器的“插件/扩展名”页面-请参阅 [Atom packages](https://atom.io/packages) and [VSCode extensions](https://marketplace.visualstudio.com/VSCode), 为例。看一看首页上的特色扩展，然后再次尝试搜索你想要的扩展类型(或者工具名称，例如在VSCode扩展页面上搜索“eslint”)。当你得到结果的时候，看看这个扩展有多少颗星或者下载了多少，作为它质量的一个指标。
  * 例如，与开发相关的论坛，询问关于使用什么工具的问题 [MDN Learn Discourse](https://discourse.mozilla.org/c/mdn/learn), or [Stack Overflow](https://stackoverflow.com/). 
  * 当您选择要使用的工具时，第一个端口应该是工具项目主页。这可能是一个完整的网站，也可能是代码库中的一个readme文档。例如 date-fns docs就很好、很完整、很容易理解。然而，有些文档可能相当技术性和学术性，并不适合您的学习需求。
  * 相反，您可能希望找到一些关于如何开始使用特定类型的工具的专门教程。好的起点是搜索网站喜欢 [CSS Tricks](https://css-tricks.com/), [Dev](https://dev.to/), [freeCodeCamp](https://www.freecodecamp.org/), and [Smashing Magazine](https://www.smashingmagazine.com/), 因为它们是为web开发行业量身定做的。 
  * 同样，在搜索适合自己的工具时，您可能会使用几种不同的工具，试用它们，看看它们是否有意义，是否得到良好的支持，并执行您希望它们执行的任务。这很好，这对学习很有好处，而且随着你获得更多经验，道路会变得更平坦。
* **对命令行最大的一个批评是它在用户体验方面非常缺乏。第一次查看命令行可能是一种令人畏惧的体验:空白屏幕和闪烁的光标，对于要做什么几乎没有明显的帮助。** 表面上看，它们并不受欢迎，但您可以使用它们做很多事情，我们保证，通过一些指导和练习，使用它们会变得更容易!这就是为什么我们提供这一章来帮助你在这个看似不友好的环境中开始。
* 边注：命令行和终端的区别是什么？ 通常你会发现这两个术语可以互换使用。从技术上讲，终端是启动并连接到shell的软件。shell是您的会话和会话环境(提示符和快捷方式等内容可以在其中定制)。命令行是您输入命令并且光标闪烁的文字行。
* 你必须使用终端吗? 尽管命令行提供了大量的工具，但是如果您使用的工具是这样像 [Visual Studio Code](https://code.visualstudio.com/), 还有大量的扩展可以作为代理使用终端命令，而不需要直接使用终端。但是，您不会找到一个代码编辑器扩展来满足您想要做的所有事情 — 你最终将会获得一些使用终端的经验。
* 快速浏览特定终端命令的一个很好的资源是 [tldr.sh](https://tldr.sh/). 这是一个社区驱动的文档服务，类似于MDN，但特定于终端命令。
* 尝试其他的工具: 如果你想尝试更多的工具，这里有一个简短的列表，很有趣的尝试：
  * [bat](https://github.com/sharkdp/bat) — 一个更好的 cat (cat 用于打印文件内容）。
  * [prettyping](http://denilson.sa.nom.br/prettyping/) — ping在命令行上，但是是可视化的(ping是检查服务器是否有响应的有用工具)。
  * [htop](http://hisham.hm/htop/) —进程查看器，当某些东西使您的CPU风扇的行为像一个喷气发动机，并且您想要识别出错的程序时，它非常有用。
  * [tldr](https://tldr.sh/#installation) —在本章前面提到的，但是可以作为命令行工具使用。
* Note: npm is not the only package manager available. A successful and popular alternative package manager is [Yarn](https://yarnpkg.com/). Yarn resolves the dependencies using a different algorithm that can mean a faster user experience. There are also a number of other emerging clients, such as [pnpm](https://pnpm.js.org/).
* Note: If you have trouble with the terminal returning a "command not found" type error, try running the above command with the npx utility, i.e. npx parcel index.html.
* Note: The npm package manager is not required to install packages from the npm registry, even though they share the same name. pnpm and yarn can consume the same package.json format as npm, and can install any package from the npm and other package registries.
* Tools used in our toolchain, In this article we're going to use the following tools and features:
  * [JSX](https://reactjs.org/docs/introducing-jsx.html), a [React](https://reactjs.org/) related set of syntax extensions that allow you to do things like defining component structures inside JavaScript. You won't need to know React to follow this tutorial, but we've included this to give you an idea of how a non-native web language could be integrated into a toolchain. 
  * The latest built-in JavaScript features (at time of writing), such as [import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import).
  * Useful development tools such as [Prettier](https://prettier.io/) for formatting and [eslint](https://eslint.org/) for linting.
  * [PostCSS](https://postcss.org/) to provide CSS nesting capabilities.
  * [Parcel](https://parceljs.org/) to build and minify our code, and to write a bunch of configuration file content automatically for us.
  * [GitHub](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/GitHub) to manage our source code control.
  * [Netlify](https://www.netlify.com/) to automate our deployment process.
* Creating a development environment:
  * This part of the toolchain is sometimes seen to be delaying the actual work, and it can be very easy to fall into a "[rabbit hole](https://www.merriam-webster.com/dictionary/rabbit%20hole)" of tooling where you spend a lot of time trying to get the environment "just right".
  * But you can look at this in the same way as setting up your physical work environment. The chair needs to be comfortable, and set up in a good position to help with your posture. You need power, Wifi, and USB ports! There might be important decorations or music that help with your mental state — these are all important to doing your best work possible, and they should also only need to be set up once, if done properly.
  * In the same way, setting up your development environment, if done well, needs doing only once and should be reusable in many future projects. You will probably want to review this part of the toolchain semi-regularly and consider if there's any upgrades or changes you should introduce, but this shouldn't be required too often.
* Note: In the command above, I use Prettier with the --write flag. Prettier understands this to mean "if there's problems in my code format, go ahead and fix them, then save my file". This is fine for our development process, but we can also use prettier without the flag and it will only check the file. Checking the file (and not saving it) is useful for purposes like checks that run before a release - i.e. "don't release any code that's not been properly formatted."
* Note: What is a git hook? Git (not GitHub) provides a system that lets us attach pre- and post- actions to the tasks you perform with git (such as committing your code). Although git hooks can be a bit overly complicated (in this author's opinion), once they're in place they can be very powerful. If you're interested in using hooks, [Husky](https://github.com/typicode/husky) is a greatly simplified route into using hooks.
* `npm init --force`, This will create a default package.json file that we can configure later on if desired. The --force flag causes the command to instantly create a default package.json file without asking you all the usual questions about what contents you want it to have (as we saw previously). We only need the defaults for now, so this saves us a bit of time.
* `npm install --save-dev eslint prettier babel-eslint`, There's two important things to note about the command you just ran. The first is that we're installing the dependencies locally to the project — installing tools locally is better for a specific project. Installing locally (not including the --global option) allows us to easily recreate this setup on other machines. The second important part of this install command is the --save-dev option. This tells the npm tool that these particular dependencies are only needed for development (npm therefore lists them in the package.json file under devDependencies, not dependencies). This means that if this project is installed in production mode these dependencies will not be installed. A "typical" project can have many development dependencies which are not needed to actually run the code in production. Keeping them as separate dependencies reduces any unnecessary work when deploying to production (which we will look at in the next chapter).
* There’s a complete [list of eslint rules](https://eslint.org/docs/rules/) that you can tweak and configure to your heart's content and many companies and teams have published their [own eslint configurations](https://www.npmjs.com/search?q=keywords:eslintconfig), which can sometimes be useful either to get inspiration or to select one that you feel suits your own standards. A forewarning though: **eslint configuration is a very deep rabbit hole!**
* Note: Cache busting is a new term that we haven't met before in the module. This is the strategy of breaking a browser's own caching mechanism, which forces the browser to download a new copy of your code. Parcel (and indeed many other tools) will generate filenames that are unique to each new build. This unique filename "busts" your browser's cache, thereby making sure the browser downloads the fresh code each time an update is made to the deployed code.
* Note: Until October 2020 the default branch on GitHub was master, which for various social reasons was switched to main. You should be aware that this older default branch may appear in various projects you encounter, but we'd suggest using main for your own projects.
* 






























































