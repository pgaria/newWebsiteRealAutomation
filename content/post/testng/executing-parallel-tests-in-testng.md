---
title: "Executing Parallel Tests in TestNG"
date: 2018-01-01
draft: false
description: "TestNG allows the tests to run in parallel or multi-threaded mode. Parallel test are helpfull for reduction in test execution time."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,test,parallel,execution"
image: "/img/testng/expected-exception-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","ParallelExecution"
]
---
TestNG allows the tests to run in parallel or multi-threaded mode. Parallel test are helpfull for reduction in test execution time.TestNG use the concept of [multi-threading](https://www.tutorialspoint.com/java/java_multithreading.htm) from Java.
In the configuration file, the parallel attribute on the <suite> tag us been used for the parallel execution using the testng.xml file.
**Example:**
```xml
<suite name="Suite" parallel="methods" thread-count="2" configfailurepolicy="continue">
```
Parallel attribute take multiple values, Below are the valid values:

 - **parallel= methods** : Run all your test methods in separate threads.
 - **parallel= tests** : Run all the methods in the same <test> tag in the same thread, but each   <test> tag will be in a separate thread.
 - **parallel= classes** : Run each class in a separate thread but all the methods in the same class in the same thread.
 - **parallel= instances** : Run all the methods in the same instance in the same thread.

**thread-count** attribute value allows you to specify how many threads should be allocated for the execution.

**configfailurepolicy** attribute value as **continue** allows Test to continue running after Failures.

Lets create an Example for all the values for parallel attribute type.

### 1. Test methods in parallel:
To Run Test methods parallel each test method will run in a separate single thread, So we will print the Thread ID for the particular Test and which should be different.
```java
package com.tutorial.testng.parallel;

import org.testng.annotations.Test;

public class MethodInParallel {
	@Test()
	public void firstTestMethod() {
		System.out.println("First test method, ThreadID:" + Thread.currentThread().getId());
	}

	@Test()
	public void secondTestMethod() {
		System.out.println("Second test method, ThreadID:" + Thread.currentThread().getId());
	}
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Suite" parallel="methods" thread-count="2" >
  <test name="Test" group-by-instances="true">
    <classes>
      <class name="com.tutorial.testng.parallel.MethodInParallel" />
    </classes>
  </test>
</suite>
```
### Output:
After running the testng.xml file we can see the output with both the test methods in different ThreadID as 11 and 10, which means both test ran in parallel Thread.
```text
Second test method, ThreadID:11
First test method, ThreadID:10
===============================================
Suite
Total tests run: 2, Failures: 0, Skips: 0
===============================================
```
### 2. Test Class in parallel:
To Run Test Class parallel meaning each Class will be executed in a different Thread and the same thread is used for Test method Execution. For this Example We need to Create 2 Class which will Run in Parallel.

```java
package com.tutorial.testng.parallel;

import org.testng.annotations.Test;

public class ClassInParallelOne {
	@Test()
	public void firstTestMethod() {
	System.out.println("First test method from Class One, ThreadID:" + Thread.currentThread().getId());
	}

	@Test()
	public void secondTestMethod() {
	System.out.println("Second test method from Class One, ThreadID:" + Thread.currentThread().getId());
	}
}
```

```java
package com.tutorial.testng.parallel;

import org.testng.annotations.Test;

public class ClassInParallelTwo {
	@Test()
	public void firstTestMethod() {
	System.out.println("First test method from Class Two, ThreadID:" + Thread.currentThread().getId());
	}

	@Test()
	public void secondTestMethod() {
	System.out.println("Second test method from Class Two, ThreadID:" + Thread.currentThread().getId());
	}
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Suite" parallel="classes" thread-count="2" >
  <test name="Test" group-by-instances="true">
    <classes>
      <class name="com.tutorial.testng.parallel.ClassInParallelOne" />
       <class name="com.tutorial.testng.parallel.ClassInParallelTwo" />
    </classes>
  </test>
</suite>
```
### Output:
After running the testng.xml file we can see the output as Class One is using ThreadID:10 and Class Two is using ThreadID:11. Both the Test methods from Class Ran in the Same Thread.
```text
First test method from Class One, ThreadID:10
Second test method from Class One, ThreadID:10

First test method from Class Two, ThreadID:11
Second test method from Class Two, ThreadID:11

===============================================
Suite
Total tests run: 4, Failures: 0, Skips: 0
===============================================
```
### 3. Tests Tag Inside Suite in parallel:
Run all the methods in the same <test> tag in the same thread, but each <test> tag will be in a separate thread. In this Example we will see executing each test inside a suite in parallel, each test that is part of the test suite execution will be executed in its own separate respective thread.
```java
package com.tutorial.testng.parallel;

import org.testng.annotations.Test;

public class TestTagInParallel {
	@Test()
	public void firstTestMethod() {
		System.out.println("First test method, ThreadID:" + Thread.currentThread().getId());
	}

	@Test()
	public void secondTestMethod() {
		System.out.println("Second test method, ThreadID:" + Thread.currentThread().getId());
	}
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Suite" parallel="tests" thread-count="2" >
  <test name="Test-One" group-by-instances="true">
    <classes>
      <class name="com.tutorial.testng.parallel.TestTagInParallel" />
    </classes>
  </test>
  <test name="Test-Two" group-by-instances="true">
    <classes>
      <class name="com.tutorial.testng.parallel.TestTagInParallel" />
    </classes>
  </test>
</suite>
```
### Output:
After running the testng.xml file we can see the output as Test <Tag> Test-One is using ThreadID:10 and Test <Tag> Test-Two is using ThreadID:11. All the Class Inside the Test Tag will Run under same Thread.
```text
First test method, ThreadID:11
Second test method, ThreadID:11
First test method, ThreadID:10
Second test method, ThreadID:10
===============================================
Suite
Total tests run: 4, Failures: 0, Skips: 0
===============================================
```
![test-tag-parallel-run-result-testng](/img/testng/test-tag-parallel-run-result-testng.png)
