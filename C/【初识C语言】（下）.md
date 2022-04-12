﻿# 2021-10-29-

### 摘要

> 常见关键字
>
> 指针
>
> 结构体

### 总结

> 

目录
---
[TOC]

------

------

### 💦常见关键字

| 关键字   | 作用                                                         |
| -------- | :----------------------------------------------------------- |
| auto     | 由auto修饰的自动变量                                         |
| break    | 跳出循环和switch分支语句                                     |
| case     | 分支语句                                                     |
| const    | 由const修饰的常变量                                          |
| continue | 跳过本次循环体开始下一次循环                                 |
| default  | 默认分支语句                                                 |
| enum     | 枚举关键                                                     |
| extern   | 引入外部符号                                                 |
| goto     | 改变函数内代码的执行顺序,跳转到函数内指定的标签地方运行,goto不能跨函数代码块跳转 |
| register | 寄存器                                                       |
| unsigned | 无符号数                                                     |
| struct   | 创建结构体类型                                               |
| switch   | 分支语句                                                     |
| typedef  | 类型重定义                                                   |
| union    | 创建联合体                                                   |
| void     | 空                                                           |
| volatile | 用volatile声明的变量表示该变量随时可能发生变化，与该变量有关的运算，不要进行编译优化，以免出错 |
| ..       |                                                              |

#### 关键字typedf

>typedef顾名思义是类型重定义，也可以理解为类型重命名。

说得再通俗一点就是给一个类型重新起个名字。
比如：

```c
#include<stdio.h>
//将unsigned重新命名为unsin，所以unsin也是一个类型名
typedef unsigned int unsin;
int main()
{
	//观察a和b，这两个变量的类型是一样的
	unsigned int a = 0;
	unsin b = 0;
	return 0;
}
```

#### 关键字static

>在C语言中，static是用来修饰变量和函数的

* 修饰局部变量-称为静态局部变量。
* 修饰全局变量-称为静态全局变量。
* 修饰函数-称为静态函数。


static修饰局部变量那究竟有什么作用呢？来对比下面两个代码。

```c
#include<stdio.h>
//代码1
void test()
{
	int i = 0;
	i++;
	printf("%d ", i);
}
int main()
{
	int i = 0;
	for (i = 0; i < 10; i++)
	{
		test();
	}
	return 0;
}
```

运行结果：
![image-20220412093220153](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120932186.png)
在屏幕上输出了10个1. 

```c
#include<stdio.h>
//代码2
void test()
{//这里将i用static修饰
	static int i = 0;
	i++;
	printf("%d ", i);
}
int main()
{
	int i = 0;
	for (i = 0; i < 10; i++)
	{
		test();
	}
	return 0;
}

```

运行结果：

![image-20220412093233880](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120932915.png)
输出了从1~10的数字。
对比代码1和代码2的效果理解static修饰局部变量的意义。

代码1：在没有static修饰的<font color=green>局部变量 i </font>中，执行程序一旦离开其被创建的那个块内，就自动销毁。

代码2：static修饰的<font color=green>局部变量 i </font> ,在程序运行到 i 所在的块时，不再重新创建，而是使用上一次保留的变量值，执行程序离开了块后也不会销毁，仍然保留，供下一次使用。



* 静态局部变量生命周期：从第一次被创建开始，一直到最后一次被调用为止，且每次调用函数时的静态变量不再初始化。

**小结：static修饰局部变量改变了变量的生命周期，让静态局部变量出了作用域依然存在，到程序结束，生命周期才结束**。

代码3：

```c
#include<stdio.h>
//代码3
void test()
{//这里将i用static修饰
	static int i = 1;
	i++;
	printf("%d ", i);
}
int main()
{
	int i = 0;
	for (i = 0; i < 10; i++)
	{
		test();
	}
	return 0;
}
```

![image-20220412093307790](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120933879.png)

有意思的是，通过调试我们可以看到：

在程序还没执行到test函数时，只要进入了该函数的函数体，static修饰的 i 就已经为1了，就像是一进来这个块，i 就已经被创建了，而不是等到 程序走到 static int i=1时才被创建，事实上赋值语句从来没有执行过。
（如果静态变量没有初始化，则默认为0.）



**小结：static修饰局部变量，一直存在于块内，不随着出作用域而消失，与全局变量有些相似之处**

* **static修饰全局变量**
  作用：改变了全局变量的作用域
  让静态的全局变量只能在自己的源文件内使用，而无法在源文件外部使用。
  全局变量作用域是整个工程，被static修饰后作用域就知识源文件内部了。

![image-20220412093318521](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120933552.png)
	上图是==global.c==源文件创建的全局变量。




==test.c==源文件代码：

```c
//test.c
#include<stdio.h>
extern n;
int main()
{
	printf("%d", n);
	return 0;
}
```

运行结果：
![image-20220412093327897](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120933931.png)
可见全局变量n已经无法使用。



**static修饰外部函数**
作用：改变了函数的外部链接属性。
普通函数具有外部链接属性，而静态函数没有外部连接属性。
也就是static修饰的函数也只能在所在的源文件内部使用。
![image-20220412093336959](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120933000.png)
上图为static修饰的外部函数。

代码：

```c
#include<stdio.h>
int  Add(int x, int y);
int main()
{
	int x = 3;
	int y = 5;
	int sum = Add(x, y);
	printf(" %d", sum);
	return 0;
}
```

运行结果：
![image-20220412093400840](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120934874.png)
可以看到这里是已经报错了。

**最后对static的三条作用做一句话总结：首先static的最主要功能是隐藏，其次因为static变量存放在静态存储区，所以它具备持久性和默认值0。**

### 💦#define定义常量和宏

```c
#include<stdio.h>
//#define定义标识符常量
#define MAX 100
//#define定义宏
# define BIG(x,y) (x>y?x:y)
int main()
{
	int max = BIG(2, 3);
	printf(" %d ", MAX);
	printf(" %d ", max);
	return 0;
}

```

一个细节：
![image-20220412093410447](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120934476.png)

注意：这里与函数的使用方式类似，注意区分。

### 💦指针

#### 内存

>内存是电脑上特别重要的存储器，计算机中程序的运行都是在内存中进行的 。
>所以为了有效的使用内存，就把内存划分成一个个小的内存单元，每个内存单元的大小是1个<font color=purple>字节</font>。
>为了能够有效的访问到内存的每个单元，就给内存单元进行了编号，这些编号被称为该<font color=purple>内存单元的地址。

![image-20220412093420247](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120934288.png)
变量是储存在内存中的（在内存中分配空间)，每个内存单元都有地址，所以变量也有地址。
取出变量地址如下：

```c
int main()
{
	//创建变量
	int i = 1;
	//&是取地址操作符，&i就是i的地址
	printf("%p", &i);//%p是以地址的方式打印
	return 0;
}
```

运行结果：
![image-20220412093433356](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120934387.png)



<font color=purple>注意：这里的 i 占四个字节，每个字节都有地址，取出的第一个字节的地址(较小的地址)。</font>
如下：
![image-20220412093444735](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120934783.png)

#### 指针变量

顾名思义：指针就是指向某一块<font color=green>内存空间</font>的。
那么有了地址，地址如何储存，需要定义指针变量。
代码：

```c
int main()
{
	int num = 10;
	int* p;//p为一个整型变量的指针
	p = &num;//赋值
	return 0;
}
```

**通过指针间接访问变量**
实例：

```c
#include<stdio.h>
int main()
{
	int num = 10;
	int* p = &num;//通过*p找到num的空间
	*p = 20;
	printf(" num=%d ", num);
	printf("*p=%d", *p);
	return 0;
}
```

运行结果：


![image-20220412093454807](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120934840.png)

**p和num在内存中的分布：**
![image-20220412093503524](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120935565.png)
以整型为例子，拓展在其他类型：
代码：

```c
#include<stdio.h>
int main()
{
	char ch = 'a';
	char* pc = &ch;
	*pc = 'h';
	printf("ch= %c ", ch);
	printf("*pc= %c ", *pc);
	return 0;
}
```

运行结果：
![image-20220412093515351](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120935386.png)
<font color=indigo>**小结：指针指向什么类型的变量，就定义什么类型的指针去接受该变量的地址**。

**指针变量的大小**

```c
#include<stdio.h>
	//指针变量的大小取决于地址的大小
	//32为平台上地址的大小是32bit
	//64位平台上地址的大小是64bit
int main()
{
	printf(" int*大小=%d\n", sizeof(int*));
	printf(" char*大小=%d\n", sizeof(char*));
	printf(" double*大小=%d\n", sizeof(double*));
	printf(" short*大小=%d\n", sizeof(short*));

	return 0;
}
```

<font color=red>**牢记：指针的大小和类型无关，只与平台有关**。

<font color=purple>**小结：在32位平台上是4个byte，在64位平台上是8个byte**。

#### 数组名：指针常量

**指针常量**
指针是形容词，常量是名词。这回是以常量为中心的一个偏正结构短语。那么，指针常量的本质是一个常量，而用指针修饰它，那么说明这个常量的值应该是一个指针，意为：指针中的<font color=darkred>常量</font>。

通常这样声明：

>int* const p ;
>
>因为const修饰的是指针p，即指针常量，<font color=purple>在声明的时候一定要给它赋初始值</font>。一旦赋值，以后这个常量再也<font color=purple>不能指向别的地址</font>。

**数组是指针常量**
如下：


![image-20220412093531006](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120935045.png)


虽然指针常量的值不能变，可是它指向的对象是可变的，因为我们并没有限制它指向的对象是常量。

下面的操作是可以的。

>例：  a[0] = 'x'; // 我们并没有限制a为常量指针（指向常量的指针）

或者

>*c[0] = 'x' // 与上面的操作一致

<font color=red>注意：指针常量和常量指针是不同概念。

**常量指针**
<font color=purple>常量</font>是形容词，<font color=midnightblue>指针</font>是名词，以指针为中心的一个偏正结构短语。这样看，常量指针本质是指针，<font color=purple>常量</font>修饰它，表示这个<font color=midnightblue>指针</font>乃是一个指向<font color=purple>常量</font>的指针（变量）。
简而言之：一个<font color=purple>常量</font>的<font color=midnightblue>指针</font>。
常量指针的声明：

       int const* p;
       const int* p;

这两种方式效果相同。

### 💦结构体

结构体是C语言中特别重要的知识点，结构体使得C语言有能力描述复杂类型。
比如描述学生，学生包含：

>  名字+年龄+性别+学号

 这几项信息,只用一个变量来描述显然不行。
这里只能使用结构体来描述了

 例如：


```c
#include<stdio.h>
//创建结构体类型
struct student
{
	char name[20];//名字
	int age;//年龄
	char sex[10];//性别
	char id[10];//学号
};
int main()
{
	struct student a = { "张三",20,"男","2020088" };//初始化结构体变量
	struct student* pa = &a;//将变量a的地址给结构体指针变量
	//使用.操作符访问结构成员，//打印结构体信息
	printf(" %s %d %s %s\n", a.name, a.age, a.sex, a.id);
	//->操作符，//打印结构体信息
	printf(" %s %d %s %s\n", pa->name, pa->age, pa->sex, pa->id);
	return 0;
}
```

运行结果：
![image-20220412093543010](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204120935044.png)



**总结：结构体是用来描述一个复杂变量的**。

