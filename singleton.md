# 单例模式

## 概念

这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

注意：

1、单例类只能有一个实例。

2、单例类必须自己创建自己的唯一实例。

3、单例类必须给所有其他对象提供这一实例。

## 模式的场景和优缺点

### 使用场景

1、要求生产唯一序列号

2、WEB 中的计数器，不用每次刷新都在数据库里加一次，用单例先缓存起来

3、创建的一个对象需要消耗的资源过多，比如 I/O 与数据库的连接等。

### 优点

1、在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例

2、避免对资源的多重占用（比如写文件操作）。

### 缺点

没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化

## 代码实现

```golang
package main

import (
	"fmt"
	"sync"
)

// Singleton 是单例模式对象
type Singleton struct {
	name string
}

var singleton *Singleton
var once sync.Once

// GetInstance 用于获取单例模式对象
func GetInstance() *Singleton {
	once.Do(func() {
		singleton = &Singleton{
			name: "singleton",
		}
	})
	return singleton
}

func main() {

	e1 := GetInstance()
	fmt.Println(e1.name)

	e2 := GetInstance()
	fmt.Println(e2.name)

}

```
