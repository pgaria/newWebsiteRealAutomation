---
title: "How to pass parameters from testng.xml file in TestNG"
date: 2017-12-28
draft: true
description: "TestNG lets you pass parameters directly to your test methods from the testng.xml file. You can also mark some parameters as optional."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,test,parameters,testng.xml,optional"
image: "/img/testng/expected-exception-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","parameters"
]
---

Making your Test Classes Thread Safe
When you parallelize your test runs by test method, you will need to make sure that shared resources within your test classes are isolated within each thread. You can do this by initializing and keeping all related resources within the test method, or by simply utilizing readily available Java libraries, which is the better solution, and will help keep your test suites clean and organized. Keeping all your resources within the test method,  while functional, will make your code a lot harder to read and maintain, and will make code reuse near impossible.

Webdriver Generation Snippet from Test Class
...
private ThreadLocal<WebDriver> webDriver = new ThreadLocal<WebDriver>();
...
protected void createDriver(String browser, String version, String os, String methodName)
           throws MalformedURLException, UnexpectedException {
        ...
       this.webDriver.set(new RemoteWebDriver(
               new URL("http://" + authentication.getUsername() + ":" + authentication.getAccessKey() +
                        seleniumURI +"/wd/hub"), capabilities));
        ...
   }
...
In addition to keeping test specific resources thread local, another best practice to keep in mind is reviewing all of your static class members, and only keeping the ones that are truly intended to be static members. This applies to all of your classes, including the test classes, page objects, and anything else that may be loaded during the test execution.
