# 中介者模式

## 概念

中介者模式（Mediator Pattern）是用来降低多个对象和类之间的通信复杂性。这种模式提供了一个中介类，该类通常处理不同类之间的通信，并支持松耦合

用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互

## 模式的场景和优缺点

### 使用场景

系统中对象之间存在比较复杂的引用关系，导致它们之间的依赖关系结构混乱而且难以复用该对象

想通过一个中间类来封装多个类中的行为，而又不想生成太多的子类

多个类相互耦合，形成了网状结构,中介者模式将上述网状结构分离为星型结构

### 优点

- 降低了类的复杂度，将一对多转化成了一对一
- 各个类之间的解耦

### 缺点

- 中介者会庞大，变得复杂难以维护

## 代码实现

```golang

package main

import (
	"fmt"
	"strings"
)

// CDDriver ...
type CDDriver struct {
	Data string
}

// ReadData ...
func (c *CDDriver) ReadData() {
	c.Data = "music,image"

	fmt.Printf("CDDriver: reading data %s\n", c.Data)
	GetMediatorInstance().changed(c)
}

// CPU ...
type CPU struct {
	Video string
	Sound string
}

// Process ...
func (c *CPU) Process(data string) {
	sp := strings.Split(data, ",")
	c.Sound = sp[0]
	c.Video = sp[1]

	fmt.Printf("CPU: split data with Sound %s, Video %s\n", c.Sound, c.Video)
	GetMediatorInstance().changed(c)
}

// VideoCard ...
type VideoCard struct {
	Data string
}

// Display ...
func (v *VideoCard) Display(data string) {
	v.Data = data
	fmt.Printf("VideoCard: display %s\n", v.Data)
	GetMediatorInstance().changed(v)
}

// SoundCard ...
type SoundCard struct {
	Data string
}

// Play ...
func (s *SoundCard) Play(data string) {
	s.Data = data
	fmt.Printf("SoundCard: play %s\n", s.Data)
	GetMediatorInstance().changed(s)
}

// Mediator ...中介者，集合了多种角色
type Mediator struct {
	CD    *CDDriver
	CPU   *CPU
	Video *VideoCard
	Sound *SoundCard
}

var mediator *Mediator

// GetMediatorInstance 单例模式
func GetMediatorInstance() *Mediator {
	if mediator == nil {
		mediator = &Mediator{}
	}
	return mediator
}

func (m *Mediator) changed(i interface{}) {
	switch inst := i.(type) {
	case *CDDriver:
		m.CPU.Process(inst.Data)
	case *CPU:
		m.Sound.Play(inst.Sound)
		m.Video.Display(inst.Video)
	}
}
func main() {

	mediator := GetMediatorInstance()
	mediator.CD = &CDDriver{}
	mediator.CPU = &CPU{}
	mediator.Video = &VideoCard{}
	mediator.Sound = &SoundCard{}

	//　Tiggle
	mediator.CD.ReadData()
	fmt.Println("")
	fmt.Printf("mediator.CD.Data:%s\n", mediator.CD.Data)
	fmt.Printf("mediator.CPU.Sound:%s\n", mediator.CPU.Sound)
	fmt.Printf("mediator.CPU.Video:%s\n", mediator.CPU.Video)
	fmt.Printf("mediator.Video.Data:%s\n", mediator.Video.Data)
	fmt.Printf("mediator.Sound.Data:%s\n", mediator.Sound.Data)
}


```
