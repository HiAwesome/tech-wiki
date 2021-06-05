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
