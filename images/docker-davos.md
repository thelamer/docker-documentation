# [linuxserver/davos](https://github.com/linuxserver/docker-davos)

[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/davos.svg)](https://microbadger.com/images/linuxserver/davos "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/davos.svg)](https://microbadger.com/images/linuxserver/davos "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/davos.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/davos.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-davos/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-davos/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/davos/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/davos/latest/index.html)

[Davos](https://github.com/linuxserver/davos) is an FTP automation tool that periodically scans given host locations for new files. It can be configured for various purposes, including listening for specific files to appear in the host location, ready for it to download and then move, if required. It also supports completion notifications as well as downstream API calls, to further the workflow.


## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/davos` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |
| arm64 | arm64v8-latest |
| armhf | arm32v6-latest |


## Usage

Here are some example snippets to help you get started creating a container from this image.

### docker

```
docker create \
  --name=davos \
  -e PUID=1001 \
  -e PGID=1001 \
  -p 8080:8080 \
  -v <path to data>:/config \
  -v <path to downloads folder>:/download \
  --restart unless-stopped \
  linuxserver/davos
```


### docker-compose

Compatible with docker-compose v2 schemas.

```yaml
---
version: "2"
services:
  davos:
    image: linuxserver/davos
    container_name: davos
    environment:
      - PUID=1001
      - PGID=1001
    volumes:
      - <path to data>:/config
      - <path to downloads folder>:/download
    ports:
      - 8080:8080
    mem_limit: 4096m
    restart: unless-stopped
```

## Parameters

Docker images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

### Ports (`-p`)

| Parameter | Function |
| :----: | --- |
| `8080` | This is the default port that davos runs under |


### Environment Variables (`-e`)

| Env | Function |
| :----: | --- |
| `PUID=1001` | for UserID - see below for explanation |
| `PGID=1001` | for GroupID - see below for explanation |

### Volume Mappings (`-v`)

| Volume | Function |
| :----: | --- |
| `/config` | davos's config location. This is where it stores its database file and logs. |
| `/download` | davos's file download location |



## User / Group Identifiers

When using volumes (`-v` flags), permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1001` and `PGID=1001`, to find yours use `id user` as below:

```
  $ id username
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Application Setup

The application does not require any set up other than starting the docker container. Further documentation can be found on the [davos GitHub repository page](https://github.com/linuxserver/davos).



## Support Info

* Shell access whilst the container is running: 
  * `docker exec -it davos /bin/bash`
* To monitor the logs of the container in realtime: 
  * `docker logs -f davos`
* Container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' davos`
* Image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/davos`

## Versions

* **22.02.19:** - Rebasing to alpine 3.9.
* **18.11.16:** - Initial Release.
