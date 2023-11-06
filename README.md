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
  - [Lab 2: Add CI/CD value with Docker images](#lab-2-add-cicd-value-with-docker-images)
    - [Create and build the Docker image](#create-and-build-the-docker-image)
    - [Run the docker image](#run-the-docker-image)
    - [Push to a central registry](#push-to-a-central-registry)

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

## Lab 2: Add CI/CD value with Docker images

### Create and build the Docker image

A Dockerfile is basically a text document that contains all the commands a user could call on the command line to assemble an image.

A typical Dockerfile looks like this:
```Dockerfile
FROM python:3.6.1-alpine
RUN pip install --upgrade pip
RUN pip install flask
CMD ["python","app.py"]
COPY app.py /app.py
```

- `FROM python:3.6.1-alpine` - This is the starting point for your Dockerfile. Every Dockerfile typically starts with a FROM line that is the starting image to build your layers on top of.
The alpine version means that it uses the alpine distribution, which is significantly smaller than an alternative flavor of Linux. A smaller image means it will download (deploy) much faster, and it is also more secure because it has a smaller attack surface.
- `RUN pip install flask` - The `RUN` command executes commands needed to set up your image for your application, such as installing packages, editing files, or changing file permissions. The `RUN` commands are executed at build time.
- `CMD ["python","app.py"]` - CMD is the command that is executed when you start a container.
There can be only one CMD per Dockerfile. If you specify more than one CMD, then the last CMD will take effect.
- `COPY app.py /app.py` - Copies the `app.py` file in the local directory (where you will run `docker image build`) into a new layer of the image.
This instruction is the last line in the Dockerfile. Layers that change frequently, such as copying source code into the image, should be placed near the bottom of the file to take full advantage of the Docker layer cache. This allows you to avoid rebuilding layers that could otherwise be cached. For instance, if there was a change in the FROM instruction, it will invalidate the cache for all subsequent layers of this image.

Verify a Dockerfile with:
```bash
vi Dockerfile
```

Build a Docker image with `docker image build`:
```bash
docker image build -t python-helloworld .
```

> The `-t` option tags the image with a name, so you can easily refer to it later.
> The `.` at the end of the command tells Docker to look for the Dockerfile in the current directory which is the root of the project.

Verify that your image shows up in the list of images on your system with:

```bash
docker image ls
```

### Run the docker image

You can run the image after a successful build with:

```bash
docker run -p 5001:5000 -d python-helloworld
```

> The -p flag maps a port running inside the container to your host. In this case, you're mapping the Python app running on port 5000 inside the container to port 5001 on your host. Note that if port 5001 is already being used by another application on your host, you might need to replace 5001 with another value, such as 5002.
> The -d flag runs the container in detached mode, leaving the container running in the background.

Check logs with:

```bash
docker container logs <container id>
```

### Push to a central registry

- Log in to docker hub with:
  
  ```bash
  docker login
  ```

- Tag the image with:

  ```bash
  docker tag <image_name> <docker_hub_username>/<image_name>
  ```

- Push the image to docker hub with:

  ```bash
  docker push <docker_hub_username>/<image_name>
  ```
