---
title: "Debugging Docker Container with RealVNC Viewer"
date: 2018-01-15
draft: false
description: "You can set up your VNC connection and then run your tests on the Docker container all viewed through the selenium container’s Chrome/Firefox web browser."
categories: [ "Automation Testing","Docker"]
keywords: "TestNG,Java,Docker,container,debug,RealVNC,viewer"
image: "/img/docker/selenium-docker-thumb.png"
logothumb: "/img/logothumb/Docker-logo.png"
tags: [
    "Test Automation",
    "Docker","Selenium WebDriver"
]
---
[In our last article](https://www.pawangaria.com/post/docker/how-to-use-docker-with-webdriver/), we talked about basics of running automation Test on Docker. Now, this article is more about debugging the Test running on the containers. It's helpful when you want to see what is happening inside the docker container.
In the event you wish to visually see what the browser is doing you will want to run the debug variant of the node or standalone images. A VNC server will run on particular port inside the container with [Selenium WebDriver](http://www.seleniumhq.org/projects/webdriver/).

### Download RealVNC viewer:
You can download the [real-vnc](https://www.realvnc.com/en/connect/download/viewer/macos/).
 and Install as per your operating system.

### Pull Selenium Chrome-Debug Image:

Step:1 For the Debugging we should require the Docker Selenium WebDriver Image with VNC server.So First Step is to download the Docker Image with Debugging.
```
docker pull selenium/standalone-chrome-debug
```
Docker pull will start downloading the image if it's not already downloaded.

Step 2: After downloading the proper debugging image, we need to start the docker container for the image:
```
docker run -d -P selenium/standalone-chrome-debug
```
Step 3: After running the Docker container we should verify the Container Process and Port Details.
```
docker ps
```
![docker-ps-command-debug-port](/img/docker/docker-ps-debug-port.png)
You can acquire the port by looking at the container details Like from the Image above we can see the
VNC server is listening on the Port No: 32771

### Start VNC and Create VNC:

Step 1: Open real VNC viewer downloaded in the previous Steps.

Step 2: From File Menu select: Create New Connection

Step 3: Enter Value of VNC Server as **localhost:<port No>**
![create-new-vnc-connection-docker](/img/docker/create-new-vnc-connection-docker.png)
Step 4: Now You will be able to see the VNC server created and Open the Connection.

Step 5: When you are prompted for the password it is `secret`
![vnc-connection-password-window](/img/docker/vnc-connection-password-window.png)

Now Our Selenium WebDriver container is Running and VNC server is Running. We can now run our Test and we will be able to see the Browser driving the Test cases on VNC server.

### Create WebDriver Test and Run Test:
Create a simple Java test which is opening the Google.com in the container and Post Number should be edited from the container details.
```Java
import java.net.URL;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.remote.RemoteWebDriver;
import org.testng.annotations.Test;

public class RunningTestOnDocker {

    @Test
    public void runTestOnDocker() throws Exception {
        DesiredCapabilities dcap = DesiredCapabilities.chrome();
        String driverPath = System.getProperty("user.dir") + "/exe/chromedriver";
        System.setProperty("webdriver.chrome.driver", driverPath);

        // You should check the Port No here.
        URL gamelan = new URL("http://localhost:32772/wd/hub");
        WebDriver driver = new RemoteWebDriver(gamelan, dcap);
        // Get URL
        driver.get("https://www.google.com/");
        // Print Title
        System.out.println(driver.getTitle());
    }
}
```
**Running Our Test on the correct Port number will Open the Google in our Container and We can see the Browser running in our VNC Server.**
![vnc-view-browser-open-docker](/img/docker/vnc-view-browser-open-docker.png)

#### Benefits of using VNC and Debug:

Using the VNC server are helpful in debugging and seeing what is happening in the Test while running inside the container. There are many Selenium WebDriver Images providing the test to be recorded as video.
