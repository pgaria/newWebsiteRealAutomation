---
title: "Integrate Gurock TestRail with Automated TestCases in Java+TestNG"
date: 2017-11-23
draft: false
description: "Integrating your Automated Usecases in java with Gurock Test Rail API's and pushing test results to Testrail UI."
keywords: "TestRail,Testing,Automation,Reporting,TestCaseManagement"
categories: [ "Automation Testing", "Selenium WebDriver"]
image: "/img/testRail/TestRail-report-test-management-thumb.png"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Java",
    "testNG",
    "TestRail",
    "Test Automation"
]
---

#### What is TestRail?
[Gurock TestRail](http://www.gurock.com/testrail/) is a modern and very nice tool for the Test Case Management by Quality Assurance and Development Team.
TestManagement tools are very helpful for managing the test cases and TestRail is pack with very useful features like web-based application, Rest API integration etc.

![TestRail-test-management-report-web](/img/testRail/TestRail-report-test-management.png)

#### Why to use TestManagement Tool with Automation?
While making Regression or any kind of Test Automation suit there is a need of maintaining Use cases and maintain the repository which anyone can access and see what use cases are been automated and what is left. It is very helpful for the continues Integration Pipeline that we can generate and see the reports in one place for Automated Test results.

![TestRail-testcase-result-status-update](/img/testRail/TestRail-testcase-result-status-update.png)

There is a need for Integrating Test Management Tool like TestRail with the Java and TestNG based Automated Test Framework to mark every test run results in the common web-based platform.
In this blog, I am going to use TestRail as Test Management tool and will explain the steps to integrate your framework using Rest API's.

#### How to integrate Automated Test with TestRail?
I am explaining the steps to write your logic in Java and you can also follow the steps to make integration work for your framework

STEP 1: First create one new *Annotation Class*
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface UseAsTestRailId
{
int testRailId() default 0;
String[] tags() default "";
}
```
STEP 2: Use the above annotation in TestNg class file with other test.

Every *@Test* here is mapped with the use case in TestRail, So we got the TestRail unique Id for every test.
```java
@Test
@UseAsTestRailId(testRailId=<TestRail Id for this Usecase>)
public void yourTestNumberSomething()
{
  // Do your testing steps here which are mapped to TestRail usecase.
}
```
STEP 3: You can use TestNG for every test, so that you can execute the TestRail API after the test execution.
```java
@Listeners(com.gurock.testrail.Listener.class)
public class TestClass {
    // All the Test will be here ...
}
```
STEP 4: You need to call the TestRail from Listener Class. Like I am calling the API in the *onTestSuccess* Method.
```java
public void onTestSuccess(ITestResult result) {
String TestID=null;
IClass obj = result.getTestClass();
Class newobj = obj.getRealClass();
Method testMethod = newobj.getMethod(result.getName());
if (testMethod.isAnnotationPresent(UseAsTestRailId.class))
{
UseAsTestRailId useAsTestName = testMethod.getAnnotation(UseAsTestRailId.class);
// Get the TestCase ID for TestRail
TestID = Integer.toString(useAsTestName.testRailId());
System.out.println("Test Rail ID = " + TestID);
}
// Now you can call the TestRail post API call to update the result in TestRail database based on the TestRun Id and TestCase Id.

JSONObject r = (JSONObject) client.sendPost("add_result_for_case/TestCaseID/TestRunID", data);
}
```
#### Final Result and Reporting
Now integrating TestRail UseCases Id with automated test, We will have the test run result history for the particular UseCases.

![testRail-usecase-updated-back-in-automation](/img/testRail/TestRail-usecase-report-automation.png)  

You can change and improve the code and logic based on your requirements and send me feedback about your experience with the integration.

Thanks for reading!
