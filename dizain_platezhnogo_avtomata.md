# Дизайн торгвого автомата


Как Вы спроектируете Торговый автомат в Java? Это один из хороших вопросов на интервью по Java, который может быть задан на собеседованиях уровня Senior developer. Вам будет дана постановка задачи разработать Торговый автомат. В течение ограниченного времени, как правило, 2-3 часа, вам необходимо  спроектировать  дизайн, написать рабочий код и unit тесты на Java.

Одним из ключевых преимуществ таких Java интервью является, что  сразу можно проверить многие основные навыки кандидата. Для завершения проектирования, кодирования и unit тестирования торгового автомата, кандидат должен быть действительно хороший во всех трех областях. Кстати, подобные задачи из реального мира  является хорошими упражнениями для улучшения навыков объектно ориентированного анализа и дизайна ([смотри тут](http://javarevisited.blogspot.sg/2014/01/10-tips-to-improve-programming-skill-become-better-programmer.html)), который является очень важным, если вы хотите стать хорошим разработчиком.

Разрабатывая автомат в Java или любом другом объектно ориентированным языке, вы не только изучаете основы например, инкапсуляция, полиморфизм или наследования, но также узнаете тонкие детали о том, как использовать абстрактный класс и интерфейс (см. здесь) при решении задач или разработке приложения.

Как правило такого рода задачи дают возможность использовать шаблоны проектирования Java, например, в этой задаче будет использован  шаблон фабрики для создания различных типов торговых автоматов. Об этом можно почитать ([здесь](http://javarevisited.blogspot.com/2012/03/10-object-oriented-design-principles.html)).

Этоа серия из двух статей будет показывать решение задачи по созданию торгвого автомата в Java. Кстати, эта задача  может быть решена различными способами. Это также дает возможность пересмотреть принципы проектирования SOLID и ООП паттерны (см. здесь) и уметь использовать их в коде. 

# Постановка задачи

Вам необходимо разработать Торговый автомат который:

1. Принимает монеты определенного номинала.
2. Предостовляет возможность пользователю выбрать продукты.
3. Разрешите клиенту получить возврат денег вслучае отмены заказа.
4. Выдать выбранные товар и если необходмо сдачу
5. Обеспечить вомзжность выполнить операцию сброса для сервисного обслуживания.

Постановка задачи является наиболее важной частью. Нужно несколько раз перечитать постановку чтобы как можно лучше понять суть проблемы и то, что вы пытаетесь решить. Обычно [требования не очень ясны](http://javarevisited.blogspot.com/2015/01/difference-between-functional-and-nonfunctional-requirements-software-development.html) и вам нужно составить их перечень самостоятельно, читая постановка задачи.

Мне нравится требования на основе списков, потому их легко  отслеживать. Некоторые требования могут быть неявные, поэтому в вашем списке лучше их сделать явными. Например в этой задаче, автомат не должен выполнять запрос, если нет необходимых средств на сдачу.а

К сожалению существует не много книгу или курсов, которые научат вас этим навыкам. Необходимо развивать их самостоятельно, выполняя рельные задачи. Хотя, две книги, которые помогли мне улучшить навыки объектно ориентированного анализа и проектирования  это [Head First Object Oriented Design and Analysis](http://www.amazon.com/dp/0596008678/?tag=javamysqlanta-20) 1-е издание, Бретт Маклафлин. Одина из лучших книги, если у вас нет большого опыта работы в объектно-ориентированном программировании.

![Read more: http://javarevisited.blogspot.com/2016/06/design-vending-machine-in-java.html#ixzz4AjWrUb9m](https://3.bp.blogspot.com/-lQExXlp7hDo/V1OQ5QaEfaI/AAAAAAAAGGw/MV2964R-1-klxt4to7VcR-iciBbkZ4qjACLcB/s320/Head%2BFirst%2BAnalaysis%2Band%2BDesign.JPG)

Еще одна очень хорошая книга по разработке приложений и дизайну систем дизайн это [UML for Java Programmers by Robert C. Martin](http://www.amazon.com/UML-Java%C2%BF-Programmers-Robert-Martin/dp/0131428489?tag=javamysqlanta-20), один из моих любимых авторов. Я прочитал несколько книг о нем, например "Clean Code", "Clean Coder" и книгу по разработке программного обеспечения с использованием Agile. Он является одним из лучших в преподавании OOП концепции.

В этой книге есть аналогичная проблема о проектировании кофе-машина. Так что, если вы хотите бльше практики или попробовать ваши навыки проектирования объектно-ориентированный, вы можете обратиться к этой проблеме. Это также очень хорошее обучение exercis

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