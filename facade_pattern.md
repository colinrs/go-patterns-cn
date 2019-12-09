# 外观模式

## 概念

外观模式（Facade Pattern）隐藏系统的复杂性，并向客户端提供了一个客户端可以访问系统的接口。这种类型的设计模式属于结构型模式，它向现有的系统添加一个接口，来隐藏系统的复杂性

这种模式涉及到一个单一的类，该类提供了客户端请求的简化方法和对现有系统类方法的委托调用

## 模式的场景和优缺点

### 使用场景

- 客户端不需要知道系统内部的复杂联系，整个系统只需提供一个"大管家"即可
- 定义系统的入口

### 优点

- 减少系统相互依赖
- 提高灵活性
- 提高了安全性

### 缺点

不符合开闭原则

## 代码实现

```golang

package main

import "fmt"

// API "大管家"
type API interface {
	Test() string
}

// apiImpl ...
type apiImpl struct {
	a AModuleAPI
	b BModuleAPI
}

// Test 统一调用了其他接口的方法
func (a *apiImpl) Test() string {
	aRet := a.a.TestA()
	bRet := a.b.TestB()
	return fmt.Sprintf("%s\n%s", aRet, bRet)
}

// NewAPI "大管家"
func NewAPI() API {
	return &apiImpl{
		a: NewAModuleAPI(),
		b: NewBModuleAPI(),
	}
}

// NewAModuleAPI return new AModuleAPI
func NewAModuleAPI() AModuleAPI {
	return &aModuleImpl{}
}

// AModuleAPI ...
type AModuleAPI interface {
	TestA() string
}

type aModuleImpl struct{}

// TestA ...
func (*aModuleImpl) TestA() string {
	return "A module running"
}

// NewBModuleAPI return new BModuleAPI
func NewBModuleAPI() BModuleAPI {
	return &bModuleImpl{}
}

// BModuleAPI ...
type BModuleAPI interface {
	TestB() string
}

type bModuleImpl struct{}

// TestB ...
func (*bModuleImpl) TestB() string {
	return "B module running"
}

func main() {

	api := NewAPI()
	ret := api.Test()
	fmt.Printf("ret:\n%s\n", ret)
}

```
