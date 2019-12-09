# 享元模式

## 概念

享元模式（Flyweight Pattern）主要用于减少创建对象的数量，以减少内存占用和提高性能。这种类型的设计模式属于结构型模式，它提供了减少对象数量从而改善应用所需的对象结构的方式。

享元模式尝试重用现有的同类对象，如果未找到匹配的对象，则创建新对象。

## 模式的场景和优缺点

### 使用场景

- 系统中有大量对象
- 这些对象消耗大量内存
- 这些对象的状态大部分可以外部化
- 这些对象可以按照内蕴状态分为很多组，当把外蕴对象从对象中剔除出来时，每一组对象都可以用一个对象来代替
- 系统不依赖于这些对象身份，这些对象是不可分辨的

### 优点

大大减少对象的创建，降低系统的内存，使效率提高

### 缺点

提高了系统的复杂度

## 代码实现

```golang

package main

import "fmt"

// ImageFlyweight ...
type ImageFlyweight struct {
	data string
}

// Data  ...
func (i *ImageFlyweight) Data() string {
	return i.data
}

// ImageFlyweightFactory ...
type ImageFlyweightFactory struct {
	// 存储对象
	maps map[string]*ImageFlyweight
}

// imageFactory ...
var imageFactory *ImageFlyweightFactory

// GetImageFlyweightFactory ...
func GetImageFlyweightFactory() *ImageFlyweightFactory {
	//　非线程安全,需要实现线程安全的话，参考单例模式
	if imageFactory == nil {
		imageFactory = &ImageFlyweightFactory{
			maps: make(map[string]*ImageFlyweight),
		}
	}
	return imageFactory
}

// Get ...
func (f *ImageFlyweightFactory) Get(filename string) *ImageFlyweight {
	image := f.maps[filename]
	// 没有则创建，非线程安全
	if image == nil {
		image = NewImageFlyweight(filename)
		f.maps[filename] = image
	}

	return image
}

// NewImageFlyweight ...
func NewImageFlyweight(filename string) *ImageFlyweight {
	// Load image file
	data := fmt.Sprintf("image data %s", filename)
	return &ImageFlyweight{
		data: data,
	}
}

// ImageViewer ...
type ImageViewer struct {
	*ImageFlyweight
}

// NewImageViewer ...
func NewImageViewer(filename string) *ImageViewer {
	// imageFactory.Get()
	image := GetImageFlyweightFactory().Get(filename)
	return &ImageViewer{
		ImageFlyweight: image,
	}
}

// Display ...
func (i *ImageViewer) Display() {
	fmt.Printf("Display: %s\n", i.Data())
}

func main() {
	viewer1 := NewImageViewer("image1.png")
	viewer2 := NewImageViewer("image1.png")
	viewer1.Display()
	viewer2.Display()
	fmt.Printf("viewer1:%p viewer:%p\n", viewer1.ImageFlyweight, viewer2.ImageFlyweight)
}
```
