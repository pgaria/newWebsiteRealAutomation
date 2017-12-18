---
title: "How ChromeDriver works in the background?"
date: 2017-12-11
draft: false
description: "What is ChromeDevTool and how ChromeDriver is able to perform actions on chrome using Selenium WebDriver? How ChromeDriver server works in the background?"
categories: [ "Automation Testing", "Selenium WebDriver"]
keywords: "Selenium,WebDriver,ChromeDriver,server,background,testing"
image: "/img/webdriver/chromedriver-server-start-messages-thumb.png"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Test Automation",
    "Selenium WebDriver"
]
---
ChromeDriver is a [Chromium project](https://www.chromium.org/) and ChromeDriver code is inside the Chromium repository. Chromium is an open source project started by Google to provide source code for Chrome browser. Chromium projects are divided into mainly two projects [Chromium](https://www.chromium.org/Home) browser and [Chromium OS](https://www.chromium.org/chromium-os) which is an open source project for OS based browsers, you can watch [What is Chrome OS Video](https://www.youtube.com/watch?v=0QRO3gKj3qw) for more information.

Chromium dev teams are now responsible for the maintenance of the ChromeDriver code and updates which we use with Selenium WebDriver for automating Chrome. As it's an open source project you can also contribute. In my previous articles I have explained about [how ChromeDriver can be used with Selenium WebDriver](https://www.pawangaria.com/post/automation/how-to-use-chromedriver-with-selenium-webdriver/) and [How ChromeDriver uses JSON Wire Protocol and perform actions on Chrome](https://www.pawangaria.com/post/automation/browser-automation-using-chromedriver-and-postman/).

In this article, we are going to deep-dive into ChromeDriver and try to understand how it works in the background and what technology is been used for making ChromeDriver so cool.

ChromeDriver is made in C++ programming language and uses the [Chrome DevTool](https://developers.google.com/web/tools/chrome-devtools/) which is a debugging and profiling tool made for Chrome DevTool has defined a set of standard protocols named as [Chrome DevTools protocols](https://chromedevtools.github.io/devtools-protocol/) which can be used by any tools to an instrument, inspect, debug Chrome browser. The protocol API's are too used for sending the request to chrome dev tool for any debugging. *So ChromeDriver also uses and follows the Chrome DevTools Protocols internally to send and receive the request to devTool for performing actions on browser.*

### What happens when we start ChromeDriver Server:
So to perform any of the actions on the browser the ChromeDrier server should be started.

`//src/chrome/test/chromedriver/server/chromedriver_server.cc` file which is a C++ file where main method start the execution and start an HTTP server using **HttpServer Class** inside the same namespace file which is accepting WebDriver protocol requests. ChromeDriver always starts on port no 9515.
![chromedriver-main-method-with-portno](/img/webdriver/chromeserver-main-method.png)

when you run your test you should have noticed in console or logs, there are messages about ChromeDriver version with starting on the port are also coming from the same file. In below screenshot, I have highlighted the printf() message in the red color.

![chromedriver-server-start-messages](/img/webdriver/chromedriver-server-start-messages.png)

### WebDriver Protocol URL's to C++ functions:
There is `//src/chrome/test/chromedriver/server/http_handler.cc` file which is responsible for handling your requests coming from the Selenium WebDriver which is nothing but JsonWireProtocol requests.
This is the namespace file **http_handler.cc** contains variable for strong sessions and methods like commandMapping, WrapToCommand, PreperaingRequest etc. types of methods.
![chromedriver-session-storage-variable](/img/webdriver/chromedriver-session-store-variable.png)

If we send a request to find element command with the following POST URL, this is the place where this request is been handled and wrapped in to command for ChromeDev Tool Protocol. Let's take an example of finding a WebElement, Send Text to the Element HTTP request or command explained below.

**WebDriver Code:** `driver.findElement(By.name("q")).sendKeys("chrome");`  
**Post URL:** `http://severIP:9515/session/:sessionID/element/:elementID/value`
**Request Body:**
```json
{"value":["chrome"]}
```
**http_handler.cc**
![chromedriver-server-http-handler-command-mapping](/img/webdriver/chromedriver-server-httphandler-commandmapping.png)

You should check out more commands wrapping in the [http_handler.cc in ChromeDriver codebase URL.](https://cs.chromium.org/chromium/src/chrome/test/chromedriver/server/http_handler.cc?sq=package:chromium&l=90)

### Chrome DevTools Protocol Commands:

When you start running your WebDriver test ChromeDriver server generates a log file in the system Temp location and you can check the Request and response server is receiving. In log files, you can find the Commands sent to the Chrome DevTool. For example, the Mouse Click ChromeDevTool commands look like below:  
```json
DEVTOOLS COMMAND Input.dispatchMouseEvent (id=32) {
   "button": "left",
   "clickCount": 1,
   "modifiers": 0,
   "type": "mousePressed",
   "x": 534,
   "y": 411
}
```
The same [Input.dispatchMouseEvent](https://chromedevtools.github.io/devtools-protocol/tot/Input/#method-dispatchMouseEvent) used to click on elements request is described in DevTool Protocol document, where we can compare the JSON request from above in the screenshot below.
![chromeDevTool-mouse-click-command](/img/webdriver/chromeDevTool-mouse-click-command.png)

So just to click on an Element there are many steps and commands executed in the background and I hope this article would be able to give a start for reading more details.

Thanks for reading!
