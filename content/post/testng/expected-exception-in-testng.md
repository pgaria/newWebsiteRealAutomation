---
title: "Expected Exceptions in TestNG"
date: 2017-12-17
draft: true
description: "How do you verify that code throws exceptions as expected? How to use an expectedException attribute in TestNG."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,test,unit,ExpectedException"
image: "/img/testng/expected-exception-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","expectedException"
]
---
All exceptions are not bad in our product, means we have to test and verify the exceptions in our product. The [ExpectedException](https://jitpack.io/com/github/cbeust/testng/master/javadoc/org/testng/annotations/ExpectedExceptions.html) is a way of verifying that your code throws a specific exception.

>Verifying that code completes normally is important, but making sure the logic behaves as expected in failure and unexpected situations are also important.

In [TestNG](http://testng.org/doc/documentation-main.html), we have an attribute as **expectedExceptions** which needs to be added to our test where we have to verify the Exception as expected. This doesn't affect your existing tests steps as it's in the attribute.

### How to Test exceptions:
Below example throw `java.lang.NullPointerException` Exception and It's an expected exception but our test is marked as failures means we are not handling the exception as expected.
```Java
package com.tutorial.testng;

import org.testng.annotations.Test;

public class ExpectedExceptionTest {

    @Test()
    public void expectedExceptionTest() {
        String ptr = null;
        // This line of code throws NullPointerException
        ptr.equals("testng");
    }
}
```
![exception-throwing-in-testng](/img/testng/exception-throwing-in-testng.png)

### With ExpectedExceptions attribute:
Now let's fix our above test and mark our test as pass by handling an expected exception. We will add the new attribute to our test  
`@Test(expectedExceptions = java.lang.NullPointerException.class)`
```Java
package com.tutorial.testng;

import org.testng.annotations.Test;

public class ExpectedExceptionTest {

    @Test(expectedExceptions = java.lang.NullPointerException.class)
    public void expectedExceptionTest() {
        String ptr = null;
        // This line of code throws NullPointerException
        ptr.equals("testng");
    }
}
```
![expected-exception-test-in-testng](/img/testng/expected-exception-test-in-testng.png)
