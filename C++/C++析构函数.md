## 析构函数

既然在创建对象时有构造函数（给成员初始化），那么在销毁对象时应该还有一个清除成员变量数据的操作咯。

### 概念

---

> 析构函数：与构造函数功能相反，析构函数不是完成对象的销毁，局部对象销毁工作是由编译器完成的。而**对象在销毁时会自动调用析构函数，完成类的一些资源清理工作。**

### 特性

---

**析构函数**是特殊的成员函数

特征如下：

* 析构函数名是~类名；
* 无参数无返回值；
* 一个类有且只有一个析构函数；
* 对象声明周期结束，编译器自动调用析构函数；

```cpp
class Stack
{
public:
	Stack(int capacity = 4)
		:
		_size(0),
		_capacity(capacity),
		_p(new int[_capacity])
	{
		cout << "Stack(int capacity = 4)" << endl;
	}
	~Stack()
	{
		cout << "~Stack()" << endl;
		if (_p)
		{
			delete[](_p);
            _p = nullptr;
		}
		_size = _capacity = 0;
	}
private:	
	int _capacity;
	int _size;
	int* _p;
};
int main()
{
	Stack s;
	return 0;//程序结束，调用s的析构函数
}
```

输出：

![image-20220521103923527](https://pic.xinsong.xyz/img/202205211039573.png)

* 关于编译器自动生成的析构函数，是否会完成一些事情呢？下面的程序我们会看到，编译器生成的析构函数，对会自定类型成员调用它的析构函数。

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
class Person
{
public:
	Person()
		:
		_age(20),
		_name()
	{
		cout << "Person()" << endl;
	}
	~Person()
	{
		cout << "~Person()" << endl;
	}
private:
	String _name;
	int _age;
};
int main()
{
	Person p;
	return 0;
}
```

输出：

![image-20220521105501976](https://pic.xinsong.xyz/img/202205211055029.png)

析构函数在程序即将结束时，调用了Person的析构函数，在Person类的析构函数即将结束接着调用String类的析构函数。



**归纳一下**：

**析构函数**是与**构造函数**执行相对的操作的，构造函数负责给对象成员变量赋值并加载资源，而析构函数则是给对象的成员变量清理资源，而不是**清理对象本身**。

### 编译器生成的默认析构函数

---

编译器默认生成的析构函数能做些什么工作呢？我们前面已经介绍了编译器生成的构造函数会去只会处理自定义类型的成员变量，那么析构既然和构造相对应，析构也应该是只去处理自定义类型的成员变量吧，确实如此，析构函数不会对内置类型有任何处理，只会在调用自身的析构后再去调用自定义类型成员的析构。

内置类型处不处理其实都无所谓嘛，反正都要归还给操作系统，但是有例外：

![image-20220521133655349](https://pic.xinsong.xyz/img/202205211336416.png)

如果成员变量含有指针，并且指针指向一块我们正使用的空间，指针也是内置类型，那如果不释放指针指向的那块空间就会造成内存泄漏，而编译器生成的析构函数是不会处理此情况的，因为需要我们在析构函数中主动释放内存，也就是说需要我们显式的去定义析构函数。

```cpp
class Stack
{
public:
	Stack(int capacity = 4)
		:
		_size(0),
		_capacity(capacity),
		_p(new int[_capacity])//使用new去申请内存
	{
		cout << "Stack(int capacity = 4)" << endl;
	}
	~Stack()
	{
		cout << "~Stack()" << endl;
		if (_p)
		{
			delete[](_p);//释放内存
            _p = nullptr;
		}
		_size = _capacity = 0;
	}
private:	
	int _capacity;
	int _size;
	int* _p;
};
int main()
{
	Stack s;
	return 0;//程序结束，调用s的析构函数
}
```

**可以不显式定义析构函数的情况**

* 类的成员都是自定义类型的；
* 类的成员都是非指针的内置类型；

如果类的成员变量有指针类型，并且我们让指针指向了一块动态分配的空间，那么就需要我们自己写析构函数了。



**总结**：不是类直接管理另一块内存资源的，就不需要写析构函数，编译器自己生成的就能处理。









