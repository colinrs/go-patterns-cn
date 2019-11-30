# 桥接模式


## 概念

桥接模式分离抽象部分和实现部分。使得两部分独立扩展。

桥接模式类似于策略模式，区别在于策略模式封装一系列算法使得算法可以互相替换。

策略模式使抽象部分和实现部分分离，可以独立变化。

这种模式涉及到一个作为桥接的接口，使得实体类的功能独立于接口实现类。这两种类型的类可被结构化改变而互不影响。

## 模式的场景和优缺点

在有多种可能会变化的情况下，用继承会造成类爆炸问题，扩展起来不灵活



### 使用场景

实现系统可能有多个角度分类，每一种角度都可能变化。

#### 优点

抽象和实现的分离、优秀的扩展能力、实现细节对客户透明

#### 缺点

桥接模式的引入会增加系统的理解与设计难度，由于聚合关联关系建立在抽象层，要求开发者针对抽象进行设计与编程

## 代码实现

```golang

package main

import (
	"fmt"
	"log"
)

// AbstractMessage 抽象部分
type AbstractMessage interface {
	SendMessage(text, to string)
}

// MessageImplementer 实现部分
type MessageImplementer interface {
	Send(text, to string)
}

// MessageSMS 实现MessageImplementer　是实现部分
type MessageSMS struct{}

// Send ...
func (*MessageSMS) Send(text, to string) {
	fmt.Printf("send %s to %s via SMS\n", text, to)
}

// ViaSMS ...
func ViaSMS() MessageImplementer {
	return &MessageSMS{}
}

// MessageEmail 实现MessageImplementer　是实现部分
type MessageEmail struct{}

// Send ...
func (*MessageEmail) Send(text, to string) {
	fmt.Printf("send %s to %s via Email\n", text, to)
}

// ViaEmail ...
func ViaEmail() MessageImplementer {
	return &MessageEmail{}
}

// CommonMessage 实现AbstractMessage 是抽象部分
type CommonMessage struct {
	method MessageImplementer
}

// SendMessage ...
func (m *CommonMessage) SendMessage(text, to string) {
	m.method.Send(fmt.Sprintf("[Common] %s", text), to)
}

// NewCommonMessage ...
func NewCommonMessage(method MessageImplementer) *CommonMessage {
	return &CommonMessage{
		method: method,
	}
}

// UrgencyMessage 实现AbstractMessage 是抽象部分
type UrgencyMessage struct {
	method MessageImplementer
}

// SendMessage ...
func (m *UrgencyMessage) SendMessage(text, to string) {
	m.method.Send(fmt.Sprintf("[Urgency] %s", text), to)
}

// NewUrgencyMessage ...
func NewUrgencyMessage(method MessageImplementer) *UrgencyMessage {
	return &UrgencyMessage{
		method: method,
	}
}

func main() {
	log.Println("hell world")
	var commonMessage *CommonMessage
	commonMessage = NewCommonMessage(ViaSMS())
	commonMessage.SendMessage("sms", "sms@qq.com")
	commonMessage = NewCommonMessage(ViaEmail())
	commonMessage.SendMessage("emal", "sms@qq.com")
	var urgencyMessage *UrgencyMessage
	urgencyMessage = NewUrgencyMessage(ViaSMS())
	urgencyMessage.SendMessage("sms", "sms@qq.com")

}

```