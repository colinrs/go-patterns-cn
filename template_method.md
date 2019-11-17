# 模板方法模式模式

## 概念

在模板模式（Template Pattern）中，一个抽象类公开定义了执行它的方法的方式/模板。它的子类可以按需要重写方法实现，但调用将以抽象类中定义的方式进行。

## 模板方法模式模式的场景和优缺点

### 使用场景

- 有多个子类共有的方法，且逻辑相同
- 重要的、复杂的方法，可以考虑作为模板方法

#### 优点

- 封装不变部分，扩展可变部分
- 提取公共代码，便于维护
- 行为由父类控制，子类实现

#### 缺点

- 每一个不同的实现都需要一个子类来实现，导致类的个数增加，使得系统更加庞大

## 代码实现

```golang
package main

import "fmt"

// Thing 定义接口
type Thing interface {
	SetName(name string)
	BeforeAction()
	Exit()
}

// Person ...
type Person struct {
	name     string
	Concrete Thing
}

// SetName ...
func (p *Person) SetName(name string) {
	p.name = name
}

// BeforeAction ...
func (p *Person) BeforeAction() {
	//
	p.Concrete.BeforeAction()
}

// Exit ...
func (p *Person) Exit() {
	p.BeforeAction()
	fmt.Println(p.name + "exit")
}

// Boy ...
type Boy struct {
	Person //匿名组合实现继承
}

// BeforeAction ...
func (b *Boy) BeforeAction() {
	fmt.Println(b.name)
}

// Girl ...
type Girl struct {
	Person //匿名组合实现继承
}

// BeforeAction ...
func (g *Girl) BeforeAction() {
	fmt.Println(g.name)
}

func main() {
	boy := &Boy{}
	person := new(Person)
	person.SetName("boy")
	person.Concrete = boy
	//赋值boy的内容, 注意要在设定了person具体值之后赋值，否则为空
	boy.Person = *person
	person.Exit()
}


```