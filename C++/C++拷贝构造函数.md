## 拷贝构造函数

我们经常会用一个变量去初始化一个同类型的变量，那么对于自定义的类型也应该有类似的操作，那么创建对象时如何使用一个已经存在的对象去创建另一个与之相同的对象呢？

> **构造函数：只有单个形参**，该形参是对本类类型对象的引用(一般常用const修饰)，**在用已存在的类类型对象创建新对象时由编译器自动调用**

拷贝构造函数是构造函数的一个重载，因此显式的定义了拷贝构造，那么编译器也不再默认生成构造函数。

### 特征

---

拷贝构造也是一个特殊的成员函数。

特征如下：

* 拷贝构造是构造函数的一个重载；
* 拷贝构造的参数只有一个并且类型必须是该类的引用，而不是使用传值调用，否则会无限递归；
* 若没有显式定义拷贝构造函数，编译器会自己生成一个默认拷贝构造，默认的拷贝构造函数对象按内存存储和字节序完成拷贝，也叫浅拷贝；

```cpp
class Date
{
public:
	Date(int year, int month, int day)
		:
		_year(year),
		_month(month),
		_day(day)
	{}
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
	Date d1(2001, 7, 28);
	Date d2(d1);
	d1.Display();
	d2.Display();
	return 0;
}
```

> 输出：
>
> 2001-7-28
>
> 2001-7-28

* 那么对于那些直接管理着内存资源的类（含有指针变量），那么简单的值拷贝还顶得住吗？显然顶不住啊。

通过图示说明：

![image-20220521142159690](https://pic.xinsong.xyz/img/202205211421772.png)

两个string类的对象指向了同一块空间，这不就乱套了吗，如果其中一个对象通过指针改变了指向内存的数据，那么另一个对象也会受到影响，这是我们不愿发生的，我们希望每个对象都能独立运作。

下面这个程序会崩溃。

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
	String s1;
	String s2(s1);
	return 0;
}
```

原因是**两个string类的成员指针都指向一块内存**，而它们又分别调用了一次**析构函数**，相当于对同一块内存空间释放了两次，程序崩溃。

因此对于这种情况的对象，我们就不能再使用编译器生成的默认拷贝构造了，而只能自己去显式的定义拷贝构造并且要实现深拷贝。

### 编译器生成的拷贝构造

---

编译器默认生成的拷贝构造会做些什么呢？

* **对于内置类型成员**

​		完成值拷贝；

* **对于自定义类型成员**

  调用成员的拷贝构造；

```cpp
class Time
{
public:
	Time(int hour = 0, int minute = 0, int second = 0)
		:
		_hour(hour),
		_minute(minute),
		_second(second)
	{}
	Time(Time& t)
	{
		_hour = t._hour;
		_minute = t._minute;
		_second = t._second;
	}
private:
	int _hour;
	int _minute;
	int _second;
};
Time top(0, 1, 1);
class Date
{
public:
	Date(int year = 1900, int month = 1, int day = 1, Time& t = top)
		:
		_year(year),
		_month(month),
		_day(day),
		_t(t)
	{}
private:
	int _year;
	int _month;
	int _day;
	Time _t;
};
int main()
{
	Time t(1, 1, 1);
	Date d1(2001, 7, 28,t);
	Date d2(d1);
	return 0;
}
```

如果默认生成的拷贝构造没有调用Time类成员的拷贝构造，那么d2的`_t`的值应该是(_hour = 0, _minute = 0, _second = 0)，而这里最终的结果是d2中的`_t`和d2中的`_t`值相同。

这里Date类中自动生成的拷贝构造函数的内置类型会进行字节序拷贝，而对于自定义类型`_t`调用了Time的拷贝构造函数。

### 拷贝构造的初始化列表

---

拷贝构造是构造函数的一个重载，因此拷贝构造函数也是有初始化列表的，所以也建议在初始化列表阶段完成对对象的初始化，养成良好习惯。

**可以不显式定义拷贝构造函数的情况**

* 成员变量没有指针；
* 成员有指针，但并没有管理内存资源；

### 显式定义拷贝构造的误区

---

之前一直存在这个误区：

我们都知道，编译器生成的构造函数在初始化列表会调用成员的构造函数，而我们显式去定义构造函数时，即使我们不写也会在初始化列表去调用自定义类型成员的构造函数。

通过类比，我就犯了一个低级错误：

就是既然编译器生成的拷贝构造可以在初始化列表自动调用自定义成员的拷贝构造，那么我们显式定义的拷贝构造即使不写，也会在初始化列表自动去调用自定义成员的拷贝构造。

于是我写出了如下代码：

```cpp
class Time
{
public:
	Time(int hour = 0, int minute = 0, int second = 0)
		:
		_hour(hour),
		_minute(minute),
		_second(second)
	{}
	Time(Time& t)
	{
		_hour = t._hour;
		_minute = t._minute;
		_second = t._second;
	}
private:
	int _hour;
	int _minute;
	int _second;
};
Time top(2, 2, 2);
class Date
{
public:
	Date(int year = 1900, int month = 1, int day = 1, Time& t = top)
		:
		_year(year),
		_month(month),
		_day(day),
		_t(t)
	{}
	Date(Date& d)//显式定义了拷贝构造
	{

	}
private:
	int _year;
	int _month;
	int _day;
	Time _t;
};
int main()
{
	Time t(1, 1, 1);
	Date d1(2001, 7, 28,t);
	Date d2(d1);
	return 0;
}
```

通过监视窗口查看d2调用拷贝构造后的值：

![image-20220523142121481](https://pic.xinsong.xyz/img/202205231421625.png)

并没有拷贝成功。

我只顾着类比它们的功能，可我恰恰忽略了拷贝构造也是一种构造函数啊，那么自然初始化列表也是和普通构造一样，会去调用自定义类的构造函数，不处理内置类型。**只不过编译器生成的是经过处理的构造函数达到了拷贝的效果**。（太傻逼了这错误）

**结论**：

**拷贝构造函数是构造函数的一种**，它也有初始化列表，如果是**编译器生成的拷贝构造**，它会对**内置类型做字节序拷贝**，**对自定义类型成员会调用自定义成员的拷贝构造**。

可如果是我们**显式定义出的拷贝构造**，它也是有初始化列表的，但是它的初始化列表可不会去调用成员的拷贝构造奥，而是和普通构造函数一样，对于内置类型成员不去初始化值，对于自定义类型成员调用自定义成员的构造函数而不是拷贝构造函数。

