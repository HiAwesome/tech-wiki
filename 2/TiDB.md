# TiDB

## Quick Start

### [TiDB 数据库快速上手指南](https://docs.pingcap.com/zh/tidb/stable/quick-start-with-tidb)

```shell
curl --proto '=https' --tlsv1.2 -sSf https://tiup-mirrors.pingcap.com/install.sh | sh

source ~/.zshrc

tiup playground

mysql --host 127.0.0.1 --port 4000 -u root

mysql> SET PASSWORD FOR 'root'@'%' = 'my-passwd';

mysql -h 127.0.0.1 -P4000 -uroot -pmy-passwd
```

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
