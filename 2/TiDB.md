# TiDB

## Quick Start

### [Deploy a TiDB Cluster from Homebrew](https://docs.pingcap.com/tidb/v3.1/deploy-tidb-from-homebrew)

```shell
brew tap pingcap/brew
brew install tidb-server

# start the tidb-server
tidb-server

brew install mysql-client
mysql -h 127.0.0.1 -P4000 -uroot
mysql> SET PASSWORD FOR 'root'@'%' = 'my-passwd';

mysql -h 127.0.0.1 -P4000 -uroot -pmy-passwd
```
