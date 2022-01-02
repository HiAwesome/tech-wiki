# Ruby

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
* T

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
