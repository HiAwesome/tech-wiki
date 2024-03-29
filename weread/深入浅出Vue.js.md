# 深入浅出Vue.js

 **刘博文**


## 划线部分


### 序一

* 早期的jQuery库之所以获得开发者们的认可，很大程度上是因为它独创的链式语法和隐式迭代语义。尽管jQuery仅仅通过巧妙设计API就能支持上述特性，并不依赖于编程语言赋予的元编程能力，但是毫无疑问，它以一种精巧的设计理念和思路，为JavaScript库和框架的设计者打开了一扇创新的大门。


### 第 1 章 Vue.js简介

* 那句话是：
“The fate of destruction is also the joy of rebirth.”
翻译成中文是：
毁灭的命运，也是重生的喜悦。

* “Your effort to remain what you are is what limits you.”
翻译成中文是：
保持本色的努力，也在限制你的发展。

* Vue.js 2.0与Vue.js 1.0之间内部变化非常大，整个渲染层都重写了，但API层面的变化却很小。可以看出，Vue.js是非常注重用户体验和学习曲线的，它尽量让开发者用起来很爽，同时在应用场景上，其他框架能做到的Vue.js都能做到，不存在其他框架可以实现而Vue.js不能实现这样的问题，所以在技术选型上，只需要考虑Vue.js的使用方式是不是符合口味，团队来了新同学能否快速融入等问题。由于无论是学习曲线还是API的设计上，Vue.js都非常优雅，所以它具有很强的竞争力。


### 第一篇 变化侦测

* 变化侦测是响应式系统的核心，没有它，就没有重新渲染。框架在运行时，视图也就无法随着状态的变化而变化。


### 第 2 章 Object的变化侦测

* 关于变化侦测，首先要问一个问题，在JavaScript（简称JS）中，如何侦测一个对象的变化？
其实这个问题还是比较简单的。学过JavaScript的人都知道，有两种方法可以侦测到变化：使用Object.defineProperty和ES6的Proxy。

* 总结起来，其实就一句话，在getter中收集依赖，在setter中触发依赖。

* 前面介绍了Object类型数据的变化侦测原理，了解了数据的变化是通过getter/setter来追踪的。也正是由于这种追踪方式，有些语法中即便是数据发生了变化，Vue.js也追踪不到。

* Vue.js通过Object.defineProperty来将对象的key转换成getter/setter的形式来追踪变化，但getter/setter只能追踪一个数据是否被修改，无法追踪新增属性和删除属性，所以才会导致上面例子中提到的问题。

* 但这也是没有办法的事，因为在ES6之前，JavaScript没有提供元编程的能力，无法侦测到一个新属性被添加到了对象中，也无法侦测到一个属性从对象中删除了。为了解决这个问题，Vue.js提供了两个API——vm.$set与vm.$delete，第4章会详细介绍它们。

* 由于在ES6之前JavaScript并没有提供元编程的能力，所以在对象上新增属性和删除属性都无法被追踪到。


### 第 3 章 Array的变化侦测

* 正因为我们可以通过Array原型上的方法来改变数组的内容，所以Object那种通过getter/setter的实现方式就行不通了。

* Vue的做法非常粗暴，如果不能使用 __proto__，就直接将arrayMethods身上的这些方法设置到被侦测的数组上

* 前面介绍过，对Array的变化侦测是通过拦截原型的方式实现的。正是因为这种实现方式，其实有些数组操作Vue.js是拦截不到的

* 因为Vue.js的实现方式决定了无法对上面举的两个例子做拦截，也就没有办法响应。在ES6之前，无法做到模拟数组的原生行为，所以拦截不到也是没有办法的事情。ES6提供了元编程的能力，所以有能力拦截，我猜测未来Vue.js很有可能会使用ES6提供的Proxy来实现这部分功能，从而解决这个问题。


### 第 4 章 变化侦测相关的API实现原理

* 要想实现deep的功能，其实就是除了要触发当前这个被监听数据的收集依赖的逻辑之外，还要把当前监听的这个值在内的所有子值都触发一遍收集依赖逻辑。这就可以实现当前这个依赖的所有子数据发生变化时，通知当前Watcher了。


* ● 用法：在object上设置一个属性，如果object是响应式的，Vue.js会保证属性被创建后也是响应式的，并且触发视图更新。这个方法主要用来避开Vue.js不能侦测属性被添加的限制。

* vm.$set就是为了解决这个问题而出现的。使用它，可以为object新增属性，然后Vue.js就可以将这个新增属性转换成响应式的。

* 这里我们在Vue.js的原型上设置 $set属性。其实我们使用的所有以vm.$ 开头的方法都是在Vue.js的原型上设置的。vm.$set的具体实现其实是在observer中抛出的set方法。

* 对于ob.vmCount，我们是陌生的，后面会详细介绍，这里只要知道通过它可以判断target是不是根数据就行了。
那么，什么是根数据？this.$data就是根数据。
接下来，我们处理target不是响应式的情况。如果target身上没有 __ob__ 属性，说明它并不是响应式的，并不需要做什么特殊处理，只需要通过key和val在target上设置就行了。
如果前面的所有判断条件都不满足，那么说明用户是在响应式数据上新增了一个属性，这种情况下需要追踪这个新增属性的变化，即使用defineReactive将新增属性转换成getter/setter的形式即可。
最后，向target的依赖触发变化通知，并返回val。

* vm.$delete的作用是删除数据中的某个属性。由于Vue.js的变化侦测是使用Object.defineProperty实现的，所以如果数据是使用delete关键字删除的，那么无法发现数据发生了变化。为了解决这个问题，Vue.js提供了vm.$delete方法来删除数据中的某个属性，并且此时Vue.js可以侦测到数据发生了变化。


### 第 5 章 虚拟DOM简介

* 这其实是命令式操作DOM的问题，虽然简单易用，但是在业务越来越复杂的今天，它会有不好维护的问题。

* Vue.js的变化侦测和它们都不一样，它在一定程度上知道具体哪些状态发生了变化，这样就可以通过更细粒度的绑定来更新视图。也就是说，在Vue.js中，当状态发生变化时，它在一定程度上知道哪些节点使用了这个状态，从而对这些节点进行更新操作，根本不需要比对。事实上，在Vue.js 1.0的时候就是这样实现的。

* 可以看出，虚拟DOM在Vue.js中所做的事情其实并没有想象中那么复杂，它主要做了两件事。
● 提供与真实DOM节点所对应的虚拟节点vnode。
● 将虚拟节点vnode和旧虚拟节点oldVnode进行比对，然后更新视图。


### 第 6 章 VNode

* 以静态节点为例，当组件内的某个状态发生变化后，当前组件会通过虚拟DOM重新渲染视图，静态节点因为它的内容不会改变，所以除了首次渲染需要执行渲染函数获取vnode之外，后续更新不需要执行渲染函数重新生成vnode。因此，这时就会使用创建克隆节点的方法将vnode克隆一份，使用克隆节点进行渲染。这样就不需要重新执行渲染函数生成新的静态节点的vnode，从而提升一定程度的性能。

* VNode是一个类，可以生成不同类型的vnode实例，而不同类型的vnode表示不同类型的真实DOM元素。
由于Vue.js对组件采用了虚拟DOM来更新视图，当属性发生变化时，整个组件都要进行重新渲染的操作，但组件内并不是所有DOM节点都需要更新，所以将vnode缓存并将当前新生成的vnode和上一次缓存的oldVnode进行对比，只对需要更新的部分进行DOM操作可以提升很多性能。
vnode有多种类型，它们本质上都是从VNode类实例化出的对象，其唯一区别只是属性不同。


### 第 7 章 patch

* 之所以要这么做，主要是因为DOM操作的执行速度远不如JavaScript的运算速度快。因此，把大量的DOM操作搬运到JavaScript中，使用patching算法来计算出真正需要更新的节点，最大限度地减少DOM操作，从而显著提升性能。

* 通过前面的介绍，可以发现整个patch的过程并不复杂。当oldVnode不存在时，直接使用vnode渲染视图；当oldVnode和vnode都存在但并不是同一个节点时，使用vnode创建的DOM元素替换旧的DOM元素；当oldVnode和vnode是同一个节点时，使用更详细的对比操作对真实的DOM节点进行更新。

* 有同学可能会对nodeOps感到奇怪，为什么不直接使用parent.removeChild(child)删除节点，而是将这个节点操作封装成函数放在nodeOps里呢？
其实这涉及跨平台渲染的知识，我们知道阿里开发的Weex可以让我们使用相同的组件模型为iOS和Android编写原生渲染的应用。也就是说，我们写的Vue.js组件可以分别在iOS和Android环境中进行原生渲染。
而跨平台渲染的本质是在设计框架的时候，要让框架的渲染机制和DOM解耦。只要把框架更新DOM时的节点操作进行封装，就可以实现跨平台渲染，在不同平台下调用节点的操作。
换言之，如果我们把这些平台下节点操作的封装看成渲染引擎，那么将这些渲染引擎所提供的节点操作的API和框架的运行时对接一下，就可以实现将框架中的代码进行原生渲染的目的。
这就是将removeChild方法封装到nodeOps中的原因。更多关于跨平台渲染的内容已超出本章的讨论范围，这里不再展开讨论。

* 节重点讨论了更新子节点的详细过程以及处理逻辑。
在本节中，我们学习了更新子节点可以分为4种操作，分别是：新增子节点、更新子节点、移动子节点和删除子节点。
在新增子节点中，我们详细讨论了什么情况下需要创建子节点，以及把创建的子节点插入到什么位置。
接下来，我们又讨论了更新子节点的过程。如果在oldChildren中可以找到与新子节点相同的节点，就需要更新它们。
如果在oldChildren中找到的节点的位置和新子节点的位置不一样，需要将DOM中的节点移动到新子节点所在的位置。
删除节点的操作发生在循环结束后。当循环结束后，oldChildren中所有未处理的节点都是需要被删除的节点。
随后我们还讨论了优化策略，通过优化策略可以避免很多循环操作。
最后，我们讨论了怎么分辨哪些子节点是未处理过的节点。


### 第 8 章 模板编译

* 最后也是最重要的是HTML解析器，它是解析器中最核心的模块，它的作用就是解析模板，每当解析到HTML标签的开始位置、结束位置、文本或者注释时，都会触发钩子函数，然后将相关信息通过参数传递出来。

* 本章中，我们主要对模板编译做了一个整体介绍。首先介绍了模板编译在整个渲染流程中的位置，然后介绍了什么是模板编译，最后介绍了如何将模板编译成渲染函数。
而将模板编译成渲染函数有三部分内容：先将模板解析成AST，然后遍历AST标记静态节点，最后使用AST生成代码字符串。这三部分内容分别对应三个模块：解析器、优化器和代码生成器


### 第 9 章 解析器

* 解析器的作用是通过模板得到AST（抽象语法树）。
生成AST的过程需要借助HTML解析器，当HTML解析器触发不同的钩子函数时，我们可以构建出不同的节点。
随后，我们可以通过栈来得到当前正在构建的节点的父节点，然后将构建出的节点添加到父节点的下面。
最终，当HTML解析器运行完毕后，我们就可以得到一个完整的带DOM层级关系的AST。
HTML解析器的内部原理是一小段一小段地截取模板字符串，每截取一小段字符串，就会根据截取出来的字符串类型触发不同的钩子函数，直到模板字符串截空停止运行


### 第 10 章 优化器

* 优化器的作用是在AST中找出静态子树并打上标记，这样做有两个好处：
● 每次重新渲染时，不需要为静态子树创建新节点；
● 在虚拟DOM中打补丁的过程可以跳过。
优化器的内部实现其实主要分为两个步骤：
(1) 在AST中找出所有静态节点并打上标记；
(2) 在AST中找出所有静态根节点并打上标记。


### 第 12 章 架构设计与项目结构

* 如果需要在客户端编译模板（比如传入一个字符串给template选项，或挂载到一个元素上并以其DOM内部的HTML作为模板），那么需要用到编译器，因此需要完整版

* 图12-1给出了Vue.js的整体结构，我们可以看到它整体分为三个部分：核心代码、跨平台相关和公用工具函数（这部分是一些辅助函数，不再单独介绍）。同时，其架构是分层的，最底层是一个普通的构造函数，最上层是一个入口，也就是将一个完整的构造函数导出给用户使用。


### 第 13 章 实例方法与全局API的实现原理

* 其中定义了Vue构造函数，然后分别调用了initMixin、stateMixin、eventsMixin、lifecycleMixin和renderMixin这5个函数，并将Vue构造函数当作参数传给了这5个函数。
这5个函数的作用就是向Vue的原型中挂载方法。

* 与数据相关的实例方法有3个，分别是vm.$watch、vm.$set和vm.$delete，它们是在stateMixin中挂载到Vue的原型上的

* 与事件相关的实例方法有4个，分别是：vm.$on、vm.$once、vm.$off和vm.$emit。这4个方法是在eventsMixin中挂载到Vue构造函数的prototype属性中的

* 事件的实现方式并不难，只需要在注册事件时将回调函数收集起来，在触发事件时将收集起来的回调函数依次调用即可。Vue.js的实现方式也是如此

* 通过上面的介绍，我们知道vm.$once和vm.$on的区别是vm.$once只能被触发一次，所以实现这个功能的一个思路是：在vm.$once中调用vm.$on来实现监听自定义事件的功能，当自定义事件触发后会执行拦截器，将监听器从事件列表中移除。

* vm.$emit的作用是触发事件。前面我们介绍过，所有的事件监听器回调函数都会存储在vm._events中，所以触发事件的实现思路是使用事件名从vm._events中取出对应的事件监听器回调函数列表，然后依次执行列表中的监听器回调并将参数传递给监听器回调。

* 与生命周期相关的实例方法有4个，分别是vm.$mount、vm.$forceUpdate、vm.$nextTick和vm.$destroy。其中有两个方法是从lifecycleMixin中挂载到Vue构造函数的prototype属性上的，分别是vm.$forceUpdate和vm.$destroy。

* vm.$forceUpdate()的作用是迫使Vue.js实例重新渲染。注意它仅仅影响实例本身以及插入插槽内容的子组件，而不是所有子组件。

* vm.$destroy的作用是完全销毁一个实例，它会清理该实例与其他实例的连接，并解绑其全部指令及监听器，同时会触发beforeDestroy和destroyed的钩子函数。

* nextTick接收一个回调函数作为参数，它的作用是将回调延迟到下次DOM更新周期之后执行。它与全局方法Vue.nextTick一样，不同的是回调的this自动绑定到调用它的实例上。如果没有提供回调且在支持Promise的环境中，则返回一个Promise。


* 我们都知道JavaScript是一门单线程且非阻塞的脚本语言，这意味着JavaScript代码在执行的任何时候都只有一个主线程来处理所有任务。而非阻塞是指当代码需要处理异步任务时，主线程会挂起（pending）这个任务，当异步任务处理完毕后，主线程再根据一定规则去执行相应回调。
事实上，当任务处理完毕后，JavaScript会将这个事件加入一个队列中，我们称这个队列为事件队列。被放入事件队列中的事件不会立刻执行其回调，而是等待当前执行栈中的所有任务执行完毕后，主线程会去查找事件队列中是否有任务。

* 我们并不常用这个方法，其原因是如果在实例化Vue.js时设置了el选项，会自动把Vue.js实例挂载到DOM元素上。但理解这个方法却非常重要，因为无论我们在实例化Vue.js时是否设置了el选项，想让Vue.js实例具有关联的DOM元素，只有使用vm.$mount方法这一种途径

* 新方法中会调用原始的方法，这种做法通常被称为函数劫持。
通过函数劫持，可以在原始功能之上新增一些其他功能。

* 在实例化Vue.js时，会有一个初始化流程，其中会向Vue.js实例上新增一些方法，这里的this.$options就是其中之一，它可以访问到实例化Vue.js时用户设置的一些参数，例如template和render。
通过这个条件会发现，如果在实例化Vue.js时给出了render选项，那么template其实是无效的，因为不会进入模板编译的流程，而是直接使用render选项中提供的渲染函数。
关于这一点，Vue.js在官方文档的template选项中也给出了相应的提示。如果没有render选项，那么需要获取模板并将模板编译成渲染函数（render函数）赋值给render选项

* 由于Watcher的第二个参数支持函数，所以当watcher执行函数时，函数中所读取的数据都将会触发getter去全局找到watcher并将其收集到函数的依赖列表中。也就是说，函数中读取的所有数据都将被watcher观察。这些数据中的任何一个发生变化时，watcher都将得到通知。

* Vue.extend的作用是创建一个子类，所以可以创建一个子类，然后让它继承Vue身上的一些功能。

* 用法：设置对象的属性。如果对象是响应式的，确保属性被创建后也是响应式的，同时触发视图更新。这个方法主要用于避开Vue不能检测属性被添加的限制

* 用法：删除对象的属性。如果对象是响应式的，确保删除能触发更新视图。这个方法主要用于避开Vue.js不能检测到属性被删除的限制

* Vue.js允许自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：双花括号插值和v-bind表达式。过滤器应该被添加在JavaScript表达式的尾部，由“管道”符号指

* 用法：注册或获取全局组件。注册组件时，还会自动使用给定的id设置组件的名称。

* 你会发现Vue.directive、Vue.filter和Vue.component这三个方法的实现方式非常相似，代码很多都是重复的。但是为了方便理解，这里我将这三个方法分别拆开单独介绍。事实上，在Vue.js的源码中，这三个方法是放在一起实现的

* 用法：安装Vue.js插件。如果插件是一个对象，必须提供install方法。如果插件是一个函数，它会被作为install方法。调用install方法时，会将Vue作为参数传入。install方法被同一个插件多次调用时，插件也只会被安装一次。

* 用法：全局注册一个混入（mixin），影响注册之后创建的每个Vue.js实例。插件作者可以使用混入向组件注入自定义行为（例如：监听生命周期钩子）。不推荐在应用代码中使用。

* 因为mixin方法修改了Vue.options属性，而之后创建的每个实例都会用到该属性，所以会影响创建的每个实例。但也正是因为有影响，所以mixin在某些场景下才堪称神器。

* 编译模板字符串并返回包含渲染函数的对象。只在完整版中才有效。

* 提供字符串形式的Vue.js安装版本号。这对社区的插件和组件来说非常有用，你可以根据不同的版本号采取不同的策略。

* 本章中，我们详细介绍了Vue.js的实例方法和全局API的实现原理。它们的区别在于：实例方法是Vue.prototype上的方法，而全局API是Vue.js上的方法。
实例方法又分为数据、事件和生命周期这三个类型。
在介绍实例方法以及全局API的实现原理的同时，我们还介绍了扩展知识，例如在介绍vm.$nextTick时我们介绍了JavaScript事件循环机制，以及微任务和宏任务之间的区别等。


### 第 14 章 生命周期

* new Vue()到created之间的阶段叫作初始化阶段。
这个阶段的主要目的是在Vue.js实例上初始化一些属性、事件以及响应式数据，如props、methods、data、computed、watch、provide和inject等。

* 事实上，errorCaptured钩子函数与Vue.js的错误处理有着千丝万缕的关系。Vue.js会捕获所有用户代码抛出的错误，然后会使用一个名叫handleError的函数来处理这些错误。

* 当我们使用Vue.js开发应用时，经常会使用一些状态，例如props、methods、data、computed和watch。在Vue.js内部，这些状态在使用之前需要进行初始化。本节将详细介绍初始化这些状态的内部原理。
通过本节的学习，我们将理解什么是props，为什么methods中的方法可以通过this访问，data在Vue.js内部是什么样的，computed是如何工作的，以及watch的原理等。

* observe函数的作用是将数据转换为响应式的。

* 如果你足够细心，就会发现初始化的顺序其实是精心安排的。先初始化props，后初始化data，这样就可以在data中使用props中的数据了。在watch中既可以观察props，也可以观察data，因为它是最后被初始化的。

* props的实现原理大体上是这样的：父组件提供数据，子组件通过props字段选择自己需要哪些内容，Vue.js内部通过子组件的props选项将需要的数据筛选出来之后添加到子组件的上下文中。

* 这里有一个技巧，即value == null用的是双等号。在双等号中，null和undefined是相等的，也就是说value是null或undefined都会为true。

* 简单来说，data中的数据最终会保存到vm._data中。然后在vm上设置一个代理，使得通过vm.x可以访问到vm._data中的x属性。最后由于这些数据并不是响应式数据，所以需要调用observe函数将data转换成响应式数据。于是，data就完成了初始化。

* 如果data中的某个key与methods发生了重复，依然会将data代理到实例中，但如果与props发生了重复，则不会将data代理到实例中。

* 简单来说，computed是定义在vm上的一个特殊的getter方法。之所以说特殊，是因为在vm上定义getter方法时，get并不是用户提供的函数，而是Vue.js内部的一个代理函数。在代理函数中可以结合Watcher实现缓存与收集依赖等功能。
我们知道计算属性的结果会被缓存，且只有在计算属性所依赖的响应式属性或者说计算属性的返回值发生变化时才会重新计算。那么，如何知道计算属性的返回值是否发生了变化？这其实是结合Watcher的dirty属性来分辨的：当dirty属性为true时，说明需要重新计算“计算属性”的返回值；当dirty属性为false时，说明计算属性的值并没有变，不需要重新计算。

* ● 计算当前计算属性的值，此时会使用Watcher去观察计算属性中用到的所有其他数据的变化。同时将计算属性的Watcher的dirty属性设置为false，这样再次读取计算属性时将不再重新计算，除非计算属性所依赖的数据发生了变化。
● 当计算属性中用到的数据发生变化时，将得到通知从而进行重新渲染操作。

* 但在Vue.js中，只有与data和props重名时，才会打印警告。如果与methods重名，并不会在控制台打印警告。所以如果与methods重名，计算属性会悄悄失效，我们在开发过程中应该尽量避免这种情况。

* 此时计算属性的getter被触发时做的事情发生了变化，它会做下面两件事。
● 使用组件的Watcher观察计算属性的Watcher，也就是把组件的Watcher添加到计算属性的Watcher的依赖列表中，让计算属性的Watcher向组件的Watcher发送通知。
● 使用计算属性的Watcher观察计算属性函数中用到的所有数据，当这些数据发生变化时，向计算属性的Watcher发送通知。

* 我们假设这个依赖是组件的Watcher，那么当计算属性所使用的数据发生变化后，会执行计算属性的Watcher的update方法。随后可以看到，发送通知的代码是在this.getAndInvoke函数的回调中执行的。可以很明确地告诉你，这个函数的作用是对比计算属性的返回值。只有计算属性的返回值真的发生了变化，才会执行回调，从而主动发送通知让组件的Watcher去执行重新渲染逻辑。


### 第 15 章 指令的奥秘

* 指令（directive）是Vue.js提供的带有v-前缀的特殊特性。指令属性的值预期是单个JavaScript表达式。指令的职责是，当表达式的值改变时，将其产生的连带影响响应式地作用于DOM。


* 除了自定义指令外，Vue.js还内置了一些常用指令，例如v-if和v-for等。有些内置指令的实现原理与自定义指令不同，它们提供的功能很难用自定义指令实现。

* 在第二篇中，我们详细介绍了虚拟DOM的实现原理。我们知道，虚拟DOM通过算法对比两个VNode之间的差异并更新真实的DOM节点。在更新真实的DOM节点时，有可能是创建新的节点，或者更新一个已有的节点，还有可能是删除一个节点等。虚拟DOM在渲染时，除了更新DOM内容外，还会触发钩子函数。例如，在更新节点时，除了更新节点的内容外，还会触发update钩子函数。这是因为标签上通常会绑定一些指令、事件或属性，这些内容也需要在更新节点时同步被更新。因此，事件、指令、属性等相关处理逻辑只需要监听钩子函数，在钩子函数触发时执行相关处理逻辑即可实现功能。


### 第 17 章 最佳实践

* 在本书最后一章，我想聊聊日常工作中使用Vue.js开发项目时的最佳实践以及风格规范，其中总结了平时工作中的一些经验、Vue.js官方推荐的最佳实践以及风格规范。
当然，风格规范中的内容不能保证对所有团队或工程都是理想的。但可取的方法是：根据过去的经验、周围的技术栈、个人价值观，对风格做出有意识的修改。

* 为列表渲染设置属性key

* 在v-if/v-if-else/v-else中使用key

* 路由切换组件不变

* 在使用Vue.js开发项目时，最常遇到的一个典型问题就是，当页面切换到同一个路由但不同参数的地址时，组件的生命周期钩子并不会重新触发。

* 组件本质上是一个映射关系，所以先销毁再重建一个相同的组件会存在很大程度上的性能浪费，复用组件才是正确的选择。但是这也意味着组件的生命周期钩子不会再被调用。

* 组件的生命周期钩子虽然不会重新触发，但是路由提供的beforeRouteUpdate守卫可以被触发。因此，只需要把每次切换路由时需要执行的逻辑放到beforeRouteUpdate守卫中即可。例如，在beforeRouteUpdate守卫中发送请求拉取数据，更新状态并重新渲染视图。这种方式是我最推荐的一种方式，在vue-router2.2之后的版本可以使用。

* 为所有路由统一添加query

* 我身边的很多朋友和同事对于组件何时从Vuex的Store获取状态，何时使用props接收父组件传递进来的状态，并没有很清晰的了解。因此，我想这个问题可能是一个普遍现象，故决定在本书中用一节来聊一聊我是如何看待这个问题的。

* 通用组件要定义细致的prop，并且尽可能详细，至少需要指定其类型。这样做的好处是：
● 写明了组件的API，所以很容易看懂组件的用法；
● 在开发环境下，如果向一个组件提供格式不正确的prop，Vue.js将会在控制台发出警告，帮助我们捕获潜在的错误来源。

* 避免v-if和v-for一起使用

* Vue.js官方强烈建议不要把v-if和v-for同时用在同一个元素上。
通常，我们在下面两种常见的情况下，会倾向于不同的做法。
● 为了过滤一个列表中的项目（比如v-for="user in users" v-if="user.isActive"），请将users替换为一个计算属性（比如activeUsers），让它返回过滤后的列表。
● 为了避免渲染本应该被隐藏的列表（比如v-for="user in users" v-if="shouldShow-Users"），请将v-if移动至容器元素上（比如ul和ol）。

* 可以更换为在如下的一个计算属性上遍历并过滤列表

* 通过将v-if移动到容器元素，我们不会再检查每个用户的shouldShowUsers，取而代之的是，我们只检查它一次，且不会在shouldShowUsers为false的时候运算v-for。


* 为组件样式设置作用域

* 在Vue.js中，可以通过scoped特性或CSS Modules（一个基于class的类似BEM的策略）来设置组件样式作用域。


* 避免隐性的父子组件通信

* 单文件组件如何命名

* 单文件组件的文件名应该始终是单词首字母大写（PascalCase）

* 单词首字母大写对于代码编辑器的自动补全最为友好，因为这会使JS(X)和模板中引用组件的方式尽可能一致。

* 只拥有单个活跃实例的组件以The前缀命名，以示其唯一性。但这并不意味着组件只可用于一个单页面，而是每个页面只使用一次。这些组件永远不接受任何prop，因为它们是为你的应用定制的，而不是应用中的上下文。如果你发现有必要添加prop，就表明这实际上是一个可复用的组件，只是目前在每个页面里只使用了一次。

* 和父组件紧密耦合的子组件应该以父组件名作为前缀命名。

* 组件名应该以高级别的（通常是一般化描述的）单词开头，以描述性的修饰词结尾。
注意　规范组件名中的单词顺序似乎有点强人所难，但如果可以做到这一点，能极大提升项目工程的可读性和开发效率。

* 组件名应该倾向于完整单词而不是缩写。编辑器中的自动补全已经让书写长命名的代价非常低了，而它带来的明确性却是非常宝贵的。尤其应该避免不常用的缩写。

* 组件名应该始终由多个单词组成，但是根组件App除外。这样做可以避免与现有的以及未来的HTML元素相冲突，因为所有的HTML元素名称都是单个单词的。

* 对于绝大多数项目来说，在单文件组件和字符串模板中的组件名应该总是单词首字母大写，但是在DOM模板中总是横线连接的。

* 在JavaScript中，单词首字母大写是类和构造函数（本质上是任何可以产生多份不同实例的东西）的命名约定。Vue.js组件也有多份实例，所以同样使用单词首字母大写是有意义的。额外的好处是，在JSX（和模板）里使用单词首字母大写能够让读者更容易分辨Vue.js组件和HTML元素。

* 在单文件组件、字符串模板和JSX中，没有内容的组件应该是自闭合的，但在DOM模板中永远不要这样做。

* 在声明prop的时候，其命名应该始终使用驼峰式命名规则，而在模板和JSX中应该始终使用横线连接的方式。
这里我们遵循每个语言的约定，在JavaScript中更多使用驼峰式命名规则，而在HTML中则是横线连接的方式。

* 多个特性的元素应该分多行撰写，每个特性一行。在JavaScript中，用多行分隔对象的多个属性是很常见的最佳实践，因为这更易读。模板和JSX值得我们做相同的考虑。

* 组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。
复杂的表达式会让模板变得不是那么声明式。我们应该尽量描述理应出现的是什么，而非如何计算那个值。而且计算属性和方法使得代码可以重用。

* 应该把复杂的计算属性分隔为尽可能多更简单的属性。简单、命名得当的计算属性具有以下特点。

* 较小的、专注的计算属性减少了信息使用时的假设性限制，所以需求变更时也不需要那么多重构了。

* 指令缩写（用:表示v-bind:、@表示v-on:）要保持统一。

* 单文件组件应该总是让 <script>、<template> 和 <style> 标签的顺序保持一致，且 <style>要放在最后，因为另外两个标签至少要有一个。

* 遵循这些规范能够在绝大多数工程中改善可读性和开发体验。当风格规范同时存在多个同样好的选项时，选择任意一个都可以确保一致性。在项目中选择统一的规则并尽可能与社区保持统一是一个好选择。接受社区的规范标准将得到以下好处。


## 个人笔记部分


### 第 15 章 指令的奥秘

* const on = vnode.data.on || {} 07    const oldOn = oldVnode.data.on || {}  （个人笔记: 这种写法学习一下）


### 第 17 章 最佳实践

* <!-- 使用CSS Modules--> 06  <style module> 07  .button { 08    border: none; 09    border-radius: 2px; 10  } 11   12  .buttonClose { 13    background-color: red; 14  } 15  </style>  （个人笔记: 查一下这种写法）

