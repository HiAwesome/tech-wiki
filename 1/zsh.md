# zsh

### set PROMPT_EOL_MARK

在默认情况下，如果在 bash/zsh 里输出一个不带换行的字符串，bash/zsh 会给字符串增加一个后缀 %，可以去掉

```shell

export PROMPT_EOL_MARK=''

```

## [scalar parameter xxxxx created globally in function xxxx](https://m.tqwba.com/x_d/jishu/139046.html)

build_prompt:1: scalar parameter RETVAL created globally in function build_prompt
build_prompt:2: scalar parameter segment created globally in function build_prompt

这个信息总是偶尔出现, 解决方案:

local RETVAL

关于segment这个变量的提示信息还没有解决

