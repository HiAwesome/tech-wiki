# Linux

### [如何清理 Linux 服务器磁盘空间](https://blog.csdn.net/u012660464/article/details/78923011)

* df -h , 这个命令用于查看服务器空间
* du -h --max-depth=1 , 这个命令用于查看当前目录，哪个文件占用最大
* du -sh * , 这个命令也用于查看当前目录下各文件及文件夹占用大小
* cp /dev/null nohup.out , 将 nohup.out 置空
* rm -rf mplogs , 删除 mplogs 目录

### [Linux 查看文件修改时间](https://blog.csdn.net/caianye/article/details/7712194)

Linux 下查看文件时，ls –l 缺省是不显示秒的：

```text
$ ls -l
total 0
-rw-r--r-- 1 gps gps 0 2012-06-12 16:21 README.txt
-rw-r--r-- 1 gps gps 0 2012-06-12 16:21 test.txt
```

要显示秒（实际更精确），可以用 –full-time 参数：

```text
$ ls --full-time
total 0
-rw-r--r-- 1 gps gps 0 2012-06-12 16:21:15.550557727 +0800 README.txt
-rw-r--r-- 1 gps gps 0 2012-06-12 16:21:23.720354220 +0800 test.txt
```

要显示更多信息，用 stat 命令：

```text
$ stat test.txt
  File: `test.txt'
  Size: 0               Blocks: 0          IO Block: 4096   regular empty file
Device: 801h/2049d      Inode: 4980751     Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/     gps)   Gid: ( 1000/     gps)
Access: 2012-06-12 16:21:23.720354220 +0800
Modify: 2012-06-12 16:21:23.720354220 +0800
Change: 2012-06-12 16:21:23.720354220 +0800
```

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
