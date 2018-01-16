---
title: "Selenium Grid setup with Docker-compose"
date: 2018-01-16
draft: false
description: "Setting up Selenium WebDriver Grid can be easy using docker-compose.yml file. In this Article we are going to talk about docker-compose and how to use it in our Grid SetUp."
categories: [ "Automation Testing","Docker"]
keywords: "TestNG,Java,Docker,grid,container,run,test,webdriver,Docker-compose"
image: "/img/docker/selenium-docker-thumb.png"
logothumb: "/img/logothumb/Docker-logo.png"
tags: [
    "Test Automation",
    "Docker","Selenium WebDriver","Docker-compose"
]
---
The most simple way to start a grid is with docker-compose, use the following snippet as your docker-compose.yaml

Step 1: Download create one yml file like this docker-compose.yml
```yaml
#To execute this docker-compose yml file use docker-compose up -d

selenium-hub:
  restart: always
  image: selenium/hub:latest
  ports:
    - "4444:4444"
#selenium-chrome
selenium-chrome:
  restart: always
  image: selenium/node-chrome-debug
  stdin_open: true
  links:
    - selenium-hub:hub
#selenium-firefox 
selenium-firefox:
  restart: always
  image: selenium/node-firefox-debug
  links:
    - selenium-hub:hub
```
Step 2: To spin up Selenium Grid just run this command where your yml file is placed, docker-compose up -d .

Step 3: http://localhost:4444/grid/console should show up hub with one chrome and one firefox node.

### Scale up Selenium Grid Docker instance
Step 1: docker-compose scale selenium-chrome=5 selenium-firefox=5

Step 2: Bang! Now you have 10 machines(Nodes / Containers) Instead of setting up 10 Mac Mini's or 10 Linux machines to run your Selenium tests.

Step 3 : Check the hub url again on your browser, you should be able to see something like this.
