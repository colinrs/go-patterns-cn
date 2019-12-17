# 迭代器模式

## 概念

用于顺序访问集合对象的元素，不需要知道集合对象的底层表示

## 模式的场景和优缺点

### 使用场景

提供一种方法顺序访问一个聚合对象中各个元素, 而又无须暴露该对象的内部表示

### 优点

- 迭代器模式将存储数据和遍历数据的职责分离，增加新的聚合类需要对应增加新的迭代器类，类的个数成对增加，这在一定程度上增加了系统的复杂性。

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

// Aggregate ...
type Aggregate interface {
	Iterator() Iterator
}

// Iterator ...
type Iterator interface {
	First()
	IsDone() bool
	Next() interface{}
}

// Numbers 实现了　Iterator
type Numbers struct {
	start, end int
}

// NewNumbers ...
func NewNumbers(start, end int) *Numbers {
	return &Numbers{
		start: start,
		end:   end,
	}
}

// Iterator 生成一个可迭代对象
func (n *Numbers) Iterator() Iterator {
	return &NumbersIterator{
		numbers: n,
		next:    n.start,
	}
}

// NumbersIterator 实现了　Iterator
type NumbersIterator struct {
	numbers *Numbers
	next    int
}

// First ...
func (i *NumbersIterator) First() {
	i.next = i.numbers.start
}

// IsDone ...
func (i *NumbersIterator) IsDone() bool {
	return i.next > i.numbers.end
}

// Next ...
func (i *NumbersIterator) Next() interface{} {
	if !i.IsDone() {
		next := i.next
		// i 变成下一个
		i.next++
		return next
	}
	return nil
}

// IteratorPrint ...
func IteratorPrint(i Iterator) {
	for i.First(); !i.IsDone(); {
		c := i.Next()
		fmt.Printf("%#v\n", c)
	}
}

func main() {

	var aggregate Aggregate
	aggregate = NewNumbers(1, 10)

	IteratorPrint(aggregate.Iterator())
}



```
