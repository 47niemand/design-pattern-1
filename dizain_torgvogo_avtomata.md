# Дизайн торгвого автомата

Как спроектировать Торговый автомат в Java? Это один из хороших вопросов на интервью по Java, который может быть задан на собеседованиях уровня Senior developer. Вам будет дано задание  разработать Торговый автомат. В течение ограниченного времени, как правило, 2-3 часа, необходимо спроектировать дизайн, написать рабочий код и unit тесты на Java.

Одним из ключевых преимуществ таких Java интервью является, что  сразу можно проверить многие основные навыки кандидата. Для завершения проектирования, кодирования и unit тестирования торгового автомата, кандидат должен быть действительно хороший во всех трех областях. Кстати, подобные задачи из реального мира  является хорошими упражнениями для улучшения навыков объектно ориентированного анализа и дизайна ([смотри тут](http://javarevisited.blogspot.sg/2014/01/10-tips-to-improve-programming-skill-become-better-programmer.html)), который является очень важным, если вы хотите стать хорошим разработчиком.

Разрабатывая автомат в Java или любом другом объектно ориентированным языке, вы не только изучаете основы например, инкапсуляция, полиморфизм или наследования, но также узнаете тонкие детали о том, как использовать абстрактный класс и интерфейс (см. здесь) при решении задач или разработке приложения.

Как правило такого рода задачи дают возможность использовать шаблоны проектирования Java, например, в этой задаче будет использован  шаблон фабрики для создания различных типов торговых автоматов. Об этом можно почитать ([здесь](http://javarevisited.blogspot.com/2012/03/10-object-oriented-design-principles.html)).

Этоа серия из двух статей будет показывать решение задачи по созданию торгового автомата в Java. Кстати, эта задача  может быть решена различными способами. Это также дает возможность пересмотреть принципы проектирования SOLID и ООП паттерны (см. здесь) и уметь использовать их в коде. 

# Постановка задачи

Вам необходимо разработать Торговый автомат который:

1. Принимает монеты определенного номинала.
2. Предоставляет возможность пользователю выбрать продукты.
3. Разрешите клиенту получить возврат денег в случае отмены заказа.
4. Выдать выбранные товар и если необходимо сдачу
5. Обеспечить возможность выполнить операцию сброса для сервисного обслуживания.

Постановка задачи является наиболее важной частью. Нужно несколько раз перечитать постановку чтобы как можно лучше понять суть проблемы и то, что вы пытаетесь решить. Обычно [требования не очень ясны](http://javarevisited.blogspot.com/2015/01/difference-between-functional-and-nonfunctional-requirements-software-development.html) и вам, читая постановку задачи, нужно составить их перечень самостоятельно.

Мне нравится требования составленные в виде списков, потому что их легко отслеживать. Некоторые требования могут быть неявные, поэтому в вашем списке лучше их сделать явными. Например в этой задаче, автомат не должен продавать товар, если нет необходимых средств на сдачу.

К сожалению, существует не много книгу или курсов, которые научат вас этим навыкам. Необходимо развивать их самостоятельно, выполняя реальные задачи. Хотя, две книги, которые помогли мне улучшить навыки объектно ориентированного анализа и проектирования это [Head First Object Oriented Design and Analysis](http://www.amazon.com/dp/0596008678/?tag=javamysqlanta-20) 1-е издание, Бретт Маклафлин. Одна из лучших книги, если у вас нет большого опыта работы в объектно-ориентированном программировании.

![Read more: http://javarevisited.blogspot.com/2016/06/design-vending-machine-in-java.html#ixzz4AjWrUb9m](https://3.bp.blogspot.com/-lQExXlp7hDo/V1OQ5QaEfaI/AAAAAAAAGGw/MV2964R-1-klxt4to7VcR-iciBbkZ4qjACLcB/s320/Head%2BFirst%2BAnalaysis%2Band%2BDesign.JPG)

Еще одна очень хорошая книга по разработке приложений и дизайну систем дизайн это [UML for Java Programmers by Robert C. Martin](http://www.amazon.com/UML-Java%C2%BF-Programmers-Robert-Martin/dp/0131428489?tag=javamysqlanta-20), один из моих любимых авторов. Я прочитал несколько книг о нем, например "Clean Code", "Clean Coder" и книгу по разработке программного обеспечения с использованием Agile. Он является одним из лучших в преподавании OOП концепции.

![](https://4.bp.blogspot.com/-GM39T4LV9wA/V1ORHdHmZ2I/AAAAAAAAGG4/cq2xSrQDYLgEZNYigkTu-P9Z9GgJzPAZACLcB/s320/UML%2Bfor%2BJava%2BProgrammers%2Bby%2BUncle%2BBob.jpg)

В этой книге рассматривается аналогичная задача о проектировании кофе-машины. Так что, если вы хотите попрактиковаться или проверить навыки ООД, вы можете обратиться к этой проблеме. Также это очень хорошее упражнение для обучения.


# Решение и Код

Моя реализация торгового автомата содержит следующие классы и интерфейсы:

#### VendingMachine

Класс определяет публичный API торгового автомата, как правило, все функции высокого уровня должны быть описаны в этом классе

#### VendingMachineImpl

Пример реализации торгового автомата

#### VendingMachineFactory
 
Класс-фабрика для создания различных видов торговых автоматов

#### Item
 
Перечисляемый тип чтобы описать набор товаров продаваемых торговым автоматом

#### Inventory
 
Класс для учета, используется для учета товаров и денег внутри торгового автомата

#### Coin
 
Перечисляемый тип для описания монет поддерживаемых торговым автоматом

#### Bucket
 
Обобщенный класс для хранения двух объектов. 

#### NotFullPaidException
 
Исключение возникает, когда пользователь пытается забрать товар, не заплатив полную сумму.

#### NotSufficientChangeException
 
Торговый автомат выдает это исключение, чтобы показать, нет необходимой суммы на сдачу.

#### SoldOutExcepiton
 
Исключение возникает если пользователь хочет приобрести продукт который уже продан.

![](vendingmachine.png)


## Как разработать торговый автомат на Java
Вот полный код торгово автомата, протестируйте код и дайте мне знать, если будут какие либо ошибки.

Код реализации из статьи можно посмотреть по [тут](https://github.com/47niemand/learning-java-vending-machine-example.git).

#### VendingMachine.java

>Публичный интерфейс торгового автомата, как правило, все функциональные возможности высокого уровня должны быть описаны в нем

```java
package vending;

import java.util.List;

/**
 * Decleare public API for Vending Machine
 * @author Javin Paul
 */
public interface VendingMachine {
    public long selectItemAndGetPrice(Item item);
    public void insertCoin(Coin coin);
    public List<Coin> refund();
    public Bucket<Item, List<Coin>> collectItemAndChange();
    public void reset();
}
```


#### VendingMachineImpl.java

>Пример реализации интерфейса автомата. Предоставляет собой реальный автомат, который есть в вашем офисе, на автобусной остановке, или общественных местах

```java
package vending;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

/**
 * Sample implementation of Vending Machine in Java
 *
 * @author Javin Paul
 */
public class VendingMachineImpl implements VendingMachine {
    private Inventory<Coin> cashInventory = new Inventory<Coin>();
    private Inventory<Item> itemInventory = new Inventory<Item>();
    private long totalSales;
    private Item currentItem;
    private long currentBalance;

    public VendingMachineImpl() {
        initialize();
    }

    private void initialize() {
        //initialize machine with 5 coins of each denomination
        //and 5 cans of each Item
        for (Coin c : Coin.values()) {
            cashInventory.put(c, 5);
        }

        for (Item i : Item.values()) {
            itemInventory.put(i, 5);
        }

    }

    @Override
    public long selectItemAndGetPrice(Item item) {
        if (itemInventory.hasItem(item)) {
            currentItem = item;
            return currentItem.getPrice();
        }
        throw new SoldOutException("Sold Out, Please buy another item");
    }

    @Override
    public void insertCoin(Coin coin) {
        currentBalance = currentBalance + coin.getDenomination();
        cashInventory.add(coin);
    }

    @Override
    public Bucket<Item, List<Coin>> collectItemAndChange() {
        Item item = collectItem();
        totalSales = totalSales + currentItem.getPrice();

        List<Coin> change = collectChange();

        return new Bucket<Item, List<Coin>>(item, change);
    }

    private Item collectItem() throws NotSufficientChangeException,
            NotFullPaidException {
        if (isFullPaid()) {
            if (hasSufficientChange()) {
                itemInventory.deduct(currentItem);
                return currentItem;
            }
            throw new NotSufficientChangeException("Not Sufficient change in Inventory");

        }
        long remainingBalance = currentItem.getPrice() - currentBalance;
        throw new NotFullPaidException("Price not full paid, remaining : ", remainingBalance);
    }

    private List<Coin> collectChange() {
        long changeAmount = currentBalance - currentItem.getPrice();
        List<Coin> change = getChange(changeAmount);
        updateCashInventory(change);
        currentBalance = 0;
        currentItem = null;
        return change;
    }

    @Override
    public List<Coin> refund() {
        List<Coin> refund = getChange(currentBalance);
        updateCashInventory(refund);
        currentBalance = 0;
        currentItem = null;
        return refund;
    }

    private boolean isFullPaid() {
        if (currentBalance >= currentItem.getPrice()) {
            return true;
        }
        return false;
    }

    private List<Coin> getChange(long amount) throws NotSufficientChangeException {

        List<Coin> changes = Collections.EMPTY_LIST;

        if (amount > 0) {
            changes = new ArrayList<Coin>();
            long balance = amount;
            while (balance > 0) {
                if (balance >= Coin.QUARTER.getDenomination()
                        && cashInventory.hasItem(Coin.QUARTER)) {
                    changes.add(Coin.QUARTER);
                    balance = balance - Coin.QUARTER.getDenomination();
                    continue;

                } else if (balance >= Coin.DIME.getDenomination()
                        && cashInventory.hasItem(Coin.DIME)) {
                    changes.add(Coin.DIME);
                    balance = balance - Coin.DIME.getDenomination();
                    continue;

                } else if (balance >= Coin.NICKLE.getDenomination()
                        && cashInventory.hasItem(Coin.NICKLE)) {
                    changes.add(Coin.NICKLE);
                    balance = balance - Coin.NICKLE.getDenomination();
                    continue;

                } else if (balance >= Coin.PENNY.getDenomination()
                        && cashInventory.hasItem(Coin.PENNY)) {
                    changes.add(Coin.PENNY);
                    balance = balance - Coin.PENNY.getDenomination();
                    continue;

                } else {
                    throw new NotSufficientChangeException("NotSufficientChange, Please try another product ");
                }
            }
        }
        return changes;
    }

    @Override
    public void reset() {
        cashInventory.clear();
        itemInventory.clear();
        totalSales = 0;
        currentItem = null;
        currentBalance = 0;
    }

    public void printStats() {
        System.out.println("Total Sales : " + totalSales);
        System.out.println("Current Item Inventory : " + itemInventory);
        System.out.println("Current Cash Inventory : " + cashInventory);
    }

    private boolean hasSufficientChange() {
        return hasSufficientChangeForAmount(currentBalance - currentItem.getPrice());
    }

    private boolean hasSufficientChangeForAmount(long amount) {
        boolean hasChange = true;
        try {
            getChange(amount);
        } catch (NotSufficientChangeException nsce) {
            return hasChange = false;
        }

        return hasChange;
    }

    private void updateCashInventory(List<Coin> change) {
        for (Coin c : change) {
            cashInventory.deduct(c);
        }
    }

    public long getTotalSales() {
        return totalSales;
    }

}
```

#### VendingMachineFactory.java

Класс-фабрика для создания различных видов торговых автоматов


#### Item.java

>Перечисляемый тип чтобы описать набор товаров продаваемых торговым автоматом


#### Inventory.java

>Класс используется для учета товаров и денег внутри торгового автомата.


#### VendingMachine.java

>Интерфейс торгового автомата, все функциональные возможности высокого уровня должны быть описаны тут.


#### Bucket.java

>Обобщенный служебный класс для хранения пары объектов.


#### NotFullPaidException.java

>Исключение вникающее когда пользователь пытается получить товар, не заплатив полную сумму.


#### NotSufficientChangeException.java

>Исключение чтобы показать, что торговый автомат не содержит достаточно монет для выполнения запроса.

#### SoldOutException.java

>Исключение вникающее если будет запрошен уже проданный товар.

---
На этом все в первой части статьи **, как спроектировать автомат по продаже в Java **. В этой части, мы решили эту проблему, создав все классы и написали весь код, пока unit тестирование и документа дизайна не реализованы, они будут во второй части этой статьи.

Если вы хотите, вы можете проверить работу это приложение создав необходимые unit тесты, или реализовать это приложение использую потоки, чтобы отдельный поток выполнял роль пользователя.

>Design a Vending Machine in Java - Interview Question
Read more: http://javarevisited.blogspot.com/2016/06/design-vending-machine-in-java.html