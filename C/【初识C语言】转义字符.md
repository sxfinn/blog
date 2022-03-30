

# 2021-11-02-

### 摘要
> 

### 总结
> 

目录
---
[TOC]

------

------

### 💦字符串

* **字符串是什么**

> 例：”hello world！“

 这种被双引号<font color=red>” “</font>引起来的一串字符叫做字符串字面值，简称字符串。

* **注意**：在我的上一篇文章已经讲过，字符串的结束标志是一个‘\0'的转义字符，在字符串的末尾被省略了。

* **字符串的存储方式**

>我们都知道，一个字符是储存在变量中的。
>那字符串储存在哪里呢？
>接下来介绍一下在C语言中存储字符串的两种方式，上代码：

```c
int main()
{
	char arr1[] = "hehe";
	char arr2[] = { 'h','e','h','e' };//字符挨个连续地储存到数组中
	char arr3[] = { 'h','e','h','e' ,'\0' };
	char*arr4   = "hehe";	//”hehe“这样的字符串在程序中其实是首字符的地址
	printf("%s\n", arr1);
	printf("%s\n", arr2);
	printf("%s\n", arr3);
	printf("%s\n", arr4);
	return 0;
}
```

 输出结果如下：

![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182048102.png)



可以看到arr1和arr3的输出结果是相同的，但是arr2的结果就让人看不懂了，虽然 =="hehe"==仍在屏幕上打印出来了，但后面跟了”烫烫烫烫hehe“的乱码。

**思考**：**出现这种情况的原因是什么呢？**
我们来调试一下代码，如下：
![调试](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182048109)
**分析**：似乎他们的不同之处也就只有arr1的末尾比arr2多了一个‘\0'，而我们也联想到了另一个函数那就是strlen函数，在使用strlen函数时也出现了这样一个问题，strlen函数在返回arr2这样的数组的字符串长度时，总是返回的随机值。

这里就与printf打印字符串的方式有关了，printf在以%s的形式打印arr2时，不断的向后打印每一个字符在标准输出上，直到遇到'\0'才会停止操作。
这里附上一条详细介绍printf函数的链接。如下：
[**printf函数详解**](http://cplusplus.com/reference/cstdio/printf/?kw=printf)

>**牢记:字符串的结束标志是一个'\0'转义字符，一定不能忽略。**

### 💦转义字符

* 如果想打印出一个文件的路径，我们该写出什么样的代码？

>例：打印出 c:\code-c++\test.cpp

我们来尝试一下我们大概率首先想到的代码。如下：

```c
int main()
{
	printf("%s\n", "c:\code - c++\test.cpp");		
	return 0;
}
```

接下来看看运行结果：

![image-20220319085557567](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203190856665.png)


这似乎与我们想要的不太一样，我们来看看少了哪些东西。

**思考**：这里红色框框内就是没有打印出来分别的是<font color=purple>'\\'和'\t'</font>,这似乎两者都带着一个 =='\\ '== 字符，那这究竟是什么呢？
这就不得不提到转义字符，顾名思义就是转变含义。



| 转义字符                   | 释义                                                         |
| -------------------------- | :----------------------------------------------------------- |
| \\?                        | 在书写连续多个问号时，防止他们被解析成三字母词               |
| \\'                        | 用于表示字符常量 ’                                           |
| \\"                        | 用于表示一个字符串内部的双引号（将”的含义从双引号的一半”—>单纯的<font color=red> “ </font>符号） |
| \\\                        | 用于表示一个反斜杠，防止它被解释为一个转义字符               |
| \\a                        | 警告字符，蜂鸣                                               |
| \\b                        | 退格符                                                       |
| \\f                        | 进纸符                                                       |
| \\n                        | 换行                                                         |
| \\r                        | 回车                                                         |
| \\t                        | 制表符                                                       |
| \\v                        | 垂直制表符                                                   |
| \\ddd                      | ddd表示3个八进制的数字                                       |
| \\xdd                      | dd表示两个十六进制的数字                                     |
|                            |                                                              |
| 接下来我们来解决两个问题： |                                                              |

>问题一：如何在屏幕上打印<font color =purple>一个单引号 '</font>
>问题二：如何在屏幕上打印一个字符串，字符串内容是<font color =purple>一个双引号 "</font>

通常在不了解转义字符的情况下，我们会写出如下代码：

```c
int main()
{
	printf("%c", "'");
	print("%c", """);
	return 0;
}
```

可以看到已经报错了这里已经报错了，错误如下图：


![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182050225.png)
既然这样不行，我们结合一下上面的转义字符表再来尝试
![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182050221.png)
接着我们修改一下代码，如下：
![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182050240)
可以看到VS报的错误已经没有了，那我们来试试是否可以编译呢？
![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182050249.png)

成功在屏幕上打印出了 ==\'和 \"==。
回到刚刚打印路径的问题：

>**例**：打印出 c:\code-c++\test.cpp

**分析**：根据上面的表格

![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182050889.png)
我们可以看到这里的<font color=purple> '\\t' 和 '\\\\' </font>都是转义字符'\'，因此我们如果想要在屏幕上打印出<font color=purple> '\\t' 和 '\\\' </font>就必须防止<font color=purple>‘\’</font>被解释为转义字符前面的那个<font color=red>'\\'</font>.
因此，我们利用 '\\\\' 这个转义字符的作用，在'\t'和'\\'前再加一个'\\'。
如下图：
![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182050888.png)



显然，我们达成了目的，与我们想要的结果一致。
为了巩固这一重要却容易被忽视的知识点，我们再来做一个题：

```c
int main()
{
	printf("%d\n", strlen("nihao"));
	printf("%d\n", strlen("c\768code-c++\test"));	
	return 0;
}
```

>先来回顾strlen的功能：strlen求得的是字符串的字符个数。

答案是多少呢？
先让我们来分析分析，除了转义字符外，其他字符都是单独存在的。
<font color=blue>"c\768code-c++\test" </font>这个字符串中有两个<font color=purple> '\\' </font>，我们来重点看看这两个地方：
![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182050900.png)
![在这里插入图片描述](E:\C语言笔记\assets\【初识C语言】转义字符\180fe06bb3014b53808383f88f74f858.png)

>结合转义字符表：

![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182051714.png)
这样一对比是不是觉得很清晰了呢？
显然，我们可以看到 =='\t' 和'\76'== 是两个转义字符。
到了这里又有一个新的疑问：

**思考：'\768'是不是转义字符呢？**
![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182051723.png)

>注意哦，这里的8不是八进制数字，所以'\768'不是一个转义字符


这里的\ddd中的d只能是八进制的数字，这也是一个大家非常容易忽略的地方。
分析结束后，我们就可以开始数字符个数啦！

运行结果：
![在这里插入图片描述](https://raw.githubusercontent.com/sxfinn/picgo/master/img/202203182051738)

>答案是15
>
>大家数对了吗？
>**小结：转义字符也是字符，大小为1个byte。**

### 💦注释

 - **注释方式**

 1. C语言的注释风格：
    /* xxxxxxxxxxx */

     > **缺陷**:不能嵌套注释

 2. C++风格的注释风格：
    //**********

    >**特点**：可以注释一行也可以注释多行

下一次将会梳理条件语句和循环语句的内容，大家记得点个赞！！

