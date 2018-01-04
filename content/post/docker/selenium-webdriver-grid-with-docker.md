---
title: "Setup Selenium WebDriver Grid with Docker"
date: 2018-01-04
draft: true
description: "Retry or ReRun failed Test cases multiple times by using IRetryAnalyzer Interface. The article explains ways to rerun the TestNG tests when they are failed."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,retry,ReRun,test,FailedTest,IRetryAnalyzer,Listener"
image: "/img/testng/dependsOnGroup-method-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "Docker","WebDriver-Grid"
]
---
Integration tests are an integral part of any modern web application, and regardless of which front-end or server side framework you choose, you'll likely be running Selenium tests. While Selenium tests are easy to write and execute on your local workstation, Works On My Machine™ won't get you past your CI system. Now you're stuck with two bad options, use a SaaS provider and live with slow builds or running your own Selenium Grid and managing a multitude of machines and browser versions. Rock, meet hard place, right? It doesn't have to be that way! In this talk you'll see how easy it is to setup a Selenium Grid with Docker, how easy it is to maintain, and how to extend and grow your Selenium grid to satisfy your team's needs. It's not all roses and sunshine, so you'll see some common issues presented and how to avoid them. Finally, a Selenium Grid you'll want to manage!

Challeneges : If you use Selenium Grid U have to install so many configurtions and tools which you test are depend on like Java,Selenum WebDriver, Browsers, VNC etc.
Why Docker : With containerized test executors, the test suites can be executed on any platforms without library dependencies.
