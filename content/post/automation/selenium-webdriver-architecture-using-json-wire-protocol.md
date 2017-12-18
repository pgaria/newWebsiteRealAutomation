---
title: "Understanding the Architecture of Selenium WebDriver"
date: 2017-11-30
draft: false
description: "Understanding the Architecture of Selenium WebDriver and what is  JSON Wire Protocol."
keywords: "Selenium,WebDriver,JSON-Wire-Protocol,HTTP,JSON,Testing"
categories: [ "Automation Testing", "Selenium WebDriver"]
image: "/img/webdriver/client-server-request-response-webdriver-thumb.png"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Test Automation",
    "JSON Wire Protocol",
    "Selenium WebDriver"
]
---
[Selenium WebDriver](http://www.seleniumhq.org/projects/webdriver/) provides a programming interface for driving the browser for automation testing. You can find the introduction of WebDriver in my other [article](https://www.pawangaria.com/post/automation/what-is-selenium-webdriver/). This article is more about the architecture and understanding how selenium WebDriver uses JSON Wire Protocol.

[JSON Wire Protocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol) is an abstract specification of how automation behavior like clicking or typing or whatever you actually want to do with your automation script is mapped to selenium or appium or HTTP requests and response.

### Why JSON Wire Protocol used in first place?
To implement a client-server architecture which can give us the following benefits.

 -  You write test in any programming language.
 - You can perform or run test on cloud services like [SauceLabs](https://saucelabs.com/), [BrowserStack](https://www.browserstack.com/) or [Selenium Grid](http://www.seleniumhq.org/projects/grid/) setup.
 - You are not bound to run test only on the local machine.
 - Different Drivers(FirefoxDriver, ChromeDriver) can be crated for browsers and separate implementation by using the same standards.

So client-server implementation requires a standard set of the specification beforehand so that Server and Client should be in sync with each other in term of what is coming and going on request and response. It's something like a language of communication with each other. So we need some common specification to solve this kind of requirement and the solution is HTTP.

### Why HTTP is the solution?
HTTP is a standard for web and can be a good base for the specification. Every [programming language](https://en.wikipedia.org/wiki/Programming_language) has a good HTTP libraries which can be used for creating client and server for request and response calls.  

### How JSON Wire protocol works with HTTP?
HTTP request and response are generally made of **verbs, route, body and response code** which I am explaining here in details:

**HTTP Verbs:**

GET: Retrieve some information from server for example: *getText, getTitle*

POST: Make something happen for example: *startSession, findElement, Click*

DELETE: Delete some resource for example: *deleteSession*   

**HTTP Route:**

There are many routes in JSON Wire Protocol used by WebDriver which you can check out [here](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol#command-summary). Few examples:
```
GET   /status

POST  /session

GET   /Session
```

![client server request response in webdriver](/img/webdriver/client-server-request-response-webdriver.png)    

**HTTP Response Code:**

HTTP status codes are not specific enough for all the kind of things might happen in Selenium WebDriver testing session. So for a specific case like *NoSuchElement* or Errors, we have a status code so that client can give a particular and useful information back to the user. There are many response codes defined in WebDriver which you can find [here](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol#response-status-codes) in details but here are some examples:
```
0:   Success
7:   NoSuchElement
11:  ElementNotVisible
200: Everything OK
500: Something is wrong
404: Resource not there
501: Valid request but action not done by the server
```
**HTTP Request and Response Body:**

Everything is JSON in request and response body. JSON is used for data transfer between client and server.

*JSON Wire Protocol Request* has route and body described in JSON like:
```
POST /session
{"desiredCapabilities" : {"browserName" : "chrome"}}
```
*JSON Wire Protocol Response* has status code and value in body like a successfull findElement request will give you the following response:
```
{"status" : 0, "value" : {"element" : "123422"}}
```
### How Test work with HTTP and JSON Wire Protocol?

HTTP is a [stateless protocol](https://stackoverflow.com/questions/13200152/why-say-that-http-is-a-stateless-protocol) which means multiple requests are not associated with each other and server is not required to track the state of a particular client's previous request. Your test might be getting an Element in one request and clicking on the same Element in some another request, So Client and Server should share [session](https://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/remote/server/Session.html), element, frame etc. with each other in subsequent requests.
So the server assigns a unique ID to these Items like session and Element and then shares them with Client. The client can decide in the request like what needs to be done on the particular ID like click on an element.

Client makes a [findElement](https://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/remote/server/handler/FindElement.html) call to server and gets the response:
  ```
  Request:  POST  /session/::sessionId/element
  Response: {“status”:0, “value”:{“element":”elementID”}}
  ```
Now client knows the elementID from the previous request's response and sends a click request with elementID.
   ```
   Request:  POST  /session/::sessionId/element/::elementID/click
   Response: {“status” : 0}
   ```
### What WebDriver is doing in JSON Wire protocol ?

Selenium WebDriver is a client which is giving an interface to write test in programming languages like Java or Python or many other languages in the market. Server doesn't care or know about what language you are using for writing your tests because it only cares about the correct protocol which is **JSON Wire protocol**. So you can also make your own Selenium Webdriver in your choice of programming language. :)  

Thanks for reading!
