# [linuxserver/unifi-controller](https://github.com/linuxserver/docker-unifi-controller)

[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/unifi-controller.svg)](https://microbadger.com/images/linuxserver/unifi-controller "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/unifi-controller.svg)](https://microbadger.com/images/linuxserver/unifi-controller "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/unifi-controller.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/unifi-controller.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-unifi-controller/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-unifi-controller/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/unifi-controller/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/unifi-controller/latest/index.html)

The [Unifi-controller](https://www.ubnt.com/enterprise/#unifi) Controller software is a powerful, enterprise wireless software engine ideal for high-density client deployments requiring low latency and high uptime performance.


## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/unifi-controller` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

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
  --name=unifi-controller \
  -e PUID=1001 \
  -e PGID=1001 \
  -p 3478:3478/udp \
  -p 10001:10001/udp \
  -p 8080:8080 \
  -p 8081:8081 \
  -p 8443:8443 \
  -p 8843:8843 \
  -p 8880:8880 \
  -p 6789:6789 \
  -v <path to data>:/config \
  --restart unless-stopped \
  linuxserver/unifi-controller
```

### Version Tags

This image provides various versions that are available via tags. `latest` tag provides the latest stable build from Unifi, but if this is a permanent setup you might consider using the LTS tag.

| Tag | Description |
| :----: | --- |
| latest | releases from the latest stable branch. |
| LTS | releases from the 5.6.x "LTS Stable" branch. |
| 5.9 | releases from the 5.9.x branch. |
| 5.8 | releases from the 5.8.x branch. |
| 5.7 | releases from the 5.7.x branch. |

## Common problems
When using a Security Gateway (router) it could be that network connected devices are unable to obtain an ip address. This can be fixed by setting "DHCP Gateway IP", under Settings > Networks > network_name, to a correct (and accessable) ip address.


### docker-compose

Compatible with docker-compose v2 schemas.

```yaml
---
version: "2"
services:
  unifi-controller:
    image: linuxserver/unifi-controller
    container_name: unifi-controller
    environment:
      - PUID=1001
      - PGID=1001
    volumes:
      - <path to data>:/config
    ports:
      - 3478:3478/udp
      - 10001:10001/udp
      - 8080:8080
      - 8081:8081
      - 8443:8443
      - 8843:8843
      - 8880:8880
      - 6789:6789
    mem_limit: 4096m
    restart: unless-stopped
```

## Parameters

Docker images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

### Ports (`-p`)

| Parameter | Function |
| :----: | --- |
| `3478/udp` | Unifi communication port |
| `10001/udp` | required for AP discovery |
| `8080` | required for Unifi to function |
| `8081` | Unifi communication port |
| `8443` | Unifi communication port |
| `8843` | Unifi communication port |
| `8880` | Unifi communication port |
| `6789` | For throughput test |


### Environment Variables (`-e`)

| Env | Function |
| :----: | --- |
| `PUID=1001` | for UserID - see below for explanation |
| `PGID=1001` | for GroupID - see below for explanation |

### Volume Mappings (`-v`)

| Volume | Function |
| :----: | --- |
| `/config` | All Unifi data stored here |



## User / Group Identifiers

When using volumes (`-v` flags), permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1001` and `PGID=1001`, to find yours use `id user` as below:

```
  $ id username
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Application Setup

The webui is at https://ip:8443, setup with the first run wizard.

For Unifi to adopt other devices, e.g. an Access Point, it is required to change the inform ip address. Because Unifi runs inside Docker by default it uses an ip address not accessable by other devices. To change this go to Settings > Controller > Controller Settings and set the Controller Hostname/IP to an ip address accessable by other devices.

Alternatively to manually adopt a device take these steps:

```
ssh ubnt@$AP-IP
mca-cli
set-inform http://$address:8080/inform
```

Use `ubnt` as the password to login and `$address` is the IP address of the host you are running this container on and `$AP-IP` is the Access Point IP address.



## Support Info

* Shell access whilst the container is running: 
  * `docker exec -it unifi-controller /bin/bash`
* To monitor the logs of the container in realtime: 
  * `docker logs -f unifi-controller`
* Container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' unifi-controller`
* Image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/unifi-controller`

## Versions

* **10.02.19:** - Initial release of new unifi-controller image with new tags and pipeline logic
