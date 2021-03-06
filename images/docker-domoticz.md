# [linuxserver/domoticz](https://github.com/linuxserver/docker-domoticz)

[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/domoticz.svg)](https://microbadger.com/images/linuxserver/domoticz "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/domoticz.svg)](https://microbadger.com/images/linuxserver/domoticz "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/domoticz.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/domoticz.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-domoticz/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-domoticz/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/domoticz/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/domoticz/latest/index.html)

[Domoticz](https://www.domoticz.com) is a Home Automation System that lets you monitor and configure various devices like: Lights, Switches, various sensors/meters like Temperature, Rain, Wind, UV, Electra, Gas, Water and much more. Notifications/Alerts can be sent to any mobile device.

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/domoticz` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |
| arm64 | arm64v8-latest |
| armhf | arm32v6-latest |

## Version Tags

This image provides various versions that are available via tags. `latest` tag usually provides the latest stable version. Others are considered under development and caution must be exercised when using them.

| Tag | Description |
| :----: | --- |
| latest | Current latest head from master at https://github.com/domoticz/domoticz. |
| stable | Latest stable version. |
| stable-4.9700 | Old stable version. Will not be updated anymore! |
| stable-3.815 | Old stable version. Will not be updated anymore! |
| stable-3.5877 | Old stable version. Will not be updated anymore! |

## Usage

Here are some example snippets to help you get started creating a container from this image.

### docker

```
docker create \
  --name=domoticz \
  -e PUID=1001 \
  -e PGID=1001 \
  -e TZ=Europe/London \
  -p 8080:8080 \
  -p 6144:6144 \
  -p 1443:1443 \
  -v <path to data>:/config \
  --device <path to device>:<path to device> \
  --restart unless-stopped \
  linuxserver/domoticz
```

### Passing Through USB Devices

To get full use of Domoticz, you probably have a USB device you want to pass through. To figure out which device to pass through, you have to connect the device and look in dmesg for the device node created. Issue the command 'dmesg | tail' after you connected your device and you should see something like below.

```
usb 1-1.2: new full-speed USB device number 7 using ehci-pci
ftdi_sio 1-1.2:1.0: FTDI USB Serial Device converter detected
usb 1-1.2: Detected FT232RL
usb 1-1.2: FTDI USB Serial Device converter now attached to ttyUSB0
```
As you can see above, the device node created is ttyUSB0. It does not say where, but it's almost always in /dev/. The correct tag for passing through this USB device is '--device /dev/ttyUSB0:/dev/ttyUSB0'


### docker-compose

Compatible with docker-compose v2 schemas.

```yaml
---
version: "2"
services:
  domoticz:
    image: linuxserver/domoticz
    container_name: domoticz
    environment:
      - PUID=1001
      - PGID=1001
      - TZ=Europe/London
    volumes:
      - <path to data>:/config
    ports:
      - 8080:8080
      - 6144:6144
      - 1443:1443
    devices:
      - <path to device>:<path to device>
    mem_limit: 4096m
    restart: unless-stopped
```

## Parameters

Docker images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

### Ports (`-p`)

| Parameter | Function |
| :----: | --- |
| `8080` | WebUI |
| `6144` | Domoticz communication port. |
| `1443` | Domoticz communication port. |


### Environment Variables (`-e`)

| Env | Function |
| :----: | --- |
| `PUID=1001` | for UserID - see below for explanation |
| `PGID=1001` | for GroupID - see below for explanation |
| `TZ=Europe/London` | Specify a timezone to use EG Europe/London. |

### Volume Mappings (`-v`)

| Volume | Function |
| :----: | --- |
| `/config` | Where Domoticz stores config files and data. |

#### Device Mappings (`--device`)
| Parameter | Function |
| :-----:   | --- |
| `<path to device>` | For passing through USB devices. |


## User / Group Identifiers

When using volumes (`-v` flags), permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1001` and `PGID=1001`, to find yours use `id user` as below:

```
  $ id username
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Application Setup

To configure Domoticz, go to the IP of your docker host on the port you configured (default 8080), and add your hardware in Setup > Hardware.
The user manual is available at [www.domoticz.com](https://www.domoticz.com)



## Support Info

* Shell access whilst the container is running: 
  * `docker exec -it domoticz /bin/bash`
* To monitor the logs of the container in realtime: 
  * `docker logs -f domoticz`
* Container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' domoticz`
* Image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/domoticz`

## Versions

* **19.02.19:** - Fix branch for version logic.
* **11.02.19:** - Add pipeline logic and multi arch.
* **02.07.18:** - Add openssh package.
* **01.07.18:** - Fix backup/restore in webgui.
* **03.04.18:** - Add dependencies for BroadlinkRM2 plugin.
* **20.01.18:** - Move telldus core to repo to prevent build fail when source site goes down.
* **18.01.18:** - Remove logging to syslog in the run command to prevent double logging.
* **04.01.18:** - Deprecate cpu_core routine lack of scaling.
* **08.12.17:** - Rebase to alpine 3.7.
* **26.11.17:** - Use cpu core counting routine to speed up build time.
* **28.05.17:** - Rebase to alpine 3.6.
* **26.02.17:** - Add curl and replace openssl with libressl.
* **11.02.17:** - Update README.
* **03.01.17:** - Initial Release.
