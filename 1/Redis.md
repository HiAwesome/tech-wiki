# Redis

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
