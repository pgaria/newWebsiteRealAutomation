---
title: "How TestNG works in the Background?"
date: 2018-01-02
draft: true
description: "In this article, we explain how TestNG works in the background and what is TestNG architecture. We will see examples creating the TestNG object and run the test class from code, It also adds a TestListener."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,TestListener,object,test,background,architecture"
image: "/img/testng/multiple-class-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","testng architecture"
]
---
TestNG is widely used Test Management Framework these days mostly used with Selenium WebDriver and Unit Testing. This article will try to Run the testNG from simple Java program and try to figure out the architecture of TestNG. We will see what happens in the background when you run your Test.

### Running TestNG from Java program:
This example creates a TestNG object and runs the @test class TestClass then we will try to add some TestListener int he same Java code.

>TestNG.class is the main entry point for running tests in the TestNG framework. Users can create their own TestNG object and invoke it in many different ways.

Please see the [JavaDocs](https://jitpack.io/com/github/cbeust/testng/master/javadoc/org/testng/package-summary.html) for the TestNG API.
### Create Test Class:
First Step is to create a Simple Test Class which contains @test method. This Test Class would be invoked by our TestNG examples.
```Java
import org.testng.annotations.Test;

public class TestClass {
    @Test
    public void runTestFromProgramme() {
        System.out.println("@TestMethod: Running @Test Method");
    }
}
```
### TestNG object Class:
Now lets create a Simple Java class Which would invoke our Test class and @test Method.
```Java
import org.testng.TestNG;

public class TestngMainClass {

    public void runTestUsingTestngClass()
    {
        TestNG testng = new TestNG();
        testng.setTestClasses(new Class[] { TestClass.class });
        testng.run();
    }

    public static void main(String args[])
    {
        TestngMainClass obj1 = new TestngMainClass();
        obj1.runTestUsingTestngClass();
    }
}
```
### Output:
If you run the above Class, It will invoke @test method and printed the Message in output. By Default all the Suite Names are **Command line suite** in TestNG.
```text
@TestMethod: Running @Test Method

===============================================
Command line suite
Total tests run: 1, Failures: 0, Skips: 0
===============================================
```

### #Add TestNG Listener to Our Class:
Now let create Listener: `ISuiteListener` and add our Listener to the same program. Adding ISuiteListener should be invoked before and after the TestNG Test Suite. You can read about  Listener in my [TestNG article](https://www.pawangaria.com/post/testng/how-to-use-testng-listeners/).
### Create Listener Class:
```Java
import org.testng.ISuite;
import org.testng.ISuiteListener;

public class TestNgListenersTest implements ISuiteListener {

    @Override
    public void onStart(ISuite suite) {
         System.out.println("On Before Suite Start");
    }

    @Override
    public void onFinish(ISuite suite) {
         System.out.println("On After Suite Finish");
    }
}
```
### Add Listener in our Test Class:
Now Let's update our main Class with TestNgListenersTest.class
```Java
import org.testng.ITestNGListener;
import org.testng.TestNG;

public class TestngMainClass {

    public void runTestWithListenerClass()
    {
        ITestNGListener iTestListener= new TestNgListenersTest();
        TestNG testng = new TestNG();
        testng.setTestClasses(new Class[] { TestClass.class });
        testng.addListener(iTestListener);
        testng.run();
    }

    public static void main(String args[])
    {
        TestngMainClass obj1 = new TestngMainClass();
        obj1.runTestWithListenerClass();
    }
}
```
### Output:
If we run the above class It will invoke our Listener before Suite method and then the @Test and then After Suite Method from our implemented Listener class.
```text
Listeners:On Before Suite Start
@TestMethod: Running @Test Method
Listeners:On After Suite Finish

===============================================
Command line suite
Total tests run: 1, Failures: 0, Skips: 0
===============================================
```

So far we have now learned about using TestNG main class and how to use TestNG to run @test from Java programs. Now let's try to dig deep and see what happens inside when you run simple Test.

### #How TestNG works in the Background:
As from our examples, we can see the most important file is [TestNg.java](https://jitpack.io/com/github/cbeust/testng/master/javadoc/org/testng/TestNG.html).This class is the main entry point for running tests in the TestNG framework. It doesn't matter where you are invoking testng in many different ways like testng.xml or command line or from Java.
You can also define which groups to include or exclude, assign parameters, etc...

Let's try to understand step by step what methods are important and what happens when you run simple Test method.
#### `TestNG testng = new TestNG();`  
TestNG first creates an object of this TestNG.Class like above line is creating an Instance and invoke the default constructor which initialize configurations.
![testng-java-constructor-init-methods](/img/testng/testng-constructor-init-method.png)

#### `testng.setTestClasses(new Class[] { TestClass.class });`  
Set the test classes to be run by this TestNG object. This method will create a dummy suite that will wrap these classes called **Command Line Test**.
If used together with threadCount, parallel, groups, excludedGroups than this one must be set first.
![testng-setTestClasses-method](/img/testng/testng-setTestClasses-method.png)
#### `ITestNGListener iTestListener= new TestNgListenersTest();`  
#### `testng.addListener(iTestListener);`  
If we are adding Listener in our test the method above is used by TestNG which is nothing but an method accepting ITestNGListener interface variable. This method checks which instance is passed in your ITestNGListener variable and based on that decide what to implement in Test.
![addListener-ITestNGListener-method](/img/testng/addListener-ITestNGListener-method.png)

#### `testng.run();`  
Final call to run the Testcases and Test Suite is performed by the run() method. TestNG call the method in which there are multiple calls to other method like `sanityCheck()`,`runExecutionListeners()`,`List<ISuite> suiteRunners = runSuites();` etc..
![testng-run-method](/img/testng/testng-run-method.png)

#### `private void generateReports(List<ISuite> suiteRunners)`
generateReports() method is also very important method call from the run() method and I want to highlight this as this method receives a List of ISuite Interface which is used to Generate report at the end of the Suite run.
![test-output-run-report-testng](/img/testng/test-output-run-report-testng.png)

I have mentioned and tried to explain few method and class as TestNG as a project contains many components, So I will suggest to please see the [JavaDocs](https://jitpack.io/com/github/cbeust/testng/master/javadoc/org/testng/TestNG.html) for the entire TestNG API.
