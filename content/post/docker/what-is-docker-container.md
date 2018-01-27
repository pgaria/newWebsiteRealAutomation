---
title: "What is Docker Container"
date: 2018-01-25
draft: true
description: "Learn about Docker container, What exactly happens when container runs in you machine. "
categories: [ "Automation Testing","Docker"]
keywords: "Docker,container"
image: "/img/docker/docker-container-thumb.png"
logothumb: "/img/logothumb/Docker-logo.png"
tags: [
    "Test Automation",
    "Docker","Container"
]
---
Containers allow you to package your application software and all its dependencies together into one process package. Containers can be managed by different versions which allows teams for easy replication of your same application instance across developers/testers or in your cluster.

A container image includes everything needed to run it: code, runtime, system tools, system libraries, settings etc. Containers are lightweight, can work stand-alone and available for both Linux and Windows based apps. The best thing about container running software is that it will always run the same, regardless of the environment because Docker containers isolate applications from one another and from the underlying infrastructure. for example containers help reduce conflicts between teams running same application but different environments like Development, Testing, Staging or Production environments.

![Runtime-Instance-process-container](/img/docker/Runtime-Instance-process-container.png)

Docker containers running on a single machine share that machine's operating system kernel; they start instantly and use less resources and RAM. Images are constructed from filesystem layers and share common files. This minimizes disk usage and image downloads are much faster. Docker containers are based on open standards and run on all major Linux distributions, Microsoft Windows.


### CONTAINERS Vs. VIRTUAL MACHINES
**Containers** are an abstraction at the app layer that packages code and dependencies together. Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space. Containers take up less space than VMs (container images are typically tens of MBs in size), and start almost instantly.

**Virtual machines (VMs)** are an abstraction of physical hardware turning one server into many servers. The hypervisor allows multiple VMs to run on a single machine. Each VM includes a full copy of an operating system, one or more apps, necessary binaries and libraries - taking up tens of GBs. VMs can also be slow to boot.
![docker-container-vs-virtual-machine](/img/docker/docker-container-vs-virtual-machine.png)

### Docker File
Docker file contains a set of docker instructions, So Docker can build images automatically by reading the instructions from a Docker file. A Docker file is a simple text document that contains all the instructions and commands a user can call on the command line to create an image. `docker build` command creates an image from a Docker file. Docker File looks Like below Example:
```text
# our base image
FROM alpine:3.5

# Install python and pip
RUN apk add --update py2-pip

# copy files required for the app to run
COPY app.py /usr/src/app/
COPY templates/index.html /usr/src/app/templates/

# tell the port number the container should expose
EXPOSE 5000

# run the application
CMD ["python", "/usr/src/app/app.py"]
```

### Docker Images
An instance of an image is called a container. An image is really a template that can be turned into a container. To turn an image into a [running container](https://github.com/docker/labs/blob/master/beginner/chapters/alpine.md), the Docker engine takes your image, adds a read-write filesystem on top and initializes various settings including network ports, container name, ID and resource limits. If you start the image, you have a running container of this image. You can have many running containers of the same image and you can scale containers from image. If we try to understand in Java terms: *Images are like Java Class and containers are the instance/object of the Java Class, Like we can create any no of instance of a class we want by single line.*

Images are stored in a Docker registry such as [hub.docker.com](https://hub.docker.com/). Because they can become quite large, images are designed to be composed of layers of other images, allowing a minimal amount of data to be sent when transferring images over the network. `docker run` command is used to start a container from the Image with other command parameters.

### Docker File => Image => Container
Below image describes the process and commands used for creating a running container from simple text file.
![docker-file-image-container-flow](/img/docker/docker-file-image-container-flow.png)

### Docker Engine
Docker Engine is a client-server application which contains components like A server which is a type of long-running program called a daemon, REST API interface to talk to daemon and instruct it what to do and Client which is nothing but command line interface.

### Docker Daemon
The background service running on the host that manages building, running and distributing Docker containers.

### Docker Client
Docker client is a command line tool that allows the user to interact with the Docker daemon in client server manner using API's.

### Docker Registry/Store
Docker Registry or store is a place to store Docker images it can be Docker Hub and Docker Cloud or your private registry. You can think of the registry as a directory of all available Docker images.

### How Containers are made:
So there are many components are running in background to run a container. Docker uses a client-server architecture where the Docker client talks to the Docker daemon, which is the responsible person for downloading image, creating, running, and scaling your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over a network interface. Below in the images I have taken from Docker Website for showing the architecture:
![docker-client-server-architecture](/img/docker/docker-client-server-architecture.png)

So it's an overview of what are containers and how docker works inside and Build Container, you can read more about docker in depth in [official Documentation](https://docs.docker.com/engine/docker-overview/#next-steps).
