---
title: "Object Oriented Programming Principles and Concepts Explained!"
date: 2018-03-04
draft: false
author: Pawan Garia
description: "Learn about the essentials of object-oriented programming principles and concepts. How the object-oriented paradigm differs from the structured-programming paradigm."
categories: [ "Java","Tutorials"]
keywords: "Java,object-oriented programming,concepts,principles,introductory tutorial"
tags: [
    "java",
    "object-oriented","programming"
]
---
### What is an object?
Object-oriented languages follow a different programming pattern from structured programming languages like C and COBOL. The Java language is not completely an object-oriented. The structured-programming paradigm is highly data-oriented: You have data structures, and then program instructions act on that data. Object-oriented languages such as the Java language combine data and program instructions into objects.

An object is a self-contained entity that contains attributes and behavior. Instead of having a data structure with fields (attributes) and passing that structure around to all the program logic that acts on it (behavior), in an object-oriented language, data and program logic are combined. It's like any physical thing in the world, One Object which we create has significant properties that distinguish from other objects. Like we know Fish is a Fish because it swims, live in water but every fish is different from other fish.

#### Parent and child objects
A parent object is one that serves as the structural basis for deriving more-complex child objects. A child object looks like its parent but is more specialized. With the object-oriented paradigm, you can reuse the common attributes and behavior of the parent object, adding to its child objects attributes and behavior that differ.

#### Objects and References
When you create an object, what happens behind the scene is that a piece of memory is reserved for containing that object. This could be anywhere in our memory managed by the operating system. Now how do you use that object if you don't know where in memory it is? How can you read a value from it if you don't know where that value is stored? This is what References do for you as they are a way of keeping in touch with the object.

![OOPS-Class-and-Object-Concept](/img/java/OOPS-Class-and-Object-Concept.png)

#### How object Communicate
Objects are conceptually like real-world objects means they too consist of state and related behavior. An object stores its state in fields like variables in Java and exposes its behavior through methods like functions in Java. So, we can say that in an object-oriented application, Objects talk to other objects by sending messages like method calls in Java. Furthermore, program code coordinates the activities among objects to perform tasks within the context of the specific application domain.

It's time to see what a [Java](https://www.java.com/en/download/faq/whatis_java.xml) object looks like.

### Example: A Human object
My first example is based on a common application-development scenario: an individual being represented by a Human object.

You know from the definition of an object that an object has two primary elements: attributes and behavior. Here's how these elements apply to the Human object.

#### Attributes
What attributes can a Human have? Some common ones include:

* Name
* Age
* Gender
* Height
* Weight

You can probably think of more (and you can always add more attributes later), but this list is a good start.

#### Behavior
An actual Human can do all sorts of activities and can have any behavior, but object behaviors usually relate to application context of some kind. In a business-application context, for instance, you might want to ask your Human object, "What is your body mass index (BMI)?" In response, Human would use the values of its height and weight attributes to calculate the BMI.

More-complex logic can be hidden inside of the Human object, but for now, suppose that Human has the following behavior:

* Calculate BMI
* Print all attributes
* State and string  

The state is an important concept in OOP. An object's state is represented at any moment in time by the values of its attributes. In the case of Human, its state is defined by attributes such as name, age, height, and weight etc. and several of those attributes can be on the list.

Using the concepts of state and string together, you can say to Human, *Tell me all about you by giving me a String of your attributes*.

### Principles of OOP
So, the attributes of a Human and any logic to retrieve (and convert) those values can be written in C and C++, So how object-oriented programming is different from other structured-programming. The simple answer is by understanding the principles of OOP like **Inheritance, Encapsulation, Abstraction, and Polymorphism**.
![Object-Oriented-Programming-Principles](/img/java/Object-Oriented-Programming-Principles.png)

#### Inheritance
In structured programming, it's common to copy a structure, give it a new name, and add or modify the attributes that make the new entity different from its original source. Over time, this approach generates a great deal of duplicated code, which can create maintenance issues.

OOP introduces the concept of inheritance, whereby specialized classes — without additional code — can "copy" the attributes and behavior of the source classes that they specialize. If some of those attributes or behaviors need to change, you override them. The source object is called the parent, and the new specialization is called the child.

Suppose that you're writing a human-resources application and want to use the Human class as the basis (also called the superclass) for a new class called Employee. Being the child of Human, Employee would have all the attributes of a Human class, along with additional ones, such as:

* Employee number
* Gender
* Address Details
* Leaves
* Salary

So, we can see Inheritance makes it very easy to create the new Employee class without copying the details from the Human class.

#### Encapsulation
Combining data and functions into a single unit or self-contained. This characteristic is the principle of encapsulation at work. Data Hiding is another term that's sometimes used to express the self-contained, protected nature of objects.

Regardless of terminology, what's important is that the object maintains a boundary between its state and behavior and the outside world. Like the data is not accessed directly, It is accessed through the functions present inside the class. Attributes of the class are kept private and public getter and setter methods are provided to manipulate these attributes.

On the Java platform, you can use access modifiers to vary the nature of object relationships from a public to private. Public access is wide open, whereas private access means the object's attributes are accessible only within the object itself. The public/private boundary enforces the object-oriented principle of encapsulation.

#### Abstraction
Abstraction is a process where you show only “relevant” data and “hide” unnecessary details of an object from the user. Consider your mobile phone, you just need to know what buttons are to be pressed to send a message or make a call, what happens when you press a button, how your messages are sent, how your calls are connected is all abstracted away from the user. So, by using abstraction, we can hide the complex systems and present simple system user can understand.

#### Polymorphism
Polymorphism means that more than one form, same object performing different operations according to the requirement. It describes the concept that different classes can be used with the same interface. Each of these classes can provide its own implementation of the interface. In Simple terms classes have different functionality while sharing a common interface.

Polymorphism is one of the more complex concepts you'll encounter in OOP on the Java platform and is beyond the scope of this introductory article, So I will try to cover the Polymorphism in depth in coming articles.

### Java is pure object oriented or not?

There are a lot of arguments around whether Java is purely object-oriented or not. I believe two qualities differentiate the Java language from purely object-oriented languages such as [Smalltalk](https://en.wikipedia.org/wiki/Smalltalk). First, the Java language is a mixture of objects and primitive types like int, float etc. (There are total of eight primitive types). Second, with Java, you can write code that exposes the inner workings of one object to any other object that uses it.
