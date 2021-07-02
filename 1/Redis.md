# Redis

#### [Using pipelining to speedup Redis queries](https://redis.io/topics/pipelining)

> Why are busy loops slow even on the loopback interface?
>
> The reason is that processes in a system are not always running, actually it is the kernel scheduler that let the process run, 
> so what happens is that, for instance, the benchmark is allowed to run, reads the reply from the Redis server (related to the last command executed), 
> and writes a new command. The command is now in the loopback interface buffer, but in order to be read by the server, 
> the kernel should schedule the server process (currently blocked in a system call) to run, and so forth. 
> So in practical terms the loopback interface still involves network-like latency, because of how the kernel scheduler works.
