---
title: "How to run multiple Test Classes in TestNG?"
date: 2017-12-23
draft: false
description: "How to run multiple TestNG test classes together as a suite."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,class,suite,test,unit"
image: "/img/testng/multiple-class-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG"
]
---
In TestNG, we can divide our test cases into multiple classes. Every class can have multiple @test methods.Sometimes Class represents a particular feature and all the test cases are inside the class. So in this article, we are going to learn how we can run multiple tests having multiple classes in TestNG.

### Lets make few example test Classes:
We will first make few Test Class with multiple @Test. To run all the Classes we should create a testng.xml file.
### 1st Test Class
```Java
package com.tutorial.testng.suitetest;

import org.testng.annotations.Test;

public class TestClassOne {
    @Test()
    public void firstTestMethod() {
        System.out.println("First test method from class 1");
    }

    @Test()
    public void secondTestMethod() {
        System.out.println("Second test method from class 1");
    }
}
```
### 2nd Test Class
```Java
package com.tutorial.testng.suitetest;

import org.testng.annotations.Test;

public class TestClassTwo {
    @Test()
    public void firstTestMethod() {
        System.out.println("First test method from class 2");
    }

    @Test()
    public void secondTestMethod() {
        System.out.println("Second test method from class 2");
    }
}
```
### 3rd Test Class
```Java
package com.tutorial.testng.suitetest;

import org.testng.annotations.Test;

public class TestClassThree {
    @Test()
    public void firstTestMethod() {
        System.out.println("First test method from class 3");
    }

    @Test()
    public void secondTestMethod() {
        System.out.println("Second test method from class 3");
    }
}
```
### TestNg.xml file:
Lets create a testng.xml file which has two test tags and contains multiple test classes.
```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Suite">
  <test name="First Test">
    <classes>
      <class name="com.tutorial.testng.suitetest.TestClassOne"/>
      <class name="com.tutorial.testng.suitetest.TestClassTwo"/>
    </classes>
  </test>
  <test name="Second Test">
    <classes>
      <class name="com.tutorial.testng.suitetest.TestClassThree"/>
    </classes>
  </test>
</suite>
```
### When you Run the testng.xml file:
When You run the above testng.xml and you can see two Test Run with multiple test in a Suite. If you are new and want to read more about testng xml files check our [previous articles](https://www.pawangaria.com/post/testng/what-is-testng-xml-file/).
![multiple-test-run-testng-result](/img/testng/multiple-test-run-testng-result.png)
