---
title: "How to Retry failed tests in TestNG?"
date: 2018-01-04
draft: false
description: "Retry or ReRun failed Test cases multiple times by using IRetryAnalyzer Interface. The article explains ways to rerun the TestNG tests when they are failed."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,retry,ReRun,test,FailedTest,IRetryAnalyzer,Listener"
image: "/img/testng/dependsOnGroup-method-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","retry-test",Listeners"
]
---
In automation Test Suits or running test cases in a bulk suite, we face unexpected Test failure or flaky test caused by network glitch, time-out or some other problem. There is a need for running the failed test cases so that we can get the proper result for the flaky TestCase.

>TestNG provides [IRetryAnalyzer interface](http://static.javadoc.io/org.testng/testng/6.11/org/testng/IRetryAnalyzer.html) which can be used to run and configure your retry related logic.

### How to use IRetryAnalyzer:
First of all, we should create a class which implements the `IRetryAnalyzer` interface which has one method `public boolean retry(ITestResult result);` which needs to be implemented. This method will return True if a test needs to be reRun or else False.

#### Create TestRetryAnalyzer.class
Update method which decides how many times a test needs to be rerun. TestNG will call this method every time a test fails. So we can put some code in here to decide when to rerun the test.
```Java
import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class TestRetryAnalyzer implements IRetryAnalyzer {
    int counter = 1;
    int retryMaxLimit = 3;

    public boolean retry(ITestResult result) {
    if (counter < retryMaxLimit) {
            counter++;
            return true;
        }
        return false;
    }
    }
```
There is variable which can be used for Number of time we want to reRun Our Testcase. `int retryMaxLimit` variable value is the max value for retry. Like Our Test will Run 3 times If you run this code. You can set the value of this variable as per your needs.

### Use TestRetryAnalyzer in Our Test:
 There can be multiple ways you can use the above class in our TestNg implementation but here we will describe 2 ways.

#### 1: TestRetryAnalyzer.class as attribute in @Test
We have to pass TestRetryAnalyzer.class as an attribute in all our @Test methods.
```Java
import org.testng.Assert;
import org.testng.annotations.Test;

public class TestRetryTestCases {

    @Test(retryAnalyzer=TestRetryAnalyzer.class)
    public void firstTestMethod() {
        System.out.println("First test method");
        Assert.assertTrue(true);
    }

    @Test(retryAnalyzer=TestRetryAnalyzer.class)
    public void secondTestMethod() {
        System.out.println("Second test method");
    //This Test will fail with Assert here
        Assert.assertTrue(false);
    }
}
```
#### Output:
In our output we can see the `secondTestMethod()` ran 3 times and finally failed.
![testRetryAnalyzer-in-attribute-testng](/img/testng/testRetryAnalyzer-in-attribute-testng.png)

### 2: TestRetryAnalyzer.class as Listener and from testng.xml:
TestNG Listeners are a very good way of implementing the retry logic on whole Suit level and need not put the TestRetryAnalyzer.class in all the @Test methods. I prefer using Listeners from the TestNG.xml file.

#### Step 1: TestRetryAnalyzerListener.class
First, we need to create TestRetryAnalyzerListener class which is implementing the IAnnotationTransformer interface. We will use the `public void transform(ITestAnnotation annotation, Class testClass,Constructor testConstructor, Method testMethod)` method.This method will be invoked by TestNG to give you a chance to modify a TestNG annotation read from your test classes. Then the Listener is for calling the *TestRetryAnalyzer.class* (*Same Class We created above*) to reRun the Failed Test.

>TestNg needs this Listener to be known very early, So We will use this directly in TestNG.xml not in our test Class because Listeners can be used in TestClass.

```Java
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
import org.testng.IAnnotationTransformer;
import org.testng.annotations.ITestAnnotation;

public class TestRetryAnalyzerListener implements IAnnotationTransformer {

    public void transform(ITestAnnotation annotation, Class testClass, Constructor testConstructor, Method testMethod) {
        annotation.setRetryAnalyzer(TestRetryAnalyzer.class);
    }
}
```
#### Step 2: Create Test Class:
```Java
import org.testng.Assert;
import org.testng.annotations.Test;

public class TestRetryXmlTestCases {

    @Test()
    public void firstTestMethod() {
        System.out.println("First test method");
        Assert.assertTrue(true);
    }

    @Test()
    public void secondTestMethod() {
        System.out.println("Second test method");
        Assert.assertTrue(false);
    }
}
```
#### Step 3: Create TestNG xml file With Listener:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Suite">
  <listeners>
    <listener class-name="com.tutorial.testng.retrytest.TestRetryAnalyzerListener" />
  </listeners>

  <test name="Test">
    <classes>
      <class name="com.tutorial.testng.retrytest.TestRetryXmlTestCases"/>
    </classes>
  </test> <!-- Test -->
</suite> <!-- Suite -->
```
Now we can run our test cases from the testng.xml file where we have mentioned our listener on the Suite level, So all the Test will inherit this listener and retry our failed Test. You can learn more about [how to use testng.xml file and run testcases in other articles](https://www.pawangaria.com/post/testng/what-is-testng-xml-file/).

### Output:
```Text
First test method
Second test method
Second test method
Second test method

===============================================
Suite
Total tests run: 4, Failures: 1, Skips: 2
===============================================
```
