# 装饰模式

## 概念

装饰模式使用对象组合的方式动态改变或增加对象行为。

Go语言借助于匿名组合和非入侵式接口可以很方便实现装饰模式。

使用匿名组合，在装饰器中不必显式定义转调原对象方法。

## 模式的场景和优缺点

### 使用场景

在不想增加很多接口的情况下扩展函数或者接口。

#### 优点

装饰函数和被装饰函数可以独立发展，不会相互耦合，装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现函数的功能。

#### 缺点

多层装饰比较复杂

## 代码实现

```golang

package main

import (
	"log"
)

type myFunc func(int) int

// LogDecorate 给myFunc这个函数类型增加日志记录
func LogDecorate(fn myFunc) (reslutFunc myFunc) {

	reslutFunc = func(n int) (finnayReslut int) {
		// LogDecorate　函数给　fn myFunc　增加的额外日志
		log.Println("Starting the execution with the integer", n)

		finnayReslut = fn(n)

		log.Println("Execution is completed with the result", finnayReslut)

		return
	}
	return
}

// Component ...
type Component interface {
	Double() int
}

// ConcreteComponent ...
type ConcreteComponent struct {
	Num1 int
}

// Double ConcreteComponent 实现了Component接口
func (c *ConcreteComponent) Double() int {
	return c.Num1 * 2
}

// MulDecorator 匿名组合组合方式
type MulDecorator struct {
	Component
	num int
}

// WarpMulDecorator 返回的是接口类型Component
func WarpMulDecorator(c Component, num int) Component {
	return &MulDecorator{
		Component: c,
		num:       num,
	}
}

// Double MulDecorator 实现了Component接口
func (d *MulDecorator) Double() int {
	return d.Component.Double() * d.num
}

// AddDecorator 匿名组合组合方式
type AddDecorator struct {
	Component
	num int
}

// WarpAddDecorator 返回的是接口类型Component
func WarpAddDecorator(c Component, num int) Component {
	return &AddDecorator{
		Component: c,
		num:       num,
	}
}

// Double AddDecorator实现了Component接口
func (d *AddDecorator) Double() int {
	return d.Component.Double() + d.num
}

func main() {
	var fn myFunc
	fn = func(n int) int {
		return n * 2
	}
	reslutFunc := LogDecorate(fn)
	log.Printf("result:%d\n", reslutFunc(5))
	log.Println("匿名组合实现装饰模式")
	c := &ConcreteComponent{
		Num1: 1,
	}

	ww := WarpMulDecorator(c, 3)
	// 1*2*3
	log.Printf("匿名组合实现装饰模式,resut:%d\n", ww.Double())
	wd := WarpAddDecorator(c, 3)
	// 1*2+2
	log.Printf("匿名组合实现装饰模式,resut:%d\n", wd.Double())
}


```