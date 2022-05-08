# Container for nextcloud-plus

This container is tailored for the [nextcloud-plus rockon](https://github.com/freaktechnik/rockons/blob/main/nextcloud-plus.json). It is based on [nextcloud-aio](https://github.com/nextcloud/all-in-one) which in turn takes [the official fpm example with supervisor](https://github.com/nextcloud/docker/blob/main/.examples/dockerfiles/full/fpm/Dockerfile) and enhances it.

## Enhancements

- Supports redis/nextcloud push
- Supports MediaDC
- Has imagemagick
- Has samba
- Does cron within the container (no need for webcron or similar)

## Routing

The setup requires a reverse proxy in front that handles the following rules:

- /push/* goes to the push port of this container (7867)
- /browser/* goes to the CODE server (9980)
- /hosting/* goes to the CODE server (9980)
- /cool/* goes to the CODE server (9980)
- FPM handler to port 9000 of the container
- everything else goes to the web directory passed to this.

## Public image

https://hub.docker.com/r/freaktechnik/nextcloud-plus
