# Redis

#### [Where is redis installed on MacOS?](https://stackoverflow.com/a/63179012)

Redis 启动报错

```shell
23522:M 28 Apr 2022 10:08:45.702 # Internal error in RDB reading offset 0, function at rdb.c:2273 -> The RDB file contains module data I can't load: no matching module type 'ReJSON-RL'
```

原因，之前安装了 redis-json 模块，有相关的数据存储在 dump.rdb 中，无法移除。

解决方案： 删除 dump.rdb

```shell
rm -rf /usr/local/var/db/redis/dump.rdb
```

### [KES pattern](https://redis.io/commands/keys)

在使用 [SCAN cursor \[MATCH pattern] \[COUNT count] \[TYPE type]](https://redis.io/commands/scan) 会用到这种匹配，即 keys 支持 glob 样式模式，而不是正则表达式。

Supported glob-style patterns:

* h?llo matches hello, hallo and hxllo
* h*llo matches hllo and heeeello
* h\[ae]llo matches hello and hallo, but not hillo
* h\[^e]llo matches hallo, hbllo, ... but not hello
* h\[a-b]llo matches hallo and hbllo

Use \ to escape special characters if you want to match them verbatim.

### [Redis Programmability](https://redis.io/topics/programmability)

### [Introduction to Eval Scripts](https://redis.io/topics/eval-intro)

### [Redis Lua API Reference](https://redis.io/topics/lua-api)

### [Redis Lua scripts debugger](https://redis.io/topics/ldb)

* 注意: Lua 里定义布尔值 true/false 返回到 redis 会得到字符串 1/0, 而不是字符串 true/false.

### [A quick guide to Redis Lua scripting](https://www.freecodecamp.org/news/a-quick-guide-to-redis-lua-scripting/)

### 单元测试中使用 Redis

#### [Embedded Redis Server with Spring Boot Test](https://www.baeldung.com/spring-embedded-redis)

这个项目依赖的 [embedded-redis](https://github.com/kstyrc/embedded-redis) 最后一次提交时间为 20180927, 已经三年多没有维护了。

从目前这个时间点 20220104 看去，这个 [jedis-mock](https://github.com/fppt/jedis-mock) 应该是提供类似功能且较新的工具，其介绍为 `A simple redis java mock for unit testing`, 最后一次提交时间为 20211229. 以这个项目为例，[supported_operations](https://github.com/fppt/jedis-mock/blob/master/supported_operations.md) 可以展示其支持 Mock 的 Redis 的方法，从列出来的命令的绝对数量上来说，support/not support 大概是 99/126, 所以在实际 Mock 中可能较容易得到 `Unsupported operation` 而发生错误。

### [Using pipelining to speedup Redis queries](https://redis.io/topics/pipelining)

> Why are busy loops slow even on the loopback interface?
>
> The reason is that processes in a system are not always running, actually it is the kernel scheduler that let the process run, 
> so what happens is that, for instance, the benchmark is allowed to run, reads the reply from the Redis server (related to the last command executed), 
> and writes a new command. The command is now in the loopback interface buffer, but in order to be read by the server, 
> the kernel should schedule the server process (currently blocked in a system call) to run, and so forth. 
> So in practical terms the loopback interface still involves network-like latency, because of how the kernel scheduler works.
