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

 
Вот пример [дизайн-документа](https://en.wikipedia.org/wiki/Software_design_description). Итак, не очень много, но представляет собой письменное описание программного продукта. Так как на реальном испытании - время критично - вы должны найти баланс временем на написание кода, тестов и составления документации. Лучше выбирать текст чем иллюстрацию. Если вы хорошо владеете UML то добавить диаграмму классов будет безусловно тут полезно.

В SDD документ обычно могут быть включены такие разделы
- Описание решения
- Разработка решения и структуры данных
- Все классы и их назначение
- Описание пакета
- Описание методов
- Обсновление решения, например почему выбран именно этот шаблон дизайна, почему используются перечисления (enum), почему используется BigDecimal  и.т.
- Блок-схема нескольких случаев использования
- UML Диаграмма
- Assumption
- Риски
- Приемущества(benefits)
  

Вот пример дизайн документа, в нем отражены только основные моменты, хорошо подходят для обучения и понимания ООД. Одна вещь, которую я хотел бы добавить к этому документу является диаграмма классов UML, потому что это еще один способ передать свой дизайн коллегам разработчиков Java. Я добавил его к обсуждению этого вопроса. 


### 1) High-level Design

Включать в раздел описание решаемой задачи важно если документ предназначен для тех кто не очень знаком с пердметной областью.

* Интерфейса, классы, и исключеня 
  * VendingMachine - интерфейс, который определяет публичный API в торговом автомате
  * VendingMachineImpl - реализация интерфейса платежного автомтаа
  * Inventory - используется для хранения объектов, реализация представляет собой обертку или адаптер над java.util.Map
  * Item - перечисление (еnum) для представления набора товаров поддерживаемых торгвым автоматаом.
  * Coin - перечисление (еnum) для представления набора монет которые принимаются торогвоым автоматом.
  * Bucket - Класс для хранения двх типов вместе.
  * SoldOutException - исключение генерируется, когда пользователь выбирает продукт, который продан
  * NotSufficientChangeException - генерируется когда не необходмой суммы на сдачу для завершения операции.
  * NotFullPaidException - генерируется, когда пользователь пытается получить товар, не заплатив его полную стоимость.

* Используемые структуры данных
  - ассоциативный массив (в Java API java.util.Map) используется для хранения товаров и денег внутри автомата.
  - Список (List) используется сдачи, поскольку он может содержать дубликаты, тоесть несколько монет одного и того же номинала.
  
 
* Обяснения выбора дизайна
  - [Паттерн Фабрика](Factory design pattern) используется для инкапсуляции логики создания торгового автомата
  - Шаблон адаптер используется для создания мехпнзма учета путем обертывания в java.util.Map
  - java.lang.Enum используется для представления Товаров и Монет потому что он обладает такми приемуществами
    * на этапе компилации исключена возможность возникновения ошибок согласования типов, нет возможности использовать неверны тип помет или товаров.
    * нет необходимости проверок что выбраны действительный товар или монета.
    * могут повторно использоваться (reusable) и хорошо инкапсулируются.
  - long используется для представления цен и суммы продаж, являющиеся целыми величинами, потому что учет ведется в центах.

Для хранения денег не используется BigDecimal, потому что автомат может содержать только ограниченную сумму денег, из-за физичеких ограничений для хранения монет.

###2) Low Leven Design

представляет собой процесс проектирования на уровне компонентов, который следует шаг за шагом процесс уточнения. Этот процесс может быть использован для проектирования структур данных, необходимой архитектуры программного обеспечения, исходный код и, в конечном счете, алгоритмы производительности. В целом, организация данных может быть определена в ходе анализа требований, а затем уточнены в процессе проектирования данных работ. После сборки, каждый компонент задается в деталях


1) Описание методов

* Методы (Methods)
   -  Метод getChange() method uses a greedy algorithm to find out whether we have sufficient coins to support an amount.

* Initialization
  - When Vending Machine will be created, it's initialized with default cash and item inventory. current with quantity 5 for each coin and item.


2) Testing Strategy
   
- Three primary test cases to testWithExactPrice(), testWithMorePrice() and testRefund() to cover general usecase.
- Negative test cases like testing SoldOutException, NotSufficientChangeException or NotFullPaidException

3) Benefits
- The design uses Abstraction to create reusable, small classes which are easier to read and test.
- Encapsulating Items and Coins on their respective class makes it easy to add new Coins and Items.
- Inventory is general purpose class and can be used elsewhere, It also encapsulates all inventory operations.

4) Assumption
- Vending Machine is single-threaded, only one user will operate at a time.
- A call to reset() will reset item and balance i.e. make them zero.

### UML Diagram Vending Machine in Java

Here is our UML diagram for this solution, generated in Eclipse by using ObjectAid plugin. It let you create UML class diagram from Java code itself. This diagram shows that VendingMachineImpl extends or implements VendingMachine interface. It also shows that it is associated with Item and Inventory classes.


---
That's all on this two-part article on designing and implementing Vending Machine in Java using Object Oriented analysis and design. If you have just started learning OOPS or Java, I recommend doing this kind of design related exercise to improve your design skills in Java. You will face a lot of challenges, which will drive further learning and understanding of core OOPS concept. If you are hungry for more of such design problems in Java, you should read  UML for Java Programmers by Robert C. Martin to practice another similar problem of designing a coffee machine in Java. Uncle Bob has explained much better than me, so there is good chance that you will learn something there as well.
