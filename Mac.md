# Mac

### M1 Mac 在某些 SSH 远端机器上无法换行

具体原因见 [stty returns 0 rows and columns on Apple M1](https://sourceforge.net/p/expect/bugs/106/), 卸载掉 brew install expect 5.45.4, 使用 Mac 系统自身的位于 `/usr/bin/expect` 的 expect, 确保 `expect -v` 返回 5.45 即可。

关于 expect 可参考 [Packages and Binaries: expect](https://www.kali.org/tools/expect/), [Expect/Expect](https://core.tcl-lang.org/expect/index).

### [在 Mac 上将 zsh 用作默认 Shell](https://support.apple.com/zh-cn/HT208050)

### [M1芯片Mac搭建前端开发环境](https://segmentfault.com/a/1190000039005726)
