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
TestNG lets you pass parameters directly to your test methods from the testng.xml file. Passing parameters from the XML file is important when you want to pass configurations like browser name, version,test-URL etc. directly into your Test. Passing these configurations is important because the values are changing and should be always dynamic.

>Passing Dynamic configuration values from testng.xml is important when you want to change parameters like browser or version or operatingSystem etc.

Let's create an example where we define the simple parameters in the testng.xml file and then use those parameters in the Java files.

### Create Test Class with parameters:
Create a Test Class in which we will use annotation **@Parameters("parameter")** in the Test method. Here we will accept 3 parameters like **browser, version and operatingSystem**. We will print the values passed from the XML file.

> Parameters can be used in @BeforeClass, @BeforeTest, @BeforeMethod, @Test and other TestNG annotations.

```java
import org.testng.annotations.Parameters;
import org.testng.annotations.Test;

public class PassParametersFromxml {

    @Parameters({"browser","version","operatingSystem"})
    @Test()
    public void firstTestMethod(String browser,String version,String operatingSystem) {

        System.out.println("Parameter browser:"+browser);
        System.out.println("Parameter version:"+version);
        System.out.println("Parameter operatingSystem:"+operatingSystem);
    }
}
```
### Create testng.xml file and pass parameters:
```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Suite">
  <test name="Test">
  <parameter name = "browser" value="Firefox"/>
  <parameter name = "version" value="61.12"/>
  <parameter name = "operatingSystem" value="Windows"/>
    <classes>
      <class name="com.tutorial.testng.testngfileparameters.PassParametersFromxml"/>
    </classes>
  </test>
</suite>
```
### Output:
```text
Parameter browser: Firefox
Parameter version: 61.12
Parameter operatingSystem: Windows

===============================================
Suite
Total tests run: 1, Failures: 0, Skips: 0
===============================================
```

> It is important to pass all the values which are expected as parameters in @Test or else you will get the Exception : org.testng.TestNGException: Parameter 'operatingSystem' is required by @Test on method and your Testcases will not Run.

### How to use @optional Parameters:
Now If we want all the parameters to be present in the @Test but don't want to pass from the testng.xml file, we can mark those variable as **@optional** and TestNG will not throw the Exception regarding parameters missing.

Below example shows us how to use our same class @test with optional Parameters.

**Marking the "operatingSystem" parameter as @optional in this example Class**
```java
import org.testng.annotations.Optional;
import org.testng.annotations.Parameters;
import org.testng.annotations.Test;

public class PassParametersFromxmlOptional {

    @Parameters({"browser","version","operatingSystem"})
    @Test()
    public void firstTestMethod(String browser,String version,@Optional String operatingSystem) {

        System.out.println("Parameter browser:"+browser);
        System.out.println("Parameter version:"+version);
        System.out.println("Parameter operatingSystem:"+operatingSystem);
    }
}
```

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Suite">
  <test name="Test">
  <parameter name = "browser" value="Firefox"/>
  <parameter name = "version" value="61.12"/>
    <classes>
      <class name="com.tutorial.testng.testngfileparameters.PassParametersFromxmlOptional"/>
    </classes>
  </test>
</suite>
```
### Output:
If we run the testng.xml file now with out the value of operatingSystem, TestNG will consider that value as optional and continue with other parameters and print the value as NULL.
```text
Parameter browser: Firefox
Parameter version: 61.12
Parameter operatingSystem: null

===============================================
Suite
Total tests run: 1, Failures: 0, Skips: 0
===============================================
```
