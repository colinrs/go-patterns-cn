# 观察者模式

## 概念

观察者模式允许类型实例将事件“发布”到希望在发生特定事件时进行更新通知其他类型实例（“观察者”)

## 使用观察者模式的场景和优缺点

### 使用场景

关联行为场景，需要注意的是，关联行为是可拆分的，而不是“组合”关系。
事件多级触发场景。
跨系统的消息交换场景，如消息队列、事件总线的处理机制。

#### 优点

- 解除耦合，让耦合的双方都依赖于抽象，从而使得各自的变换都不会影响另一边的变换。

#### 缺点

- 在应用观察者模式时需要考虑一下开发效率和运行效率的问题，程序中包括一个被观察者、多个观察者，开发、调试等内容会比较复杂，如果一个观察者卡顿，会影响整体的执行效率，在这种情况下，一般会采用异步实现。

## 代码实现

```golang

package main

import (
	"fmt"
	"time"
)

// Event 消息
type Event struct {
	Data int64
}

// Observer 观察者，观察特定的消息
type Observer interface {
	//　消息变动通知观察者
	OnNotify(Event)
}

// Notifier 被观察的对象
type Notifier interface {

	// Register　注册观察者
	Register(Observer)
	// Deregister　删除观察者
	Deregister(Observer)
	// Notify　消息通知
	Notify(Event)
}

// eventObserver Observer 实现
type eventObserver struct {
	id int
}

// eventNotifier Notifier　实现
type eventNotifier struct {
	// 用map来存放Observer
	observers map[Observer]struct{}
}

// OnNotify 收到消息变动
func (o *eventObserver) OnNotify(e Event) {
	fmt.Printf("*** 观察者 %d 接受到变动的消息: %d\n", o.id, e.Data)
}

// Register ...
func (o *eventNotifier) Register(l Observer) {
	o.observers[l] = struct{}{}
}

// Deregister ...
func (o *eventNotifier) Deregister(l Observer) {
	delete(o.observers, l)
}

// Notify 把变化的消息发送给所有的观察者
func (o *eventNotifier) Notify(e Event) {
	for i := range o.observers {
		i.OnNotify(e)
	}
}

func main() {
	// 初始化
	n := eventNotifier{
		observers: map[Observer]struct{}{},
	}

	// 注册两个观察者
	n.Register(&eventObserver{id: 1})
	n.Register(&eventObserver{id: 2})

	// 消息通知
	stop := time.NewTimer(10 * time.Second).C
	tick := time.NewTicker(time.Second).C
	for {
		select {
		case <-stop:
			return
		case t := <-tick:
			n.Notify(Event{Data: t.Unix()})
		}
	}
}

```