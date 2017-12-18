---
title: "How to use ChromeDriver with Selenium WebDriver?"
date: 2017-12-05
draft: false
description: "What is ChromeDriver and how to use ChromeDriver in our selenium WebDriver test scripts for automation testing in chrome browser."
categories: [ "Automation Testing", "Selenium WebDriver"]
keywords: "Selenium,WebDriver,ChromeDriver,Testing"
image: "/img/webdriver/ChromeDriver-performing-actions-thumb.png"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Test Automation",
    "Selenium WebDriver"
]
---
[ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/) is a separate executable which can be used by all the Client libraries to Automate and control Chrome Browser. ChromeDriver implements all the standard for the WebDriver which is [JSON WireProtocol](https://www.pawangaria.com/post/automation/selenium-webdriver-architecture-using-json-wire-protocol/) and communicates based on those protocols. It receives HTTP request and provides HTTP response to the Client. [ChromeDriver](https://github.com/SeleniumHQ/selenium/wiki/ChromeDriver) is not client dependent and separate from the clients like Java, Python, PHP etc. You can use the same ChromeDriver with any of the programming languages as the client. [Read](https://www.pawangaria.com/post/automation/what-is-selenium-webdriver/) if you want to know more about the Selenium WebDriver.

![ChromeDriver actions using WebDriver](/img/webdriver/ChromeDriver-performing-actions.png)

ChromeDriver is maintained by the Chrome Dev team in Google these days and all the improvements are coming directly from the Chrome Dev Team. It uses Chrome DevTools to control chrome browser. ChromeDriver requires Chrome Browser installed on the local system and uses the same chrome only for performing actions.

#### How to Use ChromeDriver in Our WebDriver Test?

There are multiple ways to use the Chrome Driver in our WebDriver test cases. It depends on architecture and design you are planning for your test. Here I will try to explain few example of the common ways people are using [ChromeDriver class](https://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/chrome/ChromeDriver.html) and running Testcases on Chrome.

**From where I can download ChromeDriver?**  
You can download the ChromeDriver executable from [GoogleChrome Download URL](https://sites.google.com/a/chromium.org/chromedriver/downloads). From the download page, you can download the driver as per your operating systems like mac or windows.

**1. Run Driver From Test Script.**  
This is a basic example of the test where we are setting ChromeDriver Path in the property *webdriver.chrome.driver*. When you will Run this test the ChromeDriver will Start first in the same thread and then other Steps of @Test.

You can see the Following line in your Console:  
`
Starting ChromeDriver 2.32.498537 (cb2f855cbc7b82e20387eaf9a43f6b99b6105061) on port 12302
Only local connections are allowed.
`

```java
@Test
public void searchGoogle() {
  //Assign ChromeDriver Path to the Property.
  System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");
  //Create New ChromeDriver instance
  WebDriver driver = new ChromeDriver();
  //Open URL
  driver.get("https://www.google.com");
  //Find the search textbox WebElement and enter text
  driver.findElement(By.name("q")).sendKeys("chrome");
  //Then search button and click
  driver.findElement(By.name("btnK")).submit();
  //Quit Driver
  driver.quit();
}
```

**2. Run Driver On Local Machine.**  
Our second example is, If you want to run the chromeDriver separately in your machine and then running your test case on the same ChromeDriver Instance.

So first go to the local directory where you have Downloaded the ChromeDriver executable and start the ChromeDriver like below:

```shell
$ ./chromedriver.sh  

Starting ChromeDriver 2.33.50616 (8a06c39c4c38a9173bfb1a2) on port 9515
Only local connections are allowed.
```

Now after the ChromeDriver server started successfully on your local machine you can access the Driver from your Test.  

**How to access the chrome server running on local?**  
If you look carefully above in the console messages there is a port no **9515** in which ChromeDriver is running. So we need to give this port no in our URL while creating WebDriver Instance.

Let's rewrite the same test case again with already running ChromeDriver.

```java
@Test
 public void testOnPreRunningChromeDriver(){
   //Create WebDriver Instance With URL
   WebDriver driver = new RemoteWebDriver(new URL("http://127.0.0.1:9515"), DesiredCapabilities.chrome());
   driver.get("https://www.google.com/");
   //Find Element and Enter Text
   driver.findElement(By.name("q")).sendKeys("chrome");
   //Then search button and click
   driver.findElement(By.name("btnK")).submit();
   //Quit Driver
   driver.quit();
 }
```    
As you can see there are many ways of using the chromeDriver for the same Test. It is a good practice to implement base on your testing infrastructure and design.

In my next article, I am planning to deep dive in to ChromeDriver and try to explain what exactly happens in ChromeDriver when you run your test and how it works.

Thanks for reading!
