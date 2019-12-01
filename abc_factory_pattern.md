# 抽象工厂模式

## 概念

在抽象工厂模式中，是围绕一个超级工厂创建其他工厂。该超级工厂又称为其他工厂的工厂。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

在抽象工厂模式中，接口是负责创建一个相关对象的工厂，不需要显式指定它们的类。每个生成的工厂都能按照工厂模式提供对象

如果抽象工厂退化成生成的对象无关联则成为工厂模式。

## 模式的场景和优缺点

### 使用场景

### 优点

一个产品族中的多个对象被设计成一起工作时，它能保证客户端始终只使用同一个产品族中的对象

### 缺点

产品族扩展非常困难，要增加一个系列的某一产品，既要在抽象的 `Creator` 里加代码，又要在具体的里面加代码

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

// NewLoggerFactory 在其他工厂面前又抽象的了一层,是一个超级工厂
func NewLoggerFactory(t string) (l Logger) {

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
	l = NewLoggerFactory("file")
	l.Info("test for file")
	s = NewLoggerFactory("sys")
	s.Info("test for sys")

}

```
