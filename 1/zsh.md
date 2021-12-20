# zsh

## [scalar parameter xxxxx created globally in function xxxx](https://m.tqwba.com/x_d/jishu/139046.html)

build_prompt:1: scalar parameter RETVAL created globally in function build_prompt
build_prompt:2: scalar parameter segment created globally in function build_prompt

这个信息总是偶尔出现, 解决方案:

local RETVAL

关于segment这个变量的提示信息还没有解决

