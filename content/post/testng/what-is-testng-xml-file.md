---
title: "What is the TestNG.xml file?"
date: 2017-12-17
draft: false
description: "What is the TestNG.xml and how to generate testng.xml file using eclipse. We will run multiple @test methods using testng.xml file in this article."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,testng.xml,test,Java,Eclipse,testing,unittesting"
image: "/img/testng/testng-xml-file-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG"
]
---
[TestNG.xml](http://testng.org/doc/documentation-main.html#testng-xml) file is an XML file which contains all the Test configuration and this XML file can be used to run and organize our test. TestNG eclipse plugin can be used to automatically generate testng.xml file so no need to write from scratch. In this article, we are going to learn about what is testng.xml and how to use testng.xml file to run our test.

### Create Class with multiple @test methods:
First, we need to create a simple class which contains multiple @test methods.
```Java
package com.tutorial.testng;

import org.testng.annotations.Test;

public class RunMultipleTestCasesfromXml {
    @Test()
    public void testMethod1() {
        System.out.println("Running @Test - testMethod1");
    }

    @Test()
    public void testMethod2() {
        System.out.println("Running @Test - testMethod2");
    }

    @Test()
    public void testMethod3() {
        System.out.println("Running @Test - testMethod3");
    }

    @Test()
    public void testMethod4() {
        System.out.println("Running @Test - testMethod4");
    }
}
```
We can run this test class directly by Right click and Run As testNg option from the Eclipse but we are goign to create an xml file here.

### Create TestNg.xml file:
#### Step:1
Right click on the Test Class and then select TestNG --> Convert to TestNG.
![right-click-convert-to-testng](/img/testng/right-click-convert-to-testng.png)
#### Step:2
After the first step, you will see a window where we can select the location where testng.xml file should be generated.**You can set any name for your xml file, by default name is testng.xml**.
*You should be able to edit the preview section and update the XML file here.*
![generate-testng-file-browse](/img/testng/generate-testng-file-browse.png)
#### Step:3
Next window will show you the changes you can do in yours JAVA files.*You can ignore this step as we have already updated Java class, So just ignore and click finish button.*
![changes-in-java-files](/img/testng/changes-in-java-files.png)
#### Step:4
After the last and final Step you will be able to see a **testng.xml** file generated in the base directory of your project.If you open the .xml file you can notice our Test Class is present in the **classes** tag which is under the **test name="Test"** Tag. *It means our Class would be considered as Test Class by TestNG.*
![generated-testng-file-in-eclipse](/img/testng/generated-testng-file-in-eclipse.png)

#### TestNG.xml file sample:
```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Suite">
  <test name="Test">
    <classes>
      <class name="com.tutorial.testng.RunMultipleTestCasesfromXml"/>
    </classes>
  </test>
</suite>
```
### Run TestNG.xml file:
**Right Click** on the testng.xml file and the select **Run As --> TestNG Suite**
![right-click-run-as-testng-suit](/img/testng/right-click-run-as-testng-suit.png)

When you run your **testng.xml** file, it will start running your *@test* methods from the class mentioned in the XML and generate the report. You can see our all the *@test* methods ran successfully.
![testng-multiple-test-report](/img/testng/testng-multiple-test-report.png)
