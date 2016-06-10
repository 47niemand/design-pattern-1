# Дизайн торгового автомата. Часть 2.

Это вторая часть туториала о том, как написать Торговый автомат на Java. В первой части были рассмотрены постановка задачи и само решение, но не было завершено описание решения ([design document](https://en.wikipedia.org/wiki/Software_design_description)) и не релизовано модульное тестирование, в этой части они будут рассмотрены. Как уже упоминалось, существует несколько способов реализации торгового автомата, например, мы могли бы легко использовать [шаблон проектирования состояние](https://en.wikipedia.org/wiki/State_pattern) (state pattern), и это действительно один из лучших примеров для использования шаблона. Автомат меняет свое поведение в зависимости от своего состояния,например, если товар есть в наличии будет выполнена выдача товара, в противном случае, возращены монеты, и это отлично вписывается в шаблона состояния. 
Хотя, в предложенном решении, не был использован шаблон "состояние" (state pattern), а было закодировано решение с условными блоками (if-else). Это проще, из-за небольшого числа состояний и малого количества условий изменений состояния. Но в реальном сценарии, шаблон проектирования "состояние" лучше, потому что он использует полиморфизм, и можно вынести логику, которую мы заложили внутри if-то блока.

Знание UML важно для создания дизайна-документа для программ Java. Чтобы объяснить различные аспекты дизайна, в UML используются несколько типов диаграмм. Например, диаграмма классов (Class diagram), чтобы показать зависимость объектов, диаграммы последовательности (Sequence diagram), чтобы показать, как пользователь будет взаимодействовать с системой и так далее.

Есть одна книга, которая действительно помогает лучшепонять UML это [UML for Java Programmers by Robert C. Martin](http://www.amazon.com/UML-Java%C2%BF-Programmers-Robert-Martin/dp/0131428489?tag=javamysqlanta-20). Книга очень полезна если вы хотите изучить UML с нуля или хотите освежить свои знания UML. В книге также содержаться похожие упражнения, как проектирование кофе машины.

## Unit-Тестирование релизации трогового автомата

Ниже приведен тестирующий класс, который проверяет поведение торгового автомата в различных ситуациях, например, покупка товаров при наличии точной суммы на сдачу, монет на сдачу больше чем требуется, нет необходимой сдачи, отмена покупки, выполнение команды сброс торгового автомата и т.д. Стоит признать, что модульные тесты недостаточно обширны, и можно добавить еще несколько, чтобы полностью покрыть тестами решение задачи. Убедитесь в том, чтобы ваш класс модульного тестирования в том же пакете, но на другой корневой папке, так что вы можете проверить пакет частных пользователей без смешивания производства и тестирования кода....



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


## Дизайн-документ

 
Вот пример [дизайн-документа](https://en.wikipedia.org/wiki/Software_design_description) (design document), здесь не очень много текста, но все же он представляет собой письменное описание программного продукта. Так как на реальном испытании - время критично - нужно найти баланс между временем на написание кода, тестов и составления документации. Лучше выбирать текст чем иллюстрацию. Если вы хорошо владеете UML то добавить диаграмму классов будет безусловно тут полезно.

В SDD документ обычно могут быть включены такие разделы
- Описание решения
- Разработка решения и структуры данных
- Все классы и их назначение
- Описание пакета
- Описание методов
- Обоснование решения, например, почему выбран именно этот шаблон дизайна, почему используются перечисления (enum), почему используется BigDecimal  и.т.
- Блок-схема нескольких случаев использования
- UML Диаграмма
- Предположения
- Риски
- Преимущества


Ниже приведен пример дизайн-документа, в нем отражены только основные моменты, и он больше подходит для обучения и понимания ООД. Одна вещь, которую стоит добавить к этому документу это диаграмма классов UML, потому что это еще один способ передать свой дизайн коллегам Java разработчикам. Он добавлен обсуждению этого вопроса.

### 1) High-level Design

>High-Level Design описывает предлагаемое решение на уровне архитектуры, не вдаваясь в подробности спецификаций, конфигураций и т.п...

Важно включать в раздел описание решаемой задачи если документ предназначен для тех кто не очень знаком с предметной областью.

* Интерфейсы, классы, и исключения
  * VendingMachine - интерфейс, который определяет публичный API в торговом автомате
  * VendingMachineImpl - реализация интерфейса торгового автомата
  * Inventory - используется для хранения различных объектов, реализация представляет собой обертку или адаптер над java.util.Map
  * Item - перечисление (еnum) для представления набора товаров поддерживаемых торговым автоматаом.
  * Coin - перечисление (еnum) для представления набора монет которые принимаются торговым автоматом.
  * Bucket - Класс для хранения вместе пары объектов определённых типов.
  * SoldOutException - исключение генерируется, когда пользователь выбирает продукт, который продан.
  * NotSufficientChangeException - генерируется, когда не необходимой суммы на сдачу для завершения операции.
  * NotFullPaidException - генерируется, когда пользователь пытается получить товар, не заплатив его полную стоимость.

* Используемые структуры данных
  - ассоциативный массив (в Java API java.util.Map) используется для хранения товаров и денег внутри автомата.
  - Список (List) используется сдачи, поскольку он может содержать дубликаты, то есть несколько монет одного и того же номинала.

* Обоснование выбора дизайна
  - [Паттерн Фабрика](Factory design pattern) используется для инкапсуляции логики создания торгового автомата
  - Шаблон адаптер используется для создания инвентаризации содержимого автомата путем обертывания в java.util.Map
  - java.lang.Enum используется для представления Товаров и Монет потому что он обладает таким преимуществами
    * на этапе компилации исключается возможность возникновения ошибок согласования типов, не позволяет использовать ошибочный тип помет или товаров.
    * нет необходимости проверок, что выбран действительный товар или монета.
    * могут повторно использоваться (reusable) и хорошо инкапсулируются.
  - long используется для представления цен и суммы продаж, являющиеся целыми величинами, потому что учет ведется в центах.

Для хранения денег не используется BigDecimal, потому что автомат может содержать только ограниченную сумму денег, из-за физичеких ограничений для хранения монет.

###2) Low Leven Design

1) Описание методов

* Методы (Methods)
   -  Метод getChange() реализует жадный алгоритм, чтобы вычислить, есть ли достаточное количество монет для выдачи сдачи.

* Инициализация (Initialization)
  - При создании торговый автомат инициализируется значениями по умолчанию для денег и товаров данными по умолчанию наличных денежных средств и инвентаризации товара. тока с количеством 5 для каждой монеты и ст.
  
2) Стратегия тестирования (Testing Strategy)
   
- Три основных теста это testWithExactPrice(), testWithMorePrice() и testRefund() для покрытия тестирования использования.
- Отрицательный тест проверяет, что система выполнит то что не должна SoldOutException, NotSufficientChangeException или NotFullPaidExceptio

3) Преимущества
- Дизайн использует абстракции что дает возможность переиспользовать классы. Небольшой классы легче читать и тестировать.
- Инкапсуляция Items и Coins соответствующим им классах позволяет легко добавлять новые монеты и предметы.
- Inventory шаблонный класс общего назначения он может быть использован и других реализациях, а также включает в себя все операции инвентаризации.

4) Предположения
- Реализация торгового автомата является однопоточной.
- Вызов reset() приведет к сбросу содержимого и баланс, т.е. сделает их равными нулю.

### UML Diagram Vending Machine in Java

Вот наша UML-схема для этого решения, генерируемой в Eclipse, с использованием ObjectAid плагин. Это позволяет создавать UML диаграммы классов из самого кода Java. Эта диаграмма показывает, что VendingMachineImpl расширяет или реализует интерфейс торговый автомат. Он также показывает, что оно связано с представлениями элементов и инвентаризации классов.

![](https://4.bp.blogspot.com/-Sao6FMujcmo/V1bTFdBywpI/AAAAAAAAGH8/ia5lyVjKd-YR-RGehZZDg0XkUOSGo5pVQCLcB/s640/Vending%2BMachine%2BUML%2BDiagram%2Bin%2BJava.png)

----

На этом завершается, заключительная, вторая часть статьи посвященной разработке и реализации торгового автомат на Java с использованием объектно-ориентированного анализа и проектирования. Если вы только начали изучать OOPS или Java, я рекомендую выполнять похожие упражнения чтобы улучшить свои навыки проектирования в Java. Вы будете сталкиваться с множеством проблем, которые будут направлять вас в дальнейшем изучение и понимание основной OOPS концепции. 

>Java Object Oriented Analysis and Design Problem - Vending Machine Part 2 
>
Read more: http://javarevisited.blogspot.com/2016/06/java-object-oriented-analysis-and-design-vending-machine-part-2.html

