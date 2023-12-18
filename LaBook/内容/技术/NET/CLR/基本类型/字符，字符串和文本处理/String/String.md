---
tags:
  - 技术/NET
---
## String的概念

C#中，一个String代表一个不可变（immutable）的顺序字符集，String类型直接派生自Object，所以是引用类型。因此，String对象永远存在于堆上，不会跑到线程栈。

C#将String视为基元类型，即编译器允许在源代码中直接使用字面值（literal）字符串。编译器将这些字符串放到模块的元数据中，并在运行时加载和引用他们。