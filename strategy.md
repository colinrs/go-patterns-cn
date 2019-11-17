# 策略模式

## 概念

- 定义一系列算法，让这些算法在运行时可以互换，使得分离算法，符合开闭原则

## 模式的场景和优缺点

### 使用场景

- 在有多种算法相似的情况下，使用 ```if...else``` 所带来的复杂和难以维护。
- 一个系统有许多许多类，而区分它们的只是他们直接的行为。


#### 策略模式优点

- 多重条件语句不易维护，而使用策略模式可以避免使用多重条件语句。
- 策略模式提供了一系列的可供重用的算法族，恰当使用继承可以把算法族的公共代码转移到父类里面，从而避免重复的代码。
- 策略模式可以提供相同行为的不同实现，客户可以根据不同时间或空间要求选择不同的。
- 策略模式提供了对开闭原则的完美支持，可以在不修改原代码的情况下，灵活增加新算法。
- 策略模式把算法的使用放到环境类中，而算法的实现移到具体策略类中，实现了二者的分离。

#### 策略模式缺点

- 客户端必须理解所有策略算法的区别，以便适时选择恰当的算法类。
- 策略模式造成很多的策略类。

## 代码实现

```golang

package main

import "fmt"

// Operator ...
type Operator interface {
	Apply(int, int) int
}

// Operation 包装器
type Operation struct {
	Operator Operator
}

// Addition ...
type Addition struct{}

// Apply ...
func (add *Addition) Apply(left, right int) int {
	return left + right
}

// Multiplication ...
type Multiplication struct{}

// Apply ...
func (mu *Multiplication) Apply(left, right int) int {
	return left * right
}

// Operate ...
func (o *Operation) Operate(leftValue, rightValue int) int {
	return o.Operator.Apply(leftValue, rightValue)
}

// CreateOpration ...
func CreateOpration(operator Operator) Operation {
	return Operation{operator}
}

func main() {
	var operationes []Operation
	operatorAdd := CreateOpration(new(Addition))
	operatorMul := CreateOpration(new(Multiplication))
	operationes = append(operationes, operatorAdd)
	operationes = append(operationes, operatorMul)
	for _, operator := range operationes {
		value := operator.Operate(1, 2)
		fmt.Printf("%d\n", value)
	}
}



```