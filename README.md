# 设计模式之美
<!-- vim-markdown-toc GFM -->

* [面向对象](#面向对象)
    * [封装、抽象、继承、多态](#封装抽象继承多态)
    * [贫血模型和充血模型](#贫血模型和充血模型)
* [设计模式的六大原则](#设计模式的六大原则)
    * [单一职责原则 ( Single Responsibility Principle)](#单一职责原则--single-responsibility-principle)
    * [开闭原则 (Open Closed Principle)](#开闭原则-open-closed-principle)
    * [里式替换原则 （Liskov Substitution Principle）](#里式替换原则-liskov-substitution-principle)
    * [接口隔离原则 （ Interface Segregation Principle）](#接口隔离原则--interface-segregation-principle)
    * [依赖反转原则 （Dependency Inversion Principle）](#依赖反转原则-dependency-inversion-principle)
    * [迪米特法则（Law Of Demeter）](#迪米特法则law-of-demeter)
* [设计模式的分类](#设计模式的分类)
    * [创建型模式](#创建型模式)
    * [结构型模式](#结构型模式)
    * [行为型设计模式](#行为型设计模式)
    * [设计模式 Golang 实现](#设计模式-golang-实现)

<!-- vim-markdown-toc -->
## 面向对象

* 面向对象编程是一种编程范式或编程风格。它以类或对象作为组织代码的基本单元，并将封装、抽象、继承、多态四个特性，作为代码设计和实现的基石 。
* 面向对象编程语言是支持类或对象的语法机制，并有现成的语法机制，能方便地实现面向对象编程四大特性（封装、抽象、继承、多态）的编程语言。

### 封装、抽象、继承、多态

1. 关于封装特性封装也叫作信息隐藏或者数据访问保护。类通过暴露有限的访问接口，授权外部仅能通过类提供的方式来访问内部信息或者数据。它需要编程语言提供权限访问控制语法来支持，例如 Java 中的 private、protected、public 关键字。封装特性存在的意义，一方面是保护数据不被随意修改，提高代码的可维护性；另一方面是仅暴露有限的必要接口，提高类的易用性。

2. 关于抽象特性封装主要讲如何隐藏信息、保护数据，那抽象就是讲如何隐藏方法的具体实现，让使用者只需要关心方法提供了哪些功能，不需要知道这些功能是如何实现的。抽象可以通过接口类或者抽象类来实现，但也并不需要特殊的语法机制来支持。抽象存在的意义，一方面是提高代码的可扩展性、维护性，修改实现不需要改变定义，减少代码的改动范围；另一方面，它也是处理复杂系统的有效手段，能有效地过滤掉不必要关注的信息。

3. 关于继承特性继承是用来表示类之间的 is-a 关系，分为两种模式：单继承和多继承。单继承表示一个子类只继承一个父类，多继承表示一个子类可以继承多个父类。为了实现继承这个特性，编程语言需要提供特殊的语法机制来支持。继承主要是用来解决代码复用的问题。

4. 关于多态特性多态是指子类可以替换父类，在实际的代码运行过程中，调用子类的方法实现。多态这种特性也需要编程语言提供特殊的语法机制来实现，比如继承、接口类、duck-typing。多态可以提高代码的扩展性和复用性，是很多设计模式、设计原则、编程技巧的代码实现基础。

### 贫血模型和充血模型

* 贫血模型： 是指Model 中，仅包含状态(属性），不包含行为(方法）,也就是没有业务逻辑，采用这种设计时，需要分离出DB层，专门用于数据库操作。
* 充血模型：Model 中既包括状态，又包括行为，是最符合面向对象的设计方式。

## 设计模式的六大原则

### 单一职责原则 ( Single Responsibility Principle)

一个类或者模块只负责完成一个职责（或者功能）。一个类只负责完成一个职责或者功能。不要设计大而全的类，要设计粒度小、功能单一的类。单一职责原则是为了实现代码高内聚、低耦合，提高代码的复用性、可读性、可维护性。

* 如何判断类的职责是否足够单一
  * 类中的代码行数、函数或者属性过多；
  * 类依赖的其他类过多，或者依赖类的其他类过多；
  * 私有方法过多；
  * 比较难给类起一个合适的名字；
  * 类中大量的方法都是集中操作类中的某几个属性；

### 开闭原则 (Open Closed Principle)

软件实体（模块、类、方法等）应该“对扩展开放、对修改关闭”。

简单理解就是 添加一个新的功能应该是，在已有代码基础上扩展代码（新增模块、类、方法等），而非修改已有代码（修改模块、类、方法等）。

### 里式替换原则 （Liskov Substitution Principle）

子类对象（object of subtype/derived class）能够替换程序（program）中父类对象（object of base/parent class）出现的任何地方，并且保证原来程序的逻辑行为（behavior）不变及正确性不被破坏。

### 接口隔离原则 （ Interface Segregation Principle）

使用者不应该被强迫依赖它不需要的接口。

### 依赖反转原则 （Dependency Inversion Principle）

高层模块（high-level modules）不要依赖低层模块（low-level）。高层模块和低层模块应该通过抽象（abstractions）来互相依赖。除此之外，抽象（abstractions）不要依赖具体实现细节（details），具体实现细节（details）依赖抽象（abstractions）。

所谓高层模块和低层模块的划分，简单来说就是，在调用链上，调用者属于高层，被调用者属于低层。

### 迪米特法则（Law Of Demeter）

个对象应该对其他对象保持最少的了解， 尽量避免依赖更多类型。

## 设计模式的分类

### 创建型模式

主要解决对象的创建问题，封装复杂的创建过程，解耦对象的创建代码和使用代码。

* 单例模式用来创建全局唯一的对象。
* 工厂模式用来创建不同但是相关类型的对象（继承同一父类或者接口的一组子类），由给定的参数来决定创建哪种类型的对象。
* 建造者模式是用来创建复杂对象，可以通过设置不同的可选参数，“定制化”地创建不同的对象。
* 原型模式针对创建成本比较大的对象，利用对已有对象进行复制的方式进行创建，以达到节省创建时间的目的。

### 结构型模式

结构型模式主要总结了一些类或对象组合在一起的经典结构，这些经典的结构可以解决特定应用场景的问题

结构型模式包括：代理模式、桥接模式、装饰器模式、适配器模式、门面模式、组合模式、享元模式

### 行为型设计模式

行为型设计模式主要解决的就是“类或对象之间的交互”问题。

行为型设计模式包括：观察者模式、模板模式、策略模式、职责链模式、状态模式、迭代器模式、访问者模式、备忘录模式、命令模式、解释器模式、中介模式。

### 设计模式 Golang 实现

|序号|名称|
|--|-----|
|1|[观察者模式][1]
|2|[策略模式][2]|
|3|[模板方法模式][3]|
|4|[装饰模式][4]|
|5|[桥接模式][5]|
|6|[工厂模式][6]|
|7|[抽象工厂模式][7]|
|8|[原型模式][8]|
|9|[建造者模式][9]|
|10|[单例模式][10]|
|11|[享元模式][11]|
|12|[外观模式][12]|
|13|[代理模式][13]|
|14|[适配器模式][14]|
|15|[中介者模式][15]|
|16|[状态模式][16]|
|17|[备忘录模式][17]|
|18|[组合模式][18]|
|19|[迭代器模式][19]|
|20|[职责链模式][20]|
|21|[命令模式][21]|
|22|[访问器模式][22]|
|23|[解释器模式][23]|

[0]: https://blog.csdn.net/LoveLion/article/details/17517213
[1]: https://github.com/colinrs/go-patterns-cn/blob/master/observer_pattern.md
[2]: https://github.com/colinrs/go-patterns-cn/blob/master/strategy.md
[3]: https://github.com/colinrs/go-patterns-cn/blob/master/template_method.md
[4]: https://github.com/colinrs/go-patterns-cn/blob/master/decorator.md
[5]: https://github.com/colinrs/go-patterns-cn/blob/master/bridge.md
[6]: https://github.com/colinrs/go-patterns-cn/blob/master/factory_pattern.md
[7]: https://github.com/colinrs/go-patterns-cn/blob/master/abc_factory_pattern.md
[8]: https://github.com/colinrs/go-patterns-cn/blob/master/prototype.md
[9]: https://github.com/colinrs/go-patterns-cn/blob/master/builder_pattern.md
[10]: https://github.com/colinrs/go-patterns-cn/blob/master/singleton.md
[11]: https://github.com/colinrs/go-patterns-cn/blob/master/flyweight_pattern.md
[12]: https://github.com/colinrs/go-patterns-cn/blob/master/facade_pattern.md
[13]: https://github.com/colinrs/go-patterns-cn/blob/master/proxy_pattern.md
[14]: https://github.com/colinrs/go-patterns-cn/blob/master/adapter_pattern.md
[15]: https://github.com/colinrs/go-patterns-cn/blob/master/mediator_pattern.md
[16]: https://github.com/colinrs/go-patterns-cn/blob/master/state_pattern.md
[17]: https://github.com/colinrs/go-patterns-cn/blob/master/memento_pattern.md
[18]: https://github.com/colinrs/go-patterns-cn/blob/master/composite_pattern.md
[19]: https://github.com/colinrs/go-patterns-cn/blob/master/Iterator_pattern.md
[20]: https://github.com/colinrs/go-patterns-cn/blob/master/chain_pattern.md
[21]: https://github.com/colinrs/go-patterns-cn/blob/master/command_pattern.md
[22]: https://github.com/colinrs/go-patterns-cn/blob/master/visitor_pattern.md
[23]: https://github.com/colinrs/go-patterns-cn/blob/master/interpreter_pattern.md
