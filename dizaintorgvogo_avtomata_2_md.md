# Дизайн торгвого автомата ч.2

Это вторая часть туториала  о том как, как написать Торговый автомат на Java. 
В первой части были рассмотрены постановка задачи и само решение, но не были завершены описание решения (design document) и модульное тестирование, в этой части они будут рассмотрены. 
Как я уже сказано, существует несколько способов реализации торгового автомата, например, мы могли бы легко использовать [шаблон проектирования "состояние"](https://en.wikipedia.org/wiki/State_pattern) (state pattern), и это действительно один из лучших примеров для использования шаблона.
Торговый автомат меняет свое поведение в зависимости от своего состояния. **Например действие вернуть товар, если он есть в наличии, в противном случае, вернуть монеты, поэтому он идеально вписывается в дизайн State шаблона.**  
Хотя, в предложенном решении, не был использован шраблон проектирования "состояние", а был только закодировано решение с условными блоками (If-else) потомоучто это проще, из-за небольшого числа состояний и малого количества условий изменений состояния. Но в более реальном сценарии, шаблон проектирования state лучше, потомучто он использует полиморфизм, и можно вынести логику которую мы заложили внутри if-то блока.

Знание UML важно для создания дизайна документа для программ Java.  Чтобы объяснить различные аспекты вашего дизайна, в UML используются несколько типов диаграмм. Например, диаграмма классов (Class diagram), чтобы показать зависимость объектов, диаграммы последовательности (Sequence diagram), чтобы показать, как пользователь будет взаимодействовать с системой и так далее.

Есть одна книга, которая действительно помогает понять UML лучше UML для Java-программистов Роберт С. Мартин. Это замечательная кинга если вы хотите изучить UML с нуля или хотите освежить свои знания UML. В книге также содержаться похожие упражнения, как проектирование кофемашины.

## Unit Test of Vending Machine Solution

Ниже приведен код клсса для unit тетироания, который проверяет  поведение торгового автомата в различных ситуациях, например, покупка товаров при наличии точной суммы на сдачу, монет на садчу больше чем трубется, нет необходмой сдачи, отмена покупки, выполнение команды сброс торгового автомата и т.д.
Стот признать, что модульные тесты не достаточно обширны, и  можно  добавить еще несколько, чтобы полностью покрыть тестами решамеую задачу. Убедитесь в том, чтобы ваш класс модульного тестирования в том же пакете, но на другой корневой папке, так что вы можете проверить пакет частных пользователей без смешивания производства и тестирования кода....



#### VendingMachineTest.java

```java
package vending;

import org.junit.Ignore;
import java.util.List;
import org.junit.AfterClass;
import org.junit.BeforeClass;
import org.junit.Test;
import static org.junit.Assert.;


public class VendingMachineTest {
    private static VendingMachine vm;
   
    @BeforeClass
    public static void setUp(){
        vm = VendingMachineFactory.createVendingMachine();
    }
   
    @AfterClass
    public static void tearDown(){
        vm = null;
    }
   
    @Test
    public void testBuyItemWithExactPrice() {
        //select item, price in cents
        long price = vm.selectItemAndGetPrice(Item.COKE); 
        //price should be Coke's price      
        assertEquals(Item.COKE.getPrice(), price);
        //25 cents paid              
        vm.insertCoin(Coin.QUARTER);                           
       
        Bucket<Item, List<Coin>> bucket = vm.collectItemAndChange();
        Item item = bucket.getFirst();
        List<Coin> change = bucket.getSecond();
       
        //should be Coke
        assertEquals(Item.COKE, item);
        //there should not be any change                              
        assertTrue(change.isEmpty());                              
    }
   
    @Test
    public void testBuyItemWithMorePrice(){
        long price = vm.selectItemAndGetPrice(Item.SODA);
        assertEquals(Item.SODA.getPrice(), price);
       
        vm.insertCoin(Coin.QUARTER);       
        vm.insertCoin(Coin.QUARTER);      
       
        Bucket<Item, List<Coin>> bucket = vm.collectItemAndChange();
        Item item = bucket.getFirst();
        List<Coin> change = bucket.getSecond();
       
        //should be Coke
        assertEquals(Item.SODA, item);
        //there should not be any change                                     
        assertTrue(!change.isEmpty());        
        //comparing change                             
        assertEquals(50 - Item.SODA.getPrice(), getTotal(change));  
       
    }  
  
   
    @Test
    public void testRefund(){
        long price = vm.selectItemAndGetPrice(Item.PEPSI);
        assertEquals(Item.PEPSI.getPrice(), price);       
        vm.insertCoin(Coin.DIME);
        vm.insertCoin(Coin.NICKLE);
        vm.insertCoin(Coin.PENNY);
        vm.insertCoin(Coin.QUARTER);
       
        assertEquals(41, getTotal(vm.refund()));       
    }
   
    @Test(expected=SoldOutException.class)
    public void testSoldOut(){
        for (int i = 0; i < 5; i++) {
            vm.selectItemAndGetPrice(Item.COKE);
            vm.insertCoin(Coin.QUARTER);
            vm.collectItemAndChange();
        }
     
    }
   
    @Test(expected=NotSufficientChangeException.class)
    public void testNotSufficientChangeException(){
        for (int i = 0; i < 5; i++) {
            vm.selectItemAndGetPrice(Item.SODA);
            vm.insertCoin(Coin.QUARTER);
            vm.insertCoin(Coin.QUARTER);
            vm.collectItemAndChange();
           
            vm.selectItemAndGetPrice(Item.PEPSI);
            vm.insertCoin(Coin.QUARTER);
            vm.insertCoin(Coin.QUARTER);
            vm.collectItemAndChange();
        }
    }
   
   
    @Test(expected=SoldOutException.class)
    public void testReset(){
        VendingMachine vmachine = VendingMachineFactory.createVendingMachine();
        vmachine.reset();
       
        vmachine.selectItemAndGetPrice(Item.COKE);
       
    }
   
    @Ignore
    public void testVendingMachineImpl(){
        VendingMachineImpl vm = new VendingMachineImpl();
    }
   
    private long getTotal(List<Coin> change){
        long total = 0;
        for(Coin c : change){
            total = total + c.getDenomination();
        }
        return total;
    }
}

```


## Design Document

 
Here is a sample design document for Vending Machine problem. Well, it's not that great but conveys my design decisions in text format. Since in a real test, time is critical you need to balance your time between coding, testing, and designing activity, it's better to choose text over an image. If you are good with UML than adding a class diagram would certainly help here.

In Short Design Document in Java Should include
- description of the solution
- design decision and data structures
- All classes and there responsibility
- description of the package
- description of methods
- the motivation of decision e.g. why this design pattern, why enum, why BigDecimal etc?
- Flow Chart of couple of use cases
- UML Diagram
- Assumption
- Risk
- Benefits

Here is our sample design document, it's very basic because I have generated it very quickly, but it's good for learning and doing Object Oriented Analysis and design. One thing which I would like to add on this document is UML class diagram because that's another way to convey your design to fellow Java developers. I have  added it to this discussion. If you want to learn UML then don't forget to check out UML for Java Programmers by Robert C. Martin

Java Object Oriented Analysis and Design Problem - Vending Machine Part 2


1) High-level Design
Includes overview of the problem, not necessary if you are writing this as part of the test because evaluator should be familiar with problem specification. Important if your design document is intended for someone who is not very familiar with the problem domain.

    - Main interface, classes and Exceptions
          - VendingMachine - an interface which defines public API of VendingMachine
          - VendingMachineImpl - a general purpose implementation of VendingMachine interface
          - Inventory - A type-safe inventory for holding objects, which is an ADAPTER or WRAPPER over java.util.Map
          - Item - type-safe Enum to represent items supported by vending machine.
          - Coin - type-safe Enum to represent coins, which is acceptable by vending machine.
          - Bucket - A Holder class for holding two types together.
          - SoldOutException - thrown when user selects a product which is sold out
          - NotSufficientChangeException - thrown when Vending machine doesn't have enough change to support the current transaction.

          - NotFullPaidException - thrown when the user tries to collect an item, without paying the full amount.
     
    - Data structures used
          - Map data structure is used to implement cash and item inventories.
          - The List is used to returning changes because it can contain duplicates, i.e. multiple coins of the same denomination.

   
    - Motivation behind design decisions
         - Factory design pattern is used to encapsulate creation logic of VendingMachine.
         - Adapter pattern is used to create Inventory by wrapping java.util.Map
         - java.lang.Enum is used to represent Item and Coins, because of following benefits
                - compile time safety against entering an invalid item and invalid coin.
                - no need to write code for checking if selected item or entered coin is valid.
                - reusable and well encapsulated.
         - long is used to represent price and totalSales, which are the amount because we are dealing in cents.
           Not used BigDecimal to represent money, because the vending machine can only hold limited amount, due to finite capacity of coin inventory.

         -

2) Low Leven Design
    - Methods
        -  The getChange() method uses a greedy algorithm to find out whether we have sufficient coins to support an amount.

    - Initialization
         - When Vending Machine will be created, it's initialized with default cash and item inventory. current with quantity 5 for each coin and item.



2) Testing Strategy
   - Three primary test cases to testWithExactPrice(), testWithMorePrice() and testRefund() to cover general usecase.
   - Negative test cases like testing SoldOutException, NotSufficientChangeException or NotFullPaidException
   -

3) Benefits
   - The design uses Abstraction to create reusable, small classes which are easier to read and test.
   - Encapsulating Items and Coins on their respective class makes it easy to add new Coins and Items.
   - Inventory is general purpose class and can be used elsewhere, It also encapsulates all inventory operations.

4) Assumption
   - Vending Machine is single-threaded, only one user will operate at a time.
   - A call to reset() will reset item and balance i.e. make them zero.



UML Diagram Vending Machine in Java
Here is our UML diagram for this solution, generated in Eclipse by using ObjectAid plugin. It let you create UML class diagram from Java code itself. This diagram shows that VendingMachineImpl extends or implements VendingMachine interface. It also shows that it is associated with Item and Inventory classes.

If you are new in Java and not understanding this UML diagram then you should  read UML for Java Programmers by Robert C. Martin , one of the best books to learn about UML and class diagram of  Java programs.

Vending Machine in Java coding, design and UML


That's all on this two-part article on designing and implementing Vending Machine in Java using Object Oriented analysis and design. If you have just started learning OOPS or Java, I recommend doing this kind of design related exercise to improve your design skills in Java. You will face a lot of challenges, which will drive further learning and understanding of core OOPS concept. If you are hungry for more of such design problems in Java, you should read  UML for Java Programmers by Robert C. Martin to practice another similar problem of designing a coffee machine in Java. Uncle Bob has explained much better than me, so there is good chance that you will learn something there as well.
