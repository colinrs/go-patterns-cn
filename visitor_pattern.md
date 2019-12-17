# 访问者模式

## 概念

在访问者模式（Visitor Pattern）中，类的行为元素的执行算法可以随着访问者改变而改变

## 模式的场景和优缺点

### 使用场景

需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而需要避免让这些操作"污染"这些对象的类，使用访问者模式将这些封装到类中

### 优点

- 符合单一职责原则
- 优秀的扩展性
- 灵活性

### 缺点

- 违反了依赖倒置原则，依赖了具体类，没有依赖抽象

## 代码实现

```golang

package main

import (
	"fmt"
)

// Customer 行为对象
type Customer interface {
	Accept(Visitor)
}

// Visitor 访问者
type Visitor interface {
	// 访问者某一个对象
	Visit(Customer)
}

// EnterpriseCustomer ...
type EnterpriseCustomer struct {
	name string
}

// CustomerCol ...
type CustomerCol struct {
	customers []Customer
}

// Add ...
func (c *CustomerCol) Add(customer Customer) {
	c.customers = append(c.customers, customer)
}

// Accept ...
func (c *CustomerCol) Accept(visitor Visitor) {
	for _, customer := range c.customers {
		customer.Accept(visitor)
	}
}

// NewEnterpriseCustomer ...
func NewEnterpriseCustomer(name string) *EnterpriseCustomer {
	return &EnterpriseCustomer{
		name: name,
	}
}

// Accept 我接受你来访问
func (c *EnterpriseCustomer) Accept(visitor Visitor) {
	visitor.Visit(c)
}

// IndividualCustomer ...
type IndividualCustomer struct {
	name string
}

// NewIndividualCustomer ...
func NewIndividualCustomer(name string) *IndividualCustomer {
	return &IndividualCustomer{
		name: name,
	}
}

// Accept ...
func (c *IndividualCustomer) Accept(visitor Visitor) {
	visitor.Visit(c)
}

// ServiceRequestVisitor ...
type ServiceRequestVisitor struct{}

// Visit ...
func (*ServiceRequestVisitor) Visit(customer Customer) {
	// 定义我只访问那些
	switch c := customer.(type) {
	case *EnterpriseCustomer:
		fmt.Printf("serving enterprise customer %s\n", c.name)
	case *IndividualCustomer:
		fmt.Printf("serving individual customer %s\n", c.name)
	}
}

// AnalysisVisitor ...
type AnalysisVisitor struct{}

// Visit ...
func (*AnalysisVisitor) Visit(customer Customer) {
	switch c := customer.(type) {
	case *EnterpriseCustomer:
		fmt.Printf("analysis enterprise customer %s\n", c.name)
	}
}
func main() {

	c := &CustomerCol{}
	c.Add(NewEnterpriseCustomer("A company"))
	c.Add(NewIndividualCustomer("bob"))
	c.Add(NewEnterpriseCustomer("B company"))
	c.Accept(&AnalysisVisitor{})
	c.Accept(&ServiceRequestVisitor{})
}


```
