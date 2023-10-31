# Docker Essentials: A Developer Introduction

## Contents

- [Docker Essentials: A Developer Introduction](#docker-essentials-a-developer-introduction)
  - [Contents](#contents)
  - [Lab 1: Run your first container](#lab-1-run-your-first-container)
    - [Run Container](#run-container)
    - [Run multiple containers](#run-multiple-containers)

## Lab 1: Run your first container

### Run Container

Generally, use the `docker container run` command to run a container with an image.
```bash
docker container run <image>
```

You use the `docker container run` command to run a container with the Ubuntu image by using the top command.
```bash
docker container run -it ubuntu top
```

Use `docker container start` a stopped container:
```bash
docker container start <container>
```

Run a command inside a running container:
```bash
docker container exec
```

Get running containers with:
```bash
docker container ps
```

Get all containers with:
```bash
docker container ps -a
```

### Run multiple containers

Run an NGINX (server) container:
```bash
docker container run --detach --publish 8080:80 --name nginx nginx
```

> The `--detach` option runs the container in the background.
> The `--publish` option maps port 80 in the container to port 8080 on the Docker host.
> The `--name` option assigns a name to the container. You can use the name to refer to the container in subsequent commands.
> The first nginx is the name of the image to use to create the container. The second nginx is the command to run inside the container.

Run a MongoDB (database) server container:
```bash
docker container run --detach --publish 8081:27017 --name mongo mongo:3.4
```
You must use a port other than 8080 for the host mapping because that port is already exposed on your host.
