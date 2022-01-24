# Docker

#### [Best Practices to Reduce Docker Image Size](https://www.ecloudcontrol.com/best-practices-to-reduce-docker-images-size/)

#### [Docker container will automatically stop after "docker run -d"](https://stackoverflow.com/a/30209974)

#### [Docker: Copying files from Docker container to host](https://stackoverflow.com/a/22050116)

In order to copy a file from a container to the host, you can use the command

```shell
docker cp <containerId>:/file/path/within/container /host/path/target
```

#### [Change Docker Root Dir on Red Hat Linux?](https://unix.stackexchange.com/questions/452368/change-docker-root-dir-on-red-hat-linux)

Stop all running docker containers and then docker daemon. Move "/var/lib/docker" directory to the place where you want to have this data. For you it would be:

```shell
mv /var/lib/docker /data/
```

and then create symlink for this docker directory in /var/lib path:

```shell
ln -s /data/docker /var/lib/docker
```

Start docker daemon and containers.

#### 查看 docker 中所有的 deployment

```shell
docker ps -q | xargs docker inspect | grep DEPLOYMENT
```

## [Youtube Video, 20160601: Docker for Java Developers](https://www.youtube.com/watch?v=IgJXYU3GOM4)

## Docker Troubleshoot

### driver failed programming external connectivity on endpoint

* [cadvisor prevents docker from removing monitored containers?](https://github.com/google/cadvisor/issues/771), 重启 docker 服务
* [driver failed programming external connectivity on endpoint](https://github.com/docker/compose/issues/3277)
* [csdn: driver failed programming external connectivity on endpoint](https://blog.csdn.net/whatday/article/details/86762264), 重启 docker 服务

### docker unable to remove filesystem device or resource busy

* [解决docker删除container出现device or resource busy](https://qiita.com/domino-jiang/items/d1cac56e68fba67893e3)
* [Unix: How to get over “device or resource busy”?](https://unix.stackexchange.com/a/11241)

## Docker compose

* [Overview of docker-compose CLI](https://docs.docker.com/compose/reference/overview/)

## [Docker logs](https://docs.docker.com/engine/reference/commandline/logs/)

用来查看 Container 日志。

## Docker Image

###  [移除所有 tag 为 none 的 image](https://stackoverflow.com/a/50040332)

```shell
docker images | grep none | awk '{ print $3; }' | xargs docker rmi
```

## Dockerfile

* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
* [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

## Mac Docker Proxies Config

目前 Mac 使用 Surge 代理，设定 Docker 代理中 Web Server(HTTP), Secure Web Server(HTTPS) 皆为 http://127.0.0.1:8888 实测 Docker Desktop 携带的 K8s 可正常启动。

## Docker Use Aliyun Registry Mirrors

[阿里云 Docker 镜像加速器](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors), 每个阿里云用户有私有的加速地址，配置在 daemon.json 即可，加速器页面亦有文档。

优点是拉取速度快，push 也方便。

缺点是版本和 Docker 官方有时间差，冷门镜像无法加速时更慢。

最终不加，裸 Docker 加 Surge 代理。

## [Mac: Docker logs and troubleshooting](https://docs.docker.com/docker-for-mac/troubleshoot/#diagnose-and-feedback)

## Using Docker

### Docker with Postgres

#### [docker hub: postgres image](https://hub.docker.com/_/postgres)

```shell
docker run --name some-postgres -p 54321:5432 -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```

### Docker with MySQL

#### [docker hub: mysql image](https://hub.docker.com/_/mysql)

```shell
docker run --name moqimysql -p 34567:3306 -v /Users/moqi/Dropbox/docker_volume/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=moqi -d mysql:latest --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

### [Zombie docker container that can't be killed](https://stackoverflow.com/a/52493047)

I figured it out. Turns out it was related to docker swarm. I had experimented with it at some point without fully understanding what it is and what it does and apparently it just stayed there.

All I had to do was:

```shell
docker swarm leave --force
```

and it worked like a head-shot to an actual zombie.

### [docker - how do you disable auto-restart on a container?](https://stackoverflow.com/a/37600885)

You can use the `--restart=unless-stopped` option, as @Shibashis mentioned, or update the restart policy (this requires docker 1.11 or newer);

See the documentation for docker update and [Docker restart policies](https://docs.docker.com/engine/reference/run/#restart-policies---restart).

```shell
docker update --restart=no my-container
```

that updates the restart-policy for an existing container (`my-container`)
