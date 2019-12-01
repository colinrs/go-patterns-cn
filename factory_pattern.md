# 工厂模式

## 概念

在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。

工厂方法模式使用子类的方式延迟生成对象到子类中实现。Go中不存在继承 所以使用匿名组合来实现

## 模式的场景和优缺点

### 使用场景

1、日志记录器：记录可能记录到本地硬盘、系统事件、远程服务器等，用户可以选择记录日志到什么地方

2、数据库访问，当用户不知道最后系统采用哪一类数据库，以及数据库可能有变化时。 3、设计一个连接服务器的框架，需要三个协议，"POP3"、"IMAP"、"HTTP"，可以把这三个作为产品类，共同实现一个接口

### 优点

1、一个调用者想创建一个对象，只要知道其名称就可以了

2、扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以

3、屏蔽产品的具体实现，调用者只关心产品的接口

### 缺点

每次增加一个产品时，都需要增加一个具体类和对象实现工厂，使得系统中类的个数成倍增加，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。这并不是什么好事。

## 代码实现

```golang

package main

import "fmt"

// Logger 是被封装的实际类接口
type Logger interface {
	Info(message string)
}

// LoggerFactory 是工厂接口
type LoggerFactory interface {
	Create() Logger
}

// LoggerBase 是Logger 接口实现的基类，封装公用方法
type LoggerBase struct {
}

// FileLoggerFactory 是 FileLogger 的工厂类
type FileLoggerFactory struct{}

// Create ...
func (FileLoggerFactory) Create() Logger {
	return &FileLogger{
		LoggerBase: &LoggerBase{},
	}
}

// FileLogger Logger 的实际实现,匿名组合来实现继承
type FileLogger struct {
	*LoggerBase
}

// Info 获取结果
func (f FileLogger) Info(message string) {
	fmt.Printf("file logger:%s\n", message)
}

// SysLoggerFactory 是 SysLogger 的工厂类
type SysLoggerFactory struct{}

// Create ...
func (SysLoggerFactory) Create() Logger {
	return &SysLogger{
		LoggerBase: &LoggerBase{},
	}
}

// SysLogger Logger 的实际日志记录实现,匿名组合来实现继承
type SysLogger struct {
	*LoggerBase
}

// Info 获取结果
func (s SysLogger) Info(message string) {
	fmt.Printf("sys logger:%s\n", message)
}

// NewLogger ...
func NewLogger(t string) (l Logger) {

	switch t {
	case "file":
		FileLoggerFactory := FileLoggerFactory{}
		l = FileLoggerFactory.Create()
	case "sys":
		SysLoggerFactory := SysLoggerFactory{}
		l = SysLoggerFactory.Create()
	}
	return
}

func main() {
	var l Logger
	var s Logger
	l = NewLogger("file")
	l.Info("test for file")
	s = NewLogger("sys")
	s.Info("test for sys")

}
```
