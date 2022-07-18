## 类的6个默认成员函数

如果我们写了一个类，这个类我们只写了成员变量没有定义成员函数，那么这个类中就没有函数了吗？并不是的，在我们定义类时即使我们没有写任何成员函数，编译器会自动生成下面6个默认成员函数。

```cpp
class S
{
public:
	int _a;
};
```

![image-20220520200657259](https://pic.xinsong.xyz/img/202205202006307.png)



这里就来详细介绍一下构造函数。

### 构造函数

---

使用C语言，我们用结构体创建一个变量时，变量的内容都是随机值，要想要能正确的操作变量中存储的数据，我们还需要调用对应的初始化函数，给成员变量赋一个合适的初值。那么C++呢，我们仍然使用这个方法来试试。

```cpp
class Date
{
public:
	void SetDate(int year, int month, int day)
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
	Date d1, d2;
	d1.SetDate(2018, 5, 1);//初始化
	d1.Display();
	d2.SetDate(2018, 7, 1);//初始化
	d2.Display();
	return 0;
}
```

完全没问题的，毕竟C++完全兼容C嘛，不过这样子做未免有点麻烦，能不能做到我一创建好对象，对象的成员变量就是已经被初始化了而不是我们主动去调用呢？于是C++提供了一个特殊的函数：**构造函数**。

> **构造函数**是一个**特殊的成员函数**，**名字与类名相同，无返回值**，每次使用类实例化对象时会自动调用，保证每个数据成员都有一个合适的初值，方便我们的后续使用，构造函数在对象的生命周期内只会被调用一次。

#### 特性

虽然叫**构造函数**，但它的作用并不是构造一个对象（申请空间创建对象），而是初始化对象。

**特征如下**

* 函数名与类名相同；
* 无返回值；
* 对象实例化时编译器自动调用对应的构造函数；
* 构造函数可以重载；

```cpp
class Date
{
public:
	Date()//无参的构造函数
	{

	}
	Date(int year, int month, int day)//带参的构造函数
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
	Date d1;//调用无参的构造函数
	Date d2(2001, 7, 28);//调用带参的构造函数
	d1.Display();
	d2.Display();
    Date d3();//这种写法要不得，会被当做函数名为d3，无参、返回类型为Date的函数声明。
    //调用无参的构造函数，一定不要加()否则编译器无法识别这是一个函数声明还是调用的无参构造。
	return 0;
}
```

* 如果编译器没有显式的定义构造函数，编译器会自动生成一个**无参的默认构造函数**，一旦我们显式定义，编译器就不再生成；

```cpp
class Date
{
public:
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
	Date d1;//同样能创建一个对象
	d1.Display();
	return 0;
}
```

输出：

![image-20220520210908551](https://pic.xinsong.xyz/img/202205202109596.png)

可以看到使用编译器生成的默认构造函数我们的日期仍然是随机值。

* 无参的构造函数和全缺省的构造函数都称为默认构造函数，并且默认构造函数只能有一个。注意：无参构造函数、全缺省构造函数、我们没写编译器默认生成的构造函数，都可以认为是默认构造函数。

```cpp
class Date
{
public:
	Date()//无参的默认构造函数
	{

	}
	Date(int year = 2001, int month = 7, int day = 28)//全缺省的默认构造函数
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
	Date d1;//这里无法编译通过，因为调用不明确。
	d1.Display();
	return 0;
}
```

一般我们使用全缺省的构造函数，既可以不传参用缺省值去初始化对象，也可以显式地去调用并用实参初始化对象。

#### 编译器生成的默认构造函数

欸好像这货没什么用啊，我刚刚使用这个自动生成的，我的对象的初值还是随机值啊，看起来就像这构造函数什么事都没有做。

其实C++把类型分成**内置类型**(基本类型)和**自定义类型**。

内置类型就是语法已经定义好的类型：如int/char...，自定义类型就是我们使用class/struct/union自己定义的类型，看看下面的程序，就会发现编译器生成默认的构造函数会对自定类型成员_t调用的它的默认构造函数。

不过这涉及到我们还没学过的知识——初始化列表，后面会讲。

```cpp
class Time
{
public:
	Time(int hour = 0, int minute = 0, int second = 0)
	{
		cout << "Time(int hour = 0, int minute = 0, int second = 0)" << endl;
		_hour = hour;
		_minute = minute;
		_second = second;
	}
private:
	int _hour;
	int _minute;
	int _second;
};
class Date
{
public:
    //使用编译器默认的构造函数
	void Display()
	{
		cout << _year << "-" << _month << "-" << _day << endl;
	}
private:
    //内置类型
	int _year;
	int _month;
	int _day;
    //自定义类型
	Time _t;
};
int main()
{
	Date d1;
	d1.Display();
	return 0;
}
```

> 输出：
>
> Time(int hour = 0,int minute = 0,int second = 0)
>
> -858993460--858993460--858993460

事实上编译器生成的默认构造函数并不是什么都没有做，而是只处理了成员变量中的自定义类型，而没有去初始化内置类型，调用自定义类型成员的构造函数就是在初始化列表做的，下面会详细讲。

#### 成员变量的命名风格

看看下面这种成员命名方式有什么缺陷？

```cpp
class Time
{
public:
	Time(int hour = 0, int minute = 0, int second = 0)
	{
		cout << "Time(int hour = 0, int minute = 0, int second = 0)" << endl;
		hour = hour;
		minute = minute;
		second = second;
	}
private:
	int hour;
	int minute;
	int second;
};
```

简直太难看了，hour = hour这是什么操作？？？到底哪个hour是成员变量，让人去猜吗，代码实在丑陋，因此我们在对成员变量命名时，为了初始化对象不会因为发生命名冲突，又能一眼看出来哪个形参是对应初始化哪个成员变量的，我们通常在对类的成员变量命名方式统一成成员变量名前加上下划线_。

```cpp
class Time
{
public:
	Time(int hour = 0, int minute = 0, int second = 0)
	{
		cout << "Time(int hour = 0, int minute = 0, int second = 0)" << endl;
		_hour = hour;
		_minute = minute;
		_second = second;
	}
private:
	int _hour;
	int _minute;
	int _second;
};
```

当然不止是这样，适合自己的才是最好的，不过一般都是采用加一个前缀或者加上一个后缀的方式来命名成员变量。

这里很容易弄混两个概念，在此强调一下：

* **默认构造函数**是不需要参数的构造函数，有以下三种：
  1. 编译器生成的；
  2. 显式定义的无参的构造函数；
  3. 显式定义的全缺省的构造函数;

* **默认成员函数**是我们如果不写，编译器会自动生成的函数；

#### 构造函数的初始化列表

前面说了，在实例化一个类的对象时会自动去调用类的构造函数进行对象的初始化操作，那在C++中一个自定义类型的过程可分为两个过程：

1. 为对象分配内存；
2. 对成员变量赋值；
2. 函数体内赋值；

那我们想想如果成员变量是具有常属性的，那么是不是2过程就无法生效了？那么对于那些具有常性的变量我们以前是怎么定义的呢？初始化操作可以完成这个问题。

初始化是什么？变量在定义的同时为它设定一个初值，例如：**引用必须初始化**

`int& a = 10；`，这就是一个典型的初始化操作，而我们要谈的初始化列表也是相似的，那么这种初始化操作有什么与众不同的呢？

答案是：对于那些一旦有初值就不能再被赋值的变量，初始化列表的作用就体现出来了，例如被const修饰的变量，或者是引用，这些都是具有常属性的，因此就需要在它们创建的过程中就给它们一个初值，保证其可以被正常初始化。

```cpp
class S
{
public:
	S(int i = 0, int j = 0)
	{
		_i = i;
		_j = j;
	}
private:
	const int _i;//const修饰i具有常属性
	int _j;
};
int main()
{
	S s;
	return 0;
}
```

> 报错：
>
> “S::_i”: 必须初始化常量限定类型的对象
>
> error C2166: 左值指定 const 对象

即在编译器看来，这个对象包括对象中的成员变量在进入构造函数的函数体后，就都已经初始化完成了，因此const修饰的变量i就无法再被赋值了。

既然初始化列表是在创建变量阶段对变量进行的初始化，因此就可以使用初始化列表处理给那些无法修改的变量。

初始化列表格式如下：

```cpp
class S
{
public:
	S(int i = 0, int j = 0)
		:_i(i),
		_j(j)
	{}
private:
	const int _i;
	int _j;
};
int main()
{
	S s;
	return 0;
}
```

> 程序正常运行

同时呢，建议能使用初始化列表就使用，尽量不在构造函数的函数体中为成员变量再赋值，养成好的习惯。

**下面分析编译器生成的默认构造函数到底做了什么事情**

```cpp
class Time
{
public:
	Time(int hour = 0, int minute = 0, int second = 0)
		:_hour(hour),
		_minute(minute),
		_second(second)
	{
		cout << "Time(int hour = 0, int minute = 0, int second = 0)" << endl;
	}
	void DisPlay()
	{
		cout << _hour << "-" << _minute << "-" << _second << endl;
	}
private:
	int _hour;
	int _minute;
	int _second;
	
};
class Date
{
public:
	void Display()
	{
		cout << "Date:";
		cout << _year << "-" << _month << "-" << _day << endl;
		cout << "Time:";
		_t.DisPlay();
	}
private:
	int _year;
	int _month;
	int _day;
	Time _t;
};
int main()
{
	Date d1;
	d1.Display();
	return 0;
}
```

输出：

![image-20220521081818359](https://pic.xinsong.xyz/img/202205210818421.png)

可以看到，Date类中的内置类型都未被初始化，而对于自定义类型是去调用了其**默认构造函数**并且初始化成功了的。

这里要记住一定是调用的**自定义成员的默认构造函数**，因为编译器生成的Date的默认构造函数调用Time的构造时默认是不传参的，毕竟它也不知道传什么嘛。

如果我们把Time构造函数的缺省值去掉，那么Time就没有默认构造函数，那么创建Date对象时就无法调用Time的默认构造函数，就出错了。

如下：

```cpp
class Time
{
public:
	Time(int hour, int minute, int second)
		:_hour(hour),
		_minute(minute),
		_second(second)
	{
		cout << "Time(int hour = 1, int minute = 1, int second = 1)" << endl;
	}
	void DisPlay()
	{
		cout << _hour << "-" << _minute << "-" << _second << endl;
	}
private:
	int _hour;
	int _minute;
	int _second;
	
};
class Date
{
public:
	void Display()
	{
		cout << "Date:";
		cout << _year << "-" << _month << "-" << _day << endl;
		cout << "Time:";
		_t.DisPlay();
	}
private:
	int _year;
	int _month;
	int _day;
	Time _t;
};
int main()
{
	Date d1;
	d1.Display();
	return 0;
}
```

>  报错：
>
> message : “Date::Date(void)”: 由于 数据成员“Date::_t”不具备相应的 默认构造函数

那我们现在已经知道了编译器生成的默认构造函数它能做什么了，接下来我们显式地去定义一个构造函数。

```cpp
class Time
{
public:
	Time(int hour = 0, int minute = 0, int second = 0)
		:_hour(hour),
		_minute(minute),
		_second(second)
	{
		cout << "Time(int hour = 0, int minute = 0, int second = 0)" << endl;
	}
	void DisPlay()
	{
		cout << _hour << "-" << _minute << "-" << _second << endl;
	}
private:
	int _hour;
	int _minute;
	int _second;
	
};
class Date
{
public:
	Date()
		:
		_year(),//调用了int的默认构造函数，并且会给初始化为0
		_month(),
		_day(),
		_t()//调用了Time的默认构造函数，这里不写也会自动调用
	{
		cout << "Date()" << endl;
	}
	void Display()
	{
		cout << "Date:";
		cout << _year << "-" << _month << "-" << _day << endl;
		cout << "Time:";
		_t.DisPlay();
	}
private:
	int _year;
	int _month;
	int _day;
	Time _t;
};
int main()
{
	Date d1;
	d1.Display();
	return 0;
}
```

![image-20220521083205497](https://pic.xinsong.xyz/img/202205210832560.png)

我们为Date定义一个无参的默认构造函数，在初始化列表我不止处理了内置类型还处理了自定义类型。

在C++中支持这样一种定义变量的方法：

```cpp
	int a;//a是随机值
	int b(1);
	int();//创建匿名变量，调用默认构造
```

这和自定义类型的定义是一样的格式，这是为了让内置类型也能按照自定义类型的方式去定义和初始化，看起来是去调用了int的构造函数。

这样，我们在初始化列表中调用了初始化了内置类型和自定义类型，Date的内置类型也都被初始化为0了，这也印证了编译器生成的默认构造函数并没有去调用int的默认构造，而只调用了自定义类型的默认构造。

**注意**：就算我们显式的定义了构造函数，如果在初始化列表中不显式的调用Time的构造函数，那么编译器也会默认去调用它的默认构造（创建自定义类型成员变量时就一定会调用），而我们一旦显式的去调用了，那么走我们的调用。

C++中有这样一个特性，编译器能帮你做的，就算你不做它会自动帮你完成，而你一旦做了他就会按照你的方式去完成。

具体什么意思呢？看代码：

```cpp
class Time
{
public:
	Time(int hour = 0, int minute = 0, int second = 0)
		:_hour(hour),
		_minute(minute),
		_second(second)
	{
		cout << "Time(int hour = 0, int minute = 0, int second = 0)" << endl;
	}
	void DisPlay()
	{
		cout << _hour << "-" << _minute << "-" << _second << endl;
	}
private:
	int _hour;
	int _minute;
	int _second;
	
};
class Date
{
public:
	Date()
		:
	_t()//这里即使不写，编译器会自动去调
	{
		cout << "Date()" << endl;
	}
	void Display()
	{
		cout << "Date:";
		cout << _year << "-" << _month << "-" << _day << endl;
		cout << "Time:";
		_t.DisPlay();
	}
private:
	int _year;
	int _month;
	int _day;
	Time _t;
};
int main()
{
	Date d1;
	d1.Display();
	cout << int() << endl;
	return 0;
}
```

其实这个Date类的构造函数的初始化过程可以说就是编译器默认生成的构造相同了。

**总之就是编译器生成的默认构造函数会在初始化列表中调用成员中自定义类型的默认构造，而不会处理内置类型，不论是我们显式定义的还是编译器生成的，编译器都会去调用自定义类的默认构造函数，除非我们在初始化列表中显式的去调用了成员的构造函数。**

再言简意骇，就是**无论什么构造函数**（自动生成，显式定义）编译器都会在**初始化列表调用自定义类型成员的构造函数**，而如果我们自己显式调用了成员的构造，就执行我们所写的。

**可以不显式定义构造函数的情况**

* 如果成员变量都是自定义类型并且都不需要显式调用构造函数，那么编译器生成的默认构造函数就足以处理这种情况

​		其他情况都需要我们自己去定义构造函数。
