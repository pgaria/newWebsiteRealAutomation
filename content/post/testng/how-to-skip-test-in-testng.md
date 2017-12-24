---
title: "How to Skip @Test in TestNG"
date: 2017-12-20
draft: false
description: "How to skip a Test in TestNG and marking a test as Skip should be present in Test Report as Skiped testcase."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,skip,test,unit,SkipException"
image: "/img/testng/skip-testng-test-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG"
]
---
Skipping test is also an important concept in [TestNG](http://testng.org/doc/). You can skip the test from running as you don't want to Run the @test and mark the @test as Skip in reports.

To Skip the test we need to use `org.testng.SkipException` an [Exception Class from TestNG](https://jitpack.io/com/github/cbeust/testng/master/javadoc/org/testng/SkipException.html). We have to throw an exception to the first line and code will come out from the test. You can have a look at Java document to read more about [exception throwing](https://docs.oracle.com/javase/tutorial/essential/exceptions/throwing.html).

### How to Skip @Test:

We are going to throw SkipException in the following example Class.
```Java
package com.tutorial.testng;

import org.testng.Assert;
import org.testng.SkipException;
import org.testng.annotations.Test;

public class SkipTestInTestng {
	// Skip Test
	@Test()
	public void skipTestMethod() {
		throw new SkipException("Skipped Test");
	}

	// Pass Test
	@Test()
	public void passTestMethod() {
		Assert.assertTrue(true);
	}

	// Fail Test
	@Test()
	public void failTestMethod() {
		Assert.assertTrue(false);
	}
}
```
### When You Run the Class:
When we ran the above example class TestNG report has one Test Failure and one Test marked as Skipped.
```Console
===============================================
    Default test
    Tests run: 3, Failures: 1, Skips: 1
===============================================
```
![skip-test-in-testng-results](/img/testng/skip-test-in-testng-results.png)
