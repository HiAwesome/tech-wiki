# 零散的经验

##### [在 Mac 上放大鼠标指针](https://support.apple.com/zh-cn/guide/mac-help/mchlp2920/mac)

#### Git blame

* [Bitbucket: Git blame](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-blame)
* [Git blame documentation](https://git-scm.com/docs/git-blame)

#### [iTerm2 v3 conversion of tabs to spaces on paste](https://stackoverflow.com/a/38063352)

### [I18n g11n K8s i22y 这样的缩写起源于何处？](https://www.zhihu.com/question/39756710/answer/82930540)

根据上述维基页面的说明，世界上第一个“首字母+字母个数+尾字母”形式的缩写是S12n，全称是Scherpenhuizen，是一个员工的姓氏。缩写的原因是姓氏太长，超过了用户名的限制。

[Wikipedia: Numeronym](https://en.wikipedia.org/wiki/Numeronym).

对于无法使用首字母缩写的英文元素衍生出的一种缩写形式，最有名的有这几个：i18n – internationalization; K8s – Kubernetes.

### [Failed to watch directory: bad file descriptor On MacOS](https://github.com/grafana/grafana/issues/29121#issuecomment-729020013)

```shell
ulimit -S -n 2048
```

## Mac install [tldr](https://github.com/tldr-pages/tldr)

```shell
brew install tldr
```

## [文档标记语言的比较及选择](https://en.wikipedia.org/wiki/Comparison_of_document-markup_languages)

从 Wikipedia 可以看到 [Markdown](https://en.wikipedia.org/wiki/Markdown) 最为流行，但确实目前是一个静态的标准。像 Github、Gitlab、Stack Overflow 都支持了带有各自风味的 Common Markdown, 因为无法统一这个标准本身。

另一个是 [asciidoctor](https://github.com/asciidoctor/asciidoctor/blob/master/README-zh_CN.adoc), 和 [AsciiDoc](https://en.wikipedia.org/wiki/AsciiDoc) 有一定的渊源，**后期可能转换到这个文档标记工具**，原因如下：

1. 它是一个依然在不断更新的标准，几个依然在更新的 Github Repo。
2. 有 Eclipse 基金会支持，和其他赞助。
3. 经过基础测试也被 IntelliJ IDEA、Github 支持。
4. 可以轻易转换为多种格式，最常用的是 HTML、PDF、EPUB。
5. [Compare AsciiDoc and Markdown](https://docs.asciidoctor.org/asciidoc/latest/asciidoc-vs-markdown/).
6. Use AsciiDoc for document markup. Really. It's actually **readable** by humans, easier to parse and way more flexible than XML.    ——Linus Torvalds

## [Bob](https://ripperhe.gitee.io/bob/#/) shortcuts

无罗技键鼠配合的设定快捷键如下：

* 划词翻译：Option + L
* 截图翻译：Option + Shift + F1
* 输入翻译：Option + K

## Snipaste shortcuts

Snipaste 只需要设定截屏快捷键，其他的暂时全部删除。

由于 IntelliJ IDEA 中 F1 快捷键为快速查看文档，且在大部分软件中 F1 为查看帮助文档，因此设定 Snipaste 截图为 Command + Shift + F1。

## Apple Support Official

### [Mac 上的菜单中显示的那些符号表示什么？](https://support.apple.com/zh-cn/guide/mac-help/cpmh0011/mac)

### [Mac 键盘快捷键](https://support.apple.com/zh-cn/HT201236)

## [Mac OS X 系统中 Delete 删除键的 5 种用法](http://tech.sina.com.cn/s/jq/2015-03-06/doc-icczmvun6516277.shtml)

> 第一种：按 delete 键，实现 Windows 键盘上退格键的功能，也就是删除光标之前的一个字符（默认）；
> 
> 第二种：按 fn+delete 键，删除光标之后的一个字符；
>
> 第三种：按 option+delete 键，删除光标之前的一个单词（英文有效）；
>
> 第四种：按 command+delete 键，删除光标之前整行内容；
>
> 第五种：选中文件后按 command+delete，删除掉该文件。

## [Zsh Autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

Zsh 的命令自动补全，默认基于历史命令。

## [ssh “permissions are too open” error](https://stackoverflow.com/a/9270753)

Keys need to be only readable by you:

> chmod 400 ~/.ssh/id_rsa

If Keys need to be read-writable by you:

> chmod 600 ~/.ssh/id_rsa

600 appears to be fine as well (in fact better in most cases, because you don't need to change file permissions later to edit it).

The relevant portion from the manpage (`man ssh`)

> ~/.ssh/id_rsa
         Contains the private key for authentication.  These files contain sensitive 
         data and should be readable by the user but not
         accessible by others (read/write/execute).  ssh will simply ignore a private 
         key file if it is              
         accessible by others.  It is possible to specify a
         passphrase when generating the key which will be used to encrypt the sensitive 
         part of this file using 3DES.
> 
> ~/.ssh/identity.pub
> 
> ~/.ssh/id_dsa.pub
> 
> ~/.ssh/id_ecdsa.pub
> 
> ~/.ssh/id_rsa.pub
         Contains the public key for authentication.  These files are not sensitive and 
         can (but need not) be readable by anyone.

## Mac with 罗技 MX 套装

### 设定蓝牙和连接器双连接模式

目前 Mac 使用 MX 键盘和鼠标，使用蓝牙连接比较方便，但没有连接器方便。因为 MX 可以连接三套设备，所以设定 MX 使用蓝牙连接 Mac 一次，再设定 MX 使用连接器连接 Mac 一次，这样就会占据两套设备。日常使用连接器，即插即用时使用蓝牙。

### [解决罗技 mx master3 鼠标按住控制键无法切换 Mac 桌面的问题](https://apple.stackexchange.com/a/310741)

鼠标调用的是键盘快捷键 Ctrl + <- / Ctrl + ->，因此需要在键盘设置中打开这对快捷键组合。

System Preferences -> Keyboard -> SHortcuts(top) -> Mission COntrol (left panel) -> Move left a space / Move right a space (right panel)

又因为鼠标操作与按键交互直觉上方向相反，因而把 Move left a space 设置为 Ctrl + ->, 把 Move right a space 设置为 Ctrl + <-, 直觉上比较匹配。

## Mac 使用 Surge 的技巧

### 增强模式分享代理给其他设备时

1. Mac 自身网络的 IPv4 获取设定为 Using DHCP with manual address, 并手动输入一个实体路由器中未被占用的地址
2. 其他设备连接实体路由器，配置 IPv4 为手动，输入一个实体路由器中未被占用的地址，子网掩码写 255.255.255.0, **路由器写 Mac IPv4 地址**
3. 配置 DNS 选择手动，DNS 服务器写 **198.18.0.2**, 这是 Surge 打开增强模式设定的 DNS 服务器，可在 Mac 端网络查看 DNS 得到。

### git clone ssh_address

ssh config 无需特殊配置即可使用。

## AOC 屏幕

选择 DisplayHDR 模式

## ClashX 问题

* [小白使用clashx连接线路出现问题，几乎全都是失败](https://github.com/yichengchen/clashX/issues/473), 是的，修改dns enable：false或者fallback里面添加8.8.8.8后就可以了

## [Mac iterm2 cheatsheet](https://gist.github.com/moqimoqidea/7512257f11a5342eb4df906964433d28)

## Mac 使用罗技键盘启动 Screen saver

参考 [3 ways to create a screen saver shortcut on Mac](https://www.idownloadblog.com/2020/02/06/create-screen-saver-shortcut-mac/) 里面的第二种方法。

## Mac WiFi 连接公司网络频繁中断

* 尝试 [重置 SMC](https://support.apple.com/zh-cn/HT201295), 无效；
* 在手机 APP 苹果支持里选择网络让苹果回电，客服建议 [新建网络位置](https://support.apple.com/zh-cn/HT202480), 随便写一个名字，目前有效，不咋中断。

## [Mac 调整通知内容到主屏幕](https://eshop.macsales.com/blog/59320-how-to-move-notifications-from-one-monitor-to-another-on-a-mac/)

在设置中心屏幕界面，拖拽白条到主要使用的屏幕。

## 使用 Git LFS 管理大文件

* [Github: Git LFS Repo](https://github.com/git-lfs/git-lfs)
* [Gitee: Git LFS 操作指南](https://gitee.com/help/articles/4235)

## [Git 如何提高 Github 的 pull/push 速度](https://ruby-china.org/topics/40408)

1. SSH type, edit ~/.ssh/config and append:

```
Host github.com *.github.com
    ProxyCommand nc -v -x 127.0.0.1:6152 %h %p
    HostName github.com
    User git

Host gitlab.com *.gitlab.com
    ProxyCommand nc -v -x 127.0.0.1:6152 %h %p
    HostName gitlab.com
    User git
```

2. HTTP type, edit ~/.gitconfig and append:

```
# proxy for github
[http "https://github.com"]
    proxy = http://127.0.0.1:6152
    sslVerify = false
# proxy for gitlab
[http "https://gitlab.com"]
    proxy = http://127.0.0.1:6152
    sslVerify = false
```

## 获取中文字符的 URI 编码

* 访问网页获得: [在线编码转换 URL 转码](https://tool.oschina.net/encode?type=4)
* 使用 js 代码获得: Chrome 打开任意网页控制台，输入 encodeURI('中文材料')
* 理论参考: [关于URL编码](http://www.ruanyifeng.com/blog/2010/02/url_encoding.html)

## [外接显示器时，让 Alfred 在当前显示器中显示](http://freelancer-x.com/196/%E5%A4%96%E6%8E%A5%E6%98%BE%E7%A4%BA%E5%99%A8%E6%97%B6%EF%BC%8C%E8%AE%A9-alfred-%E5%9C%A8%E5%BD%93%E5%89%8D%E6%98%BE%E7%A4%BA%E5%99%A8%E4%B8%AD%E6%98%BE%E7%A4%BA%EF%BC%88%E6%89%93%E5%BC%80%EF%BC%89/)

Preferencet-> Appearance -> Options -> Show Alfred on, choose mouse screen.

## Mac 为 APP 单独设定语言

Language & Region -> Apps: Customize language settings for the apps below.

## [解决 Mac OS X 中的蓝牙无线问题](https://support.logi.com/hc/zh-cn/articles/360023195654)

* [解决由无线干扰引起的 Wi-Fi 和蓝牙问题](https://support.apple.com/zh-cn/HT201542)
* [MacBook Pro2019款升级系统后连接蓝牙键盘不稳定的问题](https://discussionschinese.apple.com/thread/251456400)
* [Mos](https://mos.caldis.me/), 一个用于在 MacOS 上平滑你的鼠标滚动效果或单独设置滚动方向的小工具, 让你的滚轮爽如触控板。

## Mac 键盘输入温度的°符号

按下「shift + option + 数字 8」键，此方法会输出一个大一点的圆点「°」

## 使用 Pandoc 将 markdown 项目转换为 epub

* [Creating an ebook with pandoc](https://pandoc.org/epub.html)

## 使用 pyenv 管理 python 版本

* [Pyenv Github](https://github.com/pyenv/pyenv)
* [Pyenv Tutorial](https://realpython.com/intro-to-pyenv/)
* ERROR: The Python zlib extension was not compiled. Missing the zlib?  解决方案: xcode-select --install
* BUILD FAILED (OS X 11.0.1 using python-build 20180424)
  * CFLAGS="-I$(brew --prefix openssl)/include -I$(brew --prefix bzip2)/include -I$(brew --prefix readline)/include -I$(xcrun --show-sdk-path)/usr/include" LDFLAGS="-L$(brew --prefix openssl)/lib -L$(brew --prefix readline)/lib -L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib" pyenv install --patch 3.6.12 < <(curl -sSL https://github.com/python/cpython/commit/8ea6353.patch\?full_index\=1)
  * [Unable to build Python on macOS Big Sur with Xcode 12 beta ](https://github.com/pyenv/pyenv/issues/1643#issuecomment-655710632)
  * [Python cannot be installed on Mac OS 11.0.1 pyenv](https://github.com/pyenv/pyenv/issues/1737#issuecomment-738080459)
  * [issue 1738](https://github.com/pyenv/pyenv/issues/1738), 需要在 [apple develop](https://developer.apple.com/download/more/) 下载 Command Line Tools for Xcode 12.3 Release Candidate. 
  * [pyenv installでエラーが発生する](https://medium.com/@bj1024/pyenv-install-error-mac-dcbd28fdc9db).
  

## Mac set allow apps downloaded from anywhere

```shell
sudo spctl --master-disable
```

## [更改 macOS 用户帐户和个人文件夹的名称](https://support.apple.com/zh-cn/HT201548)

## Mac 可以使用密码解锁，但是无法使用密码解锁 Security & Privacy

解决方案：重置 SMC，参考[If System Preferences doesn't accept a valid administrator password when you click the lock to make changes](https://support.apple.com/en-us/HT203127)

If System Preferences doesn't accept a valid administrator password when you click the lock to make changes, try these solutions.

Your Mac asks you to enter the name and password of an admin account when it needs to verify that you have permission to make changes. For example, you must authenticate as an administrator when you click the lock  in System Preferences, enter certain commands in Terminal, or set a firmware password.

System Preferences might not accept a valid password in these circumstances:

* The password for your macOS user account is blank. If you have been using a blank password to log in to your Mac, change your password in Users & Groups preferences. Don't use a blank password.

* While using macOS Big Sur 11.1, your Mac with Apple T2 Security Chip has an issue that requires resetting the SMC. System Preferences should accept your password after you reset the SMC.


## Mac 设置一位数密码

终端输入 pwpolicy -clearaccountpolicies , 然后会出现 Clearing global account policies 即表示成功，去掉了密码必须四位数的限制。

## [在 Mac 下找出监听某个 tcp 端口的程序](https://stackoverflow.com/a/4421674)

```shell
lsof -nP -iTCP:$PORT | grep LISTEN
```

## [在 Mac 下安装 gvm 管理 go 版本](https://github.com/moovweb/gvm)

## [在 Mac 下安装 nvm 管理 node 版本](https://blog.csdn.net/Charissa2017/article/details/104497572)

安装最新的node稳定版本
```shell
nvm install --lts
```

查看本地 node 的所有版本
```shell
nvm list
```

设置默认版本的Node，每次启动终端都使用该版本的node
```shell
nvm alias default 版本号
```

切换到指定的node版本
```shell
nvm use 10.19
```

卸载指定的node版本
```shell
nvm uninstall 版本号
```

查看node的所有的版本
```shell
nvm ls-remote
```

[nvm on github](https://github.com/nvm-sh/nvm)

## [在 Mac 下运行 Lisp 程序](https://luciaca.cn/2019/03/22/running-lisp-under-mac-os/)

1. brew install mit-scheme
2. 运行 REPL: mit-scheme
3. 运行代码文件: scheme < FILENAME

## [Mac OS python2 安装 pip 工具](https://stackoverflow.com/a/18947390)

1. curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
2. python get-pip.py

## Mac OS 更新到 Big Sur 版本之后 JD-GUI 无法使用

参考 [BigSur ERROR launching 'JD-GUI'](https://github.com/java-decompiler/jd-gui/issues/332), 把 JD-GUI 包文件中 /Applications/JD-GUI.app/Contents/MacOS/universalJavaApplicationStub.sh 的 JAVACMD 固定为 JAVACMD="/Library/Java/JavaVirtualMachines/jdk1.8.0_261.jdk/Contents/Home/bin/java" 的格式，去掉 # first check system variable "$JAVA_HOME" 中冗余的 JAVACMD 判断。

## Mac OS 更新到 Big Sur 版本之后随航功能无法使用

在 iPad 上退出 iCloud 并重新登陆，然后在 Mac 的系统设置中更新 Apple ID 的信息，需要输入 iPad 的锁屏密码，最终可以正常使用。参考[Sidecar not working on Big Sur and iPad OS](https://discussions.apple.com/thread/252042262).

## Mac OS 更新到 Big Sur 版本之后使用随航连接 iPad 显示 timeout

Make sure that Handoff is not checked on both the iPad and the Mac. To Disable Handoff on your devices:

* iPad, iPhone, and iPod touch: Go to Settings , then tap General > Handoff.
* Mac: Choose Apple Menu > System Preferences > General, then turn off “Allow Handoff between this Mac and your iCloud devices.”

From: Support.Apple, 参考["Device Timed Out" error in Sidecar](https://discussions.apple.com/thread/250715445)

## homebrew through proxy

ALL_PROXY=http://127.0.0.1:6152 brew upgrade
参考: [Homebrew behind proxy](https://github.com/Homebrew/legacy-homebrew/issues/11114#issuecomment-62933075)

## the mercurial clone through proxy

http_proxy=http://proxy-server:8080 hg clone https://vim.googlecode.com/hg
参考: [To use proxy for Mercurial you can prefix your command with http_proxy](https://stackoverflow.com/a/35941669)

## Mac 上的十六进制文件编辑器

[Hex Fiend](https://ridiculousfish.com/hexfiend/)


## Mac 上终端无法自动换行

目前仍未 100% 解决，多发生在 M1 CPU 的 Mac 的 iTerm2 上。

* Google search: terminal terminal input does not wrap automatically
* [Terminal prompt not wrapping correctly](https://unix.stackexchange.com/a/105974)
* [How do I get long command lines to wrap to the next line?](https://askubuntu.com/a/24422)
* [linux 输入长命令行 会无缘无故的回到行开始,本来应该在下一行继续的!](https://www.iteye.com/blog/flytreeleft-1541616)
* [终端输出不自动换行？](https://bbs.csdn.net/topics/391833631)
