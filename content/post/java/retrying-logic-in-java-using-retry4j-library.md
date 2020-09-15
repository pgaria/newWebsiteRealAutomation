---
title: "Retrying unreliable logic in java using Retry4j Library"
date: 2020-07-22
draft: false
author: Pawan Garia
description: "How to use the Lightweight Java library for retrying unreliable logic, handle unexpected exceptions, set conditions for retry and much more can be done using Retry4j"
keywords: "Retry4j,retry,java-library"
categories: [ "Java"]
tags: [
    "Java",
    "Retry4j",
    "java-library"
]
---
While implementing specific requirements in java, we often need to deal with unreliable codes, timeout, Unexpected exceptions, and need to retry-operations because of the asynchronous process and functionalities in the application. Like while working on one of the projects which were not very new and service-oriented application we figured out that multiple factors can make the automation test and framework unreliable resulting in the wrong results and values. So we must find a way to Retry the logic in java which can be light and highly configurable.

So we decided to use nice open source library [Retry4j](https://github.com/elennick/retry4j) which is easy and simple to implement our retry logic. In this tutorial, I would like to show you how to handle those unexpected exceptions and conditions using the Retry4j library.

## Retry4j
[Retry4j](https://github.com/elennick/retry4j) is a simple Java library to assist with retrying transient failure situations or unreliable code. Retry4j aims to be readable, well documented, and streamlined.

#### Dependencies:
Can be added in both Gradle and Maven projects. Here is an example of Gradle.
#### Gradle:
```
compile group: 'com.evanlennick', name: 'retry4j', version: '0.15.0'
```
## Example for handling the Exceptions
To demonstrate the example of handling the exception and extracting a return value from a method that throws a Run time exception every time until a specific limit is reached within the method. In the end, the method should be returning the String 'Retry' instead of throwing a Run time exception.

#### RetryConfig:
First Step is define retry Configuration using the `RetryConfig.class` like below:

```Java
RetryConfig retryConfig = new RetryConfigBuilder().withMaxNumberOfTries(5)
                                   .withFixedBackoff().withDelayBetweenTries(2, SECONDS)
                                   .retryOnSpecificExceptions(RuntimeException.class).build();
```
You can see I have used the `Maximum Retries as 5` and included the `delay of 2 seconds` and in the end we have defined which exception we should be handling here like `RuntimeException.class`.

#### Callable:
After the Configuration is set we need to create a Callable statement

```Java
Callable<Object> callable = () -> {
    //code that you want to retry until success OR retries are exhausted OR an unexpected exception is thrown
};
```
#### CallExecutorBuilder
Now the final step is to execute the callable statement we have created using the Retry Configuration like the below Example:
```Java
Status<Object> result = new CallExecutorBuilder().config(config).build().execute(callable);
```
Remember the callable can have any of the return types So we can create a simple variable and define which return type result we want to store in the Status variable.

### Example:
```Java
public class RetryLogicExample {
    private int counter = 0;

    public String getRetryString() {
        System.out.println("In method trying for : " + counter);
        counter++;
        if (counter < 4) {
            System.out.println("Throw the Exception");
            throw new RuntimeException();
        }
        return "Retry";
    }

    public static void main(String[] args) {
        RetryLogicExample retry = new RetryLogicExample();
        RetryConfig retryConfig = new RetryConfigBuilder().withMaxNumberOfTries(5)
                                         .withFixedBackoff().withDelayBetweenTries(2, SECONDS)
                                         .retryOnSpecificExceptions(RuntimeException.class).build();
        Callable<Object> callable = () -> {
            return retry.getRetryString();
        };
        Status<String> result = new CallExecutorBuilder().config(retryConfig).build().execute(callable);
        System.out.println("Retry string at the End: " + result.getResult());
    }
}
```

### Output:
Running the above code produces the result like below where the method failing for `3` times(String with 0) but succeeding at last and We print the result from the method.
```
In method trying for : 0
Throw the Exception
In method trying for : 1
Throw the Exception
In method trying for : 2
Throw the Exception
In method trying for : 3
Retry string at the End: Retry
```

Retry4j provides lots of options and configurations you can set for the condition you need to handle like for example if we need to run the code again on a specific value it returns can be done through the simple config changes.

#### Value Handling Config
```Java
RetryConfig config = retryConfigBuilder
        .retryOnReturnValue("retry on this value!")
        .build();
```

#### Value and Exception Handling Config      
```Java
RetryConfig config = retryConfigBuilder
        .retryOnSpecificExceptions(FileNotFoundException.class)
        .retryOnReturnValue("retry on this value!")
        .build();  
```    

I would suggest you to check and read the [documenation](https://github.com/elennick/retry4j) and try to implement the examples from the Retry4j Git Hub Page. You can also find the same Java example as well in the [GitHub Location.](https://github.com/pgaria/Retry4JExample)
