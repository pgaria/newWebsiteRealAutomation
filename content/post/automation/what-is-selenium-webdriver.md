---
title: "What is Selenium WebDriver?"
date: "2017-11-28T12:00:00-00:00"
author: "Pawan Garia"
draft: false
description: "What is Selenium WebDriver and how it works in background."
summaryDescription: "An introduction about Selenium WebDriver."
keywords: "Selenium,WebDriver,Testing,JSON Wire protocol"
categories: [ "Automation Testing", "Selenium WebDriver"]
image: "/img/webdriver/ClientserverWebdriver-thumb.png"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Test Automation",
    "Selenium WebDriver"
]
---

[Selenium WebDriver](http://www.seleniumhq.org/projects/webdriver/) is an open source application for driving browsers for automation testing. Using WebDriver libraries you can drive the browser and perform actions like Click, Open URL, Enter Text, Clear Text, Take Screenshot of browser and many more actions.
WebDriver works on a very simple principle of [Client Server architecture](https://en.wikipedia.org/wiki/Client%E2%80%93server_model). The communication between the server and client is through [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) request and response.

WebDriver uses [JSON](https://www.json.org/) wire protocol works on [RESTful Web Services](https://docs.oracle.com/javaee/6/tutorial/doc/gijqy.html) over HTTP. In simple terms, WebDriver uses JSON wire protocol for communication between client libraries like Java, PHP, C#, and servers which are different drivers like ChromeDriver, FirefoxDriver etc. WebDriver is nothing but sending request and response.

### What is JSON Wire protocol?

JSON (JavaScript Object Notation) is a lightweight format for data exchange between client and server. Applications use JSON objects to send and receive data between each other in the web world. JSON data structure is industry standard and can be used for sending and receiving data as [Key & Value pair](https://developers.squarespace.com/what-is-json/). Some people say its a very nice alternative for [XML](https://www.w3schools.com/xml/).  We can save JSON files as .json extension.

### How JSON looks like?

A simple json file looks like below and there are many [online editors](http://www.jsoneditoronline.org/) which can be used to edit and verify JSON structure.

```json
{
 "Student":{
   "FirstName":"Pawan",
   "LastName":"Garia",
   "IdNumber":"12345",
   "City" : "New Delhi",
   "EmailID" : "email@gmail.com" }
}
```



### How WebDriver interacts with Browsers?

WebDriver does not directly interact with the browser. WebDriver creates a request and send the request to the proxy driver server which is running in between the Client and Browser. For example, to run WebDriver script on Chrome Browser we need to first start the ChromeDriver server and then driver launches the Chrome Browser and starts listening on a particular PORT like localhost:12570. Listening on the [PORT](https://www.lifewire.com/port-numbers-on-computer-networks-817939) is nothing but accepting the HTTP request methods which can be GET, POST, DELETE etc. which are HTTP standard methods.
![Client server WebDriver](/img/webdriver/ClientserverWebdriver.png)
### What is next?

In the next articles, I will be covering interaction between the Client, Driver and Browser with examples and depth. I am going to explain how a particular action get performed on browser using [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/).

Thanks for reading!
