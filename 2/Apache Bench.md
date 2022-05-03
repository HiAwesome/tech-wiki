# [Apache Bench](https://httpd.apache.org/docs/2.4/programs/ab.html)

**Note! ApacheBench (ab) is installed on macOS by default.**


```shell
ab -V
```

#### tldr ab

```shell
tldr ab

ab

Apache HTTP server benchmarking tool.
More information: <https://httpd.apache.org/docs/current/programs/ab.html>.

- Execute 100 HTTP GET requests to a given URL:
    ab -n 100 url

- Execute 100 HTTP GET requests, in concurrent batches of 10, to a URL:
    ab -n 100 -c 10 url

- Execute 100 HTTP POST requests to a URL, using a JSON payload from a file:
    ab -n 100 -T application/json -p path/to/file.json url

- Use HTTP [K]eep Alive, i.e. perform multiple requests within one HTTP session:
    ab -k url

- Set the maximum number of seconds to spend for benchmarking:
    ab -t 60 url
```


#### [Using Apache Bench for simple load testing](https://vyspiansky.github.io/2019/12/02/apache-bench-for-load-testing/)


```shell
ab -n 100 -c 10 http://example.com/
```


```shell
watch -n 1 ab -n 100 -c 10 http://example.com/
```

#### [Apache Bench - Environment Setup](https://www.tutorialspoint.com/apache_bench/apache_bench_environment_setup.htm)

```shell
ab -n 100 -c 10 -g out.data https://www.apache.org/
```
