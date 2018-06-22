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

## Restart a running container with another command

https://stackoverflow.com/questions/32353055/how-to-start-a-stopped-docker-container-with-a-different-command

## Get a container ID made of an image

```sh
docker ps --format "{{.ID}}" --filter "ancestor=${image_name}"
```
