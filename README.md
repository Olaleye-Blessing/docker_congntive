# Docker Essentials: A Developer Introduction

## Contents

- [Docker Essentials: A Developer Introduction](#docker-essentials-a-developer-introduction)
  - [Contents](#contents)
  - [Lab 1: Run your first container](#lab-1-run-your-first-container)
    - [Run Container](#run-container)
    - [Run multiple containers](#run-multiple-containers)
    - [Remove the containers](#remove-the-containers)
    - [Summary](#summary)
    - [Questions summary](#questions-summary)

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

### Remove the containers

Use ` docker container stop` to stop a container:
```bash
docker container stop [container id]
```

> You can remove multiple containers at once by specifying multiple container IDs.
> ```bash
> docker container stop d67 ead af5
> ```

> You need to enter only enough digits of the ID to be unique. Three digits is typically adequate.


Remove stopped containers with:
```bash
docker system prune
```

### Summary


- Containers are composed of Linux namespaces and control groups that provide isolation from other containers and the host.
- Because of the isolation properties of containers, you can schedule many containers on a single host without worrying about conflicting dependencies. This makes it easier to run multiple containers on a single host: using all resources allocated to that host and ultimately saving server costs.
- That you should avoid using unverified content from the Docker Store when developing your own images because these images might contain security vulnerabilities or possibly even malicious software.
- Containers include everything they need to run the processes within them, so you don't need to install additional dependencies on the host.

### Questions summary

- An image is the blueprint for spinning up containers. An image is a TAR of a file system, and a container is a file system plus a set of processes running in isolation.
- Control groups (cgroups) are used to limit and monitor resource.
- LinuxKit makes it possible to run Docker containers on operating systems other than Linux.
