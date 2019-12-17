# 责任链模式

## 概念

责任链模式（Chain of Responsibility Pattern）用于分离不同职责，并且动态组合相关职责

## 模式的场景和优缺点

### 使用场景

在处理消息的时候以过滤很多道

### 优点

- 降低耦合度。它将请求的发送者和接收者解耦
- 简化了对象。使得对象不需要知道链的结构
- 增强给对象指派职责的灵活性
- 通过改变链内的成员或者调动它们的次序，允许动态地新增或者删除责任
- 增加新的请求处理类很方便

### 缺点

- 不能保证请求一定被接收
- 系统性能将受到一定影响，而且在进行代码调试时不太方便，可能会造成循环调用
- 可能不容易观察运行时的特征，有碍于除错

## 代码实现

```golang

package main

import (
	"fmt"
)

// Manager 定义接口
type Manager interface {
	HaveRight(money int) bool
	HandleFeeRequest(name string, money int) bool
}

// RequestChain 职责链
type RequestChain struct {
	Manager
	successor *RequestChain
}

// SetSuccessor 设置处理者
func (r *RequestChain) SetSuccessor(m *RequestChain) {

	r.successor = m
}

// HandleFeeRequest 调用链路会调用处理者
func (r *RequestChain) HandleFeeRequest(name string, money int) bool {
	if r.Manager.HaveRight(money) {
		return r.Manager.HandleFeeRequest(name, money)
	}
	if r.successor != nil {
		return r.successor.HandleFeeRequest(name, money)
	}
	return false
}

// HaveRight ...
func (r *RequestChain) HaveRight(money int) bool {
	return true
}

// ProjectManager ...
type ProjectManager struct {
	Name string
}

// NewProjectManagerChain ...
func NewProjectManagerChain(name string) *RequestChain {
	return &RequestChain{
		Manager: &ProjectManager{Name: name},
	}
}

// HaveRight ...
func (p *ProjectManager) HaveRight(money int) bool {
	return money < 500
}

// HandleFeeRequest ...
func (p *ProjectManager) HandleFeeRequest(name string, money int) bool {
	if name == "bob" {
		fmt.Printf("Project manager permit %s %d fee request\n", name, money)
		return true
	}
	fmt.Printf("Project manager don't permit %s %d fee request\n", name, money)
	return false
}

// DepManager ...
type DepManager struct {
	Name string
}

// NewDepManagerChain ...
func NewDepManagerChain(name string) *RequestChain {
	return &RequestChain{
		Manager: &DepManager{Name: name},
	}
}

// HaveRight ...
func (d *DepManager) HaveRight(money int) bool {
	return money < 5000
}

// HandleFeeRequest ...
func (d *DepManager) HandleFeeRequest(name string, money int) bool {
	fmt.Printf("my name:%s\n", d.Name)
	if name == "tom" {
		fmt.Printf("Dep manager permit %s %d fee request\n", name, money)
		return true
	}
	fmt.Printf("Dep manager don't permit %s %d fee request\n", name, money)
	return false
}

// GeneralManager ...
type GeneralManager struct {
	Name string
}

// NewGeneralManagerChain ...
func NewGeneralManagerChain(name string) *RequestChain {
	return &RequestChain{
		Manager: &GeneralManager{Name: name},
	}
}

// HaveRight ...
func (g *GeneralManager) HaveRight(money int) bool {
	return true
}

// HandleFeeRequest ...
func (g *GeneralManager) HandleFeeRequest(name string, money int) bool {
	fmt.Printf("my name:%s\n", g.Name)
	if name == "ada" {
		fmt.Printf("General manager permit %s %d fee request\n", name, money)
		return true
	}
	fmt.Printf("General manager don't permit %s %d fee request\n", name, money)
	return false
}
func main() {
	c1 := NewProjectManagerChain("c1")
	c2 := NewDepManagerChain("c2")
	c3 := NewGeneralManagerChain("c3")

	//c1 ---> c2 ----> c3
	c1.SetSuccessor(c2)
	c2.SetSuccessor(c3)

	var c Manager = c1

	c.HandleFeeRequest("bob", 400)
	c.HandleFeeRequest("tom", 1400)
	c.HandleFeeRequest("ada", 10000)
	c.HandleFeeRequest("floar", 400)
}

```
