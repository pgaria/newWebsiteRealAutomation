---
title: "What is TestNG?"
date: 2017-12-15
draft: false
description: "TestNG is a testing framework inspired by JUnit and NUnit but introducing some new functionalities that make it more powerful and easier to use."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,Eclipse,testing,unittesting"
image: "/img/testng/what-is-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG"
]
---
[TestNG](http://testng.org/doc/) is a test management framework inspired from [JUnit](http://junit.org/junit5/) and [NUnit](http://nunit.org/) frameworks. TestNG provided and introduced new functionalities and features which make TestNg very powerful and mostly used test management framework in Java programming language. As per the creator of TestNG Mr. Cédric Beust *NG* stands for *next generation*.

TestNG can be used in all the phases of testing like Unit testing, integration testing or system testing. TestNG provides better support compare to Junit and other frameworks as the new release and bug fixes are very frequent compared to Junit. TestNG is supported by a variety of tools and plug-ins like (Eclipse, IntelliJ IDEA, Maven, Ant etc.

### How to use TestNG?
TestNG can be used as the [maven](https://maven.apache.org/) dependency in our project or TestNg can be placed in the build path as the jar file.In this article, we will be using maven as dependency management tool.  

**Tools we will use:**  

* TestNG
* Maven
* Eclipse


### TestNG maven Dependency
We need to add TestNG dependency in to our [Pom.xml](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html) file.
Pom is an XML file that contains information about the project and configuration details used by Maven to build the project.

```xml
<dependency>
  <groupId>org.testng</groupId>
  <artifactId>testng</artifactId>
  <version>6.11</version>
  <scope>test</scope>
</dependency>
```
You can get the latest version and dependency detail from the [maven central repository](https://mvnrepository.com/artifact/org.testng/testng).

### How to use TestNG in My Test Class?

**Create an Addition class for adding two integer values:**
```Java
package com.tutorial.testng;

public class Addition {

    public int addValues(int a, int b) {
        return a + b;
    }
}
```
**Now we are going to create Test Class:**
```Java
package com.tutorial.testng;

import org.testng.Assert;
import org.testng.annotations.Test;
import com.tutorial.testng.Addition;

public class TestngIntroductionTest {

    @Test
    public void verifyAdditionOfIntegers() {

        Addition obj = new Addition();
        int additionResult = obj.addValues(5, 4);

        Assert.assertNotNull(additionResult);
        Assert.assertEquals(additionResult, 9);
    }
}
```
This is a simple class where *@Test* is defined as Test method. So every method annoted by *@Test* in our class will be considered as Test method or test case sometime.

### How to run my Test Class?
Now the next step is running our Test and for that, we will use Eclipse TestNG plugin which is an IDE.

**Install TestNG Eclipse plugin:**  
To run and use TestNg in eclipse IDE we need to install the TestNG plugin in our Eclipse application. If you don't have TestNG installed already, you can follow the steps mentioned in the [TestNg official download](http://testng.org/doc/download.html) or [eclipse guide](http://testng.org/doc/eclipse.html).

Now to Run our test *Right Click* on the Test Class and click *Run As -> TestNG Test*.

![testng-eclipse-test-run](/img/testng/right-click-on-test-class-testng-run.png)

After successfully running the Test, You will see the test results in two places:  
**Eclipse TestNG plugin View:**
![testng-plugin-test-result](/img/testng/testng-plugin-test-result.png)

**Eclipse Console View:**
![console-test-result](/img/testng/console-test-result.png)
