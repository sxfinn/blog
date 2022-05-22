# 2022-05-22-

### 摘要
> 再谈构造函数
>
> static成员
>
> C++11的成员初始化
>
> 友元
>
> 内部类
>
> 再次理解封装

### 总结
> 

目录
---
[TOC]

------

## 再谈构造函数

之前讲过构造函数的一些特性，再在这里补充下。

### 构造函数体赋值

---

```cpp
class Date
{
public:
	Date(int year, int month, int day)
	{
		_year = year;
		_month = month;
		_day = day;
	}
private:
	int _year;
	int _month;
	int _day;
};
```

在程序进入函数体时，成员变量的初始化实际上已经完成，虽然为构造函数的函数体，可实际函数体的赋值操作并不是对成员的初始化，而只能算是对成员的赋值操作。并且初始化操作只能由一次而在函数体内却可对一个变量多次赋值，因此函数体内的赋值操作不能称为初始化操作。



### 初始化列表

---

> 初始化列表：以一个**冒号开始**，接着是以**逗号分隔**每个成员列表，每个**成员变量后跟一个括号**，括号内为需要初始化的**初值或者表达式**。

```cpp
class Stack
{
public:
	Stack()
		:
		_size(0),
		_capacity(4),//初值
		_p((int*)malloc(sizeof(int) * _capacity))//表达式
	{}		
private:
	
	int _size;
	int _capacity;
	int* _p;
};
int main()
{
	Stack s;
	return 0;
}
```



1. 每个成员变量在初始化列表中**只能出现一次**(初始化只能初始化一次)
2. 类中包含以下成员，必须放在初始化列表位置显式进行初始化：
  - [x] 引用成员变量
  - [x] const成员变量
  - [x] 自定义成员变量（该类没有默认构造函数）

```cpp
class A
{
public:
	A(int a)
		:
		_a(a)
	{}
private:
	int _a;
};
int global = 0;
class B
{
public:
	B(int b , int aa)
		:
		_b(b),
		_aa(aa),
		_ref(global)
	{}
private:
	const int _b;//const
	int& _ref;//引用
	A _aa;//没有默认构造函数
};
int main()
{
	B tmp(1, 2);
	return 0;
}
```

尽量使用初始化列表初始化，因为不论是否











