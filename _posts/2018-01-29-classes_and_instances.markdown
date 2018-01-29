---
layout: post
title:      "Classes and Instances"
date:       2018-01-29 11:54:57 -0500
permalink:  classes_and_instances
---


Here's a quick review of classes and instances. 

**Class methods and Instance methods:** 

A class method belongs to that class and is not tied to any particular single instance. You can invoke directly on a class without a need for any instance of that class. They can't access instance variables but have access to class variables.

Instance methods are methods that are called on an instance of a class. You need an instance of the class to call it. They have access to the object's instance variables and can use then to change their behavior based on the values in those variables.

You would use an instance method when you need to act on a particular instance of the class. It is tied to the identity of a particular object. When the functionality you are writing does not belong to an instance of that class, you would use a class method. The key difference is instance methods only work with an instance and so you have to create a new instance to use them.


**Class variables and Instance variables:**

Class variables are available from the class definition and any sub-classes but not available from anywhere outside.

Instance variables are available only within a specific object, across all methods in a class instance but not available directly from class definitions. They can be accessed with a setter or getter.
