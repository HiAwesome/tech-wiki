# Linux

### [Linux 向命令传入一个看上去像标志参数的普通参数](https://missing-semester-cn.github.io/2020/potpourri/)

有的时候你可能需要向工具传入一个 看上去 像标志参数的普通参数，比如：

* 使用 `rm` 删除一个叫 `-r` 的文件；
* 在通过一个程序运行另一个程序的时候（`ssh machine foo`），向内层的程序（`foo`）传递一个标志参数。

这时候你可以使用特殊参数 -- 让某个程序 停止处理 -- 后面出现的标志参数以及选项（以 - 开头的内容）：

* `rm -- -r` 会让 `rm` 将 `-r` 当作文件名；
* `ssh machine --for-ssh -- foo --for-foo` 的 `--` 会让 `ssh` 知道 `--for-foo` 不是 `ssh` 的标志参数。

### [How can I add a new user as sudoer using the command line?](https://askubuntu.com/a/7484)

```shell
sudo vim /etc/sudoers
```

#### [Don't use VPN services.](https://gist.github.com/moqimoqidea/c0f25184b6c57011470a7f76b2cfbfda)

#### 使用 Python VS Bash脚本 VS 其他语言?

通常来说，Bash 脚本对于简短的一次性脚本有效，比如当你想要运行一系列的命令的时候。但是Bash 脚本有一些比较奇怪的地方，这使得大型程序或脚本难以用 Bash 实现：

Bash 可以获取简单的用例，但是很难获得全部可能的输入。例如，脚本参数中的空格会导致Bash 脚本出错。
Bash 对于代码重用并不友好。因此，重用你先前已经写好的代码很困难。通常 Bash 中没有软件库的概念。
Bash 依赖于一些像 `$?` 或 `$@` 的特殊字符指代特殊的值。其他的语言却会显式地引用，比如 `exitCode` 或 `sys.args`。
因此，对于大型或者更加复杂的脚本我们推荐使用更加成熟的脚本语言例如 Python 和 Ruby。 你可以找到很多用这些语言编写的，用来解决常见问题的在线库。 如果你发现某种语言实现了你所需要的特定功能库，最好的方式就是直接去使用那种语言。
