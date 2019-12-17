# 命令模式

## 概念

命令模式（Command Pattern）请求以命令的形式包裹在对象中，并传给调用对象。调用对象寻找可以处理该命令的合适的对象，并把该命令传给相应的对象，该对象执行命令

## 模式的场景和优缺点

### 使用场景

"行为请求者"与"行为实现者"解耦

### 优点

- 降低了系统耦合度
- 新的命令可以很容易添加到系统中去

### 缺点

- 使用命令模式可能会导致某些系统有过多的具体命令类

## 代码实现

```golang

package main

import (
	"fmt"
)

// Command ...
type Command interface {
	Execute()
}

// StartCommand ...
type StartCommand struct {
	mb *MotherBoard
}

// NewStartCommand ...
func NewStartCommand(mb *MotherBoard) *StartCommand {
	return &StartCommand{
		mb: mb,
	}
}

// Execute ...
func (c *StartCommand) Execute() {
	c.mb.Start()
}

// RebootCommand ...
type RebootCommand struct {
	mb *MotherBoard
}

// NewRebootCommand ...
func NewRebootCommand(mb *MotherBoard) *RebootCommand {
	return &RebootCommand{
		mb: mb,
	}
}

// Execute ...
func (c *RebootCommand) Execute() {
	c.mb.Reboot()
}

// MotherBoard 执行者
type MotherBoard struct{}

// Start ...
func (*MotherBoard) Start() {
	fmt.Print("system starting\n")
}

// Reboot ...
func (*MotherBoard) Reboot() {
	fmt.Print("system rebooting\n")
}

// Box 调用者
type Box struct {
	buttion1 Command
	buttion2 Command
}

// NewBox ...
func NewBox(buttion1, buttion2 Command) *Box {
	return &Box{
		buttion1: buttion1,
		buttion2: buttion2,
	}
}

// PressButtion1 ...
func (b *Box) PressButtion1() {
	b.buttion1.Execute()
}

// PressButtion2 ...
func (b *Box) PressButtion2() {
	b.buttion2.Execute()
}
func main() {

	mb := &MotherBoard{}
	startCommand := NewStartCommand(mb)
	rebootCommand := NewRebootCommand(mb)

	box1 := NewBox(startCommand, rebootCommand)
	box1.PressButtion1()
	box1.PressButtion2()

	box2 := NewBox(rebootCommand, startCommand)
	box2.PressButtion1()
	box2.PressButtion2()
}

```
