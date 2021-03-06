# [linuxserver/muximux](https://github.com/linuxserver/docker-muximux)

[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/muximux.svg)](https://microbadger.com/images/linuxserver/muximux "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/muximux.svg)](https://microbadger.com/images/linuxserver/muximux "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/muximux.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/muximux.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-muximux/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-muximux/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/muximux/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/muximux/latest/index.html)

[Muximux](https://github.com/mescon/Muximux) is a lightweight portal to view & manage your HTPC apps without having to run anything more than a PHP enabled webserver. With Muximux you don't need to keep multiple tabs open, or bookmark the URL to all of your apps.

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/muximux` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

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
  --name=muximux \
  -e PUID=1001 \
  -e PGID=1001 \
  -e TZ=Europe/London \
  -p 80:80 \
  -v <path to data>:/config \
  --restart unless-stopped \
  linuxserver/muximux
```


### docker-compose

Compatible with docker-compose v2 schemas.

```yaml
---
version: "2"
services:
  muximux:
    image: linuxserver/muximux
    container_name: muximux
    environment:
      - PUID=1001
      - PGID=1001
      - TZ=Europe/London
    volumes:
      - <path to data>:/config
    ports:
      - 80:80
    mem_limit: 4096m
    restart: unless-stopped
```

## Parameters

Docker images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

### Ports (`-p`)

| Parameter | Function |
| :----: | --- |
| `80` | WebUI |


### Environment Variables (`-e`)

| Env | Function |
| :----: | --- |
| `PUID=1001` | for UserID - see below for explanation |
| `PGID=1001` | for GroupID - see below for explanation |
| `TZ=Europe/London` | Specify a timezone to use EG Europe/London. |

### Volume Mappings (`-v`)

| Volume | Function |
| :----: | --- |
| `/config` | Where muximux should store its files. |



## User / Group Identifiers

When using volumes (`-v` flags), permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1001` and `PGID=1001`, to find yours use `id user` as below:

```
  $ id username
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Application Setup

Find the web interface at `<your-ip>:80` , set apps you wish to use with muximux via the webui.
More info:- [Muximux](https://github.com/mescon/Muximux)



## Support Info

* Shell access whilst the container is running: 
  * `docker exec -it muximux /bin/bash`
* To monitor the logs of the container in realtime: 
  * `docker logs -f muximux`
* Container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' muximux`
* Image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/muximux`

## Versions

* **22.02.19:** - Rebasing to alpine 3.9.
* **16.01.19:** - Add pipeline logic and multi arch.
* **13.09.18:** - Rebase to alpine 3.8.
* **09.01.18:** - Rebase to alpine 3.7.
* **25.05.17:** - Rebase to alpine 3.6.
* **12.02.17:** - Rebase to alpine 3.5.
* **14.10.16:** - Add version layer information.
* **30.09.16:** - Rebase to alpine linux.
* **09.09.16:** - Add badges to README.
* **22.02.16:** - Initial release date.
