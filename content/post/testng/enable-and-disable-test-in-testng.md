---
title: "How to Enable and Disable @Test in TestNG"
date: 2017-12-17
draft: true
description: "How to Enable and Disable particular Test in our TestNG Test Class. TestNG can decide which Test should be included in the run or ignored."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,enabled,disable,test,unit"
image: "/img/testng/enable-disable-test-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","enabled"
]
---
TestNG provides an attribute for enabling and disable our @Test. There are many times when you don't want to run particular @Test and it's not a better way to remove the whole Test method from the class and then add again if you want to run the @Test again.

>By default all the TestNG @Test methods are Enabled

### How to Enable and Disable @Test:

To enable and disable @test, There is an attribute **enabled** which accepts Boolean type value as True or False.

```Java
package com.tutorial.testng;

import org.testng.Assert;
import org.testng.annotations.Test;

public class EnabledTestMethod {
    // By Default test as True
    @Test
    public void byDefaultTrue() {
        System.out.println("Running: default as true");
    }

    // Enabled Test
    @Test(enabled = true)
    public void enabledTest() {
        System.out.println("Running: enabled = true");
    }

    // Disabled Test
    @Test(enabled = false)
    public void disabledTest() {
        System.out.println("Running: enabled = false");
    }
}
```
### When You Run the Class:
When You run the above Class and you can see only Two @test case ran and the 3rd @test method `public void disabledTest()` ignored.
![enable-disable-test-testng-result](/img/testng/enable-disable-test-testng-result.png)
