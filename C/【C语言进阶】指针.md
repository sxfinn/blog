# 2022-04-02-

### 摘要
> 指针的进阶
>
> 1. 字符指针
> 2. 数组指针
> 3. 指针数组
> 4. 数组传参和指针传参
> 5. 函数指针
> 6. 函数指针数组
> 7. 指向函数指针数组的指针
> 8. 回调函数
> 9. 指针和数组面试题的解析

### 总结

> 

目录
---
[TOC]

------

**指针的概念**

1. 指针是个变量，用来存储地址。
2. 指针的大小只与是64位平台还是32位平台有关，与指针类型无关。
3. 指针类型决定了指针的解引用权限和读取方式。
4. 指针+-正数与指针所指向类型数据的长度有关。



### 字符指针

在指针的类型中我们知道有一种指针类型为字符指针 `char*` ;
一般使用:

```c
int main()
{
  	char ch = 'w';
  	char *pc = &ch;
  	*pc = 'w';
  	return 0;
}
```

还有一种使用方式如下：

```c
int main()
{
  	const char* pstr = "hello sx.";//这里是把一个字符串放到pstr指针变量里了吗？
  	printf("%s\n", pstr);
  	return 0;
}
```

`const char* pstr = "hello sx.";`这段代码的意思实际是将"hello sx"的首地址赋值给pstr.，这样pstr就拥有了访问这个字符串的能力。

但很多人会陷入一个误区：认为"hello sx"字符串被整个放到了pstr中。

**像这样" "中放入字符直接写出来的字符串的我们叫做常量字符串，它存储在静态存储区，字符串的内容不能更改，并且只有在整个程序结束后，常量字符串所使用的内存空间才会被系统回收。**

所以使用直接收常量字符串的指针时，通常使用const修饰。

实际pstr与常量字符串的关系

![image-20220402142003630](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202204021420687.png)

看一道例题：

```c
#include <stdio.h>
int main()
{
  	char str1[] = "hello bit.";
  	char str2[] = "hello bit.";
  	const char *str3 = "hello bit.";
  	const char *str4 = "hello bit.";
  	if(str1 ==str2)
		printf("str1 and str2 are same\n");
  	else
		printf("str1 and str2 are not same\n");
  
  	if(str3 ==str4)
		printf("str3 and str4 are same\n");
  	else
		printf("str3 and str4 are not same\n");
  
  	return 0;
}
```

![image-20220402142156272](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202204021421307.png)



你可能感到奇怪，字符串明明时相同的，怎么str3、str4就不同了？

**实际上：**

```c
char arr[] = "hello sx";
//先创建一块空间，然后将字符串依次放入数组中。
```

这里str3和str4指向的是一个同一个常量字符串。C/C++会把常量字符串存储到单独的一个内存区域，当
几个指针。指向同一个字符串的时候，他们实际会指向同一块内存。但是用相同的常量字符串去初始化
不同的数组的时候就会开辟出不同的内存块。所以str1和str2不同，str3和str4不同。

**注意：同一个程序中出现的相同常量字符串都是同一个，类似与python将常量字符串存储在内存池中。**



### 指针数组

指针数组即元素类型为指针的数组。

我们可以使用指针数组模拟一个二维数组。

```c
int arr1[3] = {1,2,3};
int arr2[3] = {2,3,4};
int arr3[3] = {3,4,5};
int* arr[3] = {arr1, arr2, arr3};
如果想访问arr[i]中的第下标为j的元素，可以使用 *(*(arr + i) + j).
```

同样可以使用数组下标的方式 `arr[i] [j];`

但这只是模拟二维数组，并不是真正的二维数组。

二维数组的空间是连续的，可是在这里一定是连续的吗？

二维数组的每一行的大小是相同的，这里一定相同吗？

答案是否定的，这里的arr1 、 arr2 、 arr3的地址空间是随机的，长度也并没有固定。



### 数组指针

数组指针：意为数组的指针。

那么当然这是一个指针，它是指向数组的。

内置类型的指针：

例如 `int* p, double* p`,来剖析它的结构。

p被 \* 修饰代表p是指针，那么指向什么类型？==》int。

下面代码哪个是数组指针？

```c
int *p1[10];
int (*p2)[10];
//p1, p2分别是什么？
```

先来看看最普通的类型。

```c
int arr1[10];
//arr1与[10]先结合，代表arr1是个数组，那么剩下的就是数组元素的类型。

int *arr;
//arr与*结合，代表arr是个指针，那么剩下的就是指针指向的类型。
```

p1先与[10]结合，表示p是个数组。

将p1[10]去掉剩下的就是数组的元素类型。

显然，p1这是一个指针数组，数组的每个元素是指向`int`的指针。



p2先与\*结合，代表p2是个指针。

再与[10]结合，代表指向的是十个元素的数组，那么剩下的就是数组的元素类型了。

p2是一个指针，指向有十个元素，每个元素是`int`的数组。

```c
int (*p)[10];
//解释：p先和*结合，说明p是一个指针变量，然后指着指向的是一个大小为10个整型的数组。所以p是一个
指针，指向一个数组，叫数组指针。
//这里要注意：[]的优先级要高于*号的，所以必须加上（）来保证p先和*结合。
```

#### 对数组名取地址

```c
int arr[10];
```

`arr`和`&arr`有什么区别呢？

arr是数组名，首元素地址。

那`&arr`是个什么呢？

看代码：

```c
int main()
{
  	int arr[10] = {0};
  	printf("%p\n", arr);
  	printf("%p\n", &arr);
  	return 0;
}
```

![image-20220402145558743](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202204021455779.png)

两者的值是一样的，难道就一样吗？

继续看：

```c
int main()
{
	int arr[10] = { 0 };
	printf("arr = %p\n", arr);
	printf("&arr= %p\n", &arr);
	printf("arr+1 = %p\n", arr+1);
	printf("&arr+1= %p\n", &arr+1);
	return 0;
}
```

![image-20220402145722021](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202204021457056.png)

都执行 + 1操作后就不同了，那么说明它们指向的类型长度是有差异的。

可以看到 `&arr` 和 `&arr + 1` 相差了40个字节，是`arr`这个数组的长度。

说明&arr 指向的应该是一整个数组，因此一次跳过一个数组的长度，那么也就合理了。



#### 数组指针的使用

那数组指针是怎么使用的呢？
既然数组指针指向的是数组，那数组指针中存放的应该是数组的地址。

上代码：

```c
#include <stdio.h>
int main()
{
  	int arr[10] = {1,2,3,4,5,6,7,8,9,0};
  	int (*p)[10] = &arr;//把数组arr的地址赋值给数组指针变量p
  	return 0;
}
```

这样的用法多少显得我们有点不太聪明的样子，这根本无法体现数组指针的价值。

如下使用可以完美契合二维数组：

```c
#include <stdio.h>
void print_arr1(int arr[3][5], int row, int col)
{
  	int i = 0;
  	for(i=0; i<row; i++)
	{
    	for(j=0; j<col; j++)
   		{
      		printf("%d ", arr[i][j]);
   		}
   		printf("\n");
	}
}
void print_arr2(int (*arr)[5], int row, int col)
{
  	int i = 0;
  	for(i=0; i<row; i++)
 	{
    	for(j=0; j<col; j++)
   		{
      		printf("%d ", arr[i][j]);
   		}
    	printf("\n");
 	}
}
int main()
{
  	int arr[3][5] = {1,2,3,4,5,6,7,8,9,10};
 	print_arr1(arr, 3, 5);
  	//数组名arr，表示首元素的地址
  	//但是二维数组的首元素是二维数组的第一行
  	//所以这里传递的arr，其实相当于第一行的地址，是一维数组的地址
  	//可以数组指针来接收
  	print_arr2(arr, 3, 5);
  	return 0;
}
```

二维数组的数组名是首元素的地址，而二维数组的每一个元素实际上是一个数组，那么首数组的地址就是一个数组指针，可以在二维数组传参时使用。

那么`(arr + i)`就代表第`i + 1`行的地址，解引用后就是 第 `i + 1`行的数组名，然后就可以通过对一维数组的访问去访问元素了。自然就可以用 `arr[i] [j]`的方式去访问元素。

```c
int arr[5];//元素类型为5个int的数组
int *parr1[10];//指针数组，10个元素，每个元素都是指向int的指针。
int (*parr2)[10];//数组指针，指向元素个数5，每个元素为int的数组。
int (*parr3[10])[5];//
//数组指针数组，有十个元素，每个元素都是一个数组指针，指向元素个数为5的整形数组。
```



<strong style="color:#7030a0;">总结:一维数组的数组名既可以代表整个数组，也可以是首元素地址，通常是后者，只有在取地址和使用sizeof操作符时数组名代表整个数组。
</strong>
<strong style="color:#7030a0;">
</strong>

### 数组参数、指针参数

#### 一维数组传参

```c
#include <stdio.h>
void test(int arr[])//arr是一维数组的数组名，使用数组名接收，可以
{}
void test(int arr[10])//标明元素个数和不标名效果是一样的
{}
void test(int *arr)//可以指针接收首元素地址
{}
void test2(int *arr[20])//arr2是首元素地址，每个元素为一个一级指针，那么arr就是二级指针，可以
{}
void test2(int **arr)//与上一个相同
{}
int main()
{
	int arr[10] = {0};
	int *arr2[20] = {0};
	test(arr);
	test2(arr2);
}
```



#### 二维数组传参

来看看这样传参是否可以？

```c
void test(int arr[3][5])//使用二维数组接收二维数组，可以
{}
void test(int arr[][])	//不可以，没有指定列数，无法确定一行多少元素，就无法对指针进
    					//行运算一次跳过多少字节
{}
void test(int arr[][5])//可以，可以不指定行数，一定要指定列数
{}
//总结：二维数组传参，函数形参的设计只能省略第一个[]的数字。
//因为对一个二维数组，可以不知道有多少行，但是必须知道一行多少元素。
//这样才方便运算。
void test(int *arr)//不行，类型不匹配，二维数组名是数组指针，这里是一级指针
{}
void test(int* arr[5])//不行，类型不匹配
{}
void test(int (*arr)[5])//可以，二维数组数组名传参实际是一个数组指针
{}
void test(int **arr)//不行，类型不匹配，这是二级指针，无法接收数组指针
{}
int main()
{
	int arr[3][5] = {0};
	test(arr);
}
```

**总结：只能省略行，不能省略列，数组在作为参数传参时实际上为指针，若没有传列数给形参，就不知道数组指针可以时指向多大的数组（一行几个元素），就无法对其进行加减操作从而访问元素。**



#### 一级指针传参

```c
#include <stdio.h>
void print(int *p, int sz)
{
	int i = 0;
	for(i=0; i<sz; i++)
	{
		printf("%d\n", *(p+i));
	}
}
int main()
{
	int arr[10] = {1,2,3,4,5,6,7,8,9};
	int *p = arr;
	int sz = sizeof(arr)/sizeof(arr[0]);
	//一级指针p，传给函数
	print(p, sz);
	return 0;
}
```

函数形参为一级指针我们可以传入哪些参数呢？

1. 数组名
2. 一级指针
3. 字符串

比如：

```c
void test1(int *p)
{}
//test1函数能接收什么参数？
void test2(char* p)
{}
//test2函数能接收什么参数？
```

test1可以接收 `int`类型的数组， `int*` 的指针。

test2可以接收常量字符串， 字符数组的数组名，`char*`的指针



#### 二级指针传参

```c
#include <stdio.h>
void test(int** ptr)
{
	printf("num = %d\n", **ptr);
}
int main()
{
	int n = 10;
	int*p = &n;
	int **pp = &p;
	test(pp);
	test(&p);
	return 0;
}
```

来想象二级指针可以接收哪些类型的参数呢？

```c
void test(char **p)
{
}
int main()
{
	char c = 'b';
	char*pc = &c;
	char**ppc = &pc;
	char* arr[10];
	test(&pc);//这是指向char的指针
	test(ppc);//可以
	test(arr);//可以，arr为首元素地址，首元素为char*类型，那么它的类型就是char**
	return 0;
}
```



补充：

指针数组的元素是指针，是地址，将不同的指针变量放在相邻的一块地址空间中，数组名的类型是二级指针。

```c
int a = 0;
int** ppa = &(&a);
```

这样写对吗？

&a有地址吗？

很显然没有。



**一级指针**

```c
int* p;//整形指针
char* pc;//字符指针
void* pv;//空类型的指针
```

可以对一维数组进行传参。



**二级指针**

```c
char** p;
int** p;
```

可以对存放一级指针的数组传参。



**数组指针**

```c
int(*p)[3];
```

可以对二维数组进行传参。



**指针数组**

```c
int* p[10];
```

将指向`int`的指针，拷贝其值放到一些相邻的地址空间存储，可以对这些新元素进行访问，达到和原指针相同的效果。

### 函数指针

首先看一段代码：

```c
#include <stdio.h>
void test()
{
	printf("hehe\n");
}
int main()
{
	printf("%p\n", test);
	printf("%p\n", &test);
	return 0;
}
```

![image-20220402160244685](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202204021602732.png)

输出的是两个地址，这两个地址是 test 函数的地址。

既然&test和test是相同的，那就可以使用test的地址去直接调用函数，当然也可以介意用，不过有些脱裤子放屁，多此一举了。

```c
int (*pf)(int, int) = test;
//三种调用方式
pf(x,y);
(*pf)(x,y);
test(x,y);
```



那我们的函数的地址要想保存起来，怎么保存？
下面我们看代码：

```c
void test()
{
	printf("hehe\n");
}
//下面pfun1和pfun2哪个有能力存放test函数的地址？
void (*pfun1)();
void *pfun2();
```

分析分析：

首先一定是指针才能存储地址，那么就一定是pfun1咯。

pfun1先和\* 结合，表明它是指针，接着与()结合，表明指向的是函数，那么剩下的就是返回类型了为void。



那么pfun2是个什么呢？

pfun2先与()结合，表明它是函数，剩下的就是返回类型void。



**阅读两段有趣的代码**

在定义一个变量时，例如：

```c
int p;去掉变量名就是其类型int
int (*p)[3];去掉变量名就是其类型int(*)[3]
```



```c
//代码1
(*(void (*)())0)();
//代码2
void (*signal(int , void(*)(int)))(int);
```

代码1：

我们从0 和 *入手，搞清楚括号的位置。

可以看到void (*)()是一个函数指针类型，括号括起来实际是对0的强制类型转换，强制转换为一个函数地址，

然后再解引用拿到函数，通过函数调用操作符()去调用。

1. void (*p)()  函数指针类型
2. (void (*)())0  将强制类型转换为该函数类型的地址，会将0地址那块区域当作一个存储函数的地方
3. 解引用得到函数名
4. ()调用

代码2：

从标识符入手

1. signal和()结合，说明其为函数名
2. 函数的参数类型分别为`int`, `void(*)(int)`
3. 剩下的 `void (*)(int)`是返回类型，也是一个函数指针。

代码2的简化：

```c
typedef void(*pfun_t)(int);
pfun_t signal(int, pfun_t);
```

**总结：像这样的复杂类型通常从标识符入手，没有标识符就从*，[],操作符入手，逐层分析。**



### 函数指针数组

很明显函数指针数组是一个数组，它的元素类型都是函数的指针。

那要把函数的地址存到一个数组中，那这个数组就叫函数指针数组，那函数指针的数组如何定义呢？

1. 找个喜欢的标识符 例如 `pfarr`
2. 是个数组必然先与[]结合，`pfarr[]`
3. 数组元素类型是函数指针，任意写出来一个函数指针类型`int (*p)()`
4. 将`p`替换为`pfarr[]`,`int (*pfarr[])()`

结束。

函数指针数组的用途：**转移表**

例子：（计算器）

```c
#include <stdio.h>
int add(int a, int b)
{
	return a + b;
}
int sub(int a, int b)
{
	return a - b;
}
int mul(int a, int b)
{
	return a*b;
}
int div(int a, int b)
{
	return a / b;
}
int main()
{
	int x, y;
	int input = 1;
  	int ret = 0;
 	 do
 	{
    	printf( "*************************\n" );
    	printf( " 1:add      2:sub \n" );
    	printf( " 3:mul      4:div \n" );
    	printf( "*************************\n" );
    	printf( "请选择：" );
    	scanf( "%d", &input);
    	switch (input)
   		{
   		case 1:
       		printf( "输入操作数：" );
       		scanf( "%d %d", &x, &y);
       		ret = add(x, y);
       		printf( "ret = %d\n", ret);
       		break;
    	case 2:
       		printf( "输入操作数：" );
       		scanf( "%d %d", &x, &y);
       		ret = sub(x, y);
       		printf( "ret = %d\n", ret);
       		break;
    	case 3:
       		printf( "输入操作数：" );
       		scanf( "%d %d", &x, &y);
       		ret = mul(x, y);
       		printf( "ret = %d\n", ret);
       		break;
    	case 4:
       		printf( "输入操作数：" );
       		scanf( "%d %d", &x, &y);
       		ret = div(x, y);
       		printf( "ret = %d\n", ret);
       		break;
    	case 0:
        	printf("退出程序\n");
			breark;
    	default:
       		printf( "选择错误\n" );
       		break;
   		}
} while (input);
 
  	return 0;
}
```

使用函数指针数组的实现：

```c
#include <stdio.h>
int add(int a, int b)
{
	return a + b;
}
int sub(int a, int b)
{
	return a - b;
}
int mul(int a, int b)
{
	return a * b;
}
int div(int a, int b)
{
	return a / b;
}
int main()
{
	int x, y;
	int input = 1;
	int ret = 0;
	int(*p[5])(int x, int y) = { 0, add, sub, mul, div }; //转移表
	while (input)
	{
		printf("*************************\n");
		printf(" 1:add      2:sub \n");
		printf(" 3:mul      4:div \n");
		printf("*************************\n");
		printf("请选择：");
		scanf("%d", &input);
		if ((input <= 4 && input >= 1))
		{
			printf("输入操作数：");
			scanf("%d %d", &x, &y);
			ret = (*p[input])(x, y);
		}
		else
			printf("输入有误\n");
		printf("ret = %d\n", ret);
	}
	return 0;
}

```

函数的使用变的简单实用了许多。

**转移表作用：**

将多个函数地址存放到一个数组中，让这些函数地址存放在一个相邻的地址空间，这样在调用函数时，就可以通过访问数组元素，达到调用函数的效果。

**类型书写方式：**

对于某一个类型，我们在定义该类型的变量时如何写？那么定义存放该类型的数组也是相似的。

例：

```c
int (*p)[3];
那么创建一个该元素类型的数组直接在p后面加上[n]
int(*p[n])[3];
```

就定义完成了。

### 指向函数指针数组的指针

指向函数指针数组的指针是一个 指针
指针指向一个 数组 ，数组的元素都是 函数指针 ;
如何定义？

```c
void test(const char* str)
{
	printf("%s\n", str);
}
int main()
{
	//函数指针pfun
	void (*pfun)(const char*) = test;
	//函数指针的数组pfunArr
	void (*pfunArr[5])(const char* str);
	pfunArr[0] = test;
	//指向函数指针数组pfunArr的指针ppfunArr
	void (*(*ppfunArr)[5])(const char*) = &pfunArr;
	return 0;
}
```



### 回调函数

> 回调函数就是一个通过函数指针调用的函数。如果你把函数的指针（地址）作为参数传递给另一个
> 函数，当这个指针被用来调用其所指向的函数时，我们就说这是回调函数。回调函数不是由该函数
> 的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进
> 行响应。

作用：

```c
#include <stdio.h>
//qosrt函数的使用者得实现一个比较函数
int int_cmp(const void * p1, const void * p2)
{
 	return (*( int *)p1 - *(int *) p2);
}
int main()
{
  	int arr[] = { 1, 3, 5, 7, 9, 2, 4, 6, 8, 0 };
  	int i = 0;
 
  	qsort(arr, sizeof(arr) / sizeof(arr[0]), sizeof (int), int_cmp);
  	for (i = 0; i< sizeof(arr) / sizeof(arr[0]); i++)
 	{
   		printf( "%d ", arr[i]);
 	}
  	printf("\n");
  	return 0;
}
```

使用回调函数，模拟实现qsort（采用冒泡的方式）。
注意：这里第一次使用 void* 的指针，讲解 void* 的作用

```c
#include <stdio.h>
int int_cmp(const void* p1, const void* p2)
{
	return (*(int*)p1 - *(int*)p2);
}
void _swap(void* p1, void* p2, int size)
{
	int i = 0;
	for (i = 0; i < size; i++)
	{
		char tmp = *((char*)p1 + i);
		*((char*)p1 + i) = *((char*)p2 + i);
		*((char*)p2 + i) = tmp;
	}
}
void bubble(void* base, int count, int size, int(*cmp)(void*, void*))
{
	int i = 0;
	int j = 0; for (i = 0; i < count - 1; i++)
	{
		for (j = 0; j < count - i - 1; j++)
		{
			if (cmp((char*)base + j * size, (char*)base + (j + 1) * size) > 0)
			{
				_swap((char*)base + j * size, (char*)base + (j + 1) * size, size);
			}
		}
	}
}
int main()
{
	int arr[] = { 1, 3, 5, 7, 9, 2, 4, 6, 8, 0 };
	//char *arr[] = {"aaaa","dddd","cccc","bbbb"};
	int i = 0;
	bubble(arr, sizeof(arr) / sizeof(arr[0]), sizeof(int), int_cmp);
	for (i = 0; i < sizeof(arr) / sizeof(arr[0]); i++)
	{
		printf("%d ", arr[i]);
	}
	printf("\n");
	return 0;
}

```

**回调函数：**

将一个函数的指针作为参数传递给另一个函数，当这个指针被用来调用其所指向的函数，就是回调函数。

通常来说，一个函数只能完成一件事情，而利用函数回调，在不同情况下，对函数传入不同的其他函数的指针，则可以再一个函数内实现调用多个函数，完成多种功能，且如果这个函数只有一个函数指针，调用一次只实现某一特定函数功能。



函数名和函数地址作为实参传入另一个函数的话，只能用函数指针接收。

注意：传过去的函数必须与接受的函数指针所指向的类型相同（返回类型，参数类型和个数）。

void *类型无法解引用操作。

**指针章节技巧方法：**

在书写任何一种类型的变量都是有技巧的。

1. 书写函数返回类型，数组元素类型，指针指向类型。

2. 先创建一个该类型的变量。

3. 按照下列方式替代变量名

   * 返回类型 ——用函数名(参数)替代
   * 数组元素类型  ——用数组名[]替代
   * 指针指向类型——用(*p)替代

   

**总结：定义标识符p类型**

1. p先与[]结合，说明其为数组，剩下的就是元素类型
2. p先与\* 结合，说明其为指针，剩下的就是指向类型。
3. p先与\*结合再与[]结合，说明其为指针，指向数组，剩下的就是数组元素类型



* 写一个数组时，先写出数组格式`arr[]` ， 然后确定元素类型，创建一个该类型的变量，将变量名改为`arr[]`
* 写一个指针时，先写出(*p),
* * 如果指向数组，则(*p)[]， 然后确定数组类型，创建一个该类型变量，将变量名改为 ( *p)[]。
  * 如果指向非数组，则确定其指向类型，创建一个该类型变量，将变量名改为(*p)。

