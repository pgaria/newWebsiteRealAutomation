---
title: "What is RemoteWebDriver in Selenium WebDriver?"
date: 2017-12-04
draft: false
description: "What is RemoteWebDriver in Selenium WebDriver and when where and how to use RemoteWebDriver in our test design?"
keywords: "SeleniumWebDriver,RemoteWebDriver,Testing,Automation"
categories: [ "Automation Testing", "Selenium WebDriver"]
image: "/img/webdriver/remote-webdriver-in-cloud-thumb.png"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Test Automation",
    "Selenium WebDriver"
]
---

#### What is RemoteWebDriver?
[RemoteWebDriver](https://github.com/SeleniumHQ/selenium/wiki/RemoteWebDriver) in [Selenium Webdriver](http://www.seleniumhq.org/projects/webdriver/) implements each of the [JSONWireProtocol commands](https://www.pawangaria.com/post/automation/selenium-webdriver-architecture-using-json-wire-protocol/) and maps them into an action that can be performed on a remote machine.

RemoteWebDriver is a [Class](http://guyhaas.com/bfoit/itp/JavaClass.html) in the `package org.openqa.selenium.remote` inside the Client Project of WebDiver.
 `RemoteWebDriver.class ` implements multiple interface like WebDriver(*Yes WebDriver is an Interface not a Class as I have seen many people confused about this information*), FindsById, FindsByClassName, FindsByLinkText etc.
```java
package org.openqa.selenium.remote;
public class RemoteWebDriver implements WebDriver, JavascriptExecutor,
      FindsById, FindsByClassName, FindsByLinkText, FindsByName,
      FindsByCssSelector, FindsByTagName, FindsByXPath,
      HasInputDevices, HasCapabilities, Interactive, TakesScreenshot
```
[JavaDoc](https://en.wikipedia.org/wiki/Javadoc) for the RemoteWebdriver explains all the details about the Class and its Methods,SubClasses and Interface implement in [gitHub Url](https://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/remote/RemoteWebDriver.html).  

**Now the question is who use this RemoteWebdriver.class?**

All the Browser Driver Class are the child class of RemoteDriver means they are extending the RemoteWebDriver.Class. Few examples of the drivers are below..
```java
public class ChromeDriver extends RemoteWebDriver
    implements LocationContext, WebStorage, HasTouchScreen, NetworkConnection {
```
```java
public class FirefoxDriver extends RemoteWebDriver {
```
```java
public class InternetExplorerDriver extends RemoteWebDriver {
```
So whenever in our test class we create a Driver instance by using the line
  `WebDriver driver = new ChromeDriver();` we are always creating an instance of the RemoteDriver Class automatically.

#### Can we use the RemoteWebDriver class directly in our Test?

Yes we can directly create an instance of the RemoteWebDriver Class as well because drivers are local  implementation that controls a browser running on the local machine, If we want to Run our Test Cases not only in our local browsers but on the distributed systems like cloud application [Sauce Lab](https://saucelabs.com/enterprise#automated-testing-platform) or [BrowserStack](https://www.browserstack.com/automate) or [Selenium Grid](http://www.seleniumhq.org/docs/07_selenium_grid.jsp). We must use RemoteWebDriver class instance and pass the proper DesiredCapabilities like:
```java
public void createChromeDriverForRemote(){
 WebDriver driver = new RemoteWebDriver(remoteUrl,
          DesiredCapabilities.chrome());
    }
```
![remote-webdriver-in-cloud](/img/webdriver/remote-webdriver-in-cloud.png)

#### How RemoteWebDriver Works?
Let's understand what are the methods involved and class RemoteWebDriver use to perform any action from our test programme.  I am going to use [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/) here for the explanation and will be trying to explain step by step.

```java
System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");
DesiredCapabilities desiredCapabilities = DesiredCapabilities.chrome();
WebDriver driver = new RemoteWebDriver(new URL(url), desiredCapabilities);
driver.get("http://www.google.com");
```
When we create an instance of the RemoteWebDriver by passing the URL and [DesiredCapabilities](https://github.com/SeleniumHQ/selenium/wiki/DesiredCapabilities) object using the following line  
`WebDriver driver = new RemoteWebDriver(new URL(url), desiredCapabilities);`

Constructor method of RemoteWebDriver class is invoked and create a session for your test scripts. There are multiple methods call inside and you receive the sessionId for you Test. RemoteWebDriver Class use two another important class: `DriverCommandExecutor.class` and `HttpCommandExecutor.class`

HttpCommandExecutor.class has a very important method as *execute* which is used to execute all the WebDriver commands and initializing the [HttpClient](https://hc.apache.org/httpcomponents-client-ga/httpclient/apidocs/org/apache/http/client/HttpClient.html) which is used to send and receive the HTTP requests between server and client.

```java
public Response execute(Command command) throws IOException {
    ...........
}
```

![execute-command-in-HttpCommandExecuter](/img/webdriver/execute-command-in-HttpCommandExecuter.png)

So every command like Get or findElement goes through this execute method and send to the server and return the Response Class object. All the details about the methods and class details are explained in the [selenium API javaDocs](https://seleniumhq.github.io/selenium/docs/api/java/overview-summary.html). I recommend everyone to [build webdriver](https://github.com/SeleniumHQ/selenium/wiki/Building-WebDriver) on there local machines and try to look into the classes and methods.
