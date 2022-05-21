## 赋值运算符重载

赋值是程序中最最常见的操作，对于内置类型的赋值我们直接使用=赋值即可，因为这些编译器已经帮我们做好了，但是对象的赋值呢？能直接赋值吗？

### 运算符重载

---

> **C++为了增强代码的可读性引入了运算符重载，运算符重载是具有特殊函数名的函数**，也具有其返回值类
> 型，函数名字以及参数列表，其返回值类型与参数列表与普通的函数类似。
>
> 函数名字为：**关键字operator后面接需要重载的运算符符号。**
>
> 函数原型：**返回值类型 operator操作符(参数列表)**

需要注意的几点：

* 不能通过连接其他符号来创建新的操作符：比如operator@，必须是已有的操作符；
* 重载操作符必须有一个类类型或者枚举类型的操作数；
* 用于内置类型的操作符，其含义不能改变，例如：内置的整型+，不 能改变其含义；
* 作为类成员的重载函数时，其形参看起来比操作数数目少1，成员函数的操作符有一个默认的形参this，限定为第一个形参；
* .* 、:: 、sizeof 、?: 、. 注意以上5个运算符不能重载；



特征如下：

* 参数类型相同；
* 返回值；
* 检测是否给自己赋值；
* 返回*this；
* 一个类如果没有显式的定义赋值操作符重载，编译器会自动生成一个，完成对象字节序的拷贝（浅拷贝）；

```cpp
class Date
{
public:
	Date(int year = 1900, int month = 1, int day = 1)
	{
		_year = year;
		_month = month;
		_day = day;
	}
	void Display()
	{
		cout << _year << "-" << _month << "-" << _day << endl;
	}
private:
	int _year;
	int _month;
	int _day;
};
int main()
{
	Date d1;
	Date d2(2018, 10, 1);
	// 这里d1调用的编译器生成operator=完成拷贝，d2和d1的值也是一样的。
	d1 = d2;
	d1.Display();
	d2.Display();
	return 0;
}
```

是不是很像自动生成的拷贝构造？那么它也存在一定的问题，对于日期类的对象他能很好的完成赋值操作，可对于指针类型呢？

下面的程序会崩溃

```cpp
class String
{
public:
	String(const char* str = "songxin")
	{
		cout << "String(const char* str = \"songxin\")" << endl;
		_str = (char*)malloc(strlen(str) + 1);
		strcpy(_str, str);
	}
	~String()
	{
		cout << "~String()" << endl;
		free(_str);
		_str = nullptr;
	}
private:
	char* _str;
};
int main()
{
	String s1("tanmei");
	String s2;
	s2 = s1;	
	return 0;
}
```

原因也是因为浅拷贝的关系，导致同一块内存被释放了两次，程序崩溃。

**可以不显式定义函数的情况**

* 成员变量没有指针；
* 成员变量的指针没有管理内存资源；















