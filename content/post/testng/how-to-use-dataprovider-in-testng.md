---
title: "How to use DataProvider in TestNG?"
date: 2017-12-28
draft: false
description: "How to pass multiple parameters into a @Test method using DataProvider in TestNG. DataProvider can be helpful in verifying a multiple set of Data against functionality."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,parameters,DataProvider,test"
image: "/img/testng/dependsOnGroup-method-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","DataProvider"
]
---
There are multiple cases, when we have to validate our use-cases where the logic should be same but the test data is different and we must validate our logic for multiple parameters.
TestNG provides a very easy way of passing parameters using the **DataProvider**. In this article we are going to learn how to use DataProvider with @Test.

>A Data Provider is a method on your class that returns an array of array of objects.

### How to pass String parameter in DataProvider:
```Java
package com.tutorial.testng.parameters;

import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class DataProviderInTest {

	@Test(dataProvider = "userDetails")
	public void testUserDetails(String user, String password) {
		System.out.println("Test User: " + user + " Password: " + password);
	}

	@DataProvider(name = "userDetails")
	public Object[][] userDetails() {

		return new Object[][] {
			{ "user1", "password1" },
			{ "user2", "password2" },
			{ "user3", "password3" } };
	}
}
```
#### Output:
```text
Test User: user1 Passsword: password1
Test User: user2 Passsword: password2
Test User: user3 Passsword: password3
```
![dataprovider-testng-result](/img/testng/dataprovider-testng-result.png)

I have used String here in this example but you can pass other [data types](http://cs.fit.edu/~ryan/java/language/java-data.html) also using the data provider.

### How to pass Class Object in DataProvider:
Using DataProvider you can pass Class object parameters.So we need to create multiple objects of a particular Class and passed as DataProvider to our @Test method.

**Step 1:** Create a User Class [POJO](https://www.quora.com/What-is-POJO-in-Java) which has User and Password Value.
```Java
package com.tutorial.testng.parameters;

public class User {

	private String user;
	private String password;

	public String getUser() {
		return user;
	}

	public String getPassword() {
		return password;
	}

	public User setUser(String user) {
		this.user = user;
		return this;
	}

	public User setPassword(String password) {
		this.password = password;
		return this;
	}
}
```
**Step 2:** Now create a Test Class with DataProvider and pass multiple **User.java** object and print the user details in our TestCase.
```Java
package com.tutorial.testng.parameters;

import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class DataProviderWithObjectTest {

	@Test(dataProvider = "userDetails")
	public void testUserDetails(User users) {
		System.out.println("Test User: " + users.getUser() + " Password: " + users.getPassword());
	}

	@DataProvider(name = "userDetails")
	public Object[][] userDetails() {

		return new Object[][] {
			{ new User().setUser("user1").setPassword("password1") },
			{ new User().setUser("user2").setPassword("password2") },
			{ new User().setUser("user3").setPassword("password3") }, };
	}
}
```
#### Output:
```text
Test User: user1 Password: password1
Test User: user2 Password: password2
Test User: user3 Password: password3
PASSED: testUserDetails(com.tutorial.testng.parameters.User@2f686d1f)
PASSED: testUserDetails(com.tutorial.testng.parameters.User@69ea3742)
PASSED: testUserDetails(com.tutorial.testng.parameters.User@3159c4b8)
```
![dataprovider-object-testng-results](/img/testng/dataprovider-object-testng-results.png)

### What If you pass wrong No of arguments in DataProvider:
DataProvider expectes you to pass and receive the same set of arguments, If there is a mismatch in the arguments, It will thrown an exception and TestCases will not Run.

There is [`org.testng.internal.reflect.DataProviderMethodMatcher`](http://static.javadoc.io/org.testng/testng/6.11/org/testng/internal/reflect/DataProviderMethodMatcher.html) class in testNG used for verification, using a method `getConformingArguments()` for verifying the no of arguments.
```Java
@Override
public Object[] getConformingArguments(){
  if (ThreeStateBoolean.NONE.equals(getConforms())) {
    conforms();
  }
  if (matchingMatcher != null) {
    return matchingMatcher.getConformingArguments();
  }
  throw new MethodMatcherException("Data provider mismatch", getContext().getMethod(), getContext().getArguments());
}
}
```
**In this Example below we will pass an extra Argument from the DataProvider but will not accept that argument in our @Test method and Test will fail with exception.**
```Java
package com.tutorial.testng.parameters;

import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class DataProviderFailure {

	@Test(dataProvider = "userDetails")
	public void testUserDetails(String user, String password) {
		System.out.println("Test User: " + user + " Password: " + password);
	}

	@DataProvider(name = "userDetails")
	public Object[][] userDetails() {
		return new Object[][] { { "user1","password1","extraValue" },
			                { "user2","password2","extraValue" },
			                { "user3","password3","extraValue" } };
	}
}
```
#### Output Exception:
```text
org.testng.internal.reflect.MethodMatcherException:
Data provider mismatch
Method: testUserDetails([Parameter{index=0, type=java.lang.String, declaredAnnotations=[]}, Parameter{index=1, type=java.lang.String, declaredAnnotations=[]}])
Arguments: [(java.lang.String)user1,(java.lang.String)password1,(java.lang.String)application]
```
