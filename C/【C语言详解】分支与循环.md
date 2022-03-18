# 2021-10-29-

作者：Nico
时间: `2021-10-25`
网站地址：https://github.com/sxfinn

> 

目录
---


[TOC]

## 💦什么是语句

C语句可分为以下五类：

* 表达式语句
* 函数调用语句
* 控制语句
* 复合语句
* 空语句

这里介绍的是控制语句。

控制语句用于控制程序的执行流程，以实现特定的功能和结构，它们具有特定的语句定义符组成，C语言有九种控制语句。

分为三类：

1. 分支语句：if、switch语句；
2. 循环语句：while、for、do while语句；
3. 转向语句：break、continue、goto、return语句；



## 💦分支语句(选择结构)

**if语句**

面对选择，我们会直接想或者说：我要XXX，我不要XXX。

举个例子：

面临以下选择

1. 好好学习—>收获满满
2. 无所事事—>继续废柴

可以看到二者是不可兼得的，只能选择其中的一个。



那么对于计算机呢？

if的语法结构：

```c
//一般结构
if(表达式)
	语句;
if(表达式)
	语句;
//多分支结构
if(表达式1)
	语句1;
else if(表达式2)
	语句2;
else
	语句;
```



**注意**:<strong style="color:#0070c0;">表达式</strong>的内容只看真假，可以是一个常数，可以是一个变量。

如下图：

![image-20220318205717181](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182057222.png)

由图可知：

若表达式结果为真，则对应语句执行。



那么C语言中如何表示真假？

C语言中 0为假，非0为真。



如果要执行的语句不止一条，那我们还需要将要执行的语句用大括号{}括起来。

例：

```c
#include<stdio.h>
//代码1
int main()
{
	int age = 0;
	scanf("%d", &age);
	if (age < 18)
	{
		printf("未成年\n");
	}
	else if (age > 18 && age < 60)
	{
		printf("壮年\n");
	}
	else if (age > 60)
	{
		printf("老年\n");
	}
	return 0;
}
代码2
int main()
{
	double grade_point = 0;//初始化绩点
	scanf("%lf", &grade_point);
	if (grade_point >= 4.0 && grade_point < 5.0)
	{
		printf("优秀\n");
	}
	else if (grade_point < 4.0 && grade_point >= 3.0)
	{
		printf("一般\n");
	}
	else
		printf("差\n");
	return 0;
}    
```

一个if与一个else匹配，如果有多个if或者else语句呢？

```c
int main()
{
	int a = 1;
	int b = 2;
	if (a == 5)
		if (b == 2)
			printf("true");
	else
		printf("false");
	return 0;
}
```

输出结果：

> 

无输出结果。

所以可以断定else是与第二个if匹配的。

可以看到上面代码的写法，很容易让我们产生误解，所以代码风格非常重要。

我们可以改写成以下形式：

```C
int main()
{
	int a = 1;
	int b = 2;
	if (a == 5)
    	{
		if (b == 2)
			printf("true");
    	}
		else
        	{
			printf("false");
        	}
	return 0;
}
```

<strong style="color:#7030a0;">**结论：else必须紧挨if后要执行的语句，且总是与最近且未匹配else的if匹配。**</strong>



**if语句的书写形式**

```c
	//代码1
	//函数体
if (1)
	{
		return 1;
	}
	return 2;
```

返回值结果：

> ​	1

上述代码给人模棱两可的感觉，表达不清晰，很容易产生误解。

我们来换一种写法

```c
	//代码2
	if (1)
	{
		return 1;
	}
	
	else
	{
		return 2;
	}
```

返回值结果：

>  1

显然，代码2的逻辑更加清晰，可读性更高。

<strong style="color:#002060;">**结论：代码风格很重要，可读性和逻辑要好。**</strong>



**练习：**

> 判断一个数是奇书还是偶数。

奇数：不可以被2整除的数。

偶数：可以被2整除的数。

能否被2整除就是我们的突破口，可以用此条件进行区分。

代码：

```c
int main()
{
	int i = 0;
	scanf("%d", &i);
	if (i % 2 == 0)
	{
		printf("%d是偶数\n", i);
	}
	else
	{
		printf("%d是奇数\n", i);
	}
	return 0;
}
```

> 输入：3
>
> 输出：3是奇数

> 输入：4
>
> 输出：4是偶数



分支语句（branch），如同河流的不同支流，分支少还好，但是当分支很多的情况呢？

我们再一个一个去写就太繁琐，于是switch语句的作用就体现出来了。

**switch语句**

通常是多分支的，有多种情况可以选择。

例如：我们想输出今天周几

> 输入1，输出周一
>
> 输入2，输出周二
>
> 输入3，输出周三
>
> 输入4，输出周四
>
> 输入5，输出周五
>
> 输入6，输出周六
>
> 输入7，输出周天

if语句实现：

```c
int main()
{
	int day = 0;
	scanf("%d", &day);
	if (day == 1)
		printf("周一");
	else if (day == 2)
		printf("周二");
	else if (day == 3)
		printf("周三");
	else if (day == 4)
		printf("周四");
	else if (day == 5)
		printf("周五");
	else if (day == 6)
		printf("周六");
	else if (day == 7)
		printf("周七");
	else
		printf("输入错误");
	return 0;
}
```

我们用if else语句实现看起来实在拖拉，且形式复杂。

使用switch语句呢？

```C
int main()
{
	int day = 0;
	scanf("%d", &day);
	switch (day)
	{
	case 1:
		printf("周一");
	case 2:
		printf("周二");
	case 3:
		printf("周三");
	case 4:
		printf("周四");
	case 5:
		printf("周五");
	case 6:
		printf("周六");
	case 7:
		printf("周天");		
	}
	return 0;
}
```

> 输入：5
>
> 输出：周五周六周天

与我们想要的似乎不太一样，我只想打印初周五，但是周六周天也打印出来了，可以看到，在执行完case 5后并没有跳出switch语句，而是继续执行接下来的语句，这显然不是分支。

如果我们想要执行完一个分支后立马跳出switch语句该怎么办？

**在switch语句中的break**

在switch语句中，我们没办法直接实现分支，必须依赖break与switch语句想结合，才能实现真正的分支。

```C
int main()
{
	int day = 0;
	scanf("%d", &day);
	switch (day)
	{
	case 1:
		printf("周一");
            break;
	case 2:
		printf("周二");
             break;
	case 3:
		printf("周三");
             break;
	case 4:
		printf("周四");
             break;
	case 5:
		printf("周五");
             break;
	case 6:
		printf("周六");
             break;
	case 7:
		printf("周天");	
             break;
	}
	return 0;
}
```

<strong style="color:#7030a0;">break语句</strong>的实际效果是把语句列表分成不同的分支，所有的分支都是并列的。

假如我们的需求变化，在输入1~5是输出工作日，在输入6和7后输出休息日。

> 思考：这还是分支吗？

```C
//代码1
int main()
{
	int day = 0;
	scanf("%d", &day);
	switch (day)
	{
	case 1:
		printf("工作日");
            break;
	case 2:
		printf("工作日");
             break;
	case 3:
		printf("工作日");
             break;
	case 4:
		printf("工作日");
             break;
	case 5:
		printf("工作日");
             break;
	case 6:
		printf("休息日");
             break;
	case 7:
		printf("休息日");	
             break;
	}
	return 0;
}
```

这样未免太麻烦了。

既然语句流会贯穿每一个`case`标签，那我们可以这样。

```C
int main()
{
	int day = 0;
	scanf("%d", &day);
	switch (day)
	{
	case 1:
	case 2:
	case 3:
	case 4:
	case 5:
		printf("工作日");
		break;
	case 6:
	case 7:
		printf("休息日");
		break;
	}
	return 0;
}
```

可以看到这仍然是分支，不过每一个分支的范围似乎不止是一个`case`，而是多个`case`组成的一个分支。

而将语句列表划分为每个分支的就是`break`。



switch语句一般形式：

```C
switch(type)
{
	case 1:
	case 2:
	case 3:
	default://默认实行语句
}
```

需要注意的几点：

- type只能为整数
- type为多少，就跳入case type分支，按照顺序执行
- case只决定从哪里进，不决定从哪里出
- case后面必须是整型常量
- 如果所有case都不能进入，则默认执行default，每个switch语句只能出现一个default子句

<strong style="color:#ffc000;"><strong style="color:#00b050;"><strong style="color:#00b0f0;">`default`</strong></strong></strong>：可以出现在switch语句中的任何位置，而且语句流会像贯穿case标签一样贯穿default子句，若想让`default`子句成为一个新的分支，则也需要加`break`。

<strong style="color:#002060;">编程好习惯</strong>：

* 最后一个`case`加上`break`

* 每个switch语句都放入一个`default`。



<strong style="color:#c00000;">注意：break只能在switch语句和循环语句中使用，且一个break只能跳出当前的循环或者switch语句</strong>。

**switch语句的嵌套**

例：

```C
int main()
{
	int m = 1;
	int n = 2;
	switch (n)//case 2
	{
	case 1:
		m++;
	case 2:
		n++;
	case 3:
		switch (m)
		{
		case 1:
			m++;
		case 2:
			n++;	
		}
	default:
		printf("结束\n");
	}
	printf("m=%d n=%d ", m, n);
	return 0;
}
```

> 输出：结束
>
> ​			m=4   n=2

<strong style="color:#7030a0;">**总结：分支语句就是将语句列表划分不同的分支，分支的大小是任意的，分支的内容也是任意的可以是一个语句，也可以是多条语句。**
</strong>
<strong style="color:#7030a0;">
</strong>

## 💦循环语句

生活中我们有每天重复循环的事情，C语言中也同样存在，当我们想重复做一件事情，那该怎么处理？

C语言当然可以实现：

* while
* for
* do while



**while循环**

我们已经了解了if语句：

```C
if(condition)
{
	语句;
}
```

当condition为真时，if后面的语句被执行，反之则不执行。

但是if后的语句只会被执行一次，假如我们想要他执行多次，那我怎么做呢？C语言给我们提供了while循环做到这件事。

```C
//while语法
while(condition)
{
    循环语句;
}
```

while循环流程图：

![image-20220318205817454](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182058493.png)

> 如何在屏幕上打印出1~10的数字？

难道要十个printf来输出吗？显然，我们拥有while后不再需要如此麻烦。

代码：

```C
int main()
{
	int i = 1;
	while (i <= 10)
	{
		printf("%d\n", i);
			i++;
	}
	return 0;
}

```

可以看到我们只用了一个printf，但是利用while循环做到了与十个printf的效果。

简单了解while工作流程我们接着看：

**while中的break和continue**

break介绍：

```C
#include<stdio.h>

int main()
{
	int i = 0;
	while (i < 10)
	{
		if (i == 5)
			break;
		printf("%d ", i);
		i++;
	}
	return 0;
}

```

运行结果：

> 0 1 2 3 4

可以看到在i == 5时遇到了break循环就停止了。

<span style="background:#FFDBBB;">**结论：break可以永久终止当前循环。**</span>

continue介绍：

```C
#include<stdio.h>
//代码1：
int main()
{
	int i = 0;
	while (i < 10)
	{
		if (i == 5)
			continue;
		printf("%d", i);
		i++;
	}
	return 0;
}
//代码2：
int main()
{
	int i = 0;
	while (i < 10)
	{
		i++;
		if (i == 5)
			continue;
		printf("%d ", i);
		
	}
	return 0;
}
```

代码1：

> 死循环

代码2：

> 输出： 1 2 3 4 6 7 8 9 

看起来continue似乎不像break那么暴力直接终止循环了，但是代码2的结果却少了一个5，代码一进入了死循环。

continue的作用：终止本次循环，直接跳到下一次的循环判断位置。

<strong style="color:#7030a0;">总结：break永久终止循环。
</strong><strong style="color:#7030a0;">
</strong>
<strong style="color:#7030a0;">			continue终止本次循环，进入下一次循环判断位置。</strong>

<span style="background:#bbffff;">
</span>

**getchar函数**

作用：在输入缓冲区读取一个字符、

`int getchar ( void );//函数原型`

**putchar函数**

作用：输出一个字符。

`int putchar ( int character );//函数原型`



输入缓冲区是什么呢？

可以理解为一块区域用来存储我们键盘输入的数据的这么一块区域。在输入操作时，如果缓冲区有内容就直接被scanf或者getchar读走，如果缓冲区没有内容，则等待用户输入。

示例：

```C
#include<stdio.h>

int main()
{	
	int ch = 1;
	scanf("%d", &ch);
	while ((ch = getchar())!=EOF)//将getchar得到的字符赋值给ch，判断是否与EOF相等
	{
		putchar(ch);
	}
	return 0;
}
```

> 输入：abc
>
> 输出：abc

**利用getchar清理输入缓冲区**

```C
int main()
{
	char arr[10] = { 0 };
	scanf("%s", arr);
	int ch = getchar();
	printf("%d", ch);
	return 0;
}
```

> 输入： abcd
>
> 输出：10

> 输入：zxcv
>
> 输出：10

无论输入什么输出都是10.

这是为什么呢？明明没有输入10，却每次都会在屏幕上输出一个10。

查阅ASCII码可以看到

![image-20220318205839706](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182058776.png)

10所对应的是换行符，也就是说getchar读到的是我们输入结束时敲入的回车。

于是我们面对这类问题时，可以利用getchar清理输入缓冲区。

```c
int main()
{
	int ch = 0;
	while ((ch = getchar()) != '\n')//读取到\n时就不再读取
	{
		;
	}
	return 0;
}
```

如果我们想只读取输入的某一类数据，该如何处理呢？

```C
#include<stdio.h>

int main()
{
	int ch = 0;
	while ((ch = getchar()) != EOF)
	{
        //只打印26个字母字符
		if (ch > 'a' && ch < 'z')
			putchar(ch);
	}
	return 0;
}
```

<strong style="color:#002060;">**总结：利用循环我们可以节省很多时间，处理一些步骤相同的问题时，循环可以迅速解决这类问题。**</strong>



**for循环**

既然已经有了while循环，为什么还会有for循环呢？

我们先看一看for循环的结构：

```C
for(exp1;exp2;exp3)
{
	循环语句;
}
```

* exp1为初始化部分，用于初始化循环变量。

* exp2为条件判断部分，用于判断循环执行条件。

* exp3为调整部分，用于调整循环变量。



> 打印出0~9的数字

我们使用while和for两种循环分别实现。

```C
#include<stdio.h>

int main()
{
	//代码1 while
	int i = 0;	//初始化部分
	while (i < 10)//判断部分
	{
		printf("%d ", i);
		i++;	//调整部分
	}
	return 0;
}
//代码2 for
int main()
{
	int i = 0;
	for (i = 0; i < 10; i++)
	{
		printf("%d ", i);
	}

	return 0;
}
```

可以看到代码1的初始化部分、条件判断部分和调整部分在for循环中都可以集成到一起，看起来更加简洁，使用起来更加方便。

for循环流程图

![img](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182058783.jpg)

**break和continue在for循环中**

作用与while基本相同，但有一些微小差异

```c
#include<stdio.h>
//break 
//代码1
int main()
{
    int i = 0;
    for (i = 0; i < 10; i++)
    {
        if (i == 5)
            break;
        printf("%d ", i);
    }
    return 0;
}
//continue
//代码2
int main()
{
    int i = 0;
    for (i = 0; i < 10; i++)
    {
        if (i == 5)
            continue;
        printf("%d ", i);
    }
    return 0;
}

```

代码1：

> 输出：0 1 2 3 4

代码2：

> 输出：0 1 2 3 4 6 7 8 9

与while中使用continue后死循环不同，这里在i==5时，continue被执行，直接跳入循环判断部分，并且i++，再次进入循环后 i !=5，于是程序正常进行下去。

<strong style="color:#002060;">**结论：continue跳过的是循环体部分而不包括条件判断部分。**</strong>



**for的循环控制变量**

建议：

> 1. 不要再循环体内部改变循环变量，防止循环失去控制
> 2. 建议循环控制变量的取值采用左闭右开区间

```C
#include<stdio.h>

int main()
{
	int i = 0;
	for (i = 0; i < 10; i++)//左闭右开更清爽
	{
		i = 1;
		printf("%d ", i);
	}
	return 0;
}//最终程序死循环了
```

**for循环的变种**

```C
#include<stdio.h>
//变种1
//省略了初始化部分
//条件判断部分
//调整部分
int main()
{
	int i = 0;
	for (;;)//没有判断条件默认恒为真
	{
		printf("hehe\n");
	}
	return 0;
}

```

运行结果：

> 重复的打印<span style="background:#bbffff;">hehe</span>

```C
//变种1
int main()
{
	int count = 0;
	int i = 0;
	int j = 0;
	for (i = 0; i < 10; i++)
	{
		for (j = 0; j < 10; j++)
		{
			printf("hehe\n");
			count++;
		}
	}
	printf("%d", count);
	return 0;
}
```

运行结果：

> 打印出了100个<span style="background:#bbffff;">hehe</span>

```c
//变种2
#include<stdio.h>

int main()
{
	int count = 0;
	int i = 0;
	int j = 0;
	for (; i < 10; i++)//未初始化变量
	{
		for (; j < 10; j++)//未初始化变量
		{
			printf("hehe\n");
			count++;
		}
	}
	printf("%d", count);
	return 0;
}

```

运行结果：

> 打印出了10个<span style="background:#bbffff;">hehe</span>

<strong style="color:#7030a0;">可以看到虽然只是小小的改变，但引起的效果却截然不同，所以省略for循环中的表达式要慎用！</strong>



**使用多个变量控制循环**

```c
#include<stdio.h>

int main()
{
	int x = 0;
	int y = 0;
	for (x = 0, y = 0; x < 10 && y < 5; x++, y++)
	{
		printf("haha\n");
	}
	return 0;
}
```

运行结果：

> 打印出5个<span style="background:#ffbbff;">haha</span>

**练习**

> 循环了多少次？

```C
#include<stdio.h>

int main()
{
	int i = 0;
	int j = 1;
	for (i = 0, j = 1; j = 2; j++)
		printf("haha\n");
	return 0;
}

```

> 答案：死循环

你做对了吗？



**do while()循环**

do while语句的用法

```c
do
{
    循环体语句;
}
    while(condition)
```

执行流程：

![Java中do while循环的流程图](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182059675.png)

**do while语句的特点**

无论是否满地条件判断，循环体至少会被执行一次。

```c
int main()
{
	int i = 10;
	do
	{
		printf("%d ", i);
	} while (i < 10);//不满足循环条件
	return 0;
}
```

运行结果：

> 10

**do while循环中的break和continue**

```c
#include<stdio.h>
//break
int main()
{
	int i = 0;
	do
	{
		if (i == 5)
			break;
		printf("%d ", i);
		i++;
	} while (i < 10);
	return 0;
}
```

输出结果：

> 0  1  2  3  4

```c
#include<stdio.h>

int main()
{
	int i = 0;
	do
	{
		if (i == 5)
			continue;
		printf("%d ", i);
		i++;
	} while (i < 10);
	return 0;
}
```

运行结果：

> 死循环

可以看到do while中的break和continue的作用与while循环的基本一致。

**练习**

> 在有序数组中查找一个数

```c
#include<stdio.h>
//二分法
int main()
{
	int arr[10] = { 1,2,3,4,5,6,7,8,9,10 };
	int left = 0;//要查找的范围内的左下标
	int right = sizeof(arr) / sizeof(arr[0]) - 1;//要查找范围内的右下标
	int k = 0;//要查找的数
	scanf("%d", &k);
	while (left <= right)
	{
		int mid = (left + right) / 2;//中间下标
		if (arr[mid] < k)//判断在哪个区间
		{
			left = mid + 1;
		}
		else if (arr[mid] > k)//判断在哪个区间
		{
			right = mid - 1;
		}
		else 
		{
			printf("found it！下标为%d", mid);
			break;
		}
	}
	return 0;
}
```

运行结果：

![image-20220318205959211](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182059258.png)

拥有了循环和判断，我们还可以实现一个猜数小游戏：

```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>

void menu()//菜单
{
	printf("***************************\n");
	printf("*******1.play 2.exit*******\n");
	printf("***************************\n");
}
//游戏进行
void game()
{
	int guess = 0;
	int i = (rand() % 100) + 1;	//生成随机数1~100
	//printf("%d\n", i);测试
	while (1)
	{
		printf("猜数：\n");
		scanf("%d", &guess);
		if (i < guess)
		{
			printf("猜大了\n");
		}
		else if (i > guess)
		{
			printf("猜小了\n");
		}
		else
		{
			printf("猜对了！\n");
			break;
		}
	}
}
int main()
{
	srand((unsigned int)time(NULL));//设置种子值为一个变量
	int input = 0;	
	do
	{
		menu();
		printf("请输入：->(1/0)");
		scanf("%d", &input);
		switch (input)
		{
		case 1:
			game();
			break;
		case 0:
			printf("退出游戏\n");
			break;
		default:
			printf("请重新输入：->(1/0)\n");
			break;
		}
	} while (input);
	return 0;
}
```

运行：

![image-20211030152129216](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182100450.png)



## 💦goto语句

C语言中提供了可以随意滥用的 goto语句和标记跳转的标号。

 从理论上 goto语句是没有必要的，实践中没有goto语句也可以很容易的写出代码。 但是某些场合下goto语句还是用得着的，最常见的用法就是终止程序在某些深度嵌套的结构的处理过程。

流程图：

![image-20211030154623595](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182100441.png)

>  例如：一次跳出两层或多层循环。 多层循环这种情况使用break是达不到目的的。它只能从最内层循环退出到上一层的循环。 goto语言真正适合的场景如下：



```c
#include<stdio.h>
//跳出多重循环
int main()
{
	int i = 0;
	int j = 0;
	int k = 0;
	for (i = 0; i < 10; i++)
	{
		for (j = 0; j < 10; j++)
		{
			for (k = 0; k < 10; k++)
			{
				...
				循环语句;
                goto out;
				...
			}
		}
	}
    out：
        语句;
	return 0;
}
```

**goto代替循环**

```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
    char input[10] = { 0 };
    system("shutdown -s -t 60");
    
    again:
        printf("电脑将在1分钟内关机，如果输入：“不要关机”则取消关机\n 请输入:>");
        scanf("%s", input);
        if (0 == strcmp(input, "不要关机"))
        {
            system("shutdown -a");
            break;
        }
    else
    {
        goto again;//跳转到again
    }
    
    return 0;
}
```



**总结：**
<strong style="color:#002060;">
</strong>

*  **在明确知道循环的次数时使用for循环**

*  **在循环体无论如何都至少会被执行一次时使用do while循环**

*  **其他情况使用while循环**

*  **goto语句尽量不使用**

