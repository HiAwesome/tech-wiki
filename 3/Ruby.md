# Ruby

* `__FILE__` 是一个魔法值，它存有现在运行的脚本文件的名字。`$0` 是启动脚本的名字。 代码里的比较结构的意思是 “如果这是启动脚本的话…” 这允许代码作为库调用的时候不运行启动代码， 而在作为执行脚本的时候调用启动代码。
  ```ruby
  if __FILE__ == $0 
  ```
* 代码块最有用的地方是用来处理比迭代列表更繁琐的工作。除了一般家务活之外， 您可以用它来自动安装卸载或处理运行错误。真正做到让用户省心、放心。
  ```ruby
  # Say bye to everybody
  def say_bye
    if @names.nil?
      puts "..."
    elsif @names.respond_to?("join")
      # Join the list elements with commas
      puts "Goodbye #{@names.join(", ")}.  Come back soon!"
    else
      puts "Goodbye #{@names}.  Come back soon!"
    end
  end
  ```
  
#### [从 Java 到 Ruby](https://www.ruby-lang.org/zh_cn/documentation/ruby-from-other-languages/to-ruby-from-java/)

Java 非常成熟，久经检验，且非常快（与那些反对java的人可能还在声称的相反）。但它也非常啰嗦。从 Java 到 Ruby，可以预见的是代码规模将大大缩小。你也可以期望使用相对少的时间快速出产原型。

相似点，Ruby 与 Java 一样的地方……

* 垃圾回收器帮你管理内存。
* 强类型对象。
* 有 public、 private 和 protected 方法。
* 拥有嵌入式文档工具（Ruby 的工具叫 rdoc）。rdoc 生成的文档与 javadoc 非常相似。

相异点，Ruby 与 Java 不同的地方……

* 你不需要编译你的代码。你只需要直接运行它。
* 有几个不同的流行的第三方GUI工具包。Ruby 用户可以尝试 WxRuby、 FXRuby、 Ruby-GNOME2、 Qt 或 Ruby 内置的 Tk。
* 定义像类这样的东西时，可以使用 `end` 关键字，而不使用花括号包裹代码块。
* 使用 `require` 代替 `import`。
* 所有成员变量为私有。在外部，使用方法获取所有你需要的一切。
* 方法调用的括号通常是可选的，经常被省略。
* 一切皆对象，包括像 2 和 3.14159 这样的数字。
* 没有静态类型检查。
* 变量名只是标签。它们没有相应的类型。
* 没有类型声明。按需分配变量名，及时可用（如：`a = [1,2,3]` 而不是 `int[] a = {1,2,3};`）。
* 没有显式转换。只需要调用方法。代码运行之前，单元测试应该告诉你出现异常。
* 使用 `foo = Foo.new("hi")` 创建新对象，而非 `Foo foo = new Foo("hi")`。
* 构造器总是命名为“initialize” 而不是类名称。
* 作为接口的替代，你将获得“混入（mixins）”。
* 相比 XML，倾向于使用 YAML。
* `nil` 替代 `null`。
* Ruby 对 `==` 和 `equals()` 的处理方式与 Java 不一样。测试相等性使用 `==`（Java 中是 `equals()`）。测试是否为同一对象使用 `equals?()`（Java 中是 `==`）。

#### Mac 上问题

Mac 上运行 `irb` 会出现如下警告:

```text
WARNING: This version of ruby is included in macOS for compatibility with legacy software.
In future versions of macOS the ruby runtime will not be available by
default, and may require you to install an additional package.
```

使用 rbenv 指定了全局版本为 3.1.0 依然无法消除这个问题，后面再看一下。

更新: 使用 `rbenv refresh` 修正了，具体为:

```shell
~ rbenv rehash
rbenv: cannot rehash: /Users/moqi/.rbenv/shims/.rbenv-shim exists
~ rm -rf /Users/moqi/.rbenv/shims/.rbenv-shim
~ rbenv rehash
~ irb
irb(main):001:0> puts 'hello, world'
hello, world
=> nil
irb(main):002:0> exit
~ which ruby
/Users/moqi/.rbenv/shims/ruby
~ which irb
/Users/moqi/.rbenv/shims/irb
```
