# 2022-04-02-

### 摘要
> 实现简易通讯录，拥有增删查改功能，排序功能，动态通讯录，保存功能。

### 总结
> 

目录
---
[TOC]

------

**简单功能展示**

![image-20220518225558651](https://pic.xinsong.xyz/img/202205182255770.png)

**增加联系人功能。**



![image-20220518225605310](https://pic.xinsong.xyz/img/202205182256376.png)



**按照姓名排序功能。**



![image-20220518225612398](https://pic.xinsong.xyz/img/202205182256461.png)



**保存文件，重新启动重新加载功能。**

### 头文件`contact.h`

```c
//文件保存版
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<assert.h>
#include<errno.h>
#define MAXNAME 20
#define MAXNUMBER 20
#define MAXADDR 20
#define MAXSIZE 100
#define DEFAULT_SZ 3
typedef struct Information
{
	char name[MAXNAME];
	char number[MAXNUMBER];
	int age;
	char address[MAXADDR];
}Information;

typedef struct Contact
{
	Information* data;
	int sz;
	int capacity;
}Contact;

void menu();
void Init(Contact* addrBook);
void show(Contact* addrBook);
void Add(Contact* addrBook);
void Del(Contact* addrBook);
int Find(Contact* addrBook, char name[MAXNAME]);
void Search(Contact* addrBook);
void Modify(Contact* addrBook);
void Destroy(Contact* addrBook);
int cmp(const void* e1, const void* e2);
void sort(Contact* addrBook);
void Save(Contact* addBook);
void reload(Contact* addrBook);
```

### 函数体源文件`contact.c`

```c
#include"contact.h"

void menu()
{
	printf("***************************\n");
	printf("*******1.add     2.del ****\n");
	printf("*******3.search  4.modify *\n");
	printf("*******5.show    0.exit****\n");
	printf("*******   6.sort **********\n");
}
void Init(Contact* addrBook)
{
	Information* tmp = (Information*)malloc(sizeof(Information) * DEFAULT_SZ);
	if (tmp != NULL)
	{
		addrBook->data = tmp;
	}
	else
	{
		printf("%s\n", strerror(errno));
		return;
	}
	addrBook->capacity = DEFAULT_SZ;
	addrBook->sz = 0;
}
void reload(Contact* addrBook)
{
	FILE* pf = fopen("contact.txt", "rb");
	if (pf == NULL)
	{
		printf("%s", strerror(errno));
		return;
	}
	Information* tmp = NULL;
	Information buf = { 0 };
	while (fread(&buf, sizeof(Information), 1, pf) != 0)
	{
		if (addrBook->sz == addrBook->capacity)
		{
			tmp = realloc(addrBook->data, sizeof(Information) * (addrBook->capacity + 2));
			if (tmp != NULL)
			{
				addrBook->data = tmp;
				addrBook->capacity += 2;
				printf("扩容成功\n");
			}
			else
			{
				printf("%s", strerror(errno));
				return;
			}
		}
		addrBook->data[addrBook->sz] = buf;
		addrBook->sz++;
	}
	printf("加载成功\n");
	fclose(pf);
	pf = NULL;
}
void show(Contact* addrBook)
{
	printf("%-15s\t", "姓名");
	printf("%-15s\t", "电话");
	printf("%-15s\t", "年龄");
	printf("%-15s\t", "地址");
	printf("\n");
	for (int i = 0; i < addrBook->sz; i++)
	{
		printf("%-15s\t", addrBook->data[i].name);
		printf("%-15s\t", addrBook->data[i].number);
		printf("%-15d\t", addrBook->data[i].age);
		printf("%-15s\t", addrBook->data[i].address);
		printf("\n");
	}
}

void Add(Contact* addrBook)
{
	if (addrBook->sz == addrBook->capacity)
	{
		Information* tmp = (Information*)realloc(addrBook->data, (addrBook->capacity + 2) * sizeof(Information));
		if (tmp != NULL)
		{
			addrBook->data = tmp;
		}
		else
		{
			printf("%s\n", strerror(errno));
			return;
		}
		addrBook->capacity += 2;
		printf("扩容成功\n");
	}
	printf("请输入姓名\n");
	scanf("%s", addrBook->data[addrBook->sz].name);
	printf("请输入电话\n");
	scanf("%s", addrBook->data[addrBook->sz].number);
	printf("请输入年龄\n");
	scanf("%d", &addrBook->data[addrBook->sz].age);
	printf("请输入地址\n");
	scanf("%s", addrBook->data[addrBook->sz].address);
	addrBook->sz++;
	printf("添加联系人成功\n");
}

int Find(Contact* addrBook,char name[MAXNAME])
{
	for (int i = 0; i < addrBook->sz; i++)
	{
		if (strcmp(addrBook->data[i].name, name) == 0)
			return i;
	}
	return -1;
}

void Del(Contact* addrBook)
{
	if (addrBook->sz == 0)
	{
		printf("通讯录为空\n");
		return;
	}
	char name[MAXNAME] = { 0 };
	printf("请输入要删除的人的名字\n");
	scanf("%s", name);
	int pos = Find(addrBook, name);
	if (pos == -1)
	{
		printf("查无此人\n");
		return;
	}
	for (int i = pos; i < addrBook->sz - 1; i++)
	{
		addrBook->data[i] = addrBook->data[i + 1];
	}
	addrBook->sz--;
	printf("删除成功\n");
}

void Search(Contact* addrBook)
{
	if (addrBook->sz == 0)
	{
		printf("通讯录为空\n");
		return;
	}
	char name[MAXNAME] = { 0 };
	printf("请输入要查找的人的姓名\n");
	scanf("%s", name);
	int pos = Find(addrBook, name);
	if (pos == -1)
	{
		printf("查无此人\n");
		return;
	}
	printf("%-15s\t", "姓名");
	printf("%-15s\t", "电话");
	printf("%-15s\t", "年龄");
	printf("%-15s\t", "地址");
	printf("\n");
	printf("%-15s\t", addrBook->data[pos].name);
	printf("%-15s\t", addrBook->data[pos].number);
	printf("%-15d\t", addrBook->data[pos].age);
	printf("%-15s\t", addrBook->data[pos].address);
	printf("\n");
}

void Modify(Contact* addrBook)
{
	if (addrBook->sz == 0)
	{
		printf("通讯录为空\n");
		return;
	}
	char name[MAXNAME] = { 0 };
	printf("请输入要修改的人的名字\n");
	scanf("%s", name);
	int pos = Find(addrBook, name);
	if (pos == -1)
	{
		printf("查无此人\n");
		return;
	}
	printf("请输入姓名\n");
	scanf("%s", addrBook->data[pos].name);
	printf("请输入电话\n");
	scanf("%s", addrBook->data[pos].number);
	printf("请输入年龄\n");
	scanf("%d", &addrBook->data[pos].age);
	printf("请输入地址\n");
	scanf("%s", addrBook->data[pos].address);
	printf("修改成功\n");
}

void Destroy(Contact* addrBook)
{
	free(addrBook->data);
	addrBook->data = NULL;
	addrBook->capacity = 0;
	addrBook->sz = 0;
}
int cmp(const void* e1, const void* e2)
{
	struct Information* a = (struct Information*)e1;
	struct Information* b = (struct Information*)e2;
	return strcmp(a->name, b->name);
}

void sort(Contact* addrBook)
{
	qsort(addrBook->data, addrBook->sz, sizeof(struct Information), cmp);
}

void Save(Contact* addrBook)
{
	FILE* pf = fopen("contact.txt", "wb");
	if (pf == NULL)
	{
		printf("%s", strerror(errno));
		return;
	}
	fwrite((void*)addrBook->data, sizeof(Information), addrBook->sz, pf);
	fclose(pf);
	pf = NULL;
}


```



### 主函数源文件`test.c`

```c
#include"contact.h"
int main()
{
	int input = 0;
	Contact addrBook;
	Init(&addrBook);
	reload(&addrBook);
	do
	{
		menu();
		printf("请输入\n");
		scanf("%d", &input);
		switch (input)
		{
		case 0:
			Save(&addrBook);
			Destroy(&addrBook);
			printf("退出\n");
			break;
		case 1:
			Add(&addrBook);
			break;
		case 2:
			Del(&addrBook);
			break;
		case 3:
			Search(&addrBook);
			break;
		case 4:
			Modify(&addrBook);
			break;
		case 5:
			show(&addrBook);
			break;
		case 6:
			sort(&addrBook);
			break;
		default:
			printf("输入错误，请重新输入\n");
			break;
		}
	} while (input);
	return 0;
}
```

