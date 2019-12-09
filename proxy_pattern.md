# 代理模式

## 概念

为其他对象提供一种代理以控制对这个对象的访问

代理模式用于延迟处理操作或者在进行实际操作前后进行其它处理

## 模式的场景和优缺点

### 使用场景

在直接访问对象时带来的问题，比如说：要访问的对象在远程的机器上。在面向对象系统中，有些对象由于某些原因（比如对象创建开销很大，或者某些操作需要安全控制，或者需要进程外的访问），直接访问会给使用者或者系统结构带来很多麻烦，我们可以在访问此对象时加上一个对此对象的访问层

想在访问一个对象时做一些控制

### 优点

- 职责清晰
- 高扩展性
- 智能化

### 缺点

- 由于在客户端和真实主题之间增加了代理对象，因此有些类型的代理模式可能会造成请求的处理速度变慢
- 实现代理模式需要额外的工作，有些代理模式的实现非常复杂

## 代码实现

```golang

package main

import "fmt"

// Subject ...
type Subject interface {
	Do() string
}

// RealSubject ...
type RealSubject struct{}

// Do ...
func (RealSubject) Do() string {
	return "real"
}

// Proxy ...
type Proxy struct {
	real RealSubject
}

// Do ...
func (p Proxy) Do() string {
	var res string

	fmt.Println("一些权限验证工作...")
	// 在调用真实对象之前的工作，检查缓存，判断权限，实例化真实对象等。
	res += "pre:"

	// 调用真实对象
	res += p.real.Do()

	// 调用之后的操作，如缓存结果，对结果进行处理等。。
	res += ":after"

	return res
}
func main() {

	var sub Subject
	sub = &Proxy{}

	res := sub.Do()

	fmt.Printf("res:%s\n", res)
}

```
