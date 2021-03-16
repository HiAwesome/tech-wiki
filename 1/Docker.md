# Docker

## Docker compose

* [Overview of docker-compose CLI](https://docs.docker.com/compose/reference/overview/)

## [Docker logs](https://docs.docker.com/engine/reference/commandline/logs/)

用来查看 Container 日志。

## Docker Image

* [移除所有 tag 为 none 的 image](https://stackoverflow.com/a/50040332), docker images | grep none | awk '{ print $3; }' | xargs docker rmi

## Dockerfile

* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
* [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
