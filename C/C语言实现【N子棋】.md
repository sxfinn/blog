# 2021-11-07-

### 摘要

> C语言实现一个大家小时候都玩过的小游戏的进阶版本，不止是三子棋，可以根据玩家需要设定棋盘大小。



目录
---

[TOC]

------

C语言实现一个大家小时候都玩过的小游戏的进阶版本，不止是三子棋，可以根据玩家需要设定棋盘大小。的可读性，我将源码分为了三个部分，分别是源文件`test.c`、`game.c`、`game.h`。

`test.c`部分是游戏进入、开始、结束的骨干代码。

`game.c`是游戏的具体如何实现的代码。

`game.h`是所有自定义函数以及库函数的头文件包含、函数声明以及宏定义。

<strong style="color:#7030a0;">可以根据需要自行设定ROW和COL的值达到改变棋盘大小以及胜利条件。</strong>

### 头文件

`game.h`

```c
#pragma once
#include<stdio.h>
#include<time.h>
#include<stdlib.h>
#include<Windows.h>

#define ROW 3   //棋盘的行数
#define COL 3	//棋盘的列数
//game函数
void game();
//打印简易游戏菜单
void menu();
//初始化棋盘
void init_board(char board[ROW][COL], int row, int col);
//打印棋盘
void print_board(char board[ROW][COL], int row, int col);
//电脑下棋
void computer_chess(char board[ROW][COL], int row, int col);
//玩家下棋
void player_playing_chess(char board[ROW][COL], int row, int col);
//判断是否有一方获胜
char judge_wins(char board[ROW][COL], int row, int col);
//判断是否已经平局
int is_full(char board[ROW][COL], int row, int col);
```



### 主函数

`test.c`

```c
#include"game.h"

int main()
{
    //播种由函数 rand 使用的随机数发生器
	srand((unsigned int)time(NULL));
    menu();
	int input = 0;
	do
	{
		printf("请输入->\n");
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
			printf("输入错误：\n");
			break;
		}
	} while(input);
	return 0;
}
```

### 具体实现游戏的函数

`game.c`

```c
#include"game.h"
void game()
{
	char board[ROW][COL] = { 0 };
	init_board(board, ROW, COL);
	print_board(board, ROW, COL);
	char ret = 0;
	while (1)
	{
		computer_chess(board, ROW, COL);
        //判断是否结束
		ret = judge_wins(board, ROW, COL);
		print_board(board, ROW, COL);
		if (ret)
			break;
		player_playing_chess(board, ROW, COL);
        //判断是否结束
		 ret = judge_wins(board, ROW, COL);
		 print_board(board, ROW, COL);
		if (ret)
			break;		
	}
    //结束后打印棋盘
    print_board(board, ROW, COL);
    //根据返回值ret判断胜利的一方
	if (ret == '#')
		printf("玩家胜利！\n");
	else if (ret == '*')
		printf("电脑胜利！\n");
	else if (ret == 'Y')
		printf("平局\n");
}
//菜单
void menu()
{
	printf("***************************\n");
	printf("*   ****** MENU *******   *\n");
	printf("*   ******1.play*******   *\n");
	printf("*   ******0.exit*******   *\n");
	printf("*   *******************   *\n");
	printf("***************************\n");
}
//初始化棋盘
void init_board(char board[ROW][COL],int row,int col)
{
	int i = 0;
	for (i = 0; i < row; ++i)
	{
		int j = 0;
		for (j = 0; j < col; ++j)
		{
			board[i][j] = ' ';
		}
	}
}
//打印棋盘
void print_board(char board[ROW][COL], int row, int col)
{
	int i = 0;
	for (i = 0; i < row; i++)
	{
		int j = 0;
		for (j = 0; j < col; j++)
		{
			printf(" %c ", board[i][j]);
			if(j < col - 1)
			printf("|");
		}
		printf("\n");
		if (i < row - 1)
		{
			for (j = 0; j < col; j++)
			{
				printf("---");
				if (j < col - 1)
					printf("|");
			}
			printf("\n");
		}
	}
}
//玩家下棋，玩家下棋记为 #
void player_playing_chess(char board[ROW][COL], int row, int col)
{
	int i = 0;
	int j = 0;
	printf("玩家下棋\n");
	printf("请输入坐标\n");
	while (1)
	{
		scanf("%d %d", &i, &j);
		if (i > 0 && i <= row && j > 0 && j <= col && board[i - 1][j - 1] == ' ')
		{
			board[i - 1][j - 1] = '#';
			break;
		}
		else
			printf("坐标非法，请重新输入：\n");
	}
}
//电脑下棋，记为 *
void computer_chess(char board[ROW][COL], int row, int col)
{
	printf("电脑下棋\n");
	while (1)
	{
		int i = rand() % row;//满足棋盘的坐标
		int j = rand() % col;
		if (board[i][j] == ' ')
		{
			board[i][j] = '*';
			break;
		}
	}
}
//判断是否有一方已经赢下对局
char judge_wins(char board[ROW][COL], int row, int col)
{
	int i = 0;
	int j = 0;
	char ret = 0;
	//检查行
	for (i = 0; i < row; ++i)
	{
		ret = 1;
		for (j = 0; j < col - 1; ++j)
		{
			if (board[i][j] == board[i][j + 1] && board[i][j] != ' ' )
				;
			else
			{
				ret = 0;
				break;
			}			
		}
		if (ret)
			return board[i][j];
	}
	//检查列
	for (j = 0; j < col; j++)
	{
		ret = 1;
		for (i = 0; i < row - 1; i++)
		{
			if (board[i][j] == board[i + 1][j] && board[i][j] != ' ')
				;
			else
			{
				ret = 0;
				break;
			}			
		}
		if (ret)
			return board[i][j];
	}
	//检查对角线
	ret = 1;
	for (i = 0,j = 0; i < row - 1 && j < col -1; i++,j++)
	{
		if (board[i][i] == board[i + 1][i + 1] && board[i][i] != ' ')
			;
		else
		{
			ret = 0;
			break;
		}
	}
	if (ret)
		return board[i][i];
	//检查对角线
	ret = 1;
	for (i = 0,j = col - 1; i < row - 1 && j > 0; i++,j--)
	{
		if (board[i][j] == board[i + 1][j - 1] && board[i][j] != ' ')
			;
		else
		{
			ret = 0;
			break;
		}
	}
	if (ret)
		return board[i][j];
	//检查是否平局
	if (is_full(board, row, col))
		return 'Y';
	//继续
	return 0;
}
//检查是否棋盘是否已满
int is_full(char board[ROW][COL], int row, int col)
{
	int i = 0;
	int j = 0;
	for (i = 0; i < row; i++)
	{
		for (j = 0; j < col; j++)
		{
			if (board[i][j] == ' ')
				return 0;
		}
	}
	return 1;
}
```

