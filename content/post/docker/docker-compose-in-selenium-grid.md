---
title: "Selenium Grid setup using Docker-compose.yml"
date: 2018-01-23
draft: false
description: "Setting up Selenium WebDriver Grid can be easy using docker-compose.yml file. In this Article, we are going to talk about docker-compose and how to use it in our Grid SetUp."
categories: [ "Automation Testing","Docker"]
keywords: "TestNG,Java,Docker,grid,container,run,test,webdriver,Docker-compose"
image: "/img/docker/selenium-docker-thumb.png"
logothumb: "/img/logothumb/Docker-logo.png"
tags: [
    "Test Automation",
    "Docker","Selenium WebDriver","Docker-compose"
]
---
In our last Article, we learned steps to create Selenium Grid using Docker. In our article, we used all the steps from the command line. Now there is a better way to setup Grid using docker-compose file. Using the Docker compose file It's very easy to up and down and scale our grid by just running the File.

### #What is Docker-Compose File?
[Compose](https://github.com/docker/compose) is a tool for defining and running multi-container Docker applications. With Compose, you use a Compose file to configure your application's services. Then, using a single command, you create and start and stop all the services from your configuration.
Compose tool in docker can be used using a simple Compose file which is a YAML file defining services, networks, and volumes.

### #Create a Docker-Compose File For Selenium Grid:
Below is the docker-compose.yml file which is used for the Selenium Grid. If you look into the file we can see the selenium-hub running at `4444` port and then selenium-chrome and selenium-firefox images. *We are not providing any names for the node and Docker will give a random name to all the nodes.*

**Create docker-compose.yml file**
```yaml
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
**Start up Selenium Grid:**  
To Start up Selenium Grid, Go to the directory where docker-compose.yml file is placed and Run following command:
```
docker-compose up -d
```
```
~ docker-compose up -d
Creating pawangaria_selenium-hub_1 ...
Creating pawangaria_selenium-hub_1 ... done
Creating pawangaria_selenium-firefox_1 ...
Creating pawangaria_selenium-chrome_1 ...
Creating pawangaria_selenium-firefox_1
Creating pawangaria_selenium-chrome_1 ... done
```
**Check Containers:**  
After running the docker compose file we should check the containers and selenium grid is up and running using below command:
```
docker ps
```
![docker-ps-selenium-grid-start](/img/docker/docker-ps-selenium-grid-start.png)

**Check Console:**  
Open the browser and open the URL `http://localhost:4444/grid/console` which will open the Selenium Grid Console. We can verify hub with chrome and firefox browser nodes. *You can check the Browser Version also in the console.*
![docker-selenium-grid-console-web](/img/docker/docker-selenium-grid-console-web.png)

### #Scale up Docker Selenium Grid Instance:
Now we have docker-compose file with all the configurations and It's very easy to [scale](https://docs.docker.com/compose/reference/scale/) your containers by just one command. We can create **3 chrome and 3 firefox** browser instance in below command:
```
docker-compose scale selenium-chrome=3 selenium-firefox=3
```
**Check console after Node scale:**  
Check the hub URL `http://localhost:4444/grid/console` again on your browser, You can see all the **3 chrome and 3 firefox** nodes up and running.
![docker-scale-selenium-grid-up](/img/docker/docker-scale-selenium-grid-up.png)

### #How to Stop Selenium Grid:
It's easy to stop Selenium Grid using a simple command with docker-compose file:
```
docker-compose down
```
```
➜  ~ docker-compose down
Stopping pawangaria_selenium-firefox_3 ... done
Stopping pawangaria_selenium-firefox_2 ... done
Stopping pawangaria_selenium-chrome_3  ... done
Stopping pawangaria_selenium-chrome_2  ... done
Stopping pawangaria_selenium-firefox_1 ... done
Stopping pawangaria_selenium-chrome_1  ... done
Stopping pawangaria_selenium-hub_1     ... done
Removing pawangaria_selenium-firefox_3 ... done
Removing pawangaria_selenium-firefox_2 ... done
Removing pawangaria_selenium-chrome_3  ... done
Removing pawangaria_selenium-chrome_2  ... done
Removing pawangaria_selenium-firefox_1 ... done
Removing pawangaria_selenium-chrome_1  ... done
Removing pawangaria_selenium-hub_1     ... done
➜  ~
```
### What's Next:
As Scaling Selenium Grid in docker is pretty easy and simple, Now you can try to Run test parallel on the Grid and multi browser Test cases. In next article we will try to enhance our grid setup and Alternatives for Selenium docker images.  
