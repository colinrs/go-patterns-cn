# 解释器模式

## 概念

解释器模式（Interpreter Pattern）提供了评估语言的语法或表达式的方式。这种模式实现了一个表达式接口，该接口解释一个特定的上下文。这种模式被用在 SQL 解析、符号处理引擎等。

## 模式的场景和优缺点

### 使用场景

对于一些固定文法构建一个解释句子的解释器

### 优点

- 可扩展性比较好，灵活
- 增加了新的解释表达式的方式
- 易于实现简单文法

### 缺点

- 可利用场景比较少
- 对于复杂的文法比较难维护
- 解释器模式会引起类膨胀

## 代码实现

```golang

package main

import (
	"fmt"
	"strconv"
	"strings"
)

// Node ...
type Node interface {
	Interpret() int
}

// ValNode ...
type ValNode struct {
	val int
}

// Interpret ...
func (n *ValNode) Interpret() int {
	return n.val
}

// AddNode ...
type AddNode struct {
	left, right Node
}

// Interpret ...
func (n *AddNode) Interpret() int {
	return n.left.Interpret() + n.right.Interpret()
}

// MinNode ...
type MinNode struct {
	left, right Node
}

// Interpret ...
func (n *MinNode) Interpret() int {
	return n.left.Interpret() - n.right.Interpret()
}

// Parser ...
type Parser struct {
	exp   []string
	index int
	prev  Node
}

// Parse ...
func (p *Parser) Parse(exp string) {
	p.exp = strings.Split(exp, " ")

	for {
		if p.index >= len(p.exp) {
			return
		}
		switch p.exp[p.index] {
		case "+":
			p.prev = p.newAddNode()
		case "-":
			p.prev = p.newMinNode()
		default:
			p.prev = p.newValNode()
		}
	}
}

// newAddNode ...
func (p *Parser) newAddNode() Node {
	p.index++
	return &AddNode{
		left:  p.prev,
		right: p.newValNode(),
	}
}

// newMinNode ...
func (p *Parser) newMinNode() Node {
	p.index++
	return &MinNode{
		left:  p.prev,
		right: p.newValNode(),
	}
}

// newValNode ...
func (p *Parser) newValNode() Node {
	v, _ := strconv.Atoi(p.exp[p.index])
	p.index++
	return &ValNode{
		val: v,
	}
}

// Result ...
func (p *Parser) Result() Node {
	return p.prev
}
func main() {

	p := &Parser{}
	p.Parse("1 + 2 + 3 - 4 + 5 - 6")
	res := p.Result().Interpret()
	fmt.Printf("res:%d\n", res)
}


```
