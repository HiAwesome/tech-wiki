# Github

### [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

```shell

ssh-keygen -t ed25519 -C "moqimoqidea@gmail.com"

```

* [SSH keys / Add new](https://github.com/settings/ssh/new)

### [ssh_exchange_identification: Connection closed by remote host under Git bash](https://stackoverflow.com/a/60994276)

For fixing the issues add the Hostname for Git on `~/.ssh/config`,

```text
Host github.com
    Hostname ssh.github.com
    Port 443
```

In my case GitHub!

alse see [Github: Using SSH over the HTTPS port](https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port#enabling-ssh-connections-over-https).

### [设定 master 为项目默认分支名](https://github.com/settings/repositories)

## [Github Advanced Search](https://github.com/search/advanced)

* **[举例: JSON.parseObject extension:java](https://github.com/search?q=JSON.parseObject+extension%3Ajava&type=Code)**
* [搜索代码](https://docs.github.com/cn/github/searching-for-information-on-github/searching-on-github/searching-code)
* [搜索仓库](https://docs.github.com/cn/github/searching-for-information-on-github/searching-on-github/searching-for-repositories)
* [搜索包](https://docs.github.com/cn/github/searching-for-information-on-github/searching-on-github/searching-code)
* [在 forks 中搜索](https://docs.github.com/cn/github/searching-for-information-on-github/searching-on-github/searching-in-forks)
* [搜索语法](https://docs.github.com/cn/github/searching-for-information-on-github/getting-started-with-searching-on-github/understanding-the-search-syntax)
* [搜索查询故障排除](https://docs.github.com/cn/github/searching-for-information-on-github/getting-started-with-searching-on-github/troubleshooting-search-queries)
* [排序搜索结果](https://docs.github.com/cn/github/searching-for-information-on-github/getting-started-with-searching-on-github/sorting-search-results)
