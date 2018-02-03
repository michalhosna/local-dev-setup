# Traefik - localhost
[traefik.io](traefik.io)
- Reverse proxy


## First run

```bash
docker network create proxy
docker-compose up
```


## Status page
[http://traefik.docker/](http://traefik.docker/)

## Setup containers

```yaml
version: '3.4'
services:
  example:
    labels:
      - traefik.enable=true
      - traefik.frontend.rule=Host:example.localhost
      - traefik.port=8080
      - traefik.docker.network=proxy
    networks:
      - proxy
      - default

networks:
  proxy:
    external: true
```

Other labels in [docs](https://docs.traefik.io/configuration/backends/docker/#labels-overriding-default-behaviour)

## Tips
To list all rules in terminal (you will need [jq](https://stedolan.github.io/jq))

```bash
(curl -sk https://traefik.docker/api/providers | jq -r '.docker|.frontends|.[]|.routes|.[]|.rule') | sed 's/Host:/https:\/\//'"
```
