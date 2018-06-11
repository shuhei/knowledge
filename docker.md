# Docker

## Docker Machine

```sh
docker-machine ls
docker-machine start default
eval $(docker-machine env)
```

## Spot run with terminal

```sh
docker images # to note an image ID to run
docker run -it --rm ${docker_image} /bin/sh
```

## Login to a running container

```sh
docker ps # to note an container ID to login
docker exec -it ${docker_container} /bin/sh
```
