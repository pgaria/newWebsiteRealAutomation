---
title: "Write REST Assured request and response to the log file"
date: 2020-06-07
draft: false
author: Pawan Garia
description: "Learn how the Logging works in the REST Assured and using Filter Interface how we can capture and then write the request and response to the log file!"
categories: [ "Tutorials" ]
keywords:  "Rest,API,RestAssured,logs,Filters"
tags: [
    "API",
    "REST Assured",
    "logs"
]
---
Testing and validating REST services in any application are hard and confusing, especially in java. [REST Assured](http://rest-assured.io/) brings the simplicity as we can easily use REST Assured Java API'S to send the Request and then validate anything from the response.

But while doing this simple request and response I face the problem now and again is how to configure REST Assured logs to be written to some file or test logs instead of printed to the console. Writing the exact Requests and responses to the log file will help us in future debugging of failures or a better understanding of our Test flow, But I was not able to find a better solution around the internet so that I can simply use it in our project. So this blog post intends to demonstrate how this can be achieved in a very simple manner.

### How Logging works in REST Assured:
REST Assured works with the Rest and Response Specifications concept which it utilizes for the logging of the request and response. This can be simply done like the following and prints everything on the console
```
String getUrl = "http://dummy.restapiexample.com/api/v1/employees";
given()
       .log().all().
when()
       .get(getUrl).
then()
       .log().all().statusCode(200);
```
Now If you run the above request you see the result on the console like below:
```
Request method:	GET
Request URI:	http://dummy.restapiexample.com/api/v1/employees
Proxy:			<none>
Request params:	<none>
Query params:	<none>
Form params:	<none>
Path params:	<none>
Headers:		Accept=*
Cookies:		<none>
Multiparts:		<none>
Body:			<none>
HTTP/1.1 200 OK
Date: Sun, 07 Jun 2020 15:03:47 GMT
Vary: Accept-Encoding,User-Agent,X-APP-JSON
X-Sol: pub_site
Server: nginx/1.16.0
Response: 200
{
    "status": "success",
    "data": [
        {
            "id": "1",
            "employee_name": "Tiger Nixon",
            "employee_salary": "320800",
            "employee_age": "61",
            "profile_image": ""
        }]
}
```
So what is happing in the first `log().all()` with the `GIVEN` statement logs everything in the Request Log Specification and which can print the Request part. Below is the Actual method from the [RequestSpecification.java](https://www.javadoc.io/doc/io.rest-assured/rest-assured/3.1.1/io/restassured/specification/RequestSpecification.html) Interface of Rest Assured.
```
/**
 * Returns the {@link RequestLogSpecification} that allows you to log different parts of the {@link RequestSpecification}.
 * This is mainly useful for debug purposes when writing your tests.
 *
 * @return the request log specification
 */
RequestLogSpecification log();
```
and the second `log().all()` with the `THEN` statement asks Rest Assured to Print Everything from the Validatable Response Log Specification. Below is the actual method from the [ValidatableResponseOptions.java](https://www.javadoc.io/doc/com.jayway.restassured/rest-assured/2.6.0/com/jayway/restassured/response/ValidatableResponseOptions.html) Interface.
```
/**
 * Returns the {@link ValidatableResponseLogSpec} that allows you to log different parts of the {@link Response}.
 * This is mainly useful for debug purposes when writing your tests.
 *
 * @return the validatable response log specification
 */
ValidatableResponseLogSpec<T, R> log();
```
One more thing to notice is I am using the `all()` method with the `Log()` which prints everything from the specifications but instead of printing everything we can use multiple methods for filtering the output of LOG.  
![ValidatableResponseLogSpec Interface Methods](/img/java/ValidatableResponseLogSpec-Methods.PNG)

Now this is all fine and we know how we can print logs and it will help in the debug process of tests but what if you don’t want to log messages to the console but instead logging to a file or .log4j file?

### Logging REST Assured messages to log File:
The way I found to intercept the request and response from the REST Assured and Writing the same into the log file is through the pretty simple use of the Filters. So what are these filters in REST Assured?

As per the [Filter Interface](https://github.com/rest-assured/rest-assured/blob/master/rest-assured/src/main/java/io/restassured/filter/Filter.java) in REST Assured, A filter allows you to inspect and alter a request before it's actually committed and also inspect and alter the response before it's returned to the expectations. You can regard it as an "around advice" in AOP terms. Filters can be used to implement custom authentication schemes, session management, logging, etc.

Below is the Filter Interface which has only one method filter.
![Filter Interface in RestAssured API](/img/java/Filter-Interface-in-RestAssured.PNG)

So now we just have to make our custom filter by implementing `io.restassured.filter.Filter` interface and update the method with our requirements like We can print everything in logs or we can just print whenever the request fails or we just need to print only a basic part of the request and response.

```
public class RestAssuredRequestFilter implements Filter {
    private static final Log log = LogFactory.getLog(RestAssuredRequestFilter.class);

    @Override
    public Response filter(FilterableRequestSpecification requestSpec, FilterableResponseSpecification responseSpec, FilterContext ctx) {
        Response response = ctx.next(requestSpec, responseSpec);
        if (response.statusCode() != 200) {
            log.error(requestSpec.getMethod() + " " + requestSpec.getURI() + " => " +
                    response.getStatusCode() + " " + response.getStatusLine());
        }
        log.info(requestSpec.getMethod() + " " + requestSpec.getURI() + " \n Request Body =>" + requestSpec.getBody() + "\n Response Status => " +
                response.getStatusCode() + " " + response.getStatusLine() + " \n Response Body => " + response.getBody().prettyPrint());
        return response;
    }
}
```
So in my method implementation, I am checking if the status code from the Request is 200 or not and then Print the Error message in the log statement with `ERROR`.

And Writing the normal `INFO` level log Message with the proper customization of the details I need in the log messages. You can customize the above implementation as much you want based on your needs and design.

The next Step is now to use this new Filter we have created in our Requests and see how it prints the messages in the Logs.
```
String getUrl = "http://dummy.restapiexample.com/api/v1/employees";
given()
       filter(new RestAssuredRequestFilter()).
when()
       .get(getUrl).
then()
       .statusCode(200);
```
So just need to create an Instance of our new filter and pass that into the Filter() method before the Request statement is created. As we have multiple request and multiple types of request like GET, POST, DELETE, etc. instead of passing the new Filter object for every request we can create an Interface class which can be used to make the Request method collection and all the requests are using the same methods. This will save us from some duplication and using the Filter in one place for better code maintenance. I will try to create an article about this approach as well in the future.

Below is the example of how the log messages now write in my Log4j file with the details of Request and Reponses which is relevant to us. I have used the Simple GET calls to describe the implementation and same can be done for the POST request as well with the proper request body and Response.
```
2020-06-07 15:28:17 INFO  FailedRequestFilter:24 - GET http://dummy.restapiexample.com/api/v1/employees
 Request Body =>null
 Response Status => 200 HTTP/1.1 200 OK
 Response Body => {
    "status": "success",
    "data": [
        {
            "id": "1",
            "employee_name": "Tiger Nixon",
            "employee_salary": "320800",
            "employee_age": "61",
            "profile_image": ""
        }]
      }

```
You can find the example java + Gradle project and using log4j with the proper implementation of the above example is the following [GIT Hub Location](https://github.com/pgaria/RestAssuredLoggerExample).

Please let me know how do you find this solution and how you benefited from reading this article.
