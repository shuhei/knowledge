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
