概念
========================================

COM
----------------------------------------
早期Windows大多使用C风格的API，其和操作系统底层最契合。但是C风格的API在发展中出现了缺少命名一致性、缺少逻辑分组（类似C++的命名空间）等缺点，使得 COM (Component Object Model) API机制发展起来。

COM 最开始在微软文档系列应用中使用，用于数据交互和内嵌文档。这个能力也被称作 OLE (Object Linking and Embedding)。

OLE在早期是使用 DDE (Dynamic Data Exchange) 机制实现的，而OLE在设计上存在一些缺陷，因此COM诞生了。实际上，COM在早期（1993年）发布时也被称作 OLE 2。

COM基于两个基本的原则设计。首先，clients通过由一系列良好定义方法组成的接口和objects通信。其中接口通常是C++的虚函数实现的，这带来了良好的命名兼容性。

第二个原则是接口都是动态加载的而不是静态链接的。

COM server通常指具体实现COM对象的一个DLL或EXE。

Runtime
----------------------------------------
Windows Runtime是在Windows 8引入的概念，在一些文档中被缩写为 WinRT（不同与Windows RT，Windows RT是Windows 8为ARM开发的定制版本）。

WinRT包含了一系列各个平台（Windows IoT设备、Xbox等）的面向Windows应用开发者的服务。


