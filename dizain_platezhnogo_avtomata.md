# Дизайн платежного автомата


Как Вы проектируете Торговый автомат в Java?? is one of the good Java interview questions mostly asked at Senior level Java developer Interviews. In a typical coding interview, you will be given a problem statement to develop a vending machine and within a limited time, usually, 2 to 3 hours you need to produce design document, working code and unit test in Java. One of the key advantages of such Java interviews is that you can test many essential skills or a candidate in one go. In order to complete  the design, coding, and unit testing of  a Vending machine, a candidate needs to be really good in all three departments. By the way, this kind of real world problem is also a good exercise to improve your object-oriented analysis and design skills ([see here](http://javarevisited.blogspot.sg/2014/01/10-tips-to-improve-programming-skill-become-better-programmer.html)), which is very important if you want to become a good application developer.

By designing a vending machine in Java or any other object-oriented language, you not only learn basics e.g. Encapsulation, Polymorphism or Inheritance but also learns subtle details of how to use an abstract class and interface (see here) while solving a problem or designing an application.

Usually, this kind of problem also gives you an opportunity to utilize Java design patterns, as in this problem we will be using Factory method pattern for creating different types of Vending Machine. I have talked about this question when I shared 20 software design questions in Java ([here](http://javarevisited.blogspot.com/2012/03/10-object-oriented-design-principles.html)), and after that, I receive a lot of feedback to provide a solution for that question.

This two-part series of posts will provide a solution of Vending machine problem in Java. By the way, this problem can be solved in a different way, and you should try to do that before looking into the solution given here. This is also an opportunity to revisit SOLID and OOPS design principles (see here) and get ready to use them in your code. You'll find many of them are applicable when you design vending machine in Java.

# Problem Statement

You need to design a Vending Machine which


1. Accepts coins of 1,5,10,25 Cents i.e. penny, nickel, dime, and quarter.
2. Allow user to select products Coke(25), Pepsi(35), Soda(45)
3. Allow user to take refund by canceling the request.
4. Return selected product and remaining change if any
5. Allow reset operation for vending machine supplier.

The requirement statement is the most important part of the problem. You need to read problem statement multiple times to get a high-level understanding of the problem and what are you trying to solve. Usually, [requirements are not very clear](http://javarevisited.blogspot.com/2015/01/difference-between-functional-and-nonfunctional-requirements-software-development.html) and you need to make a list of your own by reading through problem statement.

I like point based requirement because it's easy to track. Some of the requirement are also implicit but it's better to make it explicit in your list e.g. In this problem, vending machine should not accept a request if it doesn't have sufficient change to return

Unfortunately, there is not many book or course which teach you these skills, you need to develop them by yourself by doing some real world work. Though, two of the book which helped me to improve by object-oriented analysis and design skills are [Head First Object Oriented Design and Analysis](http://www.amazon.com/dp/0596008678/?tag=javamysqlanta-20) 1st edition by Brett D. McLaughlin. One of the best book if you don't have much experience in object oriented programming.

![Read more: http://javarevisited.blogspot.com/2016/06/design-vending-machine-in-java.html#ixzz4AjWrUb9m](https://3.bp.blogspot.com/-lQExXlp7hDo/V1OQ5QaEfaI/AAAAAAAAGGw/MV2964R-1-klxt4to7VcR-iciBbkZ4qjACLcB/s320/Head%2BFirst%2BAnalaysis%2Band%2BDesign.JPG)

Another book which is very good on developing application and system design skill is [UML for Java Programmers by Robert C. Martin](http://www.amazon.com/UML-Java%C2%BF-Programmers-Robert-Martin/dp/0131428489?tag=javamysqlanta-20), one of my favorite author. I have read several books of him e.g. Clean Code, Clean Coder and a book on software development using Agile. He is one of the best in teaching OOP concept.

This book has got a similar problem about designing a coffee machine. So, if you want to practice more or try your object oriented design skill, you can refer to that problem. It's also a very good learning exercis

# Solution and Coding

My implementation of Java Vending Machine has following classes and interfaces :


#### VendingMachine

It defines the public API of vending machine, usually all high-level functionality should go in this class

#### VendingMachineImpl

Sample implementation of Vending Machine

#### VendingMachineFactory
 
A Factory class to create different kinds of Vending Machine

#### Item
> 
Java Enum to represent Item served by Vending Machine

#### Inventory
 
Java class to represent an Inventory, used for creating case and item inventory inside Vending Machine

#### Coin
 
Another Java Enum to represent Coins supported by Vending Machine

#### Bucket
 
A parameterized class to hold two objects. It's kind of Pair class.

#### NotFullPaidException
 
An Exception thrown by Vending Machine when a user tries to collect an item, without paying the full amount.

#### NotSufficientChangeException
 
Vending Machine throws this exception to indicate that it doesn't have sufficient change to complete this request.

#### SoldOutExcepiton
 
Vending Machine throws this exception if the user request for a product which is sold out.

# How to design Vending Machine in Java
Here is the complete code of Vending Machine in Java, make sure to test this code, and let me know if you face any issue.

#### VendingMachine.java

The public API of vending machine, usually all high-level functionality should go in this class


#### VendingMachineImpl.java

A sample implementation of VendingMachine interface represents a real world Vending Machine , which you see in your office, bus stand, railway station and public places.


#### VendingMachineFactory.java

A Factory class to create different kinds of Vending Machine


#### Item.java

Java Enum to represent Item served by Vending Machine


#### Inventory.java

A Java class to represent an Inventory, used for creating case and item inventory inside Vending Machine.


#### VendingMachine.java

The public API of vending machine, usually all high-level functionality should go in this class


#### Bucket.java

A parameterized utility class to hold two objects.

####NotFullPaidException.java

An Exception, thrown by Vending Machine when a user tries to collect an item, without paying the full amount.


#### NotSufficientChangeException.java

Vending Machine throws this exception to indicate that it doesn't have sufficient change to complete this request.

#### SoldOutException.java

The Vending Machine throws this exception if the user request for a product which is sold out


That's all in this first part of **how to design a vending machine in Java**. In this part, we have solved the problem by creating all the classes and writing all code, but unit test and design document are still pending, which you will see in the second part of this article.

If you want you can try to run this problem by creating the Unit test, or maybe make it an application by using a thread and then use a different thread to act as the user. You can also read UML for Java Programmers by Robert C. Martin in the meantime.