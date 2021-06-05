# CentOS

环境： Cent OS 7 on Vmware Fusion.

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

### Tips

* su - 可以切换到 root 账号，效果同 sudo su
* [What's the difference between `chmod a+x` and `chmod +x`?](https://unix.stackexchange.com/a/639441)
* [Why does chmod +w not give write permission to other(o)](https://unix.stackexchange.com/a/429424)
* [umask - wikipedia](https://en.wikipedia.org/wiki/Umask)
* 为什么可执行权限缩写为 x 而不是 e？个人猜猜因为 read 和 write 中也含有 e, 而 x 是 execute 中第一个其他两个权限不包括的字母。
* cd -> change directory.
* x 权限对于 linux 文件夹来说，表示进入该文件夹的权限。
* [How To Set Up SSH Keys on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-centos7)

