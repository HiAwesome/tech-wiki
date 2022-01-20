# Linux

#### [How to Print the Longest Line(s) in a File](https://www.baeldung.com/linux/print-longest-lines-in-file)

应用: `awk '{printf "line #%d has length:%d\n",NR,length}' 20220120_web_long_info.log | awk -F ":" '{if($2 > 5000){print $0}}' | sort -nk 2 -t':' -r | head`

```text
line #39565 has length:14819607
line #38473 has length:14707696
line #38524 has length:1402603
line #38529 has length:382840
line #38528 has length:340657
line #38525 has length:303864
line #38496 has length:226641
line #38484 has length:226641
line #38517 has length:212428
line #38511 has length:198182
```

#### [Listing with `ls` and regular expression](https://unix.stackexchange.com/a/523013)

The question asked for regular expressions. Bash, and thus `ls`, does not support regular expressions here. What it supports is filename expressions ([Globbing](https://tldp.org/LDP/abs/html/globbingref.html)), a form of wildcards. Regular expressions are a lot more powerful than that.

#### [Why does man print “gimme gimme gimme” at 00:30?](https://unix.stackexchange.com/questions/405783/why-does-man-print-gimme-gimme-gimme-at-0030)

#### [Common Linux Text Search](https://www.baeldung.com/linux/common-text-search)

```text
Linux is a great system.
Learning linux is very interesting.

This Linux system has 17 users.
The uptime of this linux system: 77 hours.

File report
There are 100 directories under */*.
There are 250 files under */opt*. 
There are 300 files under */home/root*.
There are 20 mountpoints.
```

```shell
# Basic String Search
~/Downloads grep 'linux' input.txt
Learning linux is very interesting.
The uptime of this linux system: 77 hours.
~/Downloads abc=linux
~/Downloads print "$abc"
linux
~/Downloads grep "$abc" input.txt
Learning linux is very interesting.
The uptime of this linux system: 77 hours.

# Case-Insensitive Search
~/Downloads grep -i 'linux' input.txt
Linux is a great system.
Learning linux is very interesting.
This Linux system has 17 users.
The uptime of this linux system: 77 hours.

# Whole-Word Search
~/Downloads grep -w 'is' input.txt
Linux is a great system.
Learning linux is very interesting.

# Fixed String Search
~/Downloads grep -F '*/opt*' input.txt
There are 250 files under */opt*.
~/Downloads grep '\*/opt\*' input.txt
There are 250 files under */opt*.

# Inverting the Search
~/Downloads grep -v '[0-9]' input.txt
Linux is a great system.
Learning linux is very interesting.


File report

# Inverting the Search: with -P
# Mac OS have not this parameter.
[Linux] grep -vP '\d' input.txt
Linux is a great system.
Learning linux is very interesting.


File report
[Linux] man grep | grep -- -P
-P, --perl-regexp
      Interpret PATTERN as a Perl regular expression.  This is highly experimental and grep -P may warn of unimplemented features.  

# Print Only the Matched Parts
~/Downloads grep -o '/[^/*]*' input.txt
/
/opt
/home
/root

# Print Additional Context Lines Before or After Match
# -B (before a match), -A (after a match), and -C (before and after a match).
~/Downloads grep -A3 'report' input.txt
File report
There are 100 directories under */*.
There are 250 files under */opt*.
There are 300 files under */home/root*.

# Count the Matching Lines
~/Downloads grep -Fc 'are' input.txt
4
~/Downloads grep -Fc '*' input.txt
3
~/Downloads grep -Fc '7' input.txt
2
~ man grep | grep -A1 -- -c,
     -c, --count
             Only a count of selected lines is written to standard output.
--
             each file processed.  This option is ignored if -c, -L, -l, or -q
             is specified.
             
# Recurisively Search directory
~/Downloads grep -Rl 'boot' /var/log
/var/log/cron
/var/log/daily.out
~ man grep | grep -A 1 -- -R,
     -R, -r, --recursive
             Recursively search subdirectories listed.
~ man grep | grep -A 1 -- -l,
     -l, --files-with-matches
             Only the names of files containing selected lines are written to
--
             each file processed.  This option is ignored if -c, -L, -l, or -q
             is specified.
```

#### [What’s Difference Between Grep, Egrep and Fgrep in Linux?](https://www.tecmint.com/difference-between-grep-egrep-and-fgrep-in-linux/)

#### [grep the man page of a command for hyphenated options](https://unix.stackexchange.com/a/324150)

If you want to grep for a pattern beginning with a hyphen, use -- before the pattern you specify.

```shell
man find | grep -- -type
```

#### [Count Occurrences of a Char in a Text File in Linux](https://www.baeldung.com/linux/count-occurrences-of-char-in-text)

The tr command is the fastest of the three to get the character count in large files.

```shell
$ ls -lah large.txt 
-rw-r--r--. 1 root root 1.1G Jun 12 10:53 large.txt

$ time grep -o 'e' large.txt | wc -l
82256735

real	0m40.733s
user	0m39.649s
sys	0m0.714s

$ time tr -c -d 'e' &lt; large.txt | wc -c
82256735

real	0m2.542s
user	0m1.892s
sys	0m0.433s

$ time awk -Fe '{s+=(NF-1)} END {print s}' large.txt 
82256735

real	0m11.080s
user	0m9.589s
sys	0m0.933s
```

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
