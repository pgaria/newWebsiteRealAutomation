---
title: "How to use TestNG Listeners?"
date: 2017-12-30
draft: false
description: "TestNG Listeners listen to the event defined in the TestCase scripts and take action accordingly."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,test,Listeners"
image: "/img/testng/expected-exception-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","Listeners"
]
---
Listeners listen to the event defined in the Test Scripts and behave accordingly. The main purpose of using listeners is to create logs for debugging but we can also manage Listeners for Test Framework Design.

### What are TestNG Listeners?
In Simple words, they are basically Java classes, which implement some TestNG Interfaces. So that when TestNG raises certain events like Test starting, Test Failure etc. TestNG will look for all Classes which are basically looking for such events and call the respective event handlers.

Let's understand the Listener concept using the **ISuiteListener Interface** for test suites.

Create a Class `TestNgListenersTest` which implements ISuiteListener and add unimplemented methods.
```Java
import org.testng.ISuite;
import org.testng.ISuiteListener;

public class TestNgListenersTest implements ISuiteListener {

    public void onStart(ISuite suite) {
        System.out.println("on Before Suite Start");
    }

    public void onFinish(ISuite suite) {
        System.out.println("on After Suite Finish");
    }
}
```
Now we will use the above `TestNgListenersTest` class as a Listener in out Test Class.
```Java
import org.testng.annotations.Listeners;
import org.testng.annotations.Test;

@Listeners(TestNgListenersTest.class)
public class ListenerTestExample {
    @Test()
    public void firstTestMethod() {
        System.out.println("First test method");
    }
}
```
If you Run the above Test Class you can see the Listener class methods invoked indirectly.
```text
on Before Suite Start
First test method
on After Suite Finish
```
### Lets Understand Our Listener Example:
Whenever a Test Class implements the Listener, TestNG guarantees in execution that it will invoke the methods onStart() and onFinish() before and after running a TestNG Suite. So before TestNG picks up your Test Suite for execution, it first makes a call to `onStart()` method and runs the Steps mentioned in the method and then Execute the @Test method. In a similar way, it again makes a call to `onFinish()` method after a Test Suite has been finished running.

### What Listeners are available in TestNG:
TestNG provides following listener types:

- IExecutionListener
- IAnnotationTransformer
- ISuiteListener
- ITestListener
- IConfigurationListener
- IMethodInterceptor
- IInvokedMethodListener
- IHookable
- IReporter

### Ways of Using Listeners in TestNG:
We will focus on most common ways listeners been used in our Test Suite and defined in our Test. Example uses the `TestNgListenersTest.class` Listener in both the ways.

#### 1. @Listeners TestNG as annotation in Class
```Java
import org.testng.annotations.Listeners;
import org.testng.annotations.Test;

@Listeners(TestNgListenersTest.class)
public class ListenerTestExample {
    @Test()
    public void firstTestMethod() {
        System.out.println("First test method");
    }
}
```
#### 2. Listeners Tag in your TestNG.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Suite">
  <listeners>
    <listener class-name="com.tutorial.testng.listeners.TestNgListenersTest" />
  </listeners>

  <test name="Test">
    <classes>
      <class name="com.tutorial.testng.listeners.ListenerTestExample"/>
    </classes>
  </test>
</suite>
```
### Example with Multiple Listeners:
In this example we will implement major Listeners like ISuiteListener, ITestListener and IInvokedMethodListener in this class. This example can handle multiple events like test start, pass,finish,fail,skips or passes etc.
```Java
import org.testng.IInvokedMethod;
import org.testng.IInvokedMethodListener;
import org.testng.ISuite;
import org.testng.ISuiteListener;
import org.testng.ITestContext;
import org.testng.ITestListener;
import org.testng.ITestResult;

public class TestNGListener implements ITestListener, ISuiteListener, IInvokedMethodListener {

    // ISuiteListener and will execute before the Suite start
    public void onStart(ISuite suite) {
        System.out.println("Begin executing Suite:" + suite.getName());
    }

    // ISuiteListener and will execute, once the Suite is finished
    public void onFinish(ISuite suite) {
        System.out.println("After executing Suite:" + suite.getName());
    }

    // ITestListener and will execute before starting of Test set
    public void onStart(ITestContext arg0) {
        System.out.println("Begin executing Test:" + arg0.getName());
    }

    // ITestListener and will execute, once the Test set is finished
    public void onFinish(ITestContext arg0) {
        System.out.println("Completed executing test:" + arg0.getName());
    }

    // ITestListener and will execute only when the test is pass
    public void onTestSuccess(ITestResult arg0) {
        System.out.println("Completed executing test:" + arg0.getName());
    }

    // ITestListener and will execute only on the event of fail test
    public void onTestFailure(ITestResult arg0) {
        System.out.println("Test Status::" + arg0.getName());
    }

    // ITestListener and will execute before the main test start (@Test)
    public void onTestStart(ITestResult arg0) {
        System.out.println("Test Status:" + arg0.getName());
    }

    // ITestListener and will execute only if any of the main test(@Test) get skipped
    public void onTestSkipped(ITestResult arg0) {
        System.out.println("Test Status::" + arg0.getName());
    }

    // IInvokedMethodListener and will execute before every method including
    // @Before @After @Test
    public void beforeInvocation(IInvokedMethod arg0, ITestResult arg1) {
        System.out.println("Before Invocation of method:" + arg0.getTestMethod().getMethodName());
    }

    // IInvokedMethodListener and will execute after every method including
    // @Before @After @Test
    public void afterInvocation(IInvokedMethod arg0, ITestResult arg1) {
        System.out.println("After Invocation of method:" + arg0.getTestMethod().getMethodName());
    }

    // ITestListener method and Success Percentage
    public void onTestFailedButWithinSuccessPercentage(ITestResult result) {
        System.out.println("onTestFailedButWithinSuccessPercentage:" + result.getTestName());
    }
}
```
### Output:
```text
Begin executing Suite:Default suite
Begin executing Test:Default test
Test Status:testMethod
Before Invocation of method:testMethod
First test method
After Invocation of method:testMethod
Completed executing test:testMethod
Completed executing test:Default test
After executing Suite:Default suite
```
