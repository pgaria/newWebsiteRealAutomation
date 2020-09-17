---
title: "Limit certain Test Iterations in TestNG @DataProvider"
date: 2020-09-17
draft: false
author: Pawan Garia
description: "How to Limit or Skip the certain test data set from the TestNG @DataProvider. For example, if you want to run/limit only the first 10 datasets from the complete set of a dataset in @DataProvider"
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,parameters,DataProvider,test"
tags: [
    "Test Automation",
    "TestNG",
    "DataProvider"
]
---
Recently while working on one of the projects we were required to limit the number of data set provided from the TestNG DataProvider and Run for example only the first 10 Testcases and skip all others to quick run the @Test. As we are using the TestNG @DataProvider to send the Test Data and the count of the Test Data set provided from the Data Provider is generally more than 100.

So I decided to write down the solution I have created to Limit the Test Data Set passed from the @DataProvider without changing much in the current TestCase and implementation. This solution can be utilized and customized for any of the cases and it's more of the logic to limit the DataProvider array.

### What we know from TestNG about the @DataProviders?
- The number of tests is the number of rows returned by the @DataProvider method.
- @DataProvider method is returning a 2-dimensional array.
- The solution is then to return the correct number of test cases from the method annotated by @DataProvider.
- Number of test cases should be passed from the @Test Method.

## How to pass the expected No of Test count from @Test
So the first question is like how can we pass the count of Testcases or how can the Test method inform the DataProvider like how many Testcases it should return dynamically or return all the test cases.

We can achieve this case using the custom Java Annotation which can be used in the @Test and Pass the Start Index and End Index.

### Create Custom annotation
```Java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
public @interface IndexAnnotation {

    int startIndex() default 0;

    int endIndex() default 0;

}
```

### Create DataProvider Class
Now the next step is to show how to read the Annotation which we have just created above in the Data Provider and create the Array based on the values provided from the Test or If the annotation is not present in the Test just pass the complete DataProvider object.

So the Data Provider method would look like below which reads the @Annotation and then creates the Object Array at run time for the @test and Return.

```Java
import org.testng.ITestContext;
import org.testng.ITestNGMethod;
import org.testng.annotations.DataProvider;
import java.lang.reflect.Method;

public class ReadAutoData {

    @DataProvider(name = "getData")
    public Object[][] getData(Method currentTestMethod, ITestContext context) {
        Object[][] arrayObject = getTestData();
        ITestNGMethod[] iTestNGMethods = context.getAllTestMethods();
        for (ITestNGMethod iTestNGMethod : iTestNGMethods) {
            Method method = iTestNGMethod.getConstructorOrMethod().getMethod();
            if (method.getName().equalsIgnoreCase(currentTestMethod.getName())) {
                if (method.isAnnotationPresent(IndexAnnotation.class)) {
                    IndexAnnotation indexes = method.getAnnotation(IndexAnnotation.class);
                    System.out.println("Creating the DataProvider between Start Index: " + indexes.startIndex() + " End Index: " + indexes.endIndex());
                    return java.util.Arrays.copyOfRange(arrayObject, indexes.startIndex(), indexes.endIndex());
                }
            }
        }
        return arrayObject;
    }

    /**
     * Get the By Default Data for the Data Provider.
     *
     * @return
     */
    private Object[][] getTestData() {
        return new Object[][]{
                {"user1", "password1"},
                {"user2", "password2"},
                {"user3", "password3"},
                {"user4", "password4"},
                {"user5", "password5"},
                {"user6", "password6"}};
    }
}
```

### Use Annotation in Test
So the Last thing is how we can create the Test and how we can use everything we have created above in the Test class. Following is an example of the Tests where we are using different Start and End Indexes and One Test which use all the Testcases passed from the Data provider.

Use our custom `IndexAnnotation.java` on the `@Test` where you want to limit the TestCases from the DataProvider.  

```Java
import org.testng.annotations.Test;

public class TestDataProvider {

    @Test(dataProvider = "getData", dataProviderClass = ReadAutoData.class)
    public void normalTest(String userName, String password) {
        System.out.println("Running Normal Test for " + userName + " and " + password);
    }

    @IndexAnnotation(startIndex = 0, endIndex = 4)
    @Test(dataProvider = "getData", dataProviderClass = ReadAutoData.class)
    public void withCustomIndex(String userName, String password) {
        System.out.println("Running Custom Test [0 -> 4 ] for " + userName + " and " + password);
    }

    @IndexAnnotation(startIndex = 0, endIndex = 2)
    @Test(dataProvider = "getData", dataProviderClass = ReadAutoData.class)
    public void withCustomIndex2(String userName, String password) {
        System.out.println("Running Custom Test [0 -> 2 ] for " + userName + " and " + password);
    }
}
```
### Result
If you run the above Test class you will see the output like below which shows that the @Test is running for only the limited Test Data passed from the Test using the Start and End Index and One Test ran for all the TestData provided from the @DataProvider.

```
Running Normal Test for user1 and password1

Running Normal Test for user2 and password2

Running Normal Test for user3 and password3

Running Normal Test for user4 and password4

Running Normal Test for user5 and password5

Running Normal Test for user6 and password6

Creating the DataProvider between Start Index: 0 End Index: 4

Running Custom Test [0 -> 4 ] for user1 and password1

Running Custom Test [0 -> 4 ] for user2 and password2

Running Custom Test [0 -> 4 ] for user3 and password3

Running Custom Test [0 -> 4 ] for user4 and password4

Creating the DataProvider between Start Index: 0 End Index: 2

Running Custom Test [0 -> 2 ] for user1 and password1

Running Custom Test [0 -> 2 ] for user2 and password2

===============================================
Default Suite
Total tests run: 12, Passes: 12, Failures: 0, Skips: 0
===============================================
```

You can find the complete solution as a Gradle Java Project in the following GitHub Location [here](https://github.com/pgaria/CustomDataProviderExample).
