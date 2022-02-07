# Git

### [git: fatal: Could not read from remote repository](https://stackoverflow.com/a/40009598)

Your ssh key most likely had been removed from ssh agent

```shell

ssh-add ~/.ssh/id_rsa

```

where id_rsa is a ssh key associated with git repo

**Update**

You may get `Could not open a connection to your authentication agent`. error to resolve that you need to start the agent first by:

```shell

eval `ssh-agent -s`

```

### [Git基本原理介绍](https://www.escapelife.site/posts/da89563c.html)

### [Git Hard reset of a single file](https://stackoverflow.com/a/7147320/7379661)

```shell

git checkout HEAD -- my-file.txt

```

## [New in Git: switch and restore](https://www.banterly.net/2021/07/31/new-in-git-switch-and-restore/)
