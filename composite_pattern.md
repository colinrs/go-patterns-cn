# 组合模式

## 概念

组合模式（Composite Pattern），组合模式统一对象和对象集，使得使用相同接口使用对象和对象集

## 模式的场景和优缺点

### 使用场景

- 您想表示对象的部分-整体层次结构（树形结构）
- 您希望用户忽略组合对象与单个对象的不同，用户将统一地使用组合结构中的所有对象

### 优点

- 高层模块调用简单
- 节点自由增加

### 缺点

- .

## 代码实现

```golang

package main

import (
	"fmt"
)

// Component ...
type Component interface {
	Parent() Component
	SetParent(Component)
	Name() string
	SetName(string)
	AddChild(Component)
	Print(string)
}

const (
	// LeafNode ...
	LeafNode = iota
	// CompositeNode ...
	CompositeNode
)

// NewComponent 生成节点
func NewComponent(kind int, name string) Component {
	var c Component
	switch kind {
	case LeafNode:
		c = NewLeaf()
	case CompositeNode:
		c = NewComposite()
	}

	c.SetName(name)
	return c
}

// component ...
type component struct {
	parent Component
	name   string
}

// Parent ...
func (c *component) Parent() Component {
	return c.parent
}

// SetParent ...
func (c *component) SetParent(parent Component) {
	c.parent = parent
}

// Name ...
func (c *component) Name() string {
	return c.name
}

// SetName ...
func (c *component) SetName(name string) {
	c.name = name
}

// AddChild ...
func (c *component) AddChild(Component) {}

// Print ...
func (c *component) Print(string) {}

// Leaf ...
type Leaf struct {
	component
}

// NewLeaf ...
func NewLeaf() *Leaf {
	return &Leaf{}
}

// Print ...
func (c *Leaf) Print(pre string) {
	fmt.Printf("%s-%s\n", pre, c.Name())
}

// Composite ...
type Composite struct {
	component
	childs []Component
}

// NewComposite ...
func NewComposite() *Composite {
	return &Composite{
		childs: make([]Component, 0),
	}
}

// AddChild ...
func (c *Composite) AddChild(child Component) {
	child.SetParent(c)
	c.childs = append(c.childs, child)
}

// Print ...
func (c *Composite) Print(pre string) {
	fmt.Printf("%s+%s\n", pre, c.Name())
	pre += " "
	for _, comp := range c.childs {
		comp.Print(pre)
	}
}

func main() {

	root := NewComponent(CompositeNode, "root")
	c1 := NewComponent(CompositeNode, "c1")
	c2 := NewComponent(CompositeNode, "c2")
	c3 := NewComponent(CompositeNode, "c3")

	l1 := NewComponent(LeafNode, "l1")
	l2 := NewComponent(LeafNode, "l2")
	l3 := NewComponent(LeafNode, "l3")

	// 树状结构
	root.AddChild(c1)
	root.AddChild(c2)
	c1.AddChild(c3)
	c1.AddChild(l1)
	c2.AddChild(l2)
	c2.AddChild(l3)

	root.Print("")
}


```
