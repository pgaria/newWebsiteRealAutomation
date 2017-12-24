---
title: "How to use Dependencies in TestNG?"
date: 2017-12-20
draft: true
description: "How to use the attributes dependsOnMethods or dependsOnGroups, found on the @Test annotation for the dependencies."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,dependsOnMethods,dependsOnGroups,test,alwaysRun"
image: "/img/testng/dependsOnGroup-method-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","dependencies"
]
---
Sometimes we have steps or tests which are depended on each other and can not be performed alone. For example: To update Facebook account details, User should be logged in already.I personally do not recommend making test cases dependent upon each other and should be independent.  
To make dependencies between test, TestNG provides *dependsOnMethods* or *dependsOnGroups* attributes which can be used in the @Test.
As per the TestNg documentation, there are two types of dependencies:  

 - **Hard dependencies:** If a dependent method failed, all the test methods will be skipped (not failed)  
 - **Soft dependencies:** Run all the methods even if the dependent methods failed. To make soft dependencies in our test we have to use **"alwaysRun=true"** in your @Test.

 Let's see some working examples to understand the concept:

### #dependsOnMethods
Let's make two @test methods where the second method depends on the first method.  

 > secondTestMethod() is declared as independent on method firstTestMethod(), which guarantees that firstTestMethod() will always be invoked first.

```Java
package com.tutorial.testng.dependencies;

import org.testng.annotations.Test;

public class DependsOnMethodsTest {

    @Test()
    public void firstTestMethod() {
        System.out.println("First test method");
    }

    @Test(dependsOnMethods = { "firstTestMethod" })
    public void secondTestMethod() {
        System.out.println("Second test method");
    }
}
```
#### Output:
![dependsOnMethods-testng-result](/img/testng/dependsOnMethods-testng-result.png)

Let's change our test class and make the dependent method firstTestMethod() failed. Now in results dependent method **firstTestMethod()** should be FAILED but our method **secondTestMethod()** should be SKIPPED.
```Java
package com.tutorial.testng.dependencies;

import org.testng.Assert;
import org.testng.annotations.Test;

public class DependsOnMethodsTest {

    @Test()
    public void firstTestMethod() {
        System.out.println("First test method");
        Assert.assertTrue(false);
    }

    @Test(dependsOnMethods = { "firstTestMethod" })
    public void secondTestMethod() {
        System.out.println("Second test method");
    }
}
```
#### Output:
![dependsOnMethods-failure-testng-result](/img/testng/dependsOnMethods-failure-testng-result.png)

### How to use 'alwaysRun' and make Soft dependencies:
Let's change our test class again and add *alwaysRun = true* in the second method. Now in results dependent method **firstTestMethod()** should be FAILED but our method **secondTestMethod()** should be Running and PASSED.

> alwaysRun attribute is a boolean type, so only accepted values are true and false.

```Java
package com.tutorial.testng.dependencies;

import org.testng.Assert;
import org.testng.annotations.Test;

public class DependsOnMethodsTest {

    @Test()
    public void firstTestMethod() {
        System.out.println("First test method");
        Assert.assertTrue(false);
    }

    @Test(dependsOnMethods = { "firstTestMethod" }, alwaysRun = true)
    public void secondTestMethod() {
        System.out.println("Second test method");
    }
}
```
#### Output:
![dependsOnMethod-with-alwaysRun-testng](/img/testng/dependsOnMethod-with-alwaysRun-testng.png)

### #dependsOnGroups
We have two test methods which are part of a Group and **TestMethodDependOnGroup()** is dependent on the group **testGroup** itself. So If any of the tests failed from the group, all the @test dependent on the group would be SKIPPED.

```Java
package com.tutorial.testng.dependencies;

import org.testng.annotations.Test;

public class DependsOnGroupsTest {

    @Test(groups = { "testGroup" })
    public void firstGroupMethod() {
        System.out.println("First Group method");
    }

    @Test(groups = { "testGroup" })
    public void secondGroupMethod() {
        System.out.println("Second Group method");
    }

    @Test(dependsOnGroups = { "testGroup" })
    public void TestMethodDependOnGroup() {
        System.out.println("test method depending on group");
    }
}
```
#### Output:
![dependsOnGroup-testng-result](/img/testng/dependsOnGroup-testng-result.png)

Let's change above class and mark one test method from the group as Failed and our dependent method **TestMethodDependOnGroup()** should be marked as SKIPPED.
```Java
package com.tutorial.testng.dependencies;

import org.testng.Assert;
import org.testng.annotations.Test;

public class DependsOnGroupsTest {

    @Test(groups = { "testGroup" })
    public void firstGroupMethod() {
        System.out.println("First Group method");
    }

    @Test(groups = { "testGroup" })
    public void secondGroupMethod() {
        System.out.println("First Group method");
        Assert.assertTrue(false);
    }

    @Test(dependsOnGroups = { "testGroup" })
    public void TestMethodDependOnGroup() {
        System.out.println("test method depending on group");
    }
}
```
#### Output:
![dependsOnGroup-failed-test-result](/img/testng/dependsOnGroup-failed-test-result.png)
