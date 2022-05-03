# [Apache Bench](https://httpd.apache.org/docs/2.4/programs/ab.html)

**Note! ApacheBench (ab) is installed on macOS by default.**


```shell
ab -V
```


#### [Using Apache Bench for simple load testing](https://vyspiansky.github.io/2019/12/02/apache-bench-for-load-testing/)


```shell
ab -n 100 -c 10 http://example.com/
```


```shell
watch -n 1 ab -n 100 -c 10 http://example.com/
```
