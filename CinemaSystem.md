---
title:Cinema System
---
# 电影院管理系统

## 主函数

```java
int main() 
{
	print1();          //欢迎界面
	Login();           //登陆函数
	return 0;
}
```

## 头文件

```java
#pragma once
#ifndef  Cinema_h
#define  Cinema_h
typedef struct accout
{
	char pass[20];
	char name[20];
	struct accout* next;
}Accout, * pAccout;

typedef struct ticket
{
	char number[15];
	char name[30];
	char cinema[30];
	int  time;
	double price;
	double grade;
	struct ticket *next;
}ticket, *pticket;

void Registe(pAccout ahead);   //注册账户 
void Login();          //登录函数
pAccout Account(pAccout ahead);    //读取账户信息 
void loginMenu();       //登录界面 
pAccout loginJudge(pAccout ahead);//登录判断 

pticket Create(pticket phead);  //声明创建链表
void Menu();         //管理员菜单 
void print();       //主菜单   
void print1();      //主界面
void print2();      //退出界面 

pticket sortMenu(pticket phead);     //排序子系统菜单  
pticket sort1(pticket phead);        //价格排序  
pticket sort2(pticket phead);        //时长排序
pticket sort11(pticket phead);
pticket sort12(pticket phead);

void printCinema1(pticket phead);//打印电影信息 
void printCinema2();//电影信息表格 

pticket insertCinema(pticket phead); //新电影信息录入 
pticket deleteCinema(pticket phead);   //删除信息 
pticket routerCinema(pticket phead);  //信息读取
pticket reviseCinema(pticket phead); //修改电影信息 
void SaveCinema(pticket phead);//信息保存 


int Judge(char choice);         //布尔判断 
int back(char* a);    //返回整数函数 
#endif




```



## 管理员模块

登陆成功后转入此模块

负责调用管理员功能函数

```java

void Menu()
{
	pticket phead;
	phead = (pticket)malloc(sizeof(ticket));
	phead->next = NULL;
	routerCinema(phead);
	char P, a[200];
	char choice;
	int bk;
	while (1)
	{
		system("cls");
		print();
		fflush(stdin);
		printf("请输入指令:");
		scanf("%s", a);
		bk = back(a);
		switch (bk)
		{
		case 1: {phead = insertCinema(phead);     //插入 
			system("cls");
			break; }
		case 2: {printCinema1(phead);  //查看 
			system("cls");
			break; }
		case 3: {phead = sortMenu(phead);    //排序 
			system("cls");
			break; }
		case 4: {phead = deleteCinema(phead); //删除 
			system("cls");
			break; }
		case 5: {phead = reviseCinema(phead); //修改 
			system("cls");
			break; }
		case 0: {
			print2();
			exit(1);
		}
		default: {printf("输入错误！请重新输入\n");
			system("pause"); }
		}
	}
}
```



### 注册模块


```java
void Registe(pAccout ahead)
{
	system("cls");
	char name[20];
	char password[13];
	char pass[13];
	char q;
	int i;
	FILE* fp;
loop2:
	printf("\n\n\n\n");
	printf("\t\t\t\t账号:");
	scanf("%s",name);
	getchar();
	printf("\t\t\t\t密码:");
	i = 0;
	while (1)
	{
		q = getch();
		if (q != 13)
		{
			printf("*");
			password[i++] = q;
		}
		else {
			password[i] = '\0';
			printf("\n");
			break;
		}
	}
	printf("\t\t\t\t请确认密码：");
	i = 0;
	while (1)
	{
		q = getch();
		if (q != 13)
		{
			printf("*");
			pass[i++] = q;
		}
		else {
			pass[i] = '\0';
			printf("\n");
			break;
		}
	}
	if (strcmp(pass, password) == 0)
	{
		fp = fopen("AdministratorsAccout.txt", "ab+");
		fprintf(fp, " %s %s", name, password);  //把内存中的文件输入到硬盘中
		fclose(fp);
		system("cls");
		printf("\n\n\n\n注册成功\n");
	}
	else goto loop2;
}
```

### 登录模块

登录函数主要用到与登录有关的三个函数

分别是

pAccout Account(pAccout ahead);    //读取账户信息 

void Login();          //登录函数

pAccout loginJudge(pAccout ahead);//登录判断 

(int Judge(char choice);         //布尔判断 
int back(char* a);    //返回整数函数 )

```java
pAccout Account(pAccout ahead)
{
	pAccout p1, p2;
	FILE* fp;
	int m = 0;
	if ((fp = fopen("AdministratorsAccout.txt", "rw")) == NULL)
	{
		printf("读取失败!");
		exit(0);
		return ahead;
	}
	ahead == NULL;
	p1 = (pAccout)malloc(sizeof(Accout));
	while (fscanf(fp, "%s %s", p1->name, p1->pass) == 2)
	{
		//printf("%s\n",p1->name);                  //读进去了 
		m = m + 1;
		if (m == 1)
		{
			ahead->next = p1;
			p2 = p1;
			p2->next = NULL;
		}
		else
		{
			p2->next = p1;
			p2 = p1;
			p2->next = NULL;
		}
		p1 = (pAccout)malloc(sizeof(Accout));
	}
	fclose(fp);
	return ahead;
}
void Login()
{
	system("cls");
	int bk;
	int bb;
	pAccout ahead, p1;
	ahead = (pAccout)malloc(sizeof(Accout));
	ahead->next = NULL;
	while (1)
	{
		char a[200];
		char b[200];
	loop:
		loginMenu();
		printf("请输入:");
		scanf("%s", a);
		bk = back(a);
		switch (bk)
		{
		case 1: {
				Account(ahead);
				ahead = loginJudge(ahead);//管理员登录 
				Menu();
				break;
		}
		case 2: {
			Registe(ahead);
			goto loop;
			break;
		}
		case 0: {
			//	printf("BUG！！！！\n");测试bug！！！！！ 
			print2();
			exit(0);
			break;
		}
		default:printf("输入错误！");
			Sleep(1000);
			system("cls");
			break;
		}
		if (bk != 0)
			break;
	}
}
pAccout loginJudge(pAccout ahead)
{
	char name1[20], pass1[20];
	char q;
	pAccout p1;
	int i = 0, j = 0;
	system("cls");
	printf("\n\n\n\n");
loop1:
	while (1)
	{
		j++;
		p1 = ahead->next;
		printf("\t\t\t\t账号:");
		scanf("%s", name1);
		getchar();
		printf("\t\t\t\t密码:");
		i = 0;
		while (1)
		{
			j++;
			q = getch();
			if (q != 13)
			{
				printf("*");
				pass1[i++] = q;
			}
			else {
				pass1[i] = '\0';
				printf("\n");
				break;
			}
		}
		//printf("%s",pass1);密码正确（加密最初出现问题） 
		while (p1 != NULL)
		{
			if (strcmp(name1, p1->name) == 0)
			{
				if (strcmp(pass1, p1->pass) == 0)
				{
					printf("\n\n\t\t\t\t登录成功！");
					Sleep(2000);
					return ahead;
				}
				else
				{
					p1 = p1->next;
				}
			}
			else
			{
				p1 = p1->next;
			}
			if (p1 == NULL)
			{
				printf("\n\n\n\n\t\t\t\t输入有误");
				system("cls");
				printf("\n\n\n\n\t\t\t\t请重新输入\n");
				Sleep(2000);
				goto loop1;
				break;
			}
		}
		if (j >= change)
		{
			system("cls");
			printf("\n\n\n\n");
			printf("\t\t\t\t---------------\n");
			printf("\t\t\t\t请重新输入!!!!!\n");
			printf("\t\t\t\t---------------\n");
			Sleep(2000);
			print2();
			exit(0);
		}
	}
	return ahead;
}//登录成功 
int Judge(char choice)
{
	while (1)
	{
		fflush(stdin);
		choice = getchar();
		if (choice == 'y' || choice == 'Y')
			return 1;
		else if (choice == 'n' || choice == 'N')
			return 0;
		while (getchar() != '\n')
			continue;
		printf("输入错误！请重新输入您的选择:\n");
	}
}
int back(char* a)
{
	if (strlen(a) == 1)
	{
		if (a[0] >= '0' && a[0] <= '9')
			return ((int)a[0] - 48);
	}
	else
		return 10;
}
```

### 排序模块

排序模块主要用到五个函数

其中有一个总菜单，两个关于价格排序的函数，以及两个与时长排序有关的函数

pticket sortMenu(pticket phead);     //排序子系统菜单  
pticket sort1(pticket phead);        //价格排序  
pticket sort2(pticket phead);        //时长排序
pticket sort11(pticket phead);
pticket sort12(pticket phead);

```java

pticket sortMenu(pticket phead)     //排序子系统菜单
{
	fflush(stdin);
	system("cls");
	int bk;
	while (1)
	{
		system("cls");
		char P[200];
		printf("\t\t\t|-------------------------------|\n");
		printf("\t\t\t|        排序子系统菜单         |\n");
		printf("\t\t\t|-------------------------------|\n");
		printf("\t\t\t|         1.按价格排序          |\n");
		printf("\t\t\t|         2.按时长排序          |\n");
		printf("\t\t\t|-------------------------------|\n");
		printf("\t\t\t|         0.返回上一层          |\n");
		printf("\t\t\t|-------------------------------|\n\n");
		printf("请输入指令:");
		scanf("%s", P);
		bk = back(P);
		printf("%d", bk);
		switch (bk)
		{
		case 1:
		phead = sort1(phead);//按价格排序 
			break;
		case 2:
		phead = sort2(phead);//按时长排序 
			break;
		case 0:break;
		default:printf("输入错误，重新输入！");
			Sleep(200);
			system("cls");
			break;
		}
		if (bk == 0)
			break;
	}

	return phead;
}
pticket sort11(pticket phead)
{
	pticket pTemp, pj, pj_f, pj_b;
	int i, j, flag;
	for (i = 0; i < iCount; i++)
		for (j = 0, flag = 0, pj = phead; j < iCount - 1 - i; j++)
		{
			if (flag == 0)
			{
				pj_f = pj;
				pj = pj->next;
				pj_b = pj->next;
			}
			if (flag == 1)
			{
				pj_f = pj_f->next;
				pj_b = pj->next;
			}
			flag = 0;
			if (pj->price > pj_b->price)
			{
				pTemp = pj->next;
				pj->next = pj_b->next;
				pj_b->next = pTemp;

				pTemp = pj_f->next;
				pj_f->next = pj_b->next;
				pj_b->next = pTemp;
				flag = 1;
			}
		}
	return phead;
}
pticket sort22(pticket phead)
{
	pticket pTemp, pj, pj_f, pj_b;
	int i, j, flag;
	for (i = 0; i < iCount; i++)
		for (j = 0, flag = 0, pj = phead; j < iCount - 1 - i; j++)
		{
			if (flag == 0)
			{
				pj_f = pj;
				pj = pj->next;
				pj_b = pj->next;
			}
			if (flag == 1)
			{
				pj_f = pj_f->next;
				pj_b = pj->next;
			}
			flag = 0;
			if (pj->time < pj_b->time)
			{
				pTemp = pj->next;
				pj->next = pj_b->next;
				pj_b->next = pTemp;

				pTemp = pj_f->next;
				pj_f->next = pj_b->next;
				pj_b->next = pTemp;
				flag = 1;
			}
		}
	return phead;
}

pticket sort1(pticket phead)        //按价格排序 
{
	system("cls");
	if (phead->next == NULL)
	{
		printf("没有信息!\n");
		Sleep(500);
		return phead;
	}
	sort11(phead);
	printCinema1(phead);
	return phead;
}
pticket sort2(pticket phead)        //按时长排序 
{
	system("cls");
	if (phead->next == NULL)
	{
		printf("没有信息!\n");
		Sleep(1000);
		return phead;
	}
	sort22(phead);
	printCinema1(phead);
	return phead;
}
```

### 增加模块

```java

pticket insertCinema(pticket phead)
{
	system("cls");
	pticket pNew, p = phead;
	char choice;
	while (p->next != NULL)
		p = p->next;
	do
	{
		pNew = (pticket)malloc(sizeof(ticket));
		printf("请输入序号:");
		scanf("%s", pNew->number);
		printf("请输入名称:");
		scanf("%s", pNew->name);
		printf("请输入影院:");
		scanf("%s", &pNew->cinema);
		printf("请输入时长:");
		scanf("%d", &pNew->time);
		printf("请输入价格:");
		scanf("%lf", &pNew->price);
		printf("请输入评分:");
		scanf("%lf", &pNew->grade);
		p->next = pNew;
		p = pNew;
		p->next = NULL;
		iCount++;
		printf("是否继续添加信息(Y or N):");
		choice = getchar();
	} while (Judge(choice) == 1);
	printf("已增加电影信息.\n");
	Sleep(500);
	SaveCinema(phead);
	return phead;
}
```

### 输出模块

这部分是为了输出电影的基本信息

用到两个函数

void printCinema1(pticket phead);//打印电影信息 
void printCinema2();//电影信息表格 

```java
void printCinema1(pticket phead)
{
	system("cls");
	if (phead->next == NULL)
	{
		printf("没有信息!\n");
		Sleep(500);
		return;
	}
	printCinema2();
	pticket p;
	p = phead->next;
	while (p != NULL)
	{
		printf("   %-4s     ", p->number);
		printf("%-12s  ", p->name);
		printf("%-16s", p->cinema);
		printf("  %4d     ", p->time);
		printf("%.2lf   ", p->price);
		printf("  %.1lf   ", p->grade);
		printf("\n");
		p = p->next;
	}
	system("pause");
}
void printCinema2()
{
	printf("信息如下:\n");
	printf("-----------------------------------------------------------------------\n");
	printf("----序号------名称-------------影院----------时间-----价格-----评分----\n");
	printf("-----------------------------------------------------------------------\n");
}
```

### 保存模块

这部分是为了使修改增加等部分能保存到文件中而设立

在另外许多模块中都有使用

```java

void SaveCinema(pticket phead) 
{
	system("cls");
	FILE* fp;
	if ((fp = fopen("Movie.txt", "wt")) == NULL)
	{
		printf("不能打开文件\n");
		exit(1);
	}
	pticket p;
	p = phead->next;
	while (p != NULL)
	{
		fprintf(fp, "%s %s %s %d %lf %lf ",
			p->number, p->name, p->cinema, p->time, p->price, p->grade);  //把内存中的文件输入到硬盘中
		p = p->next;
	}
	fclose(fp);                   //关闭文件
	printf("文件已保存\n");     //成功保存，显示提示
	Sleep(1000);
}
```

### 读取模块

这部分是读取文件中已有的电影，并返回到头结点处。在系统初始化时被使用到。

```java
pticket routerCinema(pticket phead)
{
	system("cls");
	FILE* fp;
	int m = 0;
	if ((fp = fopen("Movie.txt", "rw")) == NULL)
	{
		printf("读取失败!");
		exit(1);
	}
	pticket p1, p2;

	p1 = (pticket)malloc(sizeof(ticket));
	while (fscanf(fp, "%s %s %s %d %lf %lf ",
		&p1->number, &p1->name, &p1->cinema, &p1->time, &p1->price, &p1->grade) == 6)
	{
		m = m + 1;
		if (m == 1)
		{
			phead->next = p1;
			p2 = p1;
			p2->next = NULL;
		}
		else
		{
			p2->next = p1;
			p2 = p1;
			p2->next = NULL;
		}
		p1 = (pticket)malloc(sizeof(ticket));
	}
	fclose(fp);
	printf("读取成功！");
	iCount = m;
	//Sleep(1000);
	return phead;
}
```

### 修改模块

用于修改某一部电影的信息

需要先输入电影的名称

```java

pticket reviseCinema(pticket phead)
{
	system("cls");
	char P[200];
	int bk;
	char name[30];
	pticket pTemp;
	if (phead->next == NULL)
	{
		printf("没有可修改的信息!\n");
		system("pause");
		return phead;
	}
	pTemp = phead->next;
	printf("\n\n\n\n\t\t\t请输入你要修改的电影名称:");
	scanf("%s", name);
	while (strcmp(pTemp->name, name) != 0 && pTemp->next != NULL)
	{
		pTemp = pTemp->next;
	}
	if (strcmp(pTemp->name, name) == 0)
	{
		while (1)
		{
			system("cls");
			printf("\t\t\t请输入你要修改的信息:\n");
			printf("\t\t\t--------------------------------\t\t\t\n");
			printf("\t\t\t   1.修改代码      2.修改名称   \t\t\t\n");
			printf("\t\t\t   3.修改影院      4.修改时长   \t\t\t\n");
			printf("\t\t\t   5.修改价格      6.修改评分   \t\t\t\n");
			printf("\t\t\t          0.返回上一层          \t\t\t\n");
			printf("\t\t\t--------------------------------\t\t\t\n");
			printf("请输入您的选择:");
			scanf("%s", P);
			bk = back(P);
			switch (bk)
			{
			case 1: {
				system("cls");
				printf("请输入新代码:");
				scanf("%s", pTemp->number);
				system("cls");
				printf("修改成功!");
				Sleep(1000);
				break;
			}
			case 2: {
				system("cls");
				printf("请输入新的名称:");
				scanf("%s", pTemp->name);
				system("cls");
				printf("修改成功!");
				Sleep(1000);
				break;
			}
			case 3: {
				system("cls");
				printf("请输入新的影院:");
				scanf("%s", &pTemp->cinema);
				printf("请输入正确的影院:");
				scanf("%s", &pTemp->cinema);

				system("cls");
				printf("修改成功!");
				Sleep(1000);
				break;
			}
			case 4: {
				system("cls");
				printf("请输入新的时长:");
				getchar();
				scanf("%d", &pTemp->time);
				system("cls");
				printf("修改成功!");
				Sleep(1000);
				break;
			}
			case 5: {
				system("cls");
				printf("请输入新的价格：");
				scanf("%lf", &pTemp->price);
				system("cls");
				printf("修改成功!");
				Sleep(1000);
				break;
			}
			case 6: {
				system("cls");
				printf("请输入新的评分:");
				scanf("%lf", &pTemp->grade);
				system("cls");
				printf("修改成功!");
				Sleep(500);
				break;
			}
			case 0:
			break;
			default:printf("输入错误,请重新输入");
				system("pause");
			}
			if (bk == 0)
				break;
		}
		SaveCinema(phead);
		return phead;
	}
	else
	{
		printf("无该电影信息!");
		Sleep(500);
		return phead;
	}
}
```

### 删除模块

用于删除电影信息，输入电影名称删除相关的基本信息

```java

pticket deleteCinema(pticket phead)
{
	system("cls");
	if (phead->next == NULL)
	{
		printf("\n\n\n\n\n\t\t\t\t没有可删除的信息!\n");
		system("pause");
		return phead;
	}
	char name1[30];
	pticket pTemp, p;
	printf("\n\n\n\n\t\t\t\t输入即将下线的电影名称:");
	scanf("%s", name1);
	pTemp = phead;
	while (strcmp(pTemp->name, name1) != 0 && pTemp->next != NULL)
	{
		p = pTemp;
		pTemp = pTemp->next;
	}
	if (strcmp(pTemp->name, name1) == 0)
	{
		if (p != phead)
		{
			p->next = pTemp->next;
		}
		else
		{
			phead = pTemp->next;
		}
		printf("已删除");
		system("pause");
	}
	else
	{
		printf("没找到");
		Sleep(1000);
	}
	getchar();
	SaveCinema(phead);
	return phead;
}
```

## 系统界面

#### 欢迎界面

```java

void print1()
{
	int i;
	system("cls");
	printf("\n\n\n\n");
	printf("\t\t\t\t|-----------------------------------------------|\n");
	printf("\t\t\t\t||---------------------------------------------||\n");
	printf("\t\t\t\t|||                                           |||\n");
	printf("\t\t\t\t|||            欢迎来到影院管理系统           |||\n");
	printf("\t\t\t\t|||           Cinema ticketing system         |||\n");
	printf("\t\t\t\t|||                                           |||\n");
	printf("\t\t\t\t||---------------------------------------------||\n");
	printf("\t\t\t\t|-----------------------------------------------|\n");
	printf("\n\t\t\t\t系统开始启动.........\n");
	printf("===================================================================\r");
	for (i = 1; i < 70; i++)
	{
		Sleep(20);
		printf(">");
	}
}
```

#### 登录注册界面

```java

void loginMenu()
{
	system("cls");
	printf("\n\n");
	printf("\n\t\t\t+------------------------------------+");
	printf("\n\t\t\t+                                    +");
	printf("\n\t\t\t+                1.登录              +");
	printf("\n\t\t\t+                2.注册              +");
	printf("\n\t\t\t+                0.退出              +");
	printf("\n\t\t\t+                                    +");
	printf("\n\t\t\t+------------------------------------+\n");
}
```

#### 管理菜单

```java

void print()        //主界面 
{
	system("cls");
	printf("\t\t\t|===============================|\n");
	printf("\t\t\t|  欢迎来到影院管理系统(主菜单) |\n");
	printf("\t\t\t|-------------------------------|\n");
	printf("\t\t\t|          1.插入信息           |\n");
	printf("\t\t\t|          2.查看信息           |\n");
	printf("\t\t\t|          3.排序信息           |\n");
	printf("\t\t\t|          4.删除信息           |\n");
	printf("\t\t\t|          5.修改信息           |\n");
	printf("\t\t\t|-------------------------------|\n");
	printf("\t\t\t|-------------------------------|\n");
	printf("\t\t\t|          0.退出程序           |\n");
	printf("\t\t\t|===============================|\n");
}
```

#### 感谢使用

```java

void print2()
{
	system("cls");
	printf("\n\n\n\n");
	printf("\t\t\t|==============================|\n");
	printf("\t\t\t||----------------------------||\n");
	printf("\t\t\t|||                          |||\n");
	printf("\t\t\t|||         谢谢使用         |||\n");
	printf("\t\t\t|||                          |||\n");
	printf("\t\t\t||----------------------------||\n");
	printf("\t\t\t|==============================|\n");
	Sleep(2000);
}
```

## 初始文档

![image-20220610205013319](C:\Users\张华婕\AppData\Roaming\Typora\typora-user-images\image-20220610205013319.png)

![image-20220610205126642](C:\Users\张华婕\AppData\Roaming\Typora\typora-user-images\image-20220610205126642.png)
