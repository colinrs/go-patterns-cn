# 备忘录模式

## 概念

备忘录模式（Memento Pattern）保存一个对象的某个状态，以便在适当的时候恢复对象

## 模式的场景和优缺点

### 使用场景

很多时候我们总是需要记录一个对象的内部状态，这样做的目的就是为了允许用户取消不确定或者错误的操作，能够恢复到他原先的状态，使得他有"后悔药"可吃

### 优点

- 给用户提供了一种可以恢复状态的机制，可以使用户能够比较方便地回到某个历史的状态
- 实现了信息的封装，使得用户不需要关心状态的保存细节

### 缺点

- 消耗资源。如果类的成员变量过多，势必会占用比较大的资源，而且每一次保存都会消耗一定的内存

## 代码实现

```golang

package main

import (
	"fmt"
)

// Memento ...
type Memento interface{}

// Game ...
type Game struct {
	hp, mp int
}

// gameMemento ...
type gameMemento struct {
	hp, mp int
}

// Play ...
func (g *Game) Play(mpDelta, hpDelta int) {
	g.mp += mpDelta
	g.hp += hpDelta
}

// Save 保存状态
func (g *Game) Save() Memento {
	return &gameMemento{
		hp: g.hp,
		mp: g.mp,
	}
}

// Load 恢复状态
func (g *Game) Load(m Memento) {
	gm := m.(*gameMemento)
	g.mp = gm.mp
	g.hp = gm.hp
}

// Status ...
func (g *Game) Status() {
	fmt.Printf("Current HP:%d, MP:%d\n", g.hp, g.mp)
}

func main() {

	game := &Game{
		hp: 10,
		mp: 10,
	}

	game.Status()
	progress := game.Save()

	game.Play(-2, -3)
	game.Status()

	game.Load(progress)
	game.Status()
}


```
