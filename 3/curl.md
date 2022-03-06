# [curl](https://curl.se/docs/manpage.html)

## --json 参数

[7.82.0](https://curl.se/changes.html#7_82_0) 增加了新参数 --json, `--json <data>` is a shortcut for `--data-binary <data> -H "Content-Type: application/json" -H "Accept: application/json"`, 详见:

```shell
~ curl --version
curl 7.82.0 (x86_64-apple-darwin21.3.0) libcurl/7.82.0 (SecureTransport) OpenSSL/1.1.1m zlib/1.2.11 brotli/1.0.9 zstd/1.5.2 libidn2/2.3.2 libssh2/1.10.0 nghttp2/1.47.0 librtmp/2.3 OpenLDAP/2.6.1
Release-Date: 2022-03-05
Protocols: dict file ftp ftps gopher gophers http https imap imaps ldap ldaps mqtt pop3 pop3s rtmp rtsp scp sftp smb smbs smtp smtps telnet tftp
Features: alt-svc AsynchDNS brotli GSS-API HSTS HTTP2 HTTPS-proxy IDN IPv6 Kerberos Largefile libz MultiSSL NTLM NTLM_WB SPNEGO SSL TLS-SRP UnixSockets zstd


~ curl -X POST 'http://httpbin.org/post' --header 'accept: application/json' --header 'Content-Type: application/json' --data-raw '{"name":"tom"}'
{
  "args": {},
  "data": "{\"name\": \"tom\"}",
  "files": {},
  "form": {},
  "headers": {
    "Accept": "application/json",
    "Accept-Encoding": "gzip",
    "Content-Length": "15",
    "Content-Type": "application/json",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.82.0",
    "X-Amzn-Trace-Id": "Root=1-62243971-5be4152b45c6edd25337cadf"
  },
  "json": {
    "name": "tom"
  },
  "origin": "54.169.164.94",
  "url": "http://httpbin.org/post"
}


~ curl --json '{"name":"tom"}' http://httpbin.org/post
{
  "args": {},
  "data": "{\"name\":\"tom\"}",
  "files": {},
  "form": {},
  "headers": {
    "Accept": "application/json",
    "Accept-Encoding": "gzip",
    "Content-Length": "14",
    "Content-Type": "application/json",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.82.0",
    "X-Amzn-Trace-Id": "Root=1-622438c2-25f3673004cb4c6a65c54cb9"
  },
  "json": {
    "name": "tom"
  },
  "origin": "54.169.164.94",
  "url": "http://httpbin.org/post"
}
~
```
