Builder Pattern
=========

# Story
Вам запропоновано створити систему планування екскурсій по Паттернленду - новому парку розваг. Гості парку можуть вибрати готель, замовити квитки на атракціони, зарезервувати місця в ресторані і навіть замовити спеціальні заходи. Поїздки можуть відрізнятися за кількістю днів і складу розважальної програми. Наприклад, місцевий житель не бажає зупинятися у готелі, але хоче замовити обід та спеціальні заходи. Другий гість прилітає літаком і йому необхідно забронювати номер в готелі, столик в ресторані і квитки на заходи. Таким чином, нам потрібна гнучка структура даних, яка може представляти програму поселення з усіма можливими варіаціями; крім того, побудова програми може складатися з декількох кроків.

# ~~Problem~~

# Solution

---
# Цель

* Отделить создание сложного объекта от его представления так, что тот же самый процесс создания может создавать различные 
представления. 
* Разбор сложного представления, создание одной из нескольких целеой.



# Structure

![](https://upload.wikimedia.org/wikipedia/commons/9/94/Builder_design_pattern.png)

Pattent Builder (строитель) роль: дать абстрактную категорию, стандартизировать строительство различных компонентов объекта продукта. Как правило, эта категория не зависит от логики приложения. Режим Создать это прямые элементы с использованием конкретного продукта строитель (бетон Builder) роль. Конкретные классы строитель должен выполнять требования этой категории методов: один метод построения, а другой является результатом метода возврата. В частности строитель (бетон Builder) роль: поскольку эта роль тесно связана с категорией приложений, они являются примерами продуктов, созданных в приложении вызвать следующий. Основными задачами этой роли включают в себя:     Реализация классов Builder роль обеспечивает шаг за шагом через процесс создания экземпляра продукта.     После завершения процесса строительства, приведены примеры продукта. Преподаватель (директор) Роль: Эта роль как класс строитель делать вызовы, определенные роли для создания элементов продукта. Директор, который не конкретные знания категории продукта, категории продукта действительно не имеют специальных знаний о конкретных объектах застройщика. Продукт (Product) Роль: Продукт является строительство сложных объектов.
# Example
>TODO: add Java example code

#Applicable scene

* Product needs to have a complex internal structure.
* Product object properties need to generate interdependence, builder pattern can force the build order.
* Object creation process will use the system in a number of other objects, as in the creation of these products is not easy to get the object.

#Effect

* Builder pattern so that the internal representation of the product can vary independently. 
* Builder pattern allows the client does not know the details of the internal composition of the product.
* Each Builder are relatively independent, but independent of the other Builder.
* Model built by the end product easier to control.

#Advantage

Builder pattern can be very good to achieve the relevant "business" logic of an object separated, which can not change the logic of the premise of the event, so that increase (or change) achieve very easily.

#Weak point

Modify builder interface will result in a revision of all execution classes.


> Ref:
> * [Builder Design Pattern](https://sourcemaking.com/design_patterns/builder)
> * [Builder](https://pokk.gitbooks.io/program-experience/content/zh-tw/Design%20Pattern/creational/builder.html)

