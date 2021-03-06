# [linuxserver/sonarr](https://github.com/linuxserver/docker-sonarr)

[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/sonarr.svg)](https://microbadger.com/images/linuxserver/sonarr "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/sonarr.svg)](https://microbadger.com/images/linuxserver/sonarr "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/sonarr.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/sonarr.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-sonarr/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-sonarr/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/sonarr/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/sonarr/latest/index.html)

[Sonarr](https://sonarr.tv/) (formerly NZBdrone) is a PVR for usenet and bittorrent users. It can monitor multiple RSS feeds for new episodes of your favorite shows and will grab, sort and rename them. It can also be configured to automatically upgrade the quality of files already downloaded when a better quality format becomes available.


## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/sonarr` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

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
  --name=sonarr \
  -e PUID=1001 \
  -e PGID=1001 \
  -e TZ=Europe/London \
  -p 8989:8989 \
  -v <path to data>:/config \
  -v <path/to/tvseries>:/tv \
  -v <path/to/downloadclient-downloads>:/downloads \
  --restart unless-stopped \
  linuxserver/sonarr
```

### Version Tags

This image provides various versions that are available via tags. `latest` tag usually provides the latest stable version. Others are considered under development and caution must be exercised when using them.

| Tag | Description |
| :----: | --- |
| latest | stable builds from the master branch of sonarr (currently v2) |
| develop | development builds from the develop branch of sonarr (currently v2) |
| preview | preview builds from the phantom-develop branch of sonarr (currently v3) |


### docker-compose

Compatible with docker-compose v2 schemas.

```yaml
---
version: "2"
services:
  sonarr:
    image: linuxserver/sonarr
    container_name: sonarr
    environment:
      - PUID=1001
      - PGID=1001
      - TZ=Europe/London
    volumes:
      - <path to data>:/config
      - <path/to/tvseries>:/tv
      - <path/to/downloadclient-downloads>:/downloads
    ports:
      - 8989:8989
    mem_limit: 4096m
    restart: unless-stopped
```

## Parameters

Docker images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

### Ports (`-p`)

| Parameter | Function |
| :----: | --- |
| `8989` | The port for the Sonarr webinterface |


### Environment Variables (`-e`)

| Env | Function |
| :----: | --- |
| `PUID=1001` | for UserID - see below for explanation |
| `PGID=1001` | for GroupID - see below for explanation |
| `TZ=Europe/London` | Specify a timezone to use EG Europe/London, this is required for Sonarr |

### Volume Mappings (`-v`)

| Volume | Function |
| :----: | --- |
| `/config` | Database and sonarr configs |
| `/tv` | Location of TV library on disk |
| `/downloads` | Location of download managers output directory |



## User / Group Identifiers

When using volumes (`-v` flags), permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1001` and `PGID=1001`, to find yours use `id user` as below:

```
  $ id username
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Application Setup

Access the webui at `<your-ip>:8989`, for more information check out [Sonarr](https://sonarr.tv/).



## Support Info

* Shell access whilst the container is running: 
  * `docker exec -it sonarr /bin/bash`
* To monitor the logs of the container in realtime: 
  * `docker logs -f sonarr`
* Container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' sonarr`
* Image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/sonarr`

## Versions

* **01.02.19:** - Multi arch images and pipeline build logic
* **15.12.17:** - Fix continuation lines.
* **12.07.17:** - Add inspect commands to README, move to jenkins build and push.
* **17.04.17:** - Switch to using inhouse mono baseimage, adds python also.
* **14.04.17:** - Change to mount /etc/localtime in README, thanks cbgj.
* **13.04.17:** - Switch to official mono repository.
* **30.09.16:** - Fix umask
* **23.09.16:** - Add cd to /opt fixes redirects with althub (issue #25) , make XDG config environment variable
* **15.09.16:** - Add libcurl3 package.
* **09.09.16:** - Add layer badges to README.
* **27.08.16:** - Add badges to README.
* **20.07.16:** - Rebase to xenial.
* **31.08.15:** - Cleanup, changed sources to fetch binarys from. also a new baseimage.
