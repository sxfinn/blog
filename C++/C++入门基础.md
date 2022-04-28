### C++关键字

---

C++关键字全集(参考 C++ Primer ):

| **asm**      | **auto**                  | **bad** _**cast** | **bad** _**typeid**  |
| ------------ | ------------------------- | ----------------- | -------------------- |
| **bool**     | **break**                 | **case**          | **catch**            |
| **char**     | **class**                 | **const**         | **const** _**cast**  |
| **continue** | **default**               | **delete**        | **do**               |
| **double**   | **dynamic** _**cast**     | **else**          | **enum**             |
| **except**   | **explicit**              | **extern**        | **false**            |
| **finally**  | **float**                 | **for**           | **friend**           |
| **goto**     | **if**                    | **inline**        | **int**              |
| **long**     | **mutable**               | **namespace**     | **new**              |
| **operator** | **private**               | **protected**     | **public**           |
| **register** | **reinterpret** _**cast** | **return**        | **short**            |
| **signed**   | **sizeof**                | **static**        | **static** _**cast** |
| **struct**   | **switch**                | **template**      | **this**             |
| **throw**    | **true**                  | **try**           | **type** _**info**   |
| **typedef**  | **typeid**                | **typename**      | **union**            |
| **unsigned** | **using**                 | **virtual**       | **void**             |
| **volatile** | **wchar_t**               | **while**         |                      |

### 命名空间

---

> 在C/C++中，变量、函数和后面要学到的类都是大量存在的，这些变量、函数和类的名称将都存在于全局作
> 用域中，可能会导致很多冲突。使用命名空间的目的是对标识符的名称进行本地化，以避免命名冲突或名字
> 污染，namespace关键字的出现就是针对这种问题的，std：c++标准库的命名空间。

在C语言中我们如果使用了同一个标识符定义了不同的函数或者是变量，会导致它们之间产生冲突，而C++为了解决这个问题，引入了命名空间的概念，不同命名空间的成员占有不同的内存空间，即使名称相同，但相互之间并不会受到影响。因此在C++中，库函数也是被定义在命名空间中的。

例如：C语言的头文件包含通常是 `#include<xxx.h>`，包含后我们便可以直接使用库函数，而在C++中我们的头文件通常是： `#include<xxx>`，并且无法直接使用库函数，必须要指定命名空间`std`才能使用。

不过C++是兼容C几乎所以语法的，因此我们可以在C++中穿插C的代码，不过有一些混用是很容易出错的，要小心并且正确的使用。

#### 命名空间定义

定义命名空间需要使用namespace关键字，后面跟上要定义的命名空间的名字，将命名空间成员定义在后面的{}内即可，类似于结构体和类的定义方式。

命名空间内可以定义变量，函数，类型，使用命名空间的类型定义出的变量不属于命名空间。

**一般的命名空间定义方式**

```c
namespace sx
{
	int a;
	void swap(int& a, int& b)
	{
		cout << " namespace:sx " << endl;
		int tmp = a;
		a = b;
		b = tmp;
	}
	struct Stu
	{
		char name[10];
		int age;
	};
}
```

**命名空间的嵌套定义**

```c
namespace sx
{
	int a;
	void swap(int& a, int& b)
	{
		cout << " namespace:sx " << endl;
		int tmp = a;
		a = b;
		b = tmp;
	}
	struct Stu
	{
		char name[10];
		int age;
	};
	namespace psm
	{
		int b;
		void print()
		{
			cout << "hello psm!" << endl;
		}
	}
}
```



**同一个工程中允许存在多个相同名称的命名空间,编译器最后会合成同一个命名空间中**

```c
namespace n1
{
	int a;
	void swap(int& a, int& b)
	{
		cout << "n1:swap" << endl;
	}
}
namespace n2
{
	int b;
}
namespace n1
{
	int c;
	void swap(int& a, int& b)
	{
		cout << "n1:swap" << endl;
	}
}
```

像这样子去定义编译时会报错：

> 函数“void n1::swap(int &,int &)”已有主体

删除其中一个即可正常编译，由此可见编译时两个名字相同的命名空间会合并，如果有重复的定义则会报错。

**注意：一个命名空间就定义了一个新的作用域，命名空间中的所有内容都局限于该命名空间中。**



#### 命名空间的使用

定义在命名空间中的变量、类型以及函数我们是无法直接使用的，由于命名空间就定义了一个新的作用域，而程序中默认是只使用两个作用域的内容的：

1. 全局作用域
2. 局部作用域

并且根据局部优先原则会程序会先检索当前作用域的内容，如果没有我们需要的，再到全局域去检索，所以默认情况下我们所定义的命名空间的作用域的内容我们是无法直接访问的。

比如：

```c
namespace sx
{
	int a;
	void swap(int& a, int& b)
	{
		cout << " namespace:sx " << endl;
		int tmp = a;
		a = b;
		b = tmp;
	}
	struct Stu
	{
		char name[10];
		int age;
	};
}
int main()
{
	cout << a << endl;//该语句编译出错，无法识别a
	return 0;
}
```

> 报错：“a”: 未声明的标识符



命名空间的使用方式有三种：

* 加命名空间名称及作用域限定符

```c
int main()
{
	cout << sx::a << endl;//指定使用在sx这个命名空间中的a
	return 0;
}
```

作用域限定符是临时的，因此每次使用时都需要加命名空间和作用域限定符（限定符限定的是成员的名称，因此限定符应紧挨着在成员名称的前面，例如：`sx::Stu s`

* 使用using将命名空间中成员引入至全局域

```c
using sx::a;//指定地将sx中的a引入

int main()
{
	cout << a << endl;	//可以使用a
    Stu s;				//无法使用Stu类型
	return 0;
}
```

这样只能使用指定引入的成员，并且引入时指定的应是**成员的名称**（变量名吗，函数名，类型名）。

* 使用using namespace 将命名空间中的成员引入至全局域

```c
using namespace sx;

int main()
{
	cout << a << endl;
	Stu s;
	return 0;
}
```

这种方法会将命名空间的所有成员一次性引入，可以直接访问其中所有的成员，平时我们可以这样使用，但是着违背了命名空间诞生的初衷，容易产生命名冲突的问题，因此在工程中通常是使用第一种或者第二种方法。

如何证明命名空间是被引入至全局域的呢？

```c
int a = 1;
using namespace sx;

int main()
{
	cout << a << endl;//“a”: 不明确的符号
	return 0;
}

using namespace sx;

int main()
{
	int a = 1;
	cout << a << endl;
	return 0;
}
```

第一个程序提示错误，而第二个程序正常运行。

引入至全局域后我们的程序即可在全局域中找到定义在命名空间`sx`中的变量 `a` ，而我们本身又在全局域中定义了一个变量 `a` ，那么自然如果不指定是哪个域中的也就产生了歧义，使得`a`变量名指代不明确。

可如果我们再次定义的`a`变量是局部的，即使命名空间中的`a`被引入至全局域，但并不会产生歧义，因为局部优先的原则，我们并不会去全局域中检索变量`a`，也就不存在命名冲突。

**注意：即使是引入至全局域，命名空间`sx`中的`a`和本身定义在全局的`a`是拥有各自的内存空间的，"引入"仅仅是让其在全局域中可被检索，它仍然是属于命名空间`sx`的，有点像环境变量，引入就像是将某个命令所在的路径添加至环境变量，环境变量路径中的命令是可以在计算机任何路径下使用的，然而其被使用的命令可能并不在当前路径，它们之间是相互独立的。**



#### 指定使用全局域中的内容：

```c
namespace sx
{
	int a;
	void swap(int& a, int& b)
	{
		int tmp = a;
		a = b;
		b = tmp;
	}
}
using namespace sx;

int main()
{
    int a = 1;
	cout << ::a << endl;//这里的a访问的是全局的
	return 0;
}
```

**即**：全局变量 a 表达为 **::a**，用于当有同名的局部变量时来区别两者。

命名空间是有一些比较坑的地方的，例如：

```c
//代码1
int a = 1;
namespace sx
{
	int a;
	void swap(int& a, int& b)
	{
		int tmp = a;
		a = b;
		b = tmp;
	}
}

using namespace sx;
int main()
{
	cout << ::a << endl;
	return 0;
}

//代码2
int a = 1;
namespace sx
{
	int a;
	void swap(int& a, int& b)
	{
		int tmp = a;
		a = b;
		b = tmp;
	}
}
using sx::a;
int main()
{
	cout << ::a << endl;
	return 0;
}
```

代码1可以正常运行，而代码2却显示a多次定义，个人觉得还是有些奇怪的，不过项目中我们并不会将命名空间展开，更不会有这样的写法。使命名空间变量具有与全局变量相同的名称**是错误的**（参考微软官方文档https://docs.microsoft.com/zh-cn/cpp/cpp/namespaces-cpp?view=msvc-170）。因此不用过于纠结这里的差异。

**总之，使用using将命名空间展开或者是引入成员，即代表着后续的代码可以使用此命名空间的成员。**



### C++输入输出

---

**向世界打个招呼！**

```c
#include<iostream>
using std::cout;

int main()
{
	cout << "hello world!" << endl;
	return 0;
}
```

> 1. 使用**cout标准输出(控制台)和cin标准输入(键盘)**时，必须**包含`< iostream >`头文件**以及std标准命名空
>    间。
>    注意：早期标准库将所有功能在全局域中实现，声明在.h后缀的头文件中，使用时只需包含对应头文件
>    即可，后来将其实现在std命名空间下，为了和C头文件区分，也为了正确使用命名空间，规定C++头文
>    件不带.h；旧编译器(vc 6.0)中还支持`<iostream.h>`格式，后续编译器已不支持，因此推荐使用
>    `<iostream> +std`的方式。
> 2. 使用C++输入输出更方便，它会自动识别类型（函数重载）而不需增加数据格式控制，比如：整形--%d，字符--%c

例如：

```c
#include<iostream>
//using namespace std;
using std::cout;
using std::endl;
using std::cin;

int main()
{
	int i = 1;
	double d = 1.1;
	cout << "i =" << i << ",d =" << d << endl;
	return 0;
}
```

但是C++的这样的输入输出方式在有些场景下使用会非常麻烦，而C语言就会很方便，例如左对齐右对齐或者是保留几位小数这样的场景，推荐使用C语言的输出方式`printf`函数。

### 缺省参数

---

缺省参数即可有可无的参数，像极了汽车备胎，带上备胎也能上路不带也不影响，除非运气实在太差。

缺省参数是**声明或定义函数时**为函数的参数**指定一个默认值**，在调用该函数时，如果没有指定实参则采用该默认值，否则使用指定的实参。

```c
void testfunc(int t = 10)
{
	cout << t << endl;
}

int main()
{
	testfunc(100);//传入100，就使用指定的实参
	testfunc();//没有形参，就使用默认的实参
	return 0;
}
```

#### 缺省参数的分类

* **全缺省参数**

​	即所有参数都有自己的默认值，传参时可以全部省略。

```c
void FAll(int x = 1, int y = 2, int z = 3)
{
	cout << x << y << z << endl;
}

int main()
{
	FAll();//全缺省
	return 0;
}
```



* **半缺省参数**

​	即只有部分参数都有自己的默认值，传参时一定需要传参。

```c
void FHalf(int x, int y = 10, int z = 30)
{
	cout << x << y << z << endl;
}
int main()
{
	FHalf(5);//半缺省
	return 0;
}
```

**注意：**

1. 半缺省参数只能依次从右到左且连续，因为形参是从左往右依次传给实参，所以必须保证没有默认值的实参一定能有形参传值给它。
2. 缺省参数不能在定义和声明中同时出现，以免给的默认值不同产生歧义。

```c
void Test(int a = 10);

void Test(int a = 20)//报错
{
	cout << a << endl;
}
```

3. 缺省值必须是常量或者是全局变量
4. C语言不支持



**注意：如果定义和声明分离，那么只能缺省在声明**

如果**缺省参数在定义**中，而声明没有，那么声明的头文件展开后，由于声明和定义在**不同的源文件中**，它们会先**分别编译**，那么包含定义的那个源文件在编译时编译器认为该函数是没有缺省参数的，但是该源文件函数的调用却没**有传入参数**，就发生了编译错误。



### 函数重载

---

在我们的中文中常常会有一词多义的情况，但是我们可以通过上下文来帮助我们判断并确定它所表达意义而不是让我们无法识别。

讲个笑话：

> 国有两个体育项目大家根本不用看，也不用担心。一个是乒乓球，一个是男足。前
> 者是“谁也赢不了！”，后者是“谁也赢不了！

那么一个相同的函数名我们想让它不只是有一种功能或者是不止能处理一种特定情况呢，函数重载可以帮助我们解决这个问题。

#### 函数重载的概念

> **函数重载**:是函数的一种特殊情况，C++允许在同一作用域中声明几个功能类似的同名函数，这些同名函数的
> **形参列表**(参数个数 或 类型 或 顺序)必须不同，常用来处理实现功能类似数据类型不同的问题

例如：

```c
int Add(int a, int b)
{
	cout << "int Add(int a, int b)" << endl;
	return a + b;
}
double Add(double a, double b)
{
	cout << "double Add(double a, double b)" << endl;
	return a + b;
}
float Add(float a, float b)
{
	cout << "float Add(float a, float b)" << endl;
	return a + b;
}

int main()
{
	int ret1 = Add(1, 2);
	int ret2 = Add(1.1, 2.2);
	int ret3 = Add((float)1.1, (float)2.2);
	return 0;
}
```

输出：

![image-20220427132225707](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204271322761.png)

相同的函数名传入不同类型的参数调用的函数实体不同。

**无法区分仅按返回类型区分的函数**

```c
short Add(short left, short right)
{
	return left + right;
}
int Add(short left, short right)
{
	return left + right;
}
```



解决一个功能相似的函数，函数需要多个函数名来区分的问题。



### 引用

---

别名：孙行者，这姓孙
