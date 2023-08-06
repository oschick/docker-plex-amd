## Plex Docker with AMD/VAAPI support.

A Plex Media Server docker container that enables VAAPI support for AMDGPU hardware accelerated decoding.

This image uses [Linuxserver/Plex](https://hub.docker.com/r/linuxserver/plex) as its base image to ensure that Plex stays up-to-date
 
## Usage 
- See [Linuxserver's Parameters](https://github.com/linuxserver/docker-plex#Parameters) for more information on parameters. 
- `VERSION=docker` must be used. Using `latest` will create errors.
- I use `/dev/shm/` as a RAMDISK for transcoding set to half the amount of my total RAM. Info on resizing [here](https://masukkhan.wordpress.com/2015/12/09/resize-devshm-filesystem-in-linux/)

docker-compose example:
``` docker-compose
services:
  plex:
    image: ghcr.io/hexeth/docker-plex-amd:master
    privileged: true
    container_name: plex
    devices:
      - /dev/dri:/dev/dri
    ports:
      - 32400:32400
      - 3005:3005
      - 5353:5353/udp
      - 8324:8324
      - 32410:32410/udp
      - 32412:32412/udp
      - 32413:32413/udp
      - 32414:32414/udp
      - 32469:32469
    environment:
      - PGID=1000
      - PUID=1000
      - TZ=${TZ}
      - PLEX_CLAIM=${YOUR_PLEX_CLAIM}
      - ADVERTISE_IP=http://${YOUR_IP}:32400
      - ALLOWED_NETWORKS=${YOUR_NETWORK}
      - HOSTNAME=${YOUR_HOST_NAME}
      - VERSION=docker
    volumes:
      - ./config:/config
      - /dev/shm:/transcode
      - /your/media:/your/media
```
