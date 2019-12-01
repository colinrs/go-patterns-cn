# 原型模式

## 概念

原型模式使对象能复制自身，并且暴露到接口中，使客户端面向接口编程时，不知道接口实际对象的情况下生成新的对象。

原型模式配合原型管理器使用，使得客户端在不知道具体类的情况下，通过接口管理器得到新的实例，并且包含部分预设定配置。

## 模式的场景和优缺点

### 使用场景

一个对象需要提供给其他对象访问，而且各个调用者可能都需要修改其值时，可以考虑使用原型模式拷贝多个对象供调用者使用

#### 优点

1、性能提高

2、逃避构造函数的约束

#### 缺点

配备克隆方法需要对类的功能进行通盘考虑，这对于全新的类不是很难，但对于已有的类不一定很容易，特别当一个类引用不支持串行化的间接对象，或者引用含有循环结构的时候

## 代码实现

```golang

package main

import "fmt"

// Example 示例结构体
type Example struct {
	Content string
}

// Clone ...
func (e *Example) Clone() *Example {
	// golang 深拷贝
	res := *e
	return &res
}

func main() {
	var e *Example
	var eClone *Example
	e = &Example{
		Content: "Example",
	}
	fmt.Printf("e:%p,Content:%s\n", e, e.Content)

	eClone = e.Clone()
	fmt.Printf("clone e:%p,Content:%s\n", eClone, eClone.Content)
}



```
