---
title: "How Selenium-WebDriver API commands work?"
date: 2017-12-03
draft: false
description: "How Selenium WebDriver API commands work in background and understanding what happens on code level when we run our test."
keywords: "Selenium,WebDriver,Commands,Testing"
categories: [ "Automation Testing", "Selenium WebDriver"]
image: "/img/webdriver/create-session-webdriver-command-thumb.png"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Test Automation",
    "Selenium WebDriver"
]
---
[Selenium WebDriver](http://www.seleniumhq.org/projects/webdriver/) provides cool interface API's to perform many actions on the browser and simulate the user behaviors like the click or enter Text in the TextBoxes. There are many good articles about the WebDriver on the internet but the purpose of this article is to understand what happens in the background when we use actions like **[click](https://saucelabs.com/resources/articles/the-selenium-click-command), finding an element or getting title** etc. I will be using [Java](https://java.com/en/download/) as a programming language in this article.

Let's have a look at one of the [@Test](http://junit.sourceforge.net/javadoc/org/junit/Test.html) code which is performing steps like Opening [Google.com](https://www.google.com), Finding Search Button, Click SearchButton, Get Page Title and then Quit Driver instance.

```java
  @Test
  public void clickGoogleSearchAndGetTitle() throws Exception{
    System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");
    // Create new ChromeDriver
    WebDriver driver = new ChromeDriver();
    // Get Google Home Page
    driver.get("https://www.google.de/");
    // Find Search Button webElement
    WebElement element = driver.findElement(By.name("btnK"));
    // Click on the webElement
    element.click();
    // Get Title of the Page
    driver.getTitle();
    // Quit Driver
    driver.quit();
  }
```
So how WebDriver managed to perform all the user actions on browser in above @Test ? Answer is by using proper [JSON Wire Protocol used in background](https://www.pawangaria.com/post/automation/selenium-webdriver-architecture-using-json-wire-protocol/).

To understand the exact behavior of commands we must take each step separately from the example @test and I will try to describe each step at Request & Response level.

#### Create Session
The first step is to create a session with the respective driver like [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/) or [FirefoxDriver](https://github.com/mozilla/geckodriver/) and use the same session Id to communicate between client and server.  
`WebDriver driver = new ChromeDriver();`

![create-session-webdriver-command](/img/webdriver/create-session-webdriver-command.png)

Session command above created *SessionID:92cda1072b127d44da8d5651e570664d* with Server and now every request must pass the same sessionId in all the next requests.

#### Opening the Test URL
Now after creating a successful session with the driver, next step is to open URL
`driver.get("https://www.google.de/");`

![open-url-webdriver-command](/img/webdriver/open-url-webdriver-command.png)

#### Find Element
Now the Test URL is opened in the browser, next step is to find the Element on webpage
`WebElement element = driver.findElement(By.name("btnK"));`

![findElement-webdriver-command](/img/webdriver/findElement-webdriver-command.png)

#### Click Element
Now we have found the Element and received the ID assigned by the server in the response. Now we will send the click command with the same ElementID. Click request contains the session and Element Id.  
`element.click();`

![click-element-webdriver-command](/img/webdriver/click-element-webdriver-command.png)

#### Get Page Title
Our Next Step in Test code is to Get the Title from the WebPage and WebDriver use the getTitle command without any of the request Body.  
`driver.getTitle();`

![getPage-title-webdriver-command](/img/webdriver/getPage-title-webdriver-command.png)

#### Quit Driver
After every Test finished you should quit the driver, Quit call close the browser and clean the session. WebDriver use the quit command now  
`driver.quit();`

![quit-webdriver-command](/img/webdriver/quit-webdriver-command.png)
