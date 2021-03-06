# [linuxserver/minisatip](https://github.com/linuxserver/docker-minisatip)

[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/minisatip.svg)](https://microbadger.com/images/linuxserver/minisatip "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/minisatip.svg)](https://microbadger.com/images/linuxserver/minisatip "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/minisatip.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/minisatip.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-minisatip/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-minisatip/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/minisatip/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/minisatip/latest/index.html)

[Minisatip](https://github.com/catalinii/minisatip) is a multi-threaded satip server version 1.2 that runs under Linux and it was tested with DVB-S, DVB-S2, DVB-T, DVB-T2, DVB-C, DVB-C2, ATSC and ISDB-T cards.

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/minisatip` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

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
  --name=minisatip \
  -e PUID=1001 \
  -e PGID=1001 \
  -e TZ=Europe/London \
  -e RUN_OPTS=<parameter> \
  -p 8875:8875 \
  -p 554:554 \
  -p 1900:1900/udp \
  -v </path/to/appdata/config>:/config \
  --device /dev/dvb:/dev/dvb \
  --restart unless-stopped \
  linuxserver/minisatip
```

### Additional runtime parameters

In some cases it might be necessary to start minisatip with additional parameters, for example to configure a unicable LNB. Add the parameters you need and restart the container. Be sure to have the right parameters set as adding the wrong once might lead to the container not starting correctly.
For a list of minisatip parameters visit [Minisatip](https://github.com/catalinii/minisatip) page.


### docker-compose

Compatible with docker-compose v2 schemas.

```yaml
---
version: "2"
services:
  minisatip:
    image: linuxserver/minisatip
    container_name: minisatip
    environment:
      - PUID=1001
      - PGID=1001
      - TZ=Europe/London
      - RUN_OPTS=<parameter>
    volumes:
      - </path/to/appdata/config>:/config
    ports:
      - 8875:8875
      - 554:554
      - 1900:1900/udp
    devices:
      - /dev/dvb:/dev/dvb
    mem_limit: 4096m
    restart: unless-stopped
```

## Parameters

Docker images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

### Ports (`-p`)

| Parameter | Function |
| :----: | --- |
| `8875` | Status Page WebUI |
| `554` | RTSP Port |
| `1900/udp` | App Discovery |


### Environment Variables (`-e`)

| Env | Function |
| :----: | --- |
| `PUID=1001` | for UserID - see below for explanation |
| `PGID=1001` | for GroupID - see below for explanation |
| `TZ=Europe/London` | Specify a timezone to use EG Europe/London. |
| `RUN_OPTS=<parameter>` | Specify specific run params for minisatip |

### Volume Mappings (`-v`)

| Volume | Function |
| :----: | --- |
| `/config` | Configuration files and minisatip data |

#### Device Mappings (`--device`)
| Parameter | Function |
| :-----:   | --- |
| `/dev/dvb` | For passing through Tv-cards |


## User / Group Identifiers

When using volumes (`-v` flags), permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1001` and `PGID=1001`, to find yours use `id user` as below:

```
  $ id username
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Application Setup

Best used in conjunction with [tvheadend](https://github.com/linuxserver/docker-tvheadend)

There is no setup per se, other than adding your cards for passthrough.

You can then use your cards as DVB inputs in apps such as tvheadend.



## Support Info

* Shell access whilst the container is running: 
  * `docker exec -it minisatip /bin/bash`
* To monitor the logs of the container in realtime: 
  * `docker logs -f minisatip`
* Container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' minisatip`
* Image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/minisatip`

## Versions

* **22.02.19:** - Rebasing to alpine 3.9.
* **20.02.19:** - Fix run options.
* **11.02.19:** - Add pipeline logic and multi arch.
* **28.08.18:** - Rebase to Alpine 3.8.
* **13.12.17:** - Rebase to Alpine 3.7.
* **28.05.17:** - Rebase to Alpine 3.6.
* **08.02.17:** - Rebase to Alpine 3.5 and only compile libs in dvb-apps.
* **14.10.16:** - Add version layer information.
* **18.09.16:** - Add support for Common Interface.
* **11.09.16:** - Add layer badges to README.
* **28.08.16:** - Add badges to README.
* **15.08.16:** - Initial Release.
