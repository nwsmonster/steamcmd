[![Build Status](https://github.com/nwsmonster/steamcmd/actions/workflows/build-and-publish.yaml/badge.svg)](https://github.com/nwsmonster/steamcmd/actions)
[![Docker Pulls](https://img.shields.io/docker/pulls/nwsmonster/steamcmd.svg)](https://hub.docker.com/r/nwsmonster/steamcmd)
[![Image Size](https://img.shields.io/docker/image-size/nwsmonster/steamcmd/latest.svg)](https://hub.docker.com/r/nwsmonster/steamcmd)

# SteamCMD

> **NOTE:** This is a fork for managing my own private servers. You probably want to use/reference [nhalase/steamcmd](https://github.com/nhalase/steamcmd) instead.

A Docker image for running the linux SteamCMD binary.

Fresh builds every 24-hours!

## Tags

- [`ubuntu`, `latest`](./Dockerfile.ubuntu) - will always follow the latest ubuntu LTS
- [`ubuntu-22.04`](./Dockerfile.ubuntu-22.04) - version locked to 22.04

## Dependencies

```plaintext
lib32gcc-s1
ca-certificates
curl
tzdata
git
locales
gcc
libc-dev
```

## Usage

```bash
# pull latest image
docker pull nwsmonster/steamcmd:latest

# download/update CS:GO
docker run -it nwsmonster/steamcmd:latest +login anonymous +app_update 740 +quit

# use interactively
docker run -it --rm --entrypoint /bin/bash nwsmonster/steamcmd:latest
```

## Environment Variables

```bash
# avoid using ids less than 10000 by default to prevent volume mapping issues with host uids
# note: if you provide different values, the entrypoint script will update the steam user uid/gid
UID=10000
GID=10001
# this image uses the tzdata, so providing a TZ env var is good enough to change the container timezone
TZ=UTC
```

## Explanation

The [ENTRYPOINT](./docker-entrypoint.sh) script utilizes [su-exec](https://github.com/ncopa/su-exec) to run steamcmd as the `steam` user with pid 1. Before dropping down to the `steam` user, the entrypoint script will update the `steam` user `uid`/`gid` if necessary.
