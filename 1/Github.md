# Github

### Some Day Use

#### 公开的 dns 解析器

* Google: 8.8.4.4
* Google: 8.8.4.4
* Cloudflare: 1.1.1.1
* Cloudflare: 1.0.0.1

#### [Troubleshooting connectivity problems](https://docs.github.com/en/get-started/using-github/troubleshooting-connectivity-problems)

#### [About GitHub's IP addresses](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-githubs-ip-addresses)

#### [Github Debug](https://github-debug.com/)

#### [Github API meta](https://api.github.com/meta)

从 web 中选取一个 ip, 在本机 hosts 中与 github.com 建立映射关系。

将对应项目的 remote 从 ssh 更新为 https.

**例如: 从 `git@github.com:HiAwesome/tech-wiki.git` 更新为 `https://github.com/HiAwesome/tech-wiki.git`, 即把前缀从 `git@github.com:` 更新为 `https://github.com/`.**

注意，在使用 https 交互的情况下，输入密码需要粘贴一次 token 而不是 github 账户的密码，例如

```shell
git push -u origin master
Username for 'https://github.com': moqimoqidea
Password for 'https://moqimoqidea@github.com':
```

原因:

remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see [Token authentication requirements for Git operations](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/) for more information.

### Feature Preview

Command Palette

Quickly navigate and jump between your organizations or repositories and search recent issues, pull requests, projects and more with the new command palette. You can also execute time saving commands all without lifting your fingers off the keyboard!

To open the command palette:

* macOS: ⌘ k or ⌘ opt k
* other: Ctrl k or Ctrl alt k

### [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

```shell

ssh-keygen -t ed25519 -C "moqimoqidea@gmail.com"

cat ~/.ssh/id_ed25519.pub

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
