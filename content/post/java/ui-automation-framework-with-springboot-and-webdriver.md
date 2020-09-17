---
title: "UI automation framework with Spring Boot and WebDriver - Part 1"
date: 2020-09-15
draft: true
author: Pawan Garia
description: "Creating an UI automation framework with Spring Boot and WebDriver. This article can be used for setting Up, learning the basics and running our first test using Spring Boot."
keywords: "SpringBoot,Testing,Test Automation,Java,WebDriver"
categories: [ "Java"]
tags: [
    "Java",
    "Spring Boot",
    "Test Automation",
    "WebDriver"
]
---
Spring is an established Java application framework and widely used in multiple projects. The past couple of weeks I am trying to write several kind of tests using the Spring Boot for our product. As we are using the Spring Boot for developing the Rest Services and we would like to create the Test automation framework as well in the Spring. we started of with some integration tests for our services but in this article I would like to describe how Spring can be used for the UI Automation Framework using Selenium WebDriver.

In this article, we will learn how we can create a simple spring application and create a basic Test automation framework. We'll start with a simple scenario – opening a browser window, navigating to www.google.com and then searching some text on the google search. We will try to use the concepts of the Spring like dependency injection, Beans and try to organize the automation testing code.

## Dependencies
I am using the following tech stack while writing this article and it might be different for you when you read this article.
- Gradle:4.8.1
- Spring Boot: 2.3.3
- Selenium WebDriver: 3.141.59
- Chrome Browser: 85
- Chrome Driver: 85.0.4183.87
- JUnit: 5.6.2

## Project Setup
In intellij IDEA create a new project.
![String new project setUp](/img/java/spring-new-project.png)

Provide the proper Group name, Select Java version, Select the project type like Gradle or Maven, We are going to use the Gradle as the build tool here in this article.
![Project Name and Type of Project](/img/java/spring-group-name-java.png)

## First Test

Add the following WebDriver dependency in the build.gradle file of the project.

 compile group: 'org.seleniumhq.selenium', name: 'selenium-java', version: '3.141.59'

After the Project is build on the idea you will see the Test class which is generated automatically in the src/test/java directory. We can use the same Test class for creating a simple Test and write all the Steps.

###
