---
title: "Introduction to Microservices"
date: 2018-01-29
draft: true
description: "Learn the basics of Microservices "
categories: [ "Automation Testing","Docker"]
keywords: "Docker,container,Microservices,Rest,API"
image: "/img/docker/docker-container-thumb.png"
logothumb: "/img/logothumb/Docker-logo.png"
tags: [
    "Test Automation",
    "Docker","Microservices"
]
---
### What are microservices?
So what is a microservice? Ask 20 developers what is a microservice, and you’re likely to get 25 different answers. Here’s my best answer. A microservice is basically a self contained process that provides a unique business capability. Different microservices act in concert as one larger system, but behind the scenes, it’s microservices all the way.

Take Amazon.com for instance. All we see is the website. Behind the scenes, Amazon has a service for accepting orders, a service for determining a list of recommended items to buy, a service to handle wish lists, a service for authenticating credit cards, and so on. All of these services are really mini applications performing a single business capability.

What makes this organization interesting is, it is not organized around software layers. We don’t create a web microservice, business logic microservice or database microservice. We are creating microservices around business capabilities. So we have a wish list microservice, a order microservice, and a processing microservice.

Microservices communicate with each other through a well-defined interface, usually REST or a messaging queue. The communication is almost always stateless.

The microservice data is federated as well. That means every microservice is responsible for its own data model and data. I think that statement just caused heart palpitations for some DBAs. There might be good business reasons to standardize the data platform for all microservices, but the idea is if we’re on the team building the microservice, we should know best which persistence model we need.

### Why use microservices?
It’s the latest buzzword, so why not? Seriously though, microservices make our system loosely coupled. If we need to upgrade, repair, or replace a microservice, we don’t need to rebuild our entire application. Just swap out the part that needs it. Rebuilding isn’t a big deal for a small desktop app, but it can be a killer for a whole enterprise system.

Breaking our system into small parts also allows each microservice to focus on a single business capability. One benefit of this is we can have a small team focused on a simple task. Jeff Bezos from Amazon created the two pizza rule, the ideal size for a microservices team is one you can feed with two pizzas.

Single benefit capabilities also help drive the focus of what our group is trying to accomplish. It becomes very easy to determine if a feature is in or out if our goal is focused.

A simple interface means we can use different languages and tools. If we’ve got a great well-defined interface, it doesn’t matter what language or architecture we use. We could write our service in Visual Basic if we wanted. As long as we publish how other services are to communicate with our service, we’re good to go. That’s why the most common methods for communication between mircoservices are HTTP and messaging. Everybody supports that.

### What are some microservice best practices?
So what makes a great microservice? We’ve touched upon a few things already.

First, I’ll say it again. It focuses on a single business capability. That isn’t to say it does only one thing. Just one business thing. It does not mean we divide into a web capability, logic capability, and database capability. That’s going back to what we had before.

Each microservice should have it’s own datastore. This limits what can change under the microservice. If another group is responsible for the data model, they can also change the data model. That can force downstream changes for many microservice teams. Changes that might not help our team at all.

microservices should work when other microservices fail - DeegeU
Microservices should work when other microservices fail

Make communication stateless. If our communication is stateless, we can add more copies of the microservice at will. Stateless communication gives our microservice the ability to scale effortlessly. Each interaction with our microservice can be handled by a different instance.

Since the communication is stateless, great microservices should be able to continue if another microservice fails. That means if we can’t communicate with another service, our service should be able to make due. If we’re trying to get a list of suggested titles from another service, we’ll have a default ready just incase. Our service should never break, just because another service broke.

Finally, a good team makes a good microservice. We need all parts represented. That’s a designer, web developer, coder, database admin, and operations all on our microservice team. Not representatives. On the team. It’s everyone we need to make a fully functional application. Up to two pizzas of course. And that doesn’t mean find lots of developers who hate pizza.
