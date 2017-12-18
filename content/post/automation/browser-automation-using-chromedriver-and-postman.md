---
title: "Browser automation using ChromeDriver and Postman"
date: 2017-12-08
draft: false
description: "How to use ChromeDriver without Selenium WebDriver Client and How can we perform actions on chrome browser using PostMan for API calls?"
categories: [ "Automation Testing", "Selenium WebDriver"]
keywords: "Selenium,WebDriver,ChromeDriver,Testing,PostMan"
image: "/img/webdriver/create-session-request-postman-thumb.png"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Test Automation",
    "JSON Wire Protocol",
    "Selenium WebDriver"
]
---
If you are using WebDriver or learning how to use Selenium for test automation, you must be using some programming language like Java or PHP, etc. for automation and performing actions on browsers like Chrome. But in this article we are not going to use WebDriver Client Like Java or PHP, etc. and we will perform actions like open URL, Click Button or Enter Text using ChromeDriver and PostMan tool.

I am going to use [JSON Wire Protocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol) for driving chrome driver. You can read more in details about [what is JSON Wire Protocol in my old posts.](https://www.pawangaria.com/post/automation/selenium-webdriver-architecture-using-json-wire-protocol/)

#### Download ChromeDriver and PostMan
The first step is to download the ChromeDrive executable from Google ChromeDriver Page.
If you already have ChromeDriver downloaded, you can use the same executable. I am going to use the ChromeDrive for MAC OS here in this article. Windows users can also download the .exe file and perform all the steps.

We also need to download [Postman](https://www.getpostman.com/). PostMan is a super cool tool for API development. You can send and receive API request from PostMan. you can read more about [Postman Download Page](https://www.getpostman.com/postman).

#### Start ChromeDriver on Terminal:
After downloading ChromeDriver we need to first start the ChromeDriver executable on the local machine.
```
$ Downloads ./chromedriver

Starting ChromeDriver 2.33.506 (8a06c39c9173bfb1a2) on port 9515
Only local connections are allowed.
```
Now we have a successfully running ChromeDriver. Please pay attention to the port no in the Starting message of ChromeDriver. It always starts on 9515 by default and start listening on the same port.

#### Open Google Chrome Browser:
Now open Postman which is installed on your machine and you should make a POST call to ChromeDriver.
![create-webdriver-session-from-postman](/img/webdriver/create-session-request-postman.png)

**POST:** `http://localhost:9515/session`  

**Request Body:**
```json
{
    "desiredCapabilities": {
        "caps": {
            "nativeEvents": false,
            "browserName": "chrome",
            "version": "",
            "platform": "ANY"
        }
    }
}
```

**Response:**
```json
{
    "sessionId": "05567e0fc54f5859a7418632a8988cc7",
    "status": 0,
    "value": {
        "acceptSslCerts": true,
        "browserName": "chrome",
        "chrome": {
            "chromedriverVersion": "2.33.506 (8a06c39c9173bfb1a2)",
            "userDataDir": "/var/folders/v2/5jldd5952fng_t6cbl3064csc8p6dn/T/.org.chromium.Chromium.0ETZRP"
        },
        "cssSelectorsEnabled": true,
        "pageLoadStrategy": "normal",
        "platform": "Mac OS X",
        "version": "62.0.3202.94",
    }
}
```
*You should save the SessionID from the Response you received as we are going to need SessionID for further communication.*

#### Now let's redirect to the URL:
Now As you can see the ChromeBrowser is open and ready to open any URL.
We need to do POST call again with the session ID which we saved from our last API response.You can replace the sessionID which I have used within the request.

![get-url-post-request-from-postman](/img/webdriver/get-url-post-request-postman.png)

**Example:** `http://localhost:9515/session/:SessionID/url`  

**Post:** `http://localhost:9515/session/05567e0fc54f5859a7418632a8988cc7/url`

**Request Body:**
```json
{"url":"https://www.google.com"}
```

**Response:**
```json
{
    "sessionId": "05567e0fc54f5859a7418632a8988cc7",
    "status": 0,
    "value": null
}
```
#### Now let's find an Element on the page:
Now we have the URL opened in our chrome browser and lets find the search textBox from the page in our next request.
![post-request-find-searchButton-postman](/img/webdriver/post-request-find-searchButton-postman.png)

**Post:** `http://localhost:9515/session/05567e0fc54f5859a7418632a8988cc7/element`

**Request Body:**
```json
{"using":"name","value":"q"}
```

**Response:**
```json
{
    "sessionId": "05567e0fc54f5859a7418632a8988cc7",
    "status": 0,
    "value": {
        "ELEMENT": "0.1450256429796304-1"
    }
}
```
*You should save the ELEMENT value from the response as this is the Element ID and we need to pass this in next request*

#### Now let's send the value "Chrome" to this search Element textBox:
Now we have received the elementID of the SearchTextBox and now we can send any Text to this TextBox using the POST call.

![enter-text-in-chrome-request-postman](/img/webdriver/enter-text-in-chrome-request-postman.png)
**Example:** `http://localhost:9515/session/:sessionID/element/:elementID/value`

**Post:** `http://localhost:9515/session/05567e0fc54f5859a7418632a8988cc7/element/0.1450256429796304-1/value`

**Request Body:**
```json
{"value":["chrome"]}
```

**Response:**
```json
{
    "sessionId": "05567e0fc54f5859a7418632a8988cc7",
    "status": 0,
    "value": null
}
```

#### Now Quit Driver instance:
Now to Quit Driver session and close the Chrome Browser, we should send the DELETE request with the session ID.

**DELETE:** `http://localhost:9515/session/05567e0fc54f5859a7418632a8988cc7`

**Response:**
```json
{
    "sessionId": "05567e0fc54f5859a7418632a8988cc7",
    "status": 0,
    "value": null
}
```
I have used [JsonWireProtocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol#command-reference) standards about requests to perform few actions on the browser. You can try to perform more actions using the PostMan tool.

If you don't want to use PostMan tool for API requests and you can use Java as programming language with any HTTPClient available.

Thanks for reading!
