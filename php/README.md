Docker Image PHP packages for Yii2 Framework 
===

This repository contains source files for the Docker Image PHP containers for Yii2 Framework.

See my [Docker Hub page](https://hub.docker.com/u/voskobovich).

Supported tags and respective Dockerfile links:

1. `7.1.0-fpm-alpine`, `7.1-fpm-alpine`, `7-fpm-alpine`, `fpm-alpine` ([7.1/fpm/alpine/Dockerfile](https://github.com/voskobovich/yii2-docker/blob/master/php/7.1/fpm/alpine/Dockerfile))

## How use?

Create a new `Dockerfile` with this content

```text
FROM voskobovich/yii2-php:7-fpm-alpine

RUN chmod -R 774 /root /root/.composer

USER operator
```

The user "operator" are necessary for the maintenance of the project with "composer".

and add new service in your `docker-compose.yml` file

```yaml
version: '2'
services:
  php:
    build:
      context: <path to folder with Dockerfile>
      args:
        - GITHUB_OAUTH_TOKEN=<your GitHub token>
    container_name: php
    volumes:
      - <path to your yii2 project root>:/var/www/html
```
