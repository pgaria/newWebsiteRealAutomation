---
title: "Browser Automation from CommandLine using Curl and ChromeDriver"
date: 2017-12-14
draft: false
description: "Chrome browser can be automated and controlled from the command line or terminal using Curl and ChromeDriver. This Article will describe how to use curl with ChromeDriver."
categories: [ "Automation Testing", "Selenium WebDriver"]
keywords: "Selenium,WebDriver,ChromeDriver,Testing,Curl,CommandLine,Terminal"
image: "/img/webdriver/chrome-curl-browser-automation-thumb.png"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Test Automation",
    "JSON Wire Protocol",
    "Selenium WebDriver"
]
---
Browser automation from [command-line](https://en.wikipedia.org/wiki/Command-line_interface) is possible after the release of HTTP servers like [ChromeDriver](https://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/chrome/ChromeDriver.html), [GeckoDriver](https://github.com/mozilla/geckodriver/releases) which are implementing the [W3C WebDriver standard](https://w3c.github.io/webdriver/webdriver-spec.html) and follow all the standards of a normal server-client architecture in HTTP.

In my other posts, I have [described what is JSONWire Protocol](https://www.pawangaria.com/post/automation/selenium-webdriver-architecture-using-json-wire-protocol/) and how it became a standard for the browser automation. I also described [how we can perform browser automation using postman tool](https://www.pawangaria.com/post/automation/browser-automation-using-chromedriver-and-postman/). Now in this article, I will show you how we can use simple curl HTTP commands to drive Chrome Browser.

#### Start ChromeDriver Server:

First, [download](https://sites.google.com/a/chromium.org/chromedriver/downloads) the ChromeDriver and unzip the ChromeDriver to a particular directory. Now we need to first start the ChromeDriver Server like :

**On Linux or Mac machines:**
```
./chromedriver &
```

**On Windows machine:**
```
chromedriver.exe
```

After the successful start of the server you will see the following message on the terminal:
```
Starting ChromeDriver 2.33.506106 (8a06c39c4582fbfbab6966dbb1c38a9173bfb1a2) on port 9515
Only local connections are allowed.
```

#### Check Curl on your machine:
We will be using the [Curl](https://curl.haxx.se/) tool for the request and response, So the curl should be installed on your machine.
**What is Curl**
Curl is a command line tool to transfer data. Curl support many of the data transfer protocols like DICT, FILE, FTP, FTPS, Gopher, HTTP, HTTPS etc. Curl is free and open source project so there is no need for license or purchase. Anyone can install and start using curl.You can read more about Curl in the Following [Git Book everything-curl.](https://www.gitbook.com/book/bagder/everything-curl/details) and [Curl documentation page](https://curl.haxx.se/docs/manpage.html).  
Curl can be downloaded from their [website.](https://curl.haxx.se/download.html). Windows Users can follow instructions from the following [link](https://stackoverflow.com/questions/9507353/how-do-i-install-set-up-and-use-curl-on-windows)

**How to Check Curl on your machine**  
Open Command Line/Terminal on your machine and type following command:
```
$ curl -V

curl 7.43.0 (x86_64-apple-darwin15.0) libcurl/7.43.0 SecureTransport zlib/1.2.5
Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtsp smb smbs smtp smtps telnet tftp
Features: AsynchDNS IPv6 Largefile GSS-API Kerberos SPNEGO NTLM NTLM_WB SSL libz UnixSockets

```
This command will print the Curl version installed on your machine.

Now after running ChromeDriver and installing Curl successfully on our machine, we can start the steps for browser automation.

#### Step 1: Create Session and Open Chrome Browser:
First step is to create a session and you will see the browser is opened after this curl command.
```
curl  -d '{ "desiredCapabilities": { "caps": { "nativeEvents": false, "browserName": "chrome", "version": "", "platform": "ANY" } } }'  http://localhost:9515/session
```
```
{"sessionId":"27f4262ab044392b05138540055a8fd6","status":0}
```

*Note down the SessionID from the response as we need the session Id for future requests*
#### Step 2: Open Google.com URL:
Now after getting the session we will open the website url www.Google.com
```
curl -d '{"url":"https://www.google.com"}'http://localhost:9515/session/27f4262ab044392b05138540055a8fd6/url
```
```
{"sessionId":"27f4262ab044392b05138540055a8fd6","status":0,"value":null}
```
#### Step 3: Find the Search TextBox WebElement:
As we have opened the URL and now we need to find the Search text box WebElement.
```
curl -d '{"using":"name","value":"q"}' http://localhost:9515/session/27f4262ab044392b05138540055a8fd6/element
```
```
{"sessionId":"27f4262ab044392b05138540055a8fd6","status":0,"value":{"ELEMENT":"0.676163294199567-1"}}
```
*Note down the ELEMENT Value from the response as this ElementID is representing the Element*

#### Step 4: Enter Text in SearchBox WebElement:
Now we have the ElementID in which we want to send some text, So below Curl request will enter the text.
```
curl -d '{"value":["chrome"]}' http://localhost:9515/session/27f4262ab044392b05138540055a8fd6/element/0.676163294199567-1/value
```
```
{"sessionId":"27f4262ab044392b05138540055a8fd6","status":0,"value":null}
```

#### Step 4: Quit the Browser:
After Running the Steps We need to close the browser using the below curl command.
```
curl -X DELETE http://localhost:9515/session/27f4262ab044392b05138540055a8fd6
```
```
{"sessionId":"27f4262ab044392b05138540055a8fd6","status":0,"value":null}}
```

I have described and used very basic actions on the browser in this article but with the knowledge of [WebDiver Json Wire Protocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol), we can perform any action which can be done by Selenium WebDriver. Below is the small GIF for all the steps and action which we have discussed in this article.

![chromedriver-and-curl-to-automate-browser](/img/webdriver/chromeDriverAndCurlupdated.gif)

Thanks for Reading!
