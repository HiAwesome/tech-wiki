# Vue.js

#### [vue中localStorage存储信息的三种方法](https://www.jianshu.com/p/2ad1d552727a)

#### [vue中警告 Violation Added non-passive event listener to a scroll-blocking...](https://www.jianshu.com/p/23850d4cade8)

#### [Vue中watch用法详解](https://www.jianshu.com/p/b70f1668d08f)

## Base

* Vue 不支持 IE8 及以下版本，因为 Vue 使用了 IE8 无法模拟的 ECMAScript 5 特性。
* 在使用 Vue 时，我们推荐在你的浏览器上安装 [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools). 它允许你在一个更友好的界面中审查和调试 Vue 应用。
* Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种 [支持类库](https://github.com/vuejs/awesome-vue) 结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。
* Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统。
* 你看到的 `v-bind` attribute 被称为指令。指令带有前缀 `v-`，以表示它们是 Vue 提供的特殊 attribute。可能你已经猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。
* 我们不仅可以把数据绑定到 DOM 文本或 attribute，还可以绑定到 DOM 结构。此外，Vue 也提供一个强大的过渡效果系统，可以在 Vue 插入/更新/移除元素时自动应用 [过渡效果](https://cn.vuejs.org/v2/guide/transitions.html).
* Vue 还提供了 `v-model` 指令，它能轻松实现表单输入和应用状态之间的双向绑定。
* 组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树。在 Vue 里，一个组件本质上是一个拥有预定义选项的一个 Vue 实例。在一个大型应用中，有必要将整个应用程序划分为组件，以使开发更易管理。
* 你可能已经注意到 Vue 组件非常类似于自定义元素——它是 [Web 组件规范](https://www.w3.org/wiki/WebComponents/) 的一部分，这是因为 Vue 的组件语法部分参考了该规范。Vue 组件提供了纯自定义元素所不具备的一些重要功能，最突出的是跨组件数据流、自定义事件通信以及构建工具集成。虽然 Vue 内部没有使用自定义元素，不过在应用使用自定义元素、或以自定义元素形式发布时，[依然有很好的互操作性](https://custom-elements-everywhere.com/#vue). Vue CLI 也支持将 Vue 组件构建成为原生的自定义元素。
* 当一个 Vue 实例被创建时，它将 `data` 对象中的所有的 property 加入到 Vue 的**响应式系统**中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。
* 每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。



































































