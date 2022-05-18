# 2021-11-10-

### 摘要

> C语言实现我们小时候玩过的扫雷游戏，最近看到了一些扫雷游戏的简单实现，但是总有功能上的缺失，玩起来不那么的“原汁原味”，因此我增加了一些新功能：
>
> 1. 确保玩家首次排雷一定不会炸死。
> 2. 加入了计时器记录结束时间。
> 3. 扩展式排雷，**展开周围的非雷区**。

### 总结

> 比较难的一点是扩展式排雷，使用用递归函数处理相对方便。

目录
---

[TOC]

------

## 难点解析

### 探索八区

探索排雷位的周围八个区域。

![image-20220518225507625](https://pic.xinsong.xyz/img/202205182255662.png)

总归情况就分三类，可探索的区域为8个，5个，3个。但这样分类实在麻烦，所以我们可以选择在创建雷盘的时候，将二维数组的维度扩大一些，使其不用考虑多种情况，而只用考虑探索周围八个雷区。

我们可以给**外侧再加一层**，即给二维数组行列分别加二，并且把外层全部设置为非雷区域，就可以解决这一问题。



### 递归展开

展开周围的非雷区

![image-20220518225514104](https://pic.xinsong.xyz/img/202205182255159.png)

**递归过程**：如果（x，y）位置周围八区的雷数为0，则从八个区域展开，展开的位置的 x坐标是从x-1到x+1，而 y 的位置是从y-1到y+1的范围中，因此嵌套两重循环。

**进入条件**：只有之前没有展开过，且坐标在雷盘内的位置才进入递归。

**终止条件**：如果探索的周围八个位置有雷，则停止，并让该位置显示雷的数量。

探索八区代码实现：

> 提醒：’*‘是未排雷的区域，’  ‘是代表已经展开过的区域。

```c
//计算排查位置处周围雷的个数
int calculate(char mine[ROWS][COLS],int x,int y)
{
	//因为雷的位置放的是字符‘1’
	// 加起来之后应该分别减去‘0’，才得到雷的个数
	return
		mine[x - 1][y - 1] +
		mine[x - 1][y] +
		mine[x - 1][y + 1] +
		mine[x][y - 1] +
		mine[x][y + 1] +
		mine[x + 1][y - 1] +
		mine[x + 1][y] +
		mine[x + 1][y + 1] - 8 * '0';
}
```

```c
//展开周围都没有雷的雷盘（扩展式排雷）
void expand(char mine[ROWS][COLS], char show[ROWS][COLS], int x, int y)//扩展函数
{	//利用calculate函数判断周围是否有雷
	if (calculate(mine, x, y) == 0) 
	{//判断周围雷的个数，若为0，则需要展开
		show[x][y] = ' ';//展开的位置都置为空格
		int i = 0;
		int j = 0;
		//该位置可以拓展才检查周围8个位置是否能拓展
		for (i = x - 1; i <= x + 1; i++)
		{
			for (j = y - 1; j <= y + 1; j++) 
			{
				if (show[i][j] == '*' && i > 0 && i <= ROW && j > 0 && j <= COL) 
				{
					//如果该位置未被扫过且在棋盘范围内则继续递归调用expand函数
					//再依次进入判断周围8个位置是能被展开还是不能
					expand(mine, show, i, j);
				}
			}
		}
	}
	else 
	{
		show[x][y] = calculate(mine, x, y)+'0';
		//不需要展开则显示附近雷的个数
	}
}

```

<strong style="color:#7030a0;">这两个函数结合起来使用便可以达到扩展展开的效果。</strong>

**使用效果：**

![image-20220518225521802](https://pic.xinsong.xyz/img/202205182255859.png)



可以看到在选择（5，6）后一片非雷区被展开了，并且边缘部分的雷个数被打印在了相应位置。

由于实在找不到什么好看的符号代替’  ‘，看着可能会有点难受，欢迎评论区给出建议！

## 完整源码

**我将代码分为了`test.c`、`game.h`、`game.c`三个部分。**

`test.c`是游戏实现的主体框架。

`game.h`是所用到的头文件以及自定义函数声明。

`game.c`是游戏的具体实现模块。

除了上面的递归有些难度外，其他的都比较易懂，不再单独阐述，下面的源码中我给出了每一步的注释，解释的很清楚，相信各位一边看代码一边想会有更多的收获。

### test.c

```c
#include"game.h"

int main()
{
	srand((unsigned)time(NULL));
	int input = 0;
	do
	{
		menu();//菜单
		printf("请输入->(1 / 0)\n");
		scanf("%d", &input);
		switch (input)
		{
		case 1:
			game();//选择1就进入game
			break;
		case 0://选择0就退出
			printf("退出游戏！\n");
			break;
		default:
			printf("输入错误，请重新输入：\n");
			break;
		}
	} while (input);
		return 0;
}

```



### game.h

```c
#pragma once
#include<Windows.h>
#include<stdio.h>
//时间戳函数头文件
#include<time.h>
//rand、srand函数头文件
#include<stdlib.h>
#define ROW 9//雷的区域的行数
#define COL 9//雷的区域的列数
#define ROWS ROW+2//数组的一维大小
#define COLS COL+2//数组的二维大小
#define NUM 50//雷的个数
//进入游戏
void game();
//打印菜单
void menu();
//计时器
void set_time();
//初始化数组
void init_array(char array[ROWS][COLS], int rows, int cols, char symbol);
//布置雷
void lay_mines(char mine[ROWS][COLS], int row, int col);
//打印
void show_interface(char show[ROWS][COLS], int row, int col);
//排查雷
void mine_detection(char mine[ROWS][COLS], char show[ROWS][COLS], int row, int col);
//计算雷的个数
int calculate(char mine[ROWS][COLS], int x, int y);
//保证第一次安全排雷
int one_safe(char mine[ROWS][COLS], int x, int y);
//扩展式排雷以及记录周围的雷数量
void expand(char mine[ROWS][COLS], char show[ROWS][COLS], int x, int y);
```



### game.c

```c
#include"game.h"

//游戏流程
void game()
{	
	system("cls");//清屏
	//创建两个数组
	char mine[ROWS][COLS] = { 0 };//布置雷的数组
	char show[ROWS][COLS] = { 0 };//游戏界面的显示雷个数的数组
	//初始化两个数组
	init_array(mine, ROWS, COLS, '0');//初始化为‘0’
	init_array(show, ROWS, COLS, '*');//初始化为‘*’
	//布置雷
	lay_mines(mine, ROW, COL);
	//打印出show数组
	show_interface(show, ROW, COL);
	//排查雷
	mine_detection(mine, show, ROW, COL);
	set_time();
}

//菜单函数
void menu()
{
	printf("***************************\n");
	printf("*   ****** MENU *******   *\n");
	printf("*   ******1.play*******   *\n");
	printf("*   ******0.exit*******   *\n");
	printf("*   *******************   *\n");
	printf("***************************\n");
}

//计时函数，
void set_time()
{
    //打印出从程序运行到目前所用的时间
	printf("本次用时：%u s\n", clock() / CLOCKS_PER_SEC);
}

//初始化数组
void init_array(char array[ROWS][COLS], int rows, int cols, char symbol)
{
	int i = 0;
	int j = 0;
	for (i = 0; i < rows; i++)
	{
		for (j = 0; j < cols; j++)
		{
			array[i][j] = symbol;
		}
	}
}

//布置地雷
void lay_mines(char mine[ROWS][COLS],int row,int col)
{
	int count = NUM;//NUM为雷的个数
	while (count)//循环条件，每次count-1
	{
		int x = rand() % row + 1;
		int y = rand() % col + 1;
		if (mine[x][y] == '0')
		{
			mine[x][y] = '1';
			count--;
		}
	}
}

//展示游戏界面
void show_interface(char show[ROWS][COLS], int row, int col)
{
	int i = 0;
	int j = 0;
	for (i = 0; i <= col; i++)//打印出列号
	{
		printf("%d ", i);
	}
	printf("\n");
	for (i = 0; i <= col; i++)
	{
		printf("—");
	}
	printf("\n");
	for (i = 1; i <= row; i++)
	{
		printf("%d|", i);//打印出行号
		for (j = 1; j <= col; j++)
		{
			printf("%c ", show[i][j]);
		}
		printf("\n");
	}
	for (i = 0; i <= col; i++)
	{
		printf("—");
	}
	printf("\n");
}

//排查雷
void mine_detection(char mine[ROWS][COLS],char show[ROWS][COLS], int row, int col)
{
	int cnt = 0;//cnt为已排查出的雷的个数
	int x = 0;
	int y = 0;
	while (cnt < ROW * COL - NUM)//循环条件是还有雷没有排查
	{
		int ret = 0;
		//输入要排查雷的坐标
		printf("请输入要排查的坐标：(X,Y)\n");
		scanf("%d %d", &x, &y);
		if (cnt == 0)//第一次排雷
		{
			if (x >= 1 && x <= row && y >= 1 && y <= col)
			{
				//首次输入的坐标如果是雷，则将雷移动到另一个位置
				one_safe(mine, x, y);//保证第一次总不是雷
				//因为无论如何都不是雷，直接跳转到记录雷和扩展的函数expand
				goto next;
			}
			else
			{	
				printf("坐标非法，请重新输入：\n");
				continue;
			}
		}
		//确保坐标在设定的范围中
		if (x >= 1 && x <= row && y >= 1 && y <= col)
		{
			if (mine[x][y] == '0')
			{
			next://goto跳转到这里
				expand(mine, show, x, y);
				//打印出排查过一次后的界面
				show_interface(show, ROW, COL);
				cnt++;//排雷成功次数加一
			}
			else
			{
				//如果排查的位置放的是‘1’则表示该位置为炸弹
				printf("很遗憾，您被炸死了!\n");
				//游戏结束，打印出所有雷的位置
				show_interface(mine, ROW, COL);
				break;
			}
		}
		else
		{
			printf("坐标非法，请重新输入：\n");
		}		
	}
	if (cnt == ROW * COL - NUM)//可排雷个数为0时
	{
		printf("恭喜您，排雷成功！\n");
		//游戏结束
		show_interface(mine, ROW, COL);		
	}
}

//计算排查位置处周围雷的个数
int calculate(char mine[ROWS][COLS],int x,int y)
{
	//因为雷的位置放的是字符‘1’
	// 加起来之后应该分别减去‘0’，才得到雷的个数
	return
		mine[x - 1][y - 1] +
		mine[x - 1][y] +
		mine[x - 1][y + 1] +
		mine[x][y - 1] +
		mine[x][y + 1] +
		mine[x + 1][y - 1] +
		mine[x + 1][y] +
		mine[x + 1][y + 1] - 8 * '0';
}

//保证第一次不会踩到雷
int one_safe(char mine[ROWS][COLS], int x, int y)
{
	int count = 1;
	//判断第一次是否是雷
	if (mine[x][y] == '1')
	{
		mine[x][y] = '0';
		//将雷随机放入另一个没有雷的位置
		while (count)
		{
			x = rand() % ROW + 1;
			y = rand() % COL + 1;
			if (mine[x][y] == '0')
			{
				mine[x][y] = '1';
				count--;
			}
		}
		return 1;//是雷则返回1
	}
	else
		return 0;//不是雷则返回0
	//其实这里可以返回值也可以不返回，因为在我最开始写代码时，最开始的思路是：
	//非雷 是雷这两种情况分别进入不同的分支，不过后来又换了一种思路
}

//展开周围都没有雷的雷盘（扩展式排雷）
void expand(char mine[ROWS][COLS], char show[ROWS][COLS], int x, int y)//扩展函数，判断是否需要递归式展开。
{	//利用calculate函数判断周围是否有雷
	if (calculate(mine, x, y) == 0) 
	{//判断周围雷的个数，若为0，则需要展开
		show[x][y] = ' ';//展开的位置都置为空格
		int i = 0;
		int j = 0;
		//该位置可以拓展才检查周围8个位置是否能拓展
		for (i = x - 1; i <= x + 1; i++)
		{
			for (j = y - 1; j <= y + 1; j++) 
			{
				if (show[i][j] == '*' && i > 0 && i <= ROW && j > 0 && j <= COL) 
				{
					//如果该位置未被扫过且在棋盘范围内则继续递归调用expand函数
					//再依次进入判断周围8个位置是能被展开还是不能
					expand(mine, show, i, j);
				}
			}
		}
	}
	else 
	{
		show[x][y] = calculate(mine, x, y)+'0';
		//不需要展开则显示附近雷的个数
	}
}

```

### 一点拓展

**计时**
运用clock函数,该函数需要的头文件为 `“time.h”`
函数原型：`clock_t clock(void);`
功能：**程序从启动到函数调用占用CPU的时间**
这个函数返回从“开启这个程序进程”到“程序中调用clock()函数”时之间的CPU时钟计时单元（clock tick）数，在MSDN中称之为挂钟时间；若挂钟时间不可取，则返回-1。其中clock_t是用来保存时间的数据类型。

```c
void set_time()//计时
{
    printf("用时:%u 秒\n", clock() / CLOCKS_PER_SEC);
}
```

如果觉得游戏太简单，可以通过改变ROW（行数）和COL（列数）的大小改变雷盘大小，增大NUM（雷的个数）增加游戏难度。



