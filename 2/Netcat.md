# Netcat

### [netcat - keep listening for connection in Debian](https://superuser.com/a/1530614)

```shell
while true; do nc -lv <listeningport> ; done
```

### [How to make a webserver with netcat (nc)](https://jameshfisher.com/2018/12/31/how-to-make-a-webserver-with-netcat-nc/)

once:

```
nc -l 8000
```

loop:

```shell
# edit index
$ cat index.http

HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Server: netcat!

<!doctype html>
<html><body><h1>A webpage served by netcat</h1></body></html>

# start sever
$ while true; do cat index.http | nc -l 8000; done
```
