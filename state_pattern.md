# 状态模式

## 概念

在状态模式（State Pattern）中，类的行为是基于它的状态改变的

状态模式用于分离状态和行为

## 模式的场景和优缺点

### 使用场景

代码中包含大量与对象状态有关的条件语句

### 优点

- 封装了转换规则
- 枚举可能的状态，在枚举状态之前需要确定状态种类
- 将所有与某个状态有关的行为放到一个类中，并且可以方便地增加新的状态，只需要改变对象状态即可改变对象的行为
- 允许状态转换逻辑与状态对象合成一体，而不是某一个巨大的条件语句块
- 可以让多个环境对象共享一个状态对象，从而减少系统中对象的个数

### 缺点

- 状态模式的使用必然会增加系统类和对象的个数
- 状态模式的结构与实现都较为复杂，如果使用不当将导致程序结构和代码的混乱
- 状态模式对"开闭原则"的支持并不太好，对于可以切换状态的状态模式，增加新的状态类需要修改那些负责状态转换的源代码，否则无法切换到新增状态，而且修改某个状态类的行为也需修改对应类的源代码

## 代码实现

```golang

package main

import (
	"fmt"
)

// Week ...
type Week interface {
	Today()
	Next(*DayContext)
}

// DayContext ...
type DayContext struct {
	today Week
}

// NewDayContext ...
func NewDayContext() *DayContext {
	return &DayContext{
		today: &Sunday{},
	}
}

// Today ...
func (d *DayContext) Today() {
	d.today.Today()
}

// Next ...
func (d *DayContext) Next() {
	d.today.Next(d)
}

// Sunday ...
type Sunday struct{}

// Today ...
func (*Sunday) Today() {
	fmt.Printf("Sunday\n")
}

// Next ...
func (*Sunday) Next(ctx *DayContext) {
	ctx.today = &Monday{}
}

// Monday ...
type Monday struct{}

// Today ...
func (*Monday) Today() {
	fmt.Printf("Monday\n")
}

// Next ...
func (*Monday) Next(ctx *DayContext) {
	ctx.today = &Tuesday{}
}

// Tuesday ...
type Tuesday struct{}

// Today ...
func (*Tuesday) Today() {
	fmt.Printf("Tuesday\n")
}

// Next ...
func (*Tuesday) Next(ctx *DayContext) {
	ctx.today = &Wednesday{}
}

// Wednesday ...
type Wednesday struct{}

// Today ...
func (*Wednesday) Today() {
	fmt.Printf("Wednesday\n")
}

// Next ...
func (*Wednesday) Next(ctx *DayContext) {
	ctx.today = &Thursday{}
}

// Thursday ...
type Thursday struct{}

// Today ...
func (*Thursday) Today() {
	fmt.Printf("Thursday\n")
}

// Next ...
func (*Thursday) Next(ctx *DayContext) {
	ctx.today = &Friday{}
}

// Friday ...
type Friday struct{}

// Today ...
func (*Friday) Today() {
	fmt.Printf("Friday\n")
}

// Next ...
func (*Friday) Next(ctx *DayContext) {
	ctx.today = &Saturday{}
}

// Saturday ...
type Saturday struct{}

// Today ...
func (*Saturday) Today() {
	fmt.Printf("Saturday\n")
}

// Next ...
func (*Saturday) Next(ctx *DayContext) {
	ctx.today = &Sunday{}
}

func main() {

	ctx := NewDayContext()
	todayAndNext := func() {
		ctx.Today()
		ctx.Next()
	}

	for i := 0; i < 10; i++ {
		todayAndNext()
	}
}


```
