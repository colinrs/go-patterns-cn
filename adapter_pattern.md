# 适配器模式

作为两个不兼容的接口之间的桥梁。这种类型的设计模式属于结构型模式，它结合了两个独立接口的功能。

## 概念

## 适配器模式的场景和优缺点

### 使用场景

主要解决在软件系统中，常常要将一些"现存的对象"放到新的环境中，而新环境要求的接口是现对象不能满足的

### 优点

- 可以让任何两个没有关联的类一起运行
- 提高了类的复用
- 增加了类的透明度
- 灵活性好

### 缺点

- 过多地使用适配器，会让系统非常零乱，不易整体进行把握

## 代码实现

```golang

package main

import (
	"fmt"
)

// Target 是适配的目标接口
type Target interface {
	Request() string
}

// Adaptee 是被适配的目标接口
type Adaptee interface {
	SpecificRequest() string
}

// AdapteeImpl 是被适配的目标类
type adapteeImpl struct{}

// NewAdaptee 是被适配接口的工厂函数
func NewAdaptee() Adaptee {
	return &adapteeImpl{}
}

// SpecificRequest 是目标类的一个方法
func (*adapteeImpl) SpecificRequest() string {
	return "adaptee method"
}

// NewAdapter 是Adapter的工厂函数
func NewAdapter(adaptee Adaptee) Target {
	return &adapter{
		Adaptee: adaptee,
	}
}

// Adapter 是转换Adaptee为Target接口的适配器
type adapter struct {
	Adaptee
}

// Request 实现Target接口
func (a *adapter) Request() string {
	//调用的是目标的方法(转换成客户希望的另外一个接口)
	return a.SpecificRequest()
}

func main() {

	adaptee := NewAdaptee()
	target := NewAdapter(adaptee)
	res := target.Request()
	fmt.Printf("res:%s\n", res)
}


```
