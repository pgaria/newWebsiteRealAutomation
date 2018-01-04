---
title: "Docker Selenium WebDriver. Getting Started"
date: 2018-01-04
draft: true
description: "Retry or ReRun failed Test cases multiple times by using IRetryAnalyzer Interface. The article explains ways to rerun the TestNG tests when they are failed."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,retry,ReRun,test,FailedTest,IRetryAnalyzer,Listener"
image: "/img/testng/dependsOnGroup-method-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "Docker"
]
---
https://www.youtube.com/watch?v=S4OkrnFb-YY

### What is Docker?
[Docker](https://www.docker.com/what-docker) containers allow you to package up an application with all the basic parts to run, such as libraries and other dependencies, and ship it out as one package.

Developers use Docker to eliminate “works on my machine” problems when collaborating on code with co-workers. Companies use Docker to build agile software delivery pipelines to ship new features faster, more securely and with confidence for both Linux and Windows Server.

### Docker Terminology
#### Images
An [Image](https://docs.docker.com/glossary/?term=image) is an inert, immutable, file that's essentially a snapshot of a container.An image typically contains a union of layered filesystems stacked on top of each other. Images are created with the build command, and they'll produce a container when started with run. Images are stored in a Docker registry such as registry.hub.docker.com
#### Containers
[Containers](https://docs.docker.com/glossary/?term=container) are the instantiations or runtime instance of images. They are one form of the image. Compare it to object oriented programming, then your class would be an image and instance of the class a container. Another comparison would be to real containers on ship. They all look same from the outside, but can look very different from the inside.

There are many docker terms but the most imp are images and containers, you can have a look for all the terms in [Docker glossary](https://docs.docker.com/glossary/).

In this article we are going to learn how we can run our simple Selenium WebDriver Test cases on Docker.

# Downloading and Installing Docker
There are Docker Community Edition and Docker Enterprise Edition, So we are going to use Community edition which is free of use.

Step 1: Download Docker from https://www.docker.com/community-edition#/download.

Step 2: Depending on the Os you are using Install Docker.

After Successfully installing Docker you can see the Docker Running on top of you machine as service.

# Verify Docker:

Now after Successfully installing docker Open Your Terminal or Command Line.

On Command line we will run a simple command to check the Version
```
docker --version
```
```Text
Docker version 17.09.0-ce, build afdb6d4
```
# docker ps
Lets see how many container I have using command `docker ps` which will list down all the containers running.
Images

We dont have any container Right now so nothing as per the image.

# docker images
Lets see if we have any images using command `docker images` which will also list nothing right now.
Images

We don't have any of the images right now so nothing i displayed, but this command is for reference to list all the images.

### How to use Docker with WebDriver:
Now as we have docker setup and running, we can start working on our Selenium WebDriver integration.
