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
* ctrl+u/ctrl+k: 分别是从光标处向前删除指令串 （ctrl+u） 及向后删除指令串 （ctrl+k）。
* ctrl+a/ctrl+e: 分别是让光标移动到整个指令串的最前面 （ctrl+a） 或最后面 （ctrl+e）。
* $:(关于本 shell 的 PID), 想要知道我们的 shell 的PID，就可以用 echo $$ 即可！出现的数字就是你的 PID 号码。
* ?:(关于上个执行指令的回传值), 一般来说，如果成功的执行该指令，则会回传一个 0 值，如果执行过程发生错误，就会回传"错误代码"才对！一般就是以非为 0 的数值来取代。
* bash 的进站与欢迎讯息：/etc/issue, /etc/motd
* cmd1 && cmd2: 1. 若 cmd1 执行完毕且正确执行（$?=0），则开始执行 cmd2。 2. 若 cmd1 执行完毕且为错误 （$?≠0），则 cmd2 不执行。
* cmd1 || cmd2: 1. 若 cmd1 执行完毕且正确执行（$?=0），则 cmd2 不执行。 2. 若 cmd1 执 行完毕且为错误 （$?≠0），则开始执行 cmd2。
* 管线命令（pipe）: 1. 管线命令仅会处理 standard output，对于 standard error output 会予以忽略 2. 管线命令必须要能够接受来自前一个指令的数据成为 standard input 继续处理才行。
* 虽然 col 有他特殊的用途，不过，很多时候，他可以用来简单的处理将 tab 按键取代成为空白键！例如：cat /etc/man_db.conf | col -x


### 正则表达式

##### 正则表达式与万用字符区别

再次强调:“正则表达式的特殊字符”与一般在命令行输入指令的“万用字符”并不相同， 例如， 在万用字符当中的 代表的是“ 0 ~ 无限多个字符”的意思，但是在正则表达式当中， 则是“重复 0 到无穷多个的前一个 RE 字符”的意思~使用的意义并不相同，不要搞混了!
举例来说，不支持正则表达式的 ls 这个工具中，若我们使用 “ls -l ” 代表的是任意文件名的文 件，而 “ls -l a ”代表的是以 a 为开头的任何文件名的文件， 但在正则表达式中，我们要找到 含有以 a 为开头的文件，则必须要这样:(需搭配支持正则表达式的工具)
`ls | grep -n '^a.*'`
例题:以 ls -l 配合 grep 找出 /etc/ 下面文件类型为链接文件属性的文件名答:由于 ls -l 列出 链接文件时标头会是“ lrwxrwxrwx ”，因此使用如下的指令即可找出结果:
`> ls -l /etc | grep '^l'`
若仅想要列出几个文件，再以“ |wc -l ” 来累加处理即可。

##### 延伸正则表达式

```text
grep -v '^$' regular_express.txt | grep -v '^#'
=====>
egrep -v '^$|^#' regular_express.txt
```


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
    * To remove the highlighting of matches enter:   :nohlsearch or :set nohls
    * If you want to ignore case for just one search command, use \c in the phrase:  /ignore\c  <ENTER>
* When typing a  :  command, press CTRL-D to see possible completions. Press <TAB> to use one completion.
  
#### 杂项

* [Copying and Moving Text -- Yank, Delete, and Put](https://docs.oracle.com/cd/E19455-01/806-2902/editorvi-53/index.html): Many word-processors allow you to "copy and paste" and "cut and paste" lines of text. The vi editor also includes these features. The vi command-mode equivalent of "copy and paste" is yank and put; the equivalent of "cut and paste" is delete and put. The methods for copying or moving small blocks of text in vi involves using a combination of the yank, delete, and put commands.
* [How do you search through Vim's command history?](https://stackoverflow.com/a/50283189): Press Ctrl+F in command mode to open the command history window. Then, you can use `/` , `?` , and other search commands. Press Enter to execute a command from the history. For more about the command history window, see [:h cmdwin](https://vimhelp.org/cmdline.txt.html) .
* [What is Vim recording and how can it be disabled?](https://stackoverflow.com/a/1527824): You start recording by `q``<letter>` and you can end it by typing `q` again. Recording is a really useful feature of Vim. It records everything you type. You can then replay it simply by typing `@``<letter>`. Record search, movement, replacement... One of the best feature of Vim IMHO.

#### 多文件编辑

一般结合 vim 第一步打开多个文件使用：`vim hhhh.txt aaaaa.txt`

* :n 编辑下一个文件
* :N 编辑上一个文件
* :files 列出目前这个 vim 的打开的所有文件

#### 多窗口功能

sp 为水平分割，vs 为垂直分割，详情参考 [vimhelp: window](https://vimhelp.org/windows.txt.html#window).

* :sp \[filename\] 打开一个新窗口，如果有加 filename，表示在新窗口打开一个新文件，否则表示两个窗口为同一个文件内容（同步显示）。
* ctrl+w+j 或 ctrl+w+↓ 按键的按法是：先按下 ctrl 不放，再按下 w 后放开所有的按键，然后再按下 j （或向下方向键），则光标可移动到下方的窗口。
* ctrl+w+k 或 ctrl+w+↑ 同上，不过光标移动到上面的窗口。
* ctrl+w+q 其实就是 :q 结束离开啦！举例来说，如果我想要结束下方的窗口，那么利用 ctrl+w+↓ 移动到下方窗口后，按下 :q 即可离开，也可以按下 ctrl+w+q 啊！

#### vim 的文字补全功能

试了一下不好用，应该要结合编程方言设定，频繁报错 `option omnifunc is not set`

* ctrl+x -> ctrl+n 通过目前正在编辑的这个"文件的内容文字"作为关键字，予以补齐
* ctrl+x -> ctrl+f 以当前目录内的"文件名"作为关键字，予以补齐
* ctrl+x -> ctrl+o 以扩展名作为语法补充，以 vim 内置的关键字，予以补齐

#### vim 的环境设置参数

* :set backup 是否自动储存备份文件？一般是 nobackup 的，如果设置 backup 的话，那么当你更动任何一个文件时，则原始文件会被另存成一个文件名为 filename~ 的文件。举例来说，我们编辑 hosts，设置 :setbackup，那么当更动 hosts 时，在同目录下，就会产生 hosts~ 文件名的文件，记录原始的 hosts 文件内容。
* :set backspace=(012) 一般来说，如果我们按下 i 进入编辑模式后，可以利用倒退键（backspace）来删除任意字符的。但是，某些 distribution 则不许如此。此时，我们就可以通过 backspace 来设置啰～当 backspace 为 2 时，就是可以删除任意值；0 或 1 时，仅可删除刚刚输入的字符，而无法删除原本就已经存在的文字了！

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

##### RANDOM

在 BASH 的环境下 RANDOM 变量的内容介于 0~32767 之间。

```text
~ echo $RANDOM
2066
~ echo $RANDOM
2400
~ echo $RANDOM
8550
~ echo $RANDOM
29081
~ echo $RANDOM
12267
```

##### source

读入环境配置文件的指令，利用 source 或小数点（.）都可以将配置文件的内容读进来目前的 shell 环境中！

```text
~ vim .zshrc
~ . .zshrc
.: no such file or directory: .zshrc
~ . ~/.zshrc
~ echo $assss
asdfasdf
```

##### 万用字符与特殊符号

* &: 工作控制（job control）：将指令变成背景下工作
* \>, >>: 数据流重导向：输出导向，分别是"取代"与"累加"
* <, <<: 数据流重导向：输入导向
* '': 单引号，不具有变量置换的功能（$ 变为纯文本）
* "": 具有变量置换的功能！（$ 可保留相关功能）
* \`: 两个"\`"中间为可以先执行的指令，亦可使用 $()
* (): 在中间为子 shell 的起始与结束
* {}: 在中间为命令区块的组合！

```text
# 范例一：找出 /etc/ 下面以 cron 为开头的文件名
~ ll -d /etc/cron*
drwxr-xr-x. 2 root root  54 Jun  1 00:11 /etc/cron.d
...

# 范例二：找出 /etc/ 下面文件名“刚好是五个字母”的文件名
~ ll -d /etc/?????
drwxr-x---. 3 root root   83 Jun  1 00:05 /etc/audit
...

# 范例三：找出 /etc/ 下面文件名含有数字的文件名
~ ll -d /etc/*[0-9]*
-rw-r--r--. 1 root root 5.6K Nov 16  2020 /etc/DIR_COLORS.256color
...

# 范例四：找出 /etc/ 下面，文件名开头非为小写字母的文件名：
~ ll -d /etc/[^a-z]*
-rw-r--r--. 1 root root 5.0K Nov 16  2020 /etc/DIR_COLORS
...

# 范例五：将范例四找到的文件复制到 /tmp/upper 中
~ mkdir /tmp/upper; cp -a /etc/[^a-z]* /tmp/upper
~ ll -d /tmp/upper
drwxr-xr-x. 6 root root 206 Jun 18 23:33 /tmp/upper
~ ll -d /tmp/upper/*
-rw-r--r--. 1 root root 5.0K Nov 16  2020 /tmp/upper/DIR_COLORS
...
```

##### cut

这个指令可以将一段讯息的某一段给他“切”出来～ 处理的讯息是 以“行”为单位喔！

```text
~ echo ${PATH}
/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# 范例一：将 PATH 变量取出，我要找出第五个路径。
~ echo ${PATH} | cut -d ':' -f 5
/usr/sbin

# 那么如果想要列出第 3 与第 5 呢？，就是这样：
~ echo ${PATH} | cut -d ':' -f 3,5
/sbin:/usr/sbin

~ last
root     pts/2        moqi-13mbp       Fri Jun 18 23:30   still logged in
root     pts/2        moqi-13mbp       Fri Jun 18 23:11 - 23:27  (00:16)
root     pts/1        moqi-13mbp       Fri Jun 18 22:59 - 23:31  (00:31)

# 范例三：用 last 将显示的登陆者的信息中，仅留下使用者大名
~ last | cut -d ' ' -f 1
root
root
root
```

##### sort

```text
# 范例一：个人帐号都记录在 /etc/passwd 下，请将帐号进行排序。
~ cat /etc/passwd | sort
abrt:x:173:173::/etc/abrt:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
bin:x:1:1:bin:/bin:/sbin/nologin
chrony:x:992:987::/var/lib/chrony:/sbin/nologin
...

# 范例二：/etc/passwd 内容是以 : 来分隔的，我想以第三栏来排序，该如何？
~ cat /etc/passwd | sort -t ':' -k 3
root:x:0:0:root:/root:/usr/local/bin/zsh
moqi:x:1000:1000:centos7:/home/moqi:/bin/bash
qemu:x:107:107:qemu user:/:/sbin/nologin
usbmuxd:x:113:113:usbmuxd user:/:/sbin/nologin
...

# 范例三：利用 last ，将输出的数据仅取帐号，并加以排序
~ last | cut -d ' ' -f 1 | sort

moqi
moqi
...
```

##### uniq

```text
# 范例一：使用 last 将帐号列出，仅取出帐号栏，进行排序后仅取出一位；
~ last | cut -d ' ' -f 1 | sort | uniq

moqi
moqi1
reboot
root
wtmp
~

# 范例二：承上题，如果我还想要知道每个人的登陆总次数呢？
~ last | cut -d ' ' -f 1 | sort | uniq -c
      1
     40 moqi
      2 moqi1
     12 reboot
     20 root
      1 wtmp
```

##### wc

```text
# 范例一：那个 /etc/man_db.conf 里面到底有多少相关字、行、字符数？
~ cat /etc/man_db.conf| wc
    131     723    5171
# 输出的三个数字中，分别代表： “行、字数、字符数”

~ cat /etc/man_db.conf| wc -l
131
~ cat /etc/man_db.conf| wc -w
723
~ cat /etc/man_db.conf| wc -m
5171
```

##### 双向重导向: tee

tee 会同时将数据流分送到文件去与屏幕(screen); 而输出到屏幕的，其实就是 stdout, 那就可以让下个指令继续处理喔！

```text
# 这个范例则是将 ls 的数据存一份到 ~/homefile ，同时屏幕也有输出讯息！
~ ls -l /home | tee ~/homefile | more
total 8
drwx------. 15 moqi moqi 4096 Jun  5 21:12 moqi
drwx--x--x. 15 1001 1001 4096 Jun  5 09:37 moqi1

# 要注意！ tee 后接的文件会被覆盖，若加上 -a 这个选项则能将讯息累加。
~ ls -l /home | tee -a ~/homefile | more
total 8
drwx------. 15 moqi moqi 4096 Jun  5 21:12 moqi
drwx--x--x. 15 1001 1001 4096 Jun  5 09:37 moqi1

~ cat homefile
total 8
drwx------. 15 moqi moqi 4096 Jun  5 21:12 moqi
drwx--x--x. 15 1001 1001 4096 Jun  5 09:37 moqi1
total 8
drwx------. 15 moqi moqi 4096 Jun  5 21:12 moqi
drwx--x--x. 15 1001 1001 4096 Jun  5 09:37 moqi1
```

##### tr

tr 可以用来删除一段讯息当中的文字，或者是进行文字讯息的替换！

```text
# 范例一：将 last 输出的讯息中，所有的小写变成大写字符：
~ last | tr '[a-z]' '[A-Z]'
ROOT     PTS/0        MOQI-13MBP       SAT JUN 19 14:27   STILL LOGGED IN
ROOT     PTS/2        MOQI-13MBP       FRI JUN 18 23:30   STILL LOGGED IN
ROOT     PTS/2        MOQI-13MBP       FRI JUN 18 23:11 - 23:27  (00:16)
ROOT     PTS/1        MOQI-13MBP       FRI JUN 18 22:59 - 23:31  (00:31)
...

# 范例二：将 /etc/passwd 输出的讯息中，将冒号(:)删除
~ cat /etc/passwd | tr -d ':'
rootx00root/root/usr/local/bin/zsh
binx11bin/bin/sbin/nologin
daemonx22daemon/sbin/sbin/nologin
admx34adm/var/adm/sbin/nologin
lpx47lp/var/spool/lpd/sbin/nologin
syncx50sync/sbin/bin/sync
...
```

##### join

join 看字面上的意义（加入/参加）就可以知道，他是在处理两个文件之间的数据， 而且，主要是在处理“两个文件当中，有"相同数据"的那一行，才将他加在一起”的意思。

```text
~ head -n 3 /etc/passwd /etc/shadow
==> /etc/passwd <==
root:x:0:0:root:/root:/usr/local/bin/zsh
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin

==> /etc/shadow <==
root:$1$lnW3mJMQ$pYVFKu7KTxcPxy8JfJ5rD.::0:99999:7:::
bin:*:18353:0:99999:7:::
daemon:*:18353:0:99999:7:::

# 范例一：用 root 的身份，将 /etc/passwd 与 /etc/shadow 相关数据整合成一栏
~ join -t ':' /etc/passwd /etc/shadow | head -n 3
root:x:0:0:root:/root:/usr/local/bin/zsh:$1$lnW3mJMQ$pYVFKu7KTxcPxy8JfJ5rD.::0:99999:7:::
bin:x:1:1:bin:/bin:/sbin/nologin:*:18353:0:99999:7:::
daemon:x:2:2:daemon:/sbin:/sbin/nologin:*:18353:0:99999:7:::

# 同样的，相同的字段部分被移动到最前面了！所以第二个文件的内容就没再显示。
~ join -t ':' -1 4 /etc/passwd -2 3 /etc/group | head -n 3
join: /etc/passwd:6: is not sorted: sync:x:5:0:sync:/sbin:/bin/sync
join: /etc/group:11: is not sorted: wheel:x:10:moqi
0:root:x:0:root:/root:/usr/local/bin/zsh:root:x:
1:bin:x:1:bin:/bin:/sbin/nologin:bin:x:
2:daemon:x:2:daemon:/sbin:/sbin/nologin:daemon:x:
```

##### 分区命令: split

在 Linux 下面就简单的多了！你要将文件分区的话，那么就使用 -b size 来将一个分区的文件限制其大小，如果是行数的话，那么就使用 -l line 来分区！好用的很！如此一来，你就可以轻易的将你的文件分区成某些软件能够支持的最大容量（例如 gmail 单一信件 25MB 之类的！），方便你 copy 啰！

```text
~ cd /tmp

# 范例一：我的 /etc/services 有六百多K，若想要分成 300K 一个文件时？
/tmp split -b 300k /etc/services services
/tmp ll -k services*
-rw-r--r--. 1 root root 300K Jun 19 15:15 servicesaa
-rw-r--r--. 1 root root 300K Jun 19 15:15 servicesab
-rw-r--r--. 1 root root  55K Jun 19 15:15 servicesac
# 那个文件名可以随意取的啦！我们只要写上前导文字，小文件就会以 
# xxxaa, xxxab, xxxac 等方式来创建小文件的！

# 范例二：如何将上面的三个小文件合成一个文件，文件名为 servicesback
/tmp cat service* >> servicesback
# 很简单吧？就用数据流重导向就好啦！简单！

/tmp ll -k services*
-rw-r--r--. 1 root root 300K Jun 19 15:15 servicesaa
-rw-r--r--. 1 root root 300K Jun 19 15:15 servicesab
-rw-r--r--. 1 root root  55K Jun 19 15:15 servicesac
-rw-r--r--. 1 root root 655K Jun 19 15:16 servicesback

# 范例三：使用 ls -al / 输出的信息中，每十行记录成一个文件
/tmp ls -al / | split -l 10 - lsroot
/tmp wc -l lsroot*
  10 lsrootaa
  10 lsrootab
   3 lsrootac
  23 total
# 重点在那个 - 啦！一般来说，如果需要 stdout/stdin 时，但偏偏又没有文件， 
# 有的只是 - 时，那么那个 - 就会被当成 stdin 或 stdout ～
```

##### 参数代换: xargs

```text
范例一：将 /etc/passwd 内的第一栏取出，仅取三行，使用 id 这个指令将每个帐号内容秀出来 
~ id root 
uid=0（root） gid=0（root） groups=0（root） 
# 这个 id 指令可以查询使用者的 UID/GID 等信息

~ id $（cut -d ':' -f 1 /etc/passwd | head -n 3） 
# 虽然使用 $（cmd） 可以预先取得参数，但可惜的是， id 这个指令“仅”能接受一个参数而已！ 
# 所以上述的这个指令执行会出现错误！根本不会显示用户的 ID 啊！

~ cut -d ':' -f 1 /etc/passwd | head -n 3 | id 
uid=1000（dmtsai） gid=1000（dmtsai） groups=1000（dmtsai）,10（wheel） 
# 我不是要查自己啊！ 
# 因为 id 并不是管线命令，因此在上面这个指令执行后，前面的东西通通不见！只会执行 id！

~ cut -d ':' -f 1 /etc/passwd | head -n 3 | xargs id 
# 依旧会出现错误！这是因为 xargs 一口气将全部的数据通通丢给 id 处理～但 id 就接受 1 个啊最多！

~ cut -d ':' -f 1 /etc/passwd | head -n 3 | xargs -n 1 id 
uid=0（root） gid=0（root） groups=0（root） 
uid=1（bin） gid=1（bin） groups=1（bin） 
uid=2（daemon） gid=2（daemon） groups=2（daemon） 
# 通过 -n 来处理，一次给予一个参数，因此上述的结果就 OK 正常的显示啰！

范例二：同上，但是每次执行 id 时，都要询问使用者是否动作？ 
~ cut -d ':' -f 1 /etc/passwd | head -n 3 | xargs -p -n 1 id 
id root ?...y 
uid=0（root） gid=0（root） groups=0（root）
id bin ?...y 
.....（下面省略）.....
# 呵呵！这个 -p 的选项可以让使用者的使用过程中，被询问到每个指令是否执行！

范例三：将所有的 /etc/passwd 内的帐号都以 id 查阅，但查到 sync 就结束指令串 
~ cut -d ':' -f 1 /etc/passwd | xargs -e'sync' -n 1 id 
# 仔细与上面的案例做比较。也同时注意，那个 -e'sync' 是连在一起的，中间没有空白键。 
# 上个例子当中，第六个参数是 sync 啊，那么我们下达 -e'sync' 后，则分析到 sync 这个字串时， 
# 后面的其他 stdin 的内容就会被 xargs 舍弃掉了！

# 范例四：找出 /usr/sbin 下面具有特殊权限的文件名，并使用 ls -l 列出详细属性
/tmp find /usr/sbin -perm /7000 | xargs ls -l
-rwx--s--x. 1 root lock      11208 Jun 10  2014 /usr/sbin/lockdev
-rwsr-xr-x. 1 root root     117432 Oct  1  2020 /usr/sbin/mount.nfs
-rwxr-sr-x. 1 root root      11224 Nov 17  2020 /usr/sbin/netreport
-rwsr-xr-x. 1 root root      11232 Apr  1  2020 /usr/sbin/pam_timestamp_check
-rwxr-sr-x. 1 root postdrop 218560 Apr  1  2020 /usr/sbin/postdrop
-rwxr-sr-x. 1 root postdrop 264128 Apr  1  2020 /usr/sbin/postqueue
-rwsr-xr-x. 1 root root      36272 Apr  1  2020 /usr/sbin/unix_chkpwd
-rws--x--x. 1 root root      40328 Aug  9  2019 /usr/sbin/userhelper
-rwsr-xr-x. 1 root root      11296 Nov 17  2020 /usr/sbin/usernetctl

# 聪明的读者应该会想到使用“ ls -l $（find /usr/sbin -perm /7000） ”来处理这个范例！ 
# 都 OK！能解决问题的方法，就是好方法！
/tmp ls -l $(find /usr/sbin -perm /7000)
-rwx--s--x. 1 root lock      11208 Jun 10  2014 /usr/sbin/lockdev
-rwsr-xr-x. 1 root root     117432 Oct  1  2020 /usr/sbin/mount.nfs
-rwxr-sr-x. 1 root root      11224 Nov 17  2020 /usr/sbin/netreport
-rwsr-xr-x. 1 root root      11232 Apr  1  2020 /usr/sbin/pam_timestamp_check
-rwxr-sr-x. 1 root postdrop 218560 Apr  1  2020 /usr/sbin/postdrop
-rwxr-sr-x. 1 root postdrop 264128 Apr  1  2020 /usr/sbin/postqueue
-rwsr-xr-x. 1 root root      36272 Apr  1  2020 /usr/sbin/unix_chkpwd
-rws--x--x. 1 root root      40328 Aug  9  2019 /usr/sbin/userhelper
-rwsr-xr-x. 1 root root      11296 Nov 17  2020 /usr/sbin/usernetctl
```

##### 关于减号 - 的用途

管线命令在 bash 的连续的处理程序中是相当重要的！另外，在 log file 的分析当中也是相当重要的一环，所以请特别留意！另外，在管线命令当中，常常会使用到前一个指令的 stdout 作为这次的 stdin，某些指令需要用到文件名称（例如 tar）来进行处理时，该 stdin 与 stdout 可以利用减号"-"来替代，举例来说：

```text
[root@study ~]# mkdir /tmp/homeback 
[root@study ~]# tar -cvf - /home &#124; tar -xvf - -C /tmp/homeback
```

上面这个例子是说：“我将 /home 里面的文件给他打包，但打包的数据不是纪录到文件，而是传送到 stdout；经过管线后，将 tar -cvf - /home 传送给后面的 tar -xvf - ”。后面的这个 - 则是取用前一个指令的 stdout，因此，我们就不需要使用 filename 了！这是很常见的例子喔！注意注意！

##### sed

sed 本身也是一个管线命令，可以分析 standard input 的啦！而且 sed 还可以将数据进行取代、删除、新增、撷取特定行等等的功能呢！

```text
# 范例一：将 /etc/passwd 的内容列出并且打印行号，同时，请将第 2~5 行删除！
~ nl /etc/passwd | sed '2,5d'
     1	root:x:0:0:root:/root:/usr/local/bin/zsh
     6	sync:x:5:0:sync:/sbin:/bin/sync
     7	shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
     8	halt:x:7:0:halt:/sbin:/sbin/halt
     9	mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    10	operator:x:11:0:operator:/root:/sbin/nologin
    11	games:x:12:100:games:/usr/games:/sbin/nologin
...
# 如果题型变化一下，举例来说，如果只要删除第 2 行，可以使用“ nl /etc/passwd | sed '2d' ”来 达成，
# 至于若是要删除第 3 到最后一行，则是“ nl /etc/passwd | sed '3,$d' ”的啦，那个钱字 号“ $ ”代表最后一行！

# 范例二：承上题，在第二行后（亦即是加在第三行）加上“drink tea?”字样！
~ nl /etc/passwd | sed '2a drink tea' | head -n 5
     1	root:x:0:0:root:/root:/usr/local/bin/zsh
     2	bin:x:1:1:bin:/bin:/sbin/nologin
drink tea
     3	daemon:x:2:2:daemon:/sbin:/sbin/nologin
     4	adm:x:3:4:adm:/var/adm:/sbin/nologin
# 在 a 后面加上的字串就已将出现在第二行后面啰！那如果是要在第二行前呢？
# nl /etc/passwd | sed '2i drink tea' 就对啦！就是将“ a ”变成“ i ”即可。

# 范例三：在第二行后面加入两行字，例如“Drink tea or .....”与“drink beer?”
~ nl /etc/passwd | sed '2a drink tea or .....\
pipe quote> drink beer ?' | head -n 10
     1	root:x:0:0:root:/root:/usr/local/bin/zsh
     2	bin:x:1:1:bin:/bin:/sbin/nologin
drink tea or .....
drink beer ?
     3	daemon:x:2:2:daemon:/sbin:/sbin/nologin
     4	adm:x:3:4:adm:/var/adm:/sbin/nologin
     5	lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
     6	sync:x:5:0:sync:/sbin:/bin/sync
     7	shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
     8	halt:x:7:0:halt:/sbin:/sbin/halt
     
# 范例四：我想将第2-5行的内容取代成为“No 2-5 number”呢？
~ nl /etc/passwd | sed '2,5c No 2-5 number' | head -n 10
     1	root:x:0:0:root:/root:/usr/local/bin/zsh
No 2-5 number
     6	sync:x:5:0:sync:/sbin:/bin/sync
     7	shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
     8	halt:x:7:0:halt:/sbin:/sbin/halt
     9	mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    10	operator:x:11:0:operator:/root:/sbin/nologin
    11	games:x:12:100:games:/usr/games:/sbin/nologin
    12	ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    13	nobody:x:99:99:Nobody:/:/sbin/nologin
    
# 范例五：仅列出 /etc/passwd 文件内的第 5-7 行
~ nl /etc/passwd | sed -n '5,7p'
     5	lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
     6	sync:x:5:0:sync:/sbin:/bin/sync
     7	shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown

# 获取网络配置
~ /sbin/ifconfig ens33
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.50.200  netmask 255.255.255.0  broadcast 192.168.50.255
        inet6 fe80::20c:29ff:fe64:48c5  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:64:48:c5  txqueuelen 1000  (Ethernet)
        RX packets 316235  bytes 131479504 (125.3 MiB)
        RX errors 0  dropped 42  overruns 0  frame 0
        TX packets 114444  bytes 18101352 (17.2 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

# 过滤 inet 行
~ /sbin/ifconfig ens33 | grep 'inet '
        inet 192.168.50.200  netmask 255.255.255.0  broadcast 192.168.50.255

# 替换 IP 前面为空
~ /sbin/ifconfig ens33 | grep 'inet ' | sed 's/^.*inet //g'
192.168.50.200  netmask 255.255.255.0  broadcast 192.168.50.255

# 替换 IP 后面为空
~ /sbin/ifconfig ens33 | grep 'inet ' | sed 's/^.*inet //g' | sed 's/ *netmask.*$//g'
192.168.50.200

# 范例六：利用 sed 将 regular_express.txt 内每一行结尾若为 . 则换成 !
~ sed -i 's/\.$/\!/g' regular_express.txt
~ cat regular_express.txt
"Open Source" is a good mechanism to develop programs!
apple is my favorite food!
Football game is not use feet only!
this dress doesn't fit me!
However, this dress is about $ 3183 dollars.
GNU is free air not free beer.
Her hair is very beauty.
I can't finish the test.
Oh! The soup taste good.
motorcycle is cheap than car!
This window is clear!
the symbol '*' is represented as start!
Oh!	My god!
The gd software is a library for drafting programs.
You are the best is mean you are the no. 1!
The world <Happy> is the same with "glad"!
I like dog!
google is the best tools for search keyword!
goooooogle yes!
go! go! Let's go!
# I am VBird

# 范例七：利用 sed 直接在 regular_express.txt 最后一行加入“# This is a test”
~ sed -i '$a #This is a test' regular_express.txt
~ cat regular_express.txt
"Open Source" is a good mechanism to develop programs!
apple is my favorite food!
Football game is not use feet only!
this dress doesn't fit me!
However, this dress is about $ 3183 dollars.
GNU is free air not free beer.
Her hair is very beauty.
I can't finish the test.
Oh! The soup taste good.
motorcycle is cheap than car!
This window is clear!
the symbol '*' is represented as start!
Oh!	My god!
The gd software is a library for drafting programs.
You are the best is mean you are the no. 1!
The world <Happy> is the same with "glad"!
I like dog!
google is the best tools for search keyword!
goooooogle yes!
go! go! Let's go!
# I am VBird

#This is a test

# sed 的“ -i ”选项可以直接修改文件内容，这功能非常有帮助！举例来说，如果你有一个 100 万行的文件，你要在第 100 行加某些文字，
# 此时使用 vim 可能会疯掉！因为文件太大了！那怎办？就利用 sed 啊！通过 sed 直接修改/取代的功能，你甚至不需要使用 vim 去修订！
```

##### awk: 好用的数据处理工具


```text
# awk 通常运行的模式是这样的：
awk '条件类型1{动作1} 条件类型2{动作2} ...' filename

# 仅取出前五行
~ last -n 5
root     pts/1        moqi-13mbp       Sun Jun 20 10:30   still logged in
root     pts/0        moqi-13mbp       Sat Jun 19 23:51   still logged in
root     pts/0        moqi-13mbp       Sat Jun 19 14:27 - 23:17  (08:50)
root     pts/2        moqi-13mbp       Fri Jun 18 23:30 - 19:20  (19:49)
root     pts/2        moqi-13mbp       Fri Jun 18 23:11 - 23:27  (00:16)

wtmp begins Tue Jun  1 00:05:25 2021

# 若我想要取出帐号与登陆者的 IP，且帐号与 IP 之间以 [tab] 隔开，则会变成这样
~ last -n 5 | awk '{print $1 "\t" $3}'
root	moqi-13mbp
root	moqi-13mbp
root	moqi-13mbp
root	moqi-13mbp
root	moqi-13mbp

wtmp	Tue

# 那么 awk 怎么知道我到底这个数据有几行？
# NF: 每一行 （$0） 拥有的字段总数
# NR: 目前 awk 所处理的是“第几行”数据
# FS: 目前的分隔字符，默认是空白键

# 注意喔，在 awk 内的 NR, NF 等变量要用大写，不需要有钱字号 $ 啦！
~ last -n 5 | awk '{print $1 "\t lines: " NR "\t columns: " NF}'
root	 lines: 1	 columns: 10
root	 lines: 2	 columns: 10
root	 lines: 3	 columns: 10
root	 lines: 4	 columns: 10
root	 lines: 5	 columns: 10
	 lines: 6	 columns: 0
wtmp	 lines: 7	 columns: 7

# 第三栏小于 10 以下的数据，并且仅列出帐号与第三栏， 那么可以这样做
~ cat /etc/passwd | awk '{FS=":"} $3 < 10 {print $1 "\t " $3}'
root:x:0:0:root:/root:/usr/local/bin/zsh
bin	 1
daemon	 2
adm	 3
lp	 4
sync	 5
shutdown	 6
halt	 7
mail	 8

# 可以预先设置 awk 的变量啊！ 利用 BEGIN 这个关键字
~ cat /etc/passwd | awk 'BEGIN {FS=":"} $3 < 10 {print $1 "\t " $3}'
root	 0
bin	 1
daemon	 2
adm	 3
lp	 4
sync	 5
shutdown	 6
halt	 7
mail	 8
```

* awk 的指令间隔：所有 awk 的动作，亦即在 {} 内的动作，如果有需要多个指令辅助时，可利用分号“;”间隔，或者直接以回车按键来隔开每个指令。
* 逻辑运算当中，如果是“等于”的情况，则务必使用两个等号“==”！
* 格式化输出时，在 printf 的格式设置当中，务必加上 \n，才能进行分行！
* 与 bash shell 的变量不同，在 awk 当中，变量可以直接使用，不需加上 $ 符号。

##### 文件比对工具: diff

diff 就是用在比对两个文件之间的差异的，并且是以行为单位来比对的！

```text
# 范例一：比对 passwd.old 与 passwd.new 的差异：
/tmp/testpw diff passwd.old passwd.new
# 左边第四行被删除 （d） 掉了，基准是右边的第三行
4d3
# 这边列出左边文件被删除的那一行内容
< adm:x:3:4:adm:/var/adm:/sbin/nologin
# 左边文件的第六行被改变 (c) 成右边文件的第五行
6c5
# 左边文件第六行内容
< sync:x:5:0:sync:/sbin:/bin/sync
---
# 右边文件第五行内容
> no six line

# 可以将两个目录比对一下
/tmp/testpw diff /etc/rc0.d /etc/rc5.d
Only in /etc/rc0.d: K90network
Only in /etc/rc5.d: S10network
```

##### 文件比对工具: cmp

cmp 主要也是在比对两个文件，他主要利用“字节”单位去比对，因此，当然也可以比对 binary file.

```text
/tmp/testpw cmp passwd.old passwd.new
passwd.old passwd.new differ: char 115, line 4
```

##### patch

先比较先旧版本的差异，并将差异档制作成为补丁文件，再由补丁文件更新旧文件。

```text
# 范例一：以 /tmp/testpw 内的 passwd.old 与 passwd.new 制作补丁文件
/tmp/testpw diff -Naur passwd.old passwd.new > passwd.patch
/tmp/testpw cat passwd.patch
# 新旧文件的信息
--- passwd.old	2021-06-20 10:56:18.354023751 +0800
+++ passwd.new	2021-06-20 10:56:49.416751063 +0800
# 新旧文件要修改数据的界定范围，旧文件在 1-9 行，新文件在 1-8 行
@@ -1,9 +1,8 @@
 root:x:0:0:root:/root:/usr/local/bin/zsh
 bin:x:1:1:bin:/bin:/sbin/nologin
 daemon:x:2:2:daemon:/sbin:/sbin/nologin
# 左侧文件删除
-adm:x:3:4:adm:/var/adm:/sbin/nologin
 lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
# 左侧文件删除
-sync:x:5:0:sync:/sbin:/bin/sync
# 右侧新文件加入
+no six line
 shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
 halt:x:7:0:halt:/sbin:/sbin/halt
 mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
 
# 范例二：将刚刚制作出来的 patch file 用来更新旧版数据
/tmp/testpw patch -p0 < passwd.patch
patching file passwd.old
/tmp/testpw ll passwd*
-rw-r--r--. 1 root root 2.3K Jun 20 10:56 passwd.new
-rw-r--r--. 1 root root 2.3K Jun 20 11:07 passwd.old
-rw-r--r--. 1 root root  489 Jun 20 11:04 passwd.patch

# 范例三：恢复旧文件的内容
/tmp/testpw patch -R -p0 < passwd.patch
patching file passwd.old
/tmp/testpw ll passwd*
-rw-r--r--. 1 root root 2.3K Jun 20 10:56 passwd.new
-rw-r--r--. 1 root root 2.3K Jun 20 11:09 passwd.old
-rw-r--r--. 1 root root  489 Jun 20 11:04 passwd.patch
/tmp/testpw diff passwd.old passwd.new
4d3
< adm:x:3:4:adm:/var/adm:/sbin/nologin
6c5
< sync:x:5:0:sync:/sbin:/bin/sync
---
> no six line
```

##### 文件打印准备: pr

```text
/tmp/testpw pr /etc/man_db.conf
2018-10-31 04:26                 /etc/man_db.conf                 Page 1
...


2018-10-31 04:26                 /etc/man_db.conf                 Page 2
...


2018-10-31 04:26                 /etc/man_db.conf                 Page 3
...
```

##### shell

```text
# hello world
#!/bin/bash
# Program:
#        This program shows "Hello world!" in your screen.
# History:
# 20210620	moqi	First release
PATH=${PATH}
export PATH
echo -e "Hello world! \a \n"
exit 0

# showname
#!/bin/bash
read -p "Please input your first name: " firstname
read -p "Please input your second name: " secondname
echo -e "\nYour full name is: ${firstname} ${secondname}"

# 利用 date 进行文件的创建
~ cat create_3_filename.sh
#!/bin/bash
echo -e "I will use 'touch' command to create 3 files."
read -p "Please input your filename: " fileuser

filename=${fileuser:-"filename"}

date1=$(date --date='2 days ago' +%Y%m%d)
date2=$(date --date='1 days ago' +%Y%m%d)
date3=$(date +%Y%m%d)
file1=${filename}${date1}
file2=${filename}${date2}
file3=${filename}${date3}

touch "${file1}"
touch "${file2}"
touch "${file3}"

# 数值运算: 简单的加减乘除
~ cat multiplaying.sh
#!/bin/bash
echo -e "You SHOULD input 2 numbers, I will multiplying them! \n"
read -p "first number: " firstnum
read -p "second number: " secondnum

total=$((${firstnum}*${secondnum}))

echo -e "\nThe result of ${firstnum} x ${secondnum} is ==> ${total}"
~ ./multiplaying.sh
You SHOULD input 2 numbers, I will multiplying them!

first number: 10
second number: 200

The result of 10 x 200 is ==> 2000

# 双引号中可带空格
~ echo $((13 % 3))
1

# 需要小数点计算时使用 bc
~ echo "123.123*55.9" | bc
6882.575

# 数值运算：通过 bc 计算 pi
~ vim cal_pi.sh
~ cat cal_pi.sh
#!/bin/bash
echo -e "This program will calculate pi value. \n"
echo -e "You should input a float number to calculate pi value.\n"

read -p "The scale number (10~10000)?" checking
num=${checking:-"10"}

echo -e "Starting calculte pi value. Be patient."
time echo "scale=${num}; 4*a(1)" | bc -lq
~ chmod a+x cal_pi.sh
~ ./cal_pi.sh
This program will calculate pi value.

You should input a float number to calculate pi value.

The scale number (10~10000)?10
Starting calculte pi value. Be patient.
3.1415926532

real	0m0.006s
user	0m0.005s
sys	0m0.003s
~ ./cal_pi.sh
This program will calculate pi value.

You should input a float number to calculate pi value.

The scale number (10~10000)?100
Starting calculte pi value. Be patient.
3.141592653589793238462643383279502884197169399375105820974944592307\
8164062862089986280348253421170676

real	0m0.011s
user	0m0.005s
sys	0m0.008s
~ ./cal_pi.sh
This program will calculate pi value.

You should input a float number to calculate pi value.

The scale number (10~10000)?5000
Starting calculte pi value. Be patient.
3.14159265358979323846264338327950288419716939937.......

real	0m17.249s
user	0m17.260s
sys	0m0.009s


```

script 的执行方式差异 （source, sh script, ./script）

* 利用直接执行的方式来执行 script: 子程序 bash 内的所有数据便被移除
* 利用 source 来执行脚本：在父程序中执行: 各项动作都会在原本的 bash 内生效

###### 利用 test 指令的测试功能

```text
~ test -e /dmdmdmd && echo "exist" || echo "not exist"
not exist
~ test -e passwd && echo "exist" || echo "not exist"
exist
~ test -e passwdxiba && echo "exist" || echo "not exist"
not exist
```

测试的标志

* -e: 该“文件名”是否存在？（常用）
* -f: 该“文件名”是否存在且为文件（file）？（常用）
* -d: 该“文件名”是否存在且为目录（directory）？（常 用）

关于文件的权限侦测，如 test -r filename 表示可读否 （但 root 权限常有例外）

* -r: 侦测该文件名是否存在且具有“可读”的权限？
* -w: 侦测该文件名是否存在且具有“可写”的权限？
* -x: 侦测该文件名是否存在且具有“可执行”的权限？
* -s: 侦测该文件名是否存在且为“非空白文件”？

两个文件之间的比较，如： test file1 -nt file2

* -nt: （newer than）判断 file1 是否比 file2 新
* -ot: （newer than）判断 file1 是否比 file2 旧
* -ef: 判断 file1 与 file2 是否为同一文件，可用在判断 hard link 的判定上。主要意义在判定，两个文件是否均指向同一个 inode 哩！

判定字串的数据

* test -z string: 判定字串是否为 0 ？若 string 为空字串，则为 true
* test -n string: 判定字串是否非为 0 ？若 string 为空字串，则为 false。 -n 亦可省略
* test str1 == str2: 判定 str1 是否等于 str2 ，若相等，则回传 true
* test str1 != str2: 判定 str1 是否不等于 str2 ，若相等，则回传 false

多重条件判定，例如： test -r filename -a -x filename

* -a:（and）两状况同时成立！例如 test -r file -a -x file，则 file 同时具有 r 与 x 权限时，才回传 true。
* -o:（or）两状况任何一个成立！例如 test -r file -o -x file，则 file 具有 r 或 x 权限时，就可回传 true。
* !: 反相状态，如 test ! -x file ，当 file 不具有 x 时，回 传 true。

```text
让使用者输入一个 文件名，我们判断：
1. 这个文件是否存在，若不存在则给予一个“Filename does not exist”的讯息，并中断程序； 
2. 若这个文件存在，则判断他是个文件或目录，结果输出“Filename is regular file” 或 “Filename is directory”
3. 判断一下，执行者的身份对这个文件或目录所拥有的权限，并输出权限数据！
~ cat file_perm.sh
#!/bin/bash
# Program:
#	User input a filename, program will check the flowing:
#	1. exist? 2. file/directory? 3. file permissions

export PATH

echo -e "Please input a filename, I will check the filename's type and permission. \n\n"
read -p "Input a filename: " filename

test -z ${filename} && echo "You MUST input a filename." && exit 0

test ! -e ${filename} && echo "The filename '${filename}' DO NOT exist" && exit 0

test -f ${filename} && filetype="regulare file"
test -d ${filename} && filetype="directory"
test -r ${filename} && perm="readable"
test -w ${filename} && perm="${perm} writable"
test -x ${filename} && perm="${perm} executable"

echo "The filename: ${filename} is a ${filetype}"
echo "And the permissions for you are: ${perm}"

~ ./file_perm.sh
Please input a filename, I will check the filename's type and permission.


Input a filename: xixixi20210618
The filename: xixixi20210618 is a regulare file
And the permissions for you are: readable writable
~ ./file_perm.sh
Please input a filename, I will check the filename's type and permission.


Input a filename: showname.sh
The filename: showname.sh is a regulare file
And the permissions for you are: readable writable executable
~ ./file_perm.sh
Please input a filename, I will check the filename's type and permission.


Input a filename: moqi
The filename: moqi is a directory
And the permissions for you are: readable writable executable
```

###### 利用判断符号 \[\]

```text
~ [ -z "${HOME}" ]; echo $?
1
~ [ -z "${HOME123}" ]; echo $?
0
```

使用中括号必须要特别注意，因为中括号用在很多地方，包括万用字符与正则表达式等等，所以如果要在 bash 的语法当中使用中括号作为 shell 的判断式时，必须要注意中括号的两端需要有空白字符来分隔喔！

```text
[ "$HOME" == "$MAIL" ]
[□"$HOME"□==□"$MAIL"□] 
 ↑       ↑  ↑       ↑
```

1. 当执行一个程序的时候，这个程序会让使用者选择 Y 或 N ，
2. 如果使用者输入 Y 或 y 时，就显示“ OK, continue ”
3. 如果使用者输入 n 或 N 时，就显示“ Oh, interrupt ！”
4. 如果不是 Y/y/N/n 之内的其他字符，就显示“ I don't know what your choice is ”

```text
~ cat ans_yn.sh
#!/bin/bash
# Program:
#	This program shows the user's choice

export PATH

read -p "Please input (Y/N):" yn
[ "${yn}" == "Y" -o "${yn}" == "y" ] && echo "OK, continue" && exit 0
[ "${yn}" == "N" -o "${yn}" == "n" ] && echo "Oh, interrpt!" && exit 0

echo "I dont konw what your choice is" && exit 0

~ ./ans_yn.sh
Please input (Y/N):y
OK, continue
~ ./ans_yn.sh
Please input (Y/N):Y
OK, continue
~ ./ans_yn.sh
Please input (Y/N):N
Oh, interrpt!
~ ./ans_yn.sh
Please input (Y/N):n
Oh, interrpt!
~ ./ans_yn.sh
Please input (Y/N):asfd
I dont konw what your choice is
```

###### Shell script 的默认变量（$0, $1...）

其实 script 针对参数已经有设置好一些变量名称了

```text
/path/to/scriptname opt1 opt2 opt3 opt4
      $0             $1   $2   $3   $4
```



















































