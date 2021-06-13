# CentOS

环境： Cent OS 7 on Vmware Fusion.

## Tips

* su - 可以切换到 root 账号，效果同 sudo su
* [What's the difference between `chmod a+x` and `chmod +x`?](https://unix.stackexchange.com/a/639441)
* [Why does chmod +w not give write permission to other(o)](https://unix.stackexchange.com/a/429424)
* [umask - wikipedia](https://en.wikipedia.org/wiki/Umask)
* 为什么可执行权限缩写为 x 而不是 e？个人猜猜因为 read 和 write 中也含有 e, 而 x 是 execute 中第一个其他两个权限不包括的字母。
* cd -> change directory.
* x 权限对于 linux 文件夹来说，表示进入该文件夹的权限。
* [How To Set Up SSH Keys on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-centos7)
* 很多读者都会误会 /usr 为 user 的缩写，其实 usr 是 Unix Software Resource 的缩写， 也就 是"Unix操作系统软件资源"所放置的目录，而不是使用者的数据啦！这点要注意。 FHS建议所有软件开发者，应该将他们的数据合理的分别放置到这个目录下的次目录，而不要自行创建该软件自己独立的目录。
* 例题：网络文件常常提到类似"./run.sh"之类的数据，这个指令的意义为何？答：由于指令的执行需要变量（bash章节才会提到）的支持，若你的可执行文件放置在本目录，并且本目录并非正规的可执行文件目录（/bin, /usr/bin等为正规），此时要执行指令就得要严格指定该可执行文件。"./"代表"本目录"的意思，所以"./run.sh"代表"执行本目录下，名为run.sh的文件"啰！
* `~account` 代表 account 这个使用者的主文件夹（account是个帐号名称）
* 另外，针对`cd`的使用方法，如果仅输入`cd`时，代表的就是`cd ~`的意思喔，亦即是会回到自己的主文件夹啦！而那个`cd -`比较难以理解，请自行多做几次练习，就会比较明白了。
* `pwd`是`Print Working Directory`的缩写，也就是显示目前所在目录的指令。
* `mkdir`命令`-p`的含义：`-p, --parents`, no error if existing, make parent directories as needed.
* `rmdir`命令`-p`的含义：`-p, --parents`, remove DIRECTORY and its ancestors; e.g., `rmdir -p a/b/c` is similar to `rmdir a/b/c a/b a`.
* `ls`命令`-f`的含义：直接列出结果，而不进行排序 （ls 默认会以文件名排序！）
* `ls`命令`-r`的含义：将排序结果反向输出，例如：原本文件名由小到大，反向则为由大到小；
* `ls`命令`-S`的含义：以文件大小大小排序，而不是用文件名排序；
* `ls`命令`-t`的含义：依时间排序，而不是用文件名。
* `ls`命令`--full-time`的含义：以完整时间模式 （包含年、月、日、时、分） 输出。
* 无论如何，`ls`最常被使用到的功能还是那个`-l`的选项，为此，很多 distribution 在默认的情况中，已经将`ll`（L 的小写）设置成为`ls -l`的意思了！其实，那个功能是 Bash shell 的 alias 功能呢～也就是说，我们直接输入`ll`就等于是输入`ls -l`是一样的。
* 嘿嘿！Linux 里面有"猫"指令？喔！不是的， cat 是 Concatenate （连续） 的简写， 主要的功能是将一个文件的内容连续的印出在屏幕上面！
* `head`命令 -n 选项后面的参数较有趣，如果接的是负数，例如上面范例的 -n -100 时，代表列前的所有行数，但不包括后面100行。举例来说 CentOS 7.1 的 /etc/mandb.conf 共有131行，则上述的指令`head -n -100 /etc/man_db.conf`就会列出前面 31 行，后面 100 行不会打印出来 了。
* `tail`命令，当下达`tail -n +100 /etc/man_db.conf`代表该文件从 100 行以后都会被列出来，同样的，在 man_db.conf 共有 131 行，因此第 100~131 行就会被列出来啦！前面的 99 行都不会被显示出来喔！
* 文件隐藏属性：`chattr [+-=][ASacdistu] (file|dir)`, 常见的包括只能新增数据的 +a 与完全不能更动文件的 +i 属性。

### 学习 VI 的理由

* 所有的 Unix Like 系统都会内置 vi 文书编辑器，其他的文书编辑器则不一定会存在；
* 很多个别软件的编辑接口都会主动调用 vi （例如未来会谈到的 crontab, visudo, edquota 等指令）；
* vim 具有程序编辑的能力，可以主动的以字体颜色辨别语法的正确性，方便程序设计；
* 因为程序简单，编辑速度相当快速。

### VIM Tips

[missing-semester: vim](https://missing-semester-cn.github.io/2020/editors/)

#### 移动

* 基本移动: hjkl （左， 下， 上， 右）
* 词： w （下一个词）， b （词初）， e （词尾）
* 行： 0 （行初）， ^ （第一个非空格字符）， $ （行尾）
* 屏幕： H （屏幕首行）， M （屏幕中间）， L （屏幕底部）
* G：移动到这个文件的最后一列。
* 你G：n 为数字。移动到这个文件的第 n 列。（可配合 :set nu）
* gg：移动到这个文件的第一列，相当于 1G 啊！
* n<Enter>：n 为数字。光标向下移动 n 列。
* 翻页：ctrl + f（下一页）b（上一页）d（下半页）u（上半页）。
* 杂项：% （找到配对，比如括号或者 /* */ 之类的注释对）

#### 选择

可视化模式:

* 可视化：v
* 可视化行： V
* 可视化块：Ctrl+v

可以用移动命令来选中。

#### 编辑

所有你需要用鼠标做的事，你现在都可以用键盘：采用编辑命令和移动命令的组合来完成。这就是 Vim 的界面开始看起来像一个程序语言的时候。Vim 的编辑命令也被称为"动词"， 因为动词可以施动于名词。Many commands that change text are made from an operator and a motion. The format for a delete command with the  d  delete operator is as follows: `d motion` Where: `d` is the delete operator. `motion` is what the operator will operate on (listed below).

* i 进入插入模式, 但是对于操纵/编辑文本，不单想用退格键完成
* O / o 在之上/之下插入行
* d{移动命令} 删除 {移动命令}，例如， dw 删除词, d$ 删除到行尾, d0 删除到行头。
* c{移动命令} 改变 {移动命令}，例如， cw 改变词；比如 d{移动命令} 再 i
* x 删除字符（等同于 dl）
* s 替换字符（等同于 xi）
* 可视化模式 + 操作，选中文字, d 删除 或者 c 改变
* u 撤销, <C-r> 重做
* y 复制 / “yank” （其他一些命令比如 d 也会复制）
* p 粘贴
* 更多值得学习的: 比如 ~ 改变字符的大小写

#### 正则替换

* :n1,n2s/word1/word2/g：n1 与 n2 为数字。在第 n1 与 n2 列之间寻找 word1 这个字串，并将该字串取代为 word2举例来说，在 100 到 200 列之间搜寻 vbird 并取代为 VBIRD 则：":100,200s/vbird/VBIRD/g"。
* :1,$s/word1/word2/g：从第一列到最后一列寻找 word1 字串，并将该字串取代为 word2，使用 :%s/word1/word2/g 更好。
* :1,$s/word1/word2/gc：从第一列到最后一列寻找 word1 字串，并将该字串取代为 word2 且在取代前显示提示字符给使用者确认（confirm）是否需要取代，使用 :%s/word1/word2/gc 更好。

#### 计数

你可以用一个计数来结合“名词”和“动词”，这会执行指定操作若干次。USING A COUNT FOR A MOTION, Typing a number before a motion repeats it that many times.

* 3w 向前移动三个词
* 5j 向下移动5行
* 7dw 删除7个词

#### 修饰语

你可以用修饰语改变“名词”的意义。修饰语有 i，表示“内部”或者“在内“，和 a， 表示”周围“。

* ci( 改变当前括号内的内容
* ci[ 改变当前方括号内的内容
* da' 删除一个单引号字符串， 包括周围的单引号

#### vimtutor

`vimtutor` 是一个 Vim 安装时自带的教程。 

* NOTE: As you go through this tutor, do not try to memorize, learn by usage.
* NOTE: Remember that you should be learning by doing, not memorization.
* Type CTRL-G to show your location in the file and the file status. Type  G  to move to a line in the file.
* Lesson 4.3: MATCHING PARENTHESES SEARCH: Type  %  to find a matching ),], or } . NOTE: This is very useful in debugging a program with unmatched parentheses!
* Lesson 4.4: THE SUBSTITUTE COMMAND: To change every occurrence of a character string between two lines,
   * type   :#,#s/old/new/g    where #,# are the line numbers of the range of lines where the substitution is to be done.
   * Type   :%s/old/new/g      to change every occurrence in the whole file.
   * Type   :%s/old/new/gc     to find every occurrence in the whole file, with a prompt whether to substitute or not.
* CTRL-O takes you back to older positions, CTRL-I to newer positions.
* Lesson 6.5: SET OPTION
    * Set the 'ic' (Ignore case) option by entering:   :set ic
    * Set the 'hlsearch' and 'incsearch' options:  :set hls is
    * To disable ignoring case enter:  :set noic
    * To remove the highlighting of matches enter:   :nohlsearch
    * If you want to ignore case for just one search command, use \c in the phrase:  /ignore\c  <ENTER>

#### 杂项

* [Copying and Moving Text -- Yank, Delete, and Put](https://docs.oracle.com/cd/E19455-01/806-2902/editorvi-53/index.html): Many word-processors allow you to "copy and paste" and "cut and paste" lines of text. The vi editor also includes these features. The vi command-mode equivalent of "copy and paste" is yank and put; the equivalent of "cut and paste" is delete and put. The methods for copying or moving small blocks of text in vi involves using a combination of the yank, delete, and put commands.

## 20200605

### Keyboard & Mouse

设置 CentOS 7 的键盘映射，从标准的 `Profile - Default` 复制一份命名为 `Profile - CentOS7`, 然后将 `Key Mappings` -> `Virtual Machine Shortcut` 的部分全部带上 `Shift`, 例如 `Control - C` 变更为 `Shift - Control - C`.

### 设置静态 IP

1. Vmware Fusion 外面设置连接方式为 Bridged Networking -> Wi-Fi.
2. /etc/sysconfig/network-scripts/ifcfg-ens33 从 BOOTPROTO=dhcp 变更为:
    ```text
    BOOTPROTO=static
    IPADDR=192.168.50.200
    NETMASK=255.255.255.0
    GATEWAY=192.168.50.1
    DNS1=192.168.50.1
    ```
3. /etc/sysconfig/network 设定 DNS:
    ```text
    DNS1=192.168.50.1
    ```
4. 运行 service network restart

参考：

* [Centos 7 学习之静态IP设置(续)](https://blog.csdn.net/johnnycode/article/details/50184073)
* [Questions about CentOS-7](https://wiki.centos.org/FAQ/CentOS7#head-a21a9e454157700367c9b7e9ccb1ff9954bec881)

### man 命令移动

```text
e  ^E  j  ^N  CR  *  Forward  one line   (or N lines).
y  ^Y  k  ^K  ^P  *  Backward one line   (or N lines).
f  ^F  ^V  SPACE  *  Forward  one window (or N lines).
b  ^B  ESC-v      *  Backward one window (or N lines).
z                 *  Forward  one window (and set window to N).
w                 *  Backward one window (and set window to N).
ESC-SPACE         *  Forward  one window, but don't stop at end-of-file.
d  ^D             *  Forward  one half-window (and set half-window to N).
u  ^U             *  Backward one half-window (and set half-window to N).
ESC-)  RightArrow *  Left  one half screen width (or N positions).
ESC-(  LeftArrow  *  Right one half screen width (or N positions).
F                    Forward forever; like "tail -f".
r  ^R  ^L            Repaint screen.
R                    Repaint screen, discarding buffered input.
```
### 安装 [shellcheck](https://github.com/koalaman/shellcheck)

```shell
yum -y install epel-release && yum install ShellCheck
```

### 安装 node.js

```shell
curl -sL https://rpm.nodesource.com/setup_14.x | bash -

## Run `sudo yum install -y nodejs` to install Node.js 14.x and npm.
## You may also need development tools to build native addons:
     sudo yum install gcc-c++ make
## To install the Yarn package manager, run:
     curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
     sudo yum install yarn
```

### 安装 [tldr](https://github.com/tldr-pages/tldr)

依赖较新版本的 node.js

```shell
npm install -g tldr
```

### 安装 [ripgrep](https://github.com/BurntSushi/ripgrep#installation)

```shell
$ sudo yum-config-manager --add-repo=https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo
$ sudo yum install ripgrep
```

### 安装最新 Git

**[https://www.digitalocean.com/community/tutorials/how-to-install-git-on-centos-7](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-centos-7)**.

### 安装 [fzf](https://github.com/junegunn/fzf#using-git)

```shell
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

### 安装 zsh 及其插件

1. [How to install Zsh version 5.6.2 into CentOS 7](https://gist.github.com/moqimoqidea/e4b0dd31741370ce91bd638f83ed1a25)
2. 将 `~/.zshrc` 中 `ZSH_THEME` 属性设置为 `simple`.
3. 安装 [ohmyzsh](https://github.com/ohmyzsh/ohmyzsh#basic-installation)
4. 安装 [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)

### 安装 jq

* [How to Install jq(JSON processor) on RHEL/CentOS 7/8](https://www.cyberithub.com/how-to-install-jq-json-processor-on-rhel-centos-7-8/)

## 20200606

### 命令练习

#### uname

```text
~ uname --help
Usage: uname [OPTION]...
Print certain system information.  With no OPTION, same as -s.

  -a, --all                print all information, in the following order,
                             except omit -p and -i if unknown:
  -s, --kernel-name        print the kernel name
  -n, --nodename           print the network node hostname
  -r, --kernel-release     print the kernel release
  -v, --kernel-version     print the kernel version
  -m, --machine            print the machine hardware name
  -p, --processor          print the processor type or "unknown"
  -i, --hardware-platform  print the hardware platform or "unknown"
  -o, --operating-system   print the operating system
      --help     display this help and exit
      --version  output version information and exit

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
For complete documentation, run: info coreutils 'uname invocation'
~ uname -a
Linux localhost 3.10.0-1160.25.1.el7.x86_64 #1 SMP Wed Apr 28 21:49:45 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
~ uname -s
Linux
~ uname -n
localhost
~ uname -r
3.10.0-1160.25.1.el7.x86_64
~ uname -v
#1 SMP Wed Apr 28 21:49:45 UTC 2021
~ uname -m
x86_64
~ uname -p
x86_64
~ uname -i
x86_64
~ uname -o
GNU/Linux
```

#### pwd

```text
~ cd /var/mail
/var/mail pwd
/var/mail
/var/mail pwd -P
/var/spool/mail
/var ls -ld mail
lrwxrwxrwx. 1 root root 10 May 31 23:56 mail -> spool/mail
```

#### cp

```text
~ cp ~/.zshrc /tmp/zshrc
~ cp -i ~/.zshrc /tmp/zshrc
cp: overwrite ‘/tmp/zshrc’? y
~ cd /tmp
/tmp cp /var/log/wtmp .
/tmp ls -ls /var/log/wtmp wtmp
44 -rw-rw-r--. 1 root utmp 42240 Jun  6 09:23 /var/log/wtmp
44 -rw-r--r--. 1 root root 42240 Jun  6 10:28 wtmp
/tmp cp -a /var/log/wtmp wtmp_2
/tmp ls -ls /var/log/wtmp wtmp_2
44 -rw-rw-r--. 1 root utmp 42240 Jun  6 09:23 /var/log/wtmp
44 -rw-rw-r--. 1 root utmp 42240 Jun  6 09:23 wtmp_2
/tmp cp /etc/ /tmp
cp: omitting directory ‘/etc/’
/tmp cp -r /etc/ /tmp
/tmp ls -l zshrc
-rw-r--r--. 1 root root 3703 Jun  6 10:27 zshrc
/tmp cp -s zshrc zshrc_slink
/tmp cp -l zshrc zshrc_hlink
/tmp ls -l zshrc*
-rw-r--r--. 2 root root 3703 Jun  6 10:27 zshrc
-rw-r--r--. 2 root root 3703 Jun  6 10:27 zshrc_hlink
lrwxrwxrwx. 1 root root    5 Jun  6 10:32 zshrc_slink -> zshrc
```

### 文件的时间

* modification time（mtime）：当该文件的“内容数据”变更时，就会更新这个时间！内容数据指的是文件的内容，而不是文件的属性或权限喔！
* status time（ctime）：当该文件的“状态（status）”改变时，就会更新这个时间，举例来说，像是权限与属性被更改了，都会更新这个时间啊。
* access time（atime）：当“该文件的内容被取用”时，就会更新这个读取时间（access）。举例来说，我们使用 cat 去读取 /etc/man_db.conf，就会更新该文件的 atime 了。

```text
~ date; ls -l /etc/man_db.conf ; ls -l --time=atime /etc/man_db.conf ; ls -l --time=ctime /etc/man_db.conf
Sun Jun  6 13:42:06 CST 2021
-rw-r--r--. 1 root root 5171 Oct 31  2018 /etc/man_db.conf
-rw-r--r--. 1 root root 5171 Jun  5 11:10 /etc/man_db.conf
-rw-r--r--. 1 root root 5171 May 31 23:59 /etc/man_db.conf
```

#### touch

```text
/tmp touch -d '2 days ago' zshrc
/tmp date; ll zshrc; ll --time=atime zshrc; ll --time=ctime zshrc
Sun Jun  6 13:53:26 CST 2021
-rw-r--r--. 1 root root 3.7K Jun  4 13:53 zshrc
-rw-r--r--. 1 root root 3.7K Jun  4 13:53 zshrc
-rw-r--r--. 1 root root 3.7K Jun  6 13:53 zshrc
/tmp touch -t 201406150202 zshrc
/tmp date; ll zshrc; ll --time=atime zshrc; ll --time=ctime zshrc
Sun Jun  6 13:53:51 CST 2021
-rw-r--r--. 1 root root 3.7K Jun 15  2014 zshrc
-rw-r--r--. 1 root root 3.7K Jun 15  2014 zshrc
-rw-r--r--. 1 root root 3.7K Jun  6 13:53 zshrc
```

#### od

```text
od -t oCc /etc/issue
0000000 134 123 012 113 145 162 156 145 154 040 134 162 040 157 156 040
          \   S  \n   K   e   r   n   e   l       \   r       o   n
0000020 141 156 040 134 155 012 012
          a   n       \   m  \n  \n
0000027
~ echo 'password' | od -t oCc
0000000 160 141 163 163 167 157 162 144 012
          p   a   s   s   w   o   r   d  \n
0000011
```

#### chattr

```text
/tmp touch attrtest
/tmp chattr +i attrtest
/tmp rm attrtest
rm: cannot remove ‘attrtest’: Operation not permitted
/tmp rm -f attrtest
rm: cannot remove ‘attrtest’: Operation not permitted
/tmp chattr -i attrtest
/tmp rm -f attrtest
/tmp touch aaaaa
/tmp lsattr aaaaa
---------------- aaaaa
/tmp chattr +aiS aaaaa
/tmp lsattr aaaaa
--S-ia---------- aaaaa
```

#### 文件特殊权限

SUID, SGID, SBIT

```text
/tmp ls -ld /tmp; ls -l /usr/bin/passwd
drwxrwxrwt. 38 root root 4096 Jun  6 21:21 /tmp
-rwsr-xr-x. 1 root root 27856 Apr  1  2020 /usr/bin/passwd
/tmp ls -l /usr/bin/locate
-rwx--s--x. 1 root slocate 40520 Apr 11  2018 /usr/bin/locate
/tmp touch t1
/tmp chmod 4755 t1
/tmp ll t1
-rwsr-xr-x. 1 root root 0 Jun  6 21:30 t1
/tmp chmod 6755 t1
/tmp ll t1
-rwsr-sr-x. 1 root root 0 Jun  6 21:30 t1
/tmp chmod 1755 t1
/tmp ll t1
-rwxr-xr-t. 1 root root 0 Jun  6 21:30 t1
/tmp chmod 7666 t1
/tmp ll t1
-rwSrwSrwT. 1 root root 0 Jun  6 21:30 t1
```

#### which

寻找可执行文件

```text
/tmp which ifconfig
/usr/sbin/ifconfig
/tmp which which
which: shell built-in command
/tmp which history
history: aliased to omz_history
/tmp which -a which
which: shell built-in command
/usr/bin/which
/tmp which -a history
history: aliased to omz_history
history: shell built-in command
```

#### 文件文件名的搜寻

再来谈一谈怎么搜寻文件吧！在 Linux 下面也有相当优异的搜寻指令呦！通常 find 不很常用的！因为速度慢之外，也很操硬盘！一般我们都是先使用 whereis 或者是 locate 来检查，如果真的找不到了，才以 find 来搜寻呦！为什么呢？因为 whereis 只找系统中某些特定目录下面的文件而已，locate 则是利用数据库来搜寻文件名，当然两者就相当的快速，并且没有实际的搜寻硬盘内的文件系统状态，比较省时间啦！

##### whereis

由一些特定的目录中寻找文件文件名

```text
/tmp whereis ifconfg
ifconfg:#
/tmp whereis passwd
passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1.gz /usr/share/man/man5/passwd.5.gz
/tmp whereis -m passwd
passwd: /usr/share/man/man1/passwd.1.gz /usr/share/man/man5/passwd.5.gz
```

##### localte

```text
/tmp locate keyword
/usr/lib/node_modules/npm/node_modules/har-validator/node_modules/ajv/lib/keyword.js
/usr/lib64/perl5/CORE/keywords.h
/usr/lib64/python2.7/keyword.py
/usr/lib64/python2.7/keyword.pyc
/usr/lib64/python2.7/keyword.pyo
/usr/share/systemtap/examples/keyword-index.html
/usr/share/systemtap/examples/keyword-index.txt
/usr/src/kernels/3.10.0-1160.25.1.el7.x86_64/scripts/genksyms/keywords.gperf
/usr/src/kernels/3.10.0-1160.25.1.el7.x86_64/scripts/genksyms/keywords.hash.c
/usr/src/kernels/3.10.0-1160.25.1.el7.x86_64/scripts/genksyms/keywords.hash.c_shipped
/usr/src/kernels/3.10.0-1160.el7.x86_64/scripts/genksyms/keywords.gperf
/usr/src/kernels/3.10.0-1160.el7.x86_64/scripts/genksyms/keywords.hash.c
/usr/src/kernels/3.10.0-1160.el7.x86_64/scripts/genksyms/keywords.hash.c_shipped
/tmp locate -S
Database /var/lib/mlocate/mlocate.db:
	21,871 directories
	201,072 files
	10,587,933 bytes in file names
	4,773,354 bytes used to store database
/tmp updatedb
/tmp locate -S
Database /var/lib/mlocate/mlocate.db:
	21,901 directories
	201,476 files
	10,593,897 bytes in file names
	4,777,549 bytes used to store database
```

##### find

```text
范例一：将过去系统上面 24 小时内有更动过内容 （mtime） 的文件列出
/tmp find /etc -mtime 0
/etc
/etc/pki/nssdb
/etc/profile.d
/etc/ld.so.conf.d
/etc/ld.so.cache
/etc/alternatives
/etc/alternatives/pax
/etc/alternatives/pax-man
/etc/foomatic
/etc/redhat-lsb
/etc/lsb-release.d

范例二：寻找 /etc/cups/ 下面的文件，如果文件日期比 /etc/passwd 新就列出
/tmp find /etc/cups/ -newer /etc/passwd
/etc/cups/
/etc/cups/subscriptions.conf.O
/etc/cups/subscriptions.conf

范例三：搜寻 /home/moqi/.dbus 下面属于 dmtsai 的文件
/tmp find /home/moqi/.dbus -user moqi
/home/moqi/.dbus
/home/moqi/.dbus/session-bus
/home/moqi/.dbus/session-bus/e74863b8ab774b2cb2bc6f4a24973651-0
/home/moqi/.dbus/session-bus/e74863b8ab774b2cb2bc6f4a24973651-1

范例四：搜寻系统中不属于任何人的文件
/tmp find / -nouser
find: ‘/proc/24890/task/24890/fd/5’: No such file or directory
find: ‘/proc/24890/task/24890/fdinfo/5’: No such file or directory
find: ‘/proc/24890/fd/6’: No such file or directory
find: ‘/proc/24890/fdinfo/6’: No such file or directory
/root/.fzf/bin/fzf
/var/spool/mail/moqi1
/tmp/.ICE-unix/1880
/tmp/tracker-extract-files.1001
/tmp/.esd-1001
^C

范例五：找出文件名为 passwd 这个文件
/tmp find / -name passwd
/sys/fs/selinux/class/passwd
/sys/fs/selinux/class/passwd/perms/passwd
/etc/pam.d/passwd
/etc/passwd
/tmp/etc/pam.d/passwd
/tmp/etc/passwd
/usr/bin/passwd
/usr/share/bash-completion/completions/passwd
^C

范例五：找出文件名包含了 passwd 这个关键字的文件
/tmp find / -name "*passwd*"
/sys/fs/selinux/class/passwd
/sys/fs/selinux/class/passwd/perms/passwd
/etc/security/opasswd
/etc/pam.d/passwd
/etc/passwd
/etc/passwd-
/root/.tldr/cache/pages/linux/smbpasswd.md
/root/.tldr/cache/pages/linux/gpasswd.md
/root/.tldr/cache/pages/common/passwd.md

范例六：找出 /run 目录下，文件类型为 Socket 的文件名有哪些？
/tmp find /run -type s
/run/abrt/abrt.socket
/run/vmware/guestServicePipe
/run/gssproxy.sock
/run/chrony/chronyd.sock
/run/dbus/system_bus_socket
/run/rpcbind.sock
/run/avahi-daemon/socket
/run/libvirt/libvirt-admin-sock
/run/libvirt/libvirt-sock-ro
/run/libvirt/libvirt-sock
/run/libvirt/virtlogd-sock
/run/libvirt/virtlockd-sock
/run/cups/cups.sock
/run/user/42/pulse/native
/run/lsm/ipc/simc
/run/lsm/ipc/sim
/run/lvm/lvmpolld.socket
/run/lvm/lvmetad.socket
/run/udev/control
/run/systemd/shutdownd
/run/systemd/private
/run/systemd/journal/socket
/run/systemd/journal/stdout
/run/systemd/cgroups-agent
/run/systemd/notify

范例七：搜寻文件当中含有 SGID 或 SUID 或 SBIT 的属性
/tmp find / -perm /7000
/dev/mqueue
/dev/shm
find: ‘/proc/25091/task/25091/fd/5’: No such file or directory
find: ‘/proc/25091/task/25091/fdinfo/5’: No such file or directory
find: ‘/proc/25091/fd/6’: No such file or directory
find: ‘/proc/25091/fdinfo/6’: No such file or directory
/run/log/journal
/run/log/journal/e74863b8ab774b2cb2bc6f4a24973651
/var/tmp
/var/tmp/systemd-private-efa4e514bf3142b4ab078aab0b48d78e-chronyd.service-7raPJK/tmp
/var/tmp/systemd-private-efa4e514bf3142b4ab078aab0b48d78e-rtkit-daemon.service-ZH3Ni4/tmp
/var/tmp/systemd-private-efa4e514bf3142b4ab078aab0b48d78e-cups.service-jAqP0G/tmp
/var/tmp/systemd-private-efa4e514bf3142b4ab078aab0b48d78e-bolt.service-rL7r3D/tmp
/var/tmp/systemd-private-efa4e514bf3142b4ab078aab0b48d78e-colord.service-RLDmrT/tmp
/var/tmp/systemd-private-efa4e514bf3142b4ab078aab0b48d78e-fwupd.service-pcqOaI/tmp
^C

范例八：将上个范例找到的文件使用 ls -l 列出来～
/tmp find /usr/bin /usr/sbin -perm /7000 -exec ls -l {} \;
-rwsr-xr-x. 1 root root 32096 Oct 31  2018 /usr/bin/fusermount
-r-xr-sr-x. 1 root tty 15344 Jun 10  2014 /usr/bin/wall
-rwsr-xr-x. 1 root root 61320 Oct  1  2020 /usr/bin/ksu
-rws--x--x. 1 root root 23968 Feb  3 00:31 /usr/bin/chfn
-rws--x--x. 1 root root 23880 Feb  3 00:31 /usr/bin/chsh
-rwsr-xr-x. 1 root root 27856 Apr  1  2020 /usr/bin/passwd
-rwsr-xr-x. 1 root root 32128 Feb  3 00:31 /usr/bin/su
-rwsr-xr-x. 1 root root 73888 Aug  9  2019 /usr/bin/chage
-rwsr-xr-x. 1 root root 78408 Aug  9  2019 /usr/bin/gpasswd
-rwsr-xr-x. 1 root root 41936 Aug  9  2019 /usr/bin/newgrp
-rwsr-xr-x. 1 root root 44264 Feb  3 00:31 /usr/bin/mount
-rwsr-xr-x. 1 root root 23576 Apr  1  2020 /usr/bin/pkexec
-rwsr-xr-x. 1 root root 31984 Feb  3 00:31 /usr/bin/umount
...

范例九：找出系统中，大于 1MB 的文件
/tmp find / -size +1M
/boot/efi/EFI/centos/MokManager.efi
/boot/efi/EFI/centos/mmx64.efi
/boot/efi/EFI/centos/shim.efi
/boot/efi/EFI/centos/shimx64-centos.efi
/boot/efi/EFI/centos/shimx64.efi
/boot/efi/EFI/BOOT/BOOTX64.EFI
/boot/grub2/fonts/unicode.pf2
/boot/System.map-3.10.0-1160.el7.x86_64
/boot/vmlinuz-3.10.0-1160.el7.x86_64
/boot/initramfs-0-rescue-e74863b8ab774b2cb2bc6f4a24973651.img
/boot/vmlinuz-0-rescue-e74863b8ab774b2cb2bc6f4a24973651
/boot/System.map-3.10.0-1160.25.1.el7.x86_64
/boot/vmlinuz-3.10.0-1160.25.1.el7.x86_64
/boot/initramfs-3.10.0-1160.25.1.el7.x86_64.img
/boot/initramfs-3.10.0-1160.el7.x86_64.img
/proc/kcore
find: ‘/proc/25248/task/25248/fd/5’: No such file or directory
find: ‘/proc/25248/task/25248/fdinfo/5’: No such file or directory
find: ‘/proc/25248/fd/6’: No such file or directory
find: ‘/proc/25248/fdinfo/6’: No such file or directory
^C
```

##### uuidgen

```text
~/test tldr uuidgen

  uuidgen

  Generate unique identifiers (UUIDs).
  See also uuid.
  More information: https://manned.org/uuidgen.

  - Create a random UUIDv4:
    uuidgen --random

  - Create a UUIDv1 based on the current time:
    uuidgen --time

  - Create a UUIDv5 of the name with a specified namespace prefix:
    uuidgen --sha1 --namespace @dns|@url|@oid|@x500 --name object_name


See also: uuid

~/test uuidgen
b0ed356a-d435-411a-bb8f-2675f105a09f
~/test uuidgen
4d960056-6894-4849-a0a6-905150a16eee
~/test uuidgen -t
68e04460-cacd-11eb-9b18-000c296448c5
~/test uuidgen -t
69c56536-cacd-11eb-ad57-000c296448c5
```
