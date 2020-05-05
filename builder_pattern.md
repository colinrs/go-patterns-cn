# 建造者模式

## 概念

建造者模式（Builder Pattern）使用多个简单的对象一步一步构建成一个复杂的对象。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式

## 模式的场景和优缺点

### 使用场景

1、需要生成的对象具有复杂的内部结构

2、需要生成的对象内部属性本身相互依赖

### 优点

1、建造者独立，易扩展

2、便于控制细节风险

### 缺点

1、产品必须有共同点，范围有限制

2、如内部变化复杂，会有很多的建造类

## 代码实现

```golang

package main

import "fmt"

//Builder 是生成器接口
type Builder interface {
	Part1()
	Part2()
	Part3()
}

// Director ...
type Director struct {
	builder Builder
}

// NewDirector ...
func NewDirector(builder Builder) *Director {
	return &Director{
		builder: builder,
	}
}

// Construct Product 指挥者，指挥不同的Build建造对象
func (d *Director) Construct() {
	d.builder.Part1()
	d.builder.Part2()
	d.builder.Part3()
}

// Builder1 ...
type Builder1 struct {
	result string
}

// Part1 ...
func (b *Builder1) Part1() {
	b.result += "1"
}

// Part2 ...
func (b *Builder1) Part2() {
	b.result += "2"
}

// Part3 ...
func (b *Builder1) Part3() {
	b.result += "3"
}

// GetResult ...
func (b *Builder1) GetResult() string {
	return b.result
}

// Builder2 ...
type Builder2 struct {
	result int
}

// Part1 ...
func (b *Builder2) Part1() {
	b.result += 1
}

// Part2 ...
func (b *Builder2) Part2() {
	b.result += 2
}

// Part3 ...
func (b *Builder2) Part3() {
	b.result += 3
}

// GetResult ...
func (b *Builder2) GetResult() int {
	return b.result
}

func main() {
	var b1 *Builder1
	var b2 *Builder2
	var d *Director
	b1 = &Builder1{}
	d = NewDirector(b1)
	d.Construct()
	fmt.Println(b1.GetResult())
	b2 = &Builder2{}
	d = NewDirector(b2)
	d.Construct()
	fmt.Println(b2.GetResult())

}
```
