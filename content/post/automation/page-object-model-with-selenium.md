---
title: "Page Object Model- With Selenium WebDriver"
date: 2018-02-24
draft: false
author: Pawan Garia
description: "Page Object Model and Abstraction techniques can be combined to hide the complexity of logic and establish desired actions on top of the page we are trying to Test."
categories: [ "Automation Testing", "Selenium WebDriver"]
keywords: "Selenium,WebDriver,Page Object model,Abstraction,framework"
logothumb: "/img/logothumb/SeleniumWebDriver.png"
tags: [
    "Test Automation",
    "Selenium WebDriver","Page Object model"
]
---
This post will elaborate more about the simple idea behind page abstraction while using page object model in test automation frameworks.

[Abstraction](https://www.tutorialspoint.com/java/java_abstraction.htm) is a technique used in software design that aims to simplify logical entities code representation and allows the software developer to “hide” complex implementations. The goal is to create an abstract (simple) layers which will only expose the most relevant properties and capabilities for the specific entity.

The Abstraction technique comes handy also when writing a test automation project; We can use it to hide complexity and establish desired actions on top of the page we are working on. In a matter of fact, we can define the project as a multilayer structure, this structure will help to implement the logic of functional testing with minimum effort and exposure to the complexity of the infrastructure.

### Page Object Model in Selenium
Page Object model is an object design pattern in User Interface Testing, It can be Website, M-Site or Mobile Apps, where web pages are represented as simple classes, and the various elements on the page are defined as variables and used in methods as a single entity. Selenium WebDrive or Appium is best example which is utilizing the page object model in automation industry and All possible user actions and interactions can then be implemented as methods on the class:
```
setUserName(String username);
setPassword(String password);
clickForgotPassword();
```
We can see from the example above the well-named methods in classes are very easy to read, this works as an elegant way to implement test routines that are both readable and easier to maintain or update in the future. Anyone can open the class and update methods mean our class is the so-called document for our Test.

A page object simply models these as objects within the test code. For each web page of your application, you can create the corresponding class which encapsulates the mechanics required to perform some action on that page.

So every web page representing a class or Page Objects containing all the web elements need to be initialized before the web elements can be used in our Test Class. This can be done simply through the use of the new keyword in Java.
```Java
LoginPage page = new LoginPage(WebDriverInstance);
```
Assume that in our application we have a login page which offers regular log in functionality. We can create a page object for login page that will represent all login methods as actions, Web page looks like below where the constructor is accepting the WebDriver instance.
```Java
import org.openqa.selenium.WebDriver;

public class LoginPage {
    private WebDriver driver;

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }
}
```
Now after making our basic Page model for login and initializing the constructor which is accepting the WebDriver instance and we can start adding our methods which will serve as functionalities our login page offers. Our login page contains a login method which returns us the Home Page after login.
###LoginPage
```Java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;

public class LoginPage {

    private WebDriver driver;

    private static final String LOGIN_PAGE_URL = "https://www.facebook.com/";
    private static final By USERNAME_INPUT = By.name("email");
    private static final By PASSWORD_INPUT = By.name("pass");
    private static final By LOG_IN_BUTTON = By.id("loginbutton");

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    public LoginPage open() {
        driver.get(LOGIN_PAGE_URL);
        return this;
    }

    public HomePage loginAs(String username, String password) {
        driver.findElement(USERNAME_INPUT).sendKeys(username);
        driver.findElement(PASSWORD_INPUT).sendKeys(password);
        driver.findElement(LOG_IN_BUTTON).click();

        return new HomePage(driver);
    }
}
```
###HomePage
```Java
import org.openqa.selenium.WebDriver;

public class HomePage {
    private WebDriver driver;

    public HomePage(WebDriver driver) {
        this.driver = driver;
    }
}
```
### Why should we use page objects?
They reduce the amount of duplicated code. If there is some change in the website or mobile User interface, in the perfect situation we need to apply the fix in only one place. Page objects increase code readable because they make the test easier to understand. They hide the details of the UI structure from the tests so we can clearly see the logic and the intention of the test. So, The public methods represent the services that the page offers
and Page objects shouldn’t expose the internals of the page.

### How to Use Page Object Class in Test
With Page Object our test are able to log in without worrying how the steps and functionality is implemented because it shouldn’t matter to the test. So lets create a simple implementation of our Login Page in our Test.
```Java
import java.util.concurrent.TimeUnit;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

public class LoginTest {

    private WebDriver driver;
    private HomePage homePage;

    @BeforeClass(alwaysRun = true)
    public void setUp() {
        driver = new ChromeDriver();
        driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
    }

    @Test(description = "Open Facebook Page and Login with username and password")
    public void loginFacebookTest() {

        LoginPage loginPage = new LoginPage(driver);
        homePage = loginPage.open().loginAs("username", "password");
    }
}
```
This is a very simple example for understanding the concept of the Page object and data abstraction concept in our Test Automation. You can follow the approach used in this post and start extending the page objects and create more Classes.That’s all in this part. I hope that I gave you a brief idea of what Page Object Pattern is. You have learned what page objects are and what benefits they bring to your tests. You have seen how to use page objects on a real example.
