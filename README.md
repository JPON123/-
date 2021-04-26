/****设备管理系统****/
# -///头文件
#include <stdio.h>
#include <iostream>
#include <string.h>
#include <string>
#include <stdlib.h>s
#include <conio.h>

using namespace std;

struct Date {  ///日期结构体
	int Year;
	int Month;
	int Day;
};

typedef struct Node {     ///实用链表结构体
	char  De_num[9];     //设备编号
	char De_name[50];    ///仪器名称
	char De_Type[50];      ///型号规格
	char De_Price[10];     ///单价
	struct Date De_date;     //购入日期
	char De_User[10];               //领用人
	char De_stitution[100];           //使用情况及备注
	struct Node *next;
	struct Node *pre;
}Node;

#define set (Node *)malloc(sizeof(Node))///动态创建定义

Node *Create()//数据记录链表的创建
{
	Node *head = NULL;
	if(NULL == head)
		head = set,head->next = NULL;
	return head;
}

Node *head = Create();//头结点的创建

void Fin()     //把链表中的数据写入文件
{
	FILE *fp1 ;
	if((fp1 = fopen("设备信息.txt","w"))==NULL)
	{
		printf("can't open file 设备信息");
		exit(0);
	}
	Node *q = head;
	while(q->next)
	{
		fprintf(fp1,"%s %s %s %s %d %d %d %s %s\n",q->next->De_num,q->next->De_name,q->next->De_Type,q->next->De_Price,q->next->De_date.Year,q->next->De_date.Month,q->next->De_date.Day,q->next->De_User,q->next->De_stitution);
		q = q->next;
	}
	fclose(fp1);
}

void Fout()     ///文件中数据读入链表
{
	FILE *fp1 ;
	if((fp1 = fopen("设备信息.txt","r"))==NULL)
	{
		printf("can't open file 设备信息");
		exit(0);
	}
	Node *q = head;
	while(!feof(fp1))
	{
		fscanf(fp1,"%s %s %s %s %d %d %d %s %s",q->next->De_num,q->next->De_name,q->next->De_Type,q->next->De_Price,&q->next->De_date.Year,&q->next->De_date.Month,&q->next->De_date.Day,q->next->De_User,q->next->De_stitution);
		q = q->next;
	}
	fclose(fp1);
}

void Menu() //菜单模块
{
	printf("* * * * * * * * * * * * * * * * * * * \n");
	printf("* * * * *  Menu(功能菜单) * * * * * *\n");
	printf("*       a. Add(添加记录)            *\n");
	printf("*       b. Display(显示特定记录)    *\n");
	printf("*       c. Del(删除记录)            *\n");
	printf("*       d. Modify(修改记录)         *\n");
	printf("*       e. DisplayAll(显示所有记录) *\n");
	printf("*       f. Find(查找记录)           *\n");
	printf("*       g. Exit(退出)               *\n");
	printf("*       h. Help(菜单)               *\n");
	printf("* * * * * * * * * * * * * * * * * * * \n");
	printf("请输入所需操作的功能编号a-h:\n");
}

void Add()//添加数据函数
{
	printf("请输入所需添加的设备的编号 仪器名称 型号规格 单价 购入日期(如2018 12 15) 领用人 使用状况(备注)：\n");
	char s1[9],s2[50],s3[50],s4[10];
	int s5_1,s5_2,s5_3; 
	char s6[10],s7[100];
	scanf("%s %s %s %s %d %d %d %s %s",s1,s2,s3,s4,&s5_1,&s5_2,&s5_3,s6,s7);
	Node *p = head;
	while(p->next)
		p = p->next ;
	Node *q = NULL;
	q = set;q->next = NULL;
	p->next = q;
	strcpy(q->De_num,s1);
	strcpy(q->De_name,s2);
	strcpy(q->De_Type,s3);
	strcpy(q->De_Price,s4);
	//strcpy(q->De_date.Year,s5_1);
	//strcpy(q->De_date,s6);
	q->De_date.Year = s5_1;
	q->De_date.Month = s5_2;
	q->De_date.Day = s5_2;
	strcpy(q->De_User,s6);
	strcpy(q->De_stitution,s7);
}

void Del()      ///删除数据函数
{
	printf("请输入所需删除的设备的编号：\n");
	bool flag = false;
	char num[9];
	scanf("%s",num);
	Node *q = head;
	while(q->next)
	{
		if(strcmp(q->next->De_num,num)==0)
		{
			flag = true;
			q->next = q->next->next;
			break;
		}
		q = q->next;
	}
	if(!flag)
		printf("未找到您想要删除的设备\n");
}

void Display()   //显示特定数据信息函数
{
	printf("请输入所需显示的设备编号：\n");
	bool flag = false;
	char num[9];
	scanf("%s",num);
	Node *q = head;
	while(q->next)
	{
		if(strcmp(q->next->De_num,num)==0)
		{
			printf("%s %s %s %s %d %d %d %s %s\n",q->next->De_num,q->next->De_name,q->next->De_Type,q->next->De_Price,q->next->De_date.Year,q->next->De_date.Month,q->next->De_date.Day,q->next->De_User,q->next->De_stitution);
			flag = true;
		}
		q = q->next;
	}
	if(!flag)
		printf("未找到您想要显示的设备\n");
}

void DisplayAll()    ///显示文件中所有设备的信息函数
{
	Node *q = head;
	if(q->next == NULL)
	{
		printf("无设备信息记录\n");
		return ;
	}
	while(q->next)
	{
		printf("%s %s %s %s %d %d %d %s %s\n",q->next->De_num,q->next->De_name,q->next->De_Type,q->next->De_Price,q->next->De_date.Year,q->next->De_date.Month,q->next->De_date.Day,q->next->De_User,q->next->De_stitution);
		q = q->next;
	}
}

void Modify()       ///修改用户指定数据
{
	bool flag = false;
	printf("请输入需要修改的设备编号：\n");
	char num[9];
	scanf("%s",num);
	printf("请输入需要修改后的设备编号的仪器名称 型号规格 单价 购入日期(如2018 12 15) 领用人 使用状况(备注)：\n");
	char s1[9],s2[50],s3[50],s4[10];
	int s5_1,s5_2,s5_3; 
	char s6[10],s7[100];
	scanf("%s %s %s %s %d %d %d %s %s",s1,s2,s3,s4,&s5_1,&s5_2,&s5_3,s6,s7);
	Node *q = head;
	while(q->next)
	{
		if(strcmp(q->next->De_num,num)==0)
		{
			flag = true;
			strcpy(q->next->De_num,s1);
			strcpy(q->next->De_name,s2);
			strcpy(q->next->De_Type,s3);
			strcpy(q->next->De_Price,s4);
			//strcpy(q->De_date.Year,s5_1);
			//strcpy(q->De_date,s6);
			q->next->De_date.Year = s5_1;
			q->next->De_date.Month = s5_2;
			q->next->De_date.Day = s5_2;
			strcpy(q->next->De_User,s6);
			strcpy(q->next->De_stitution,s7);
			break;
		}
		q = q->next;
	}
	if(!flag)
		printf("未找到您想要修改的设备\n");
}

int main()
{
	char i;
	system("cls");//clrscr();     //清屏幕
	Menu();
	//Fout();
	while(~scanf("%c",&i))
	{
		//printf("%c",i);
		fflush(stdin);     ///清理缓存
		//i = getchar();
		bool flag = false;    ///用来作为结束程序的判断
		//if(i=='f') break;
	    switch(i)
		{
		case 'a': Add();fflush(stdin);printf("成功执行a\n\n请输入所需操作的功能编号a-h:\n")/*调用添加函数*/;break;
		case 'b': Display();fflush(stdin);printf("成功执行b\n\n请输入所需操作的功能编号a-h:\n")/*调用显示特定数据函数*/;break;
		case 'c': Del();fflush(stdin);printf("成功执行c\n\n请输入所需操作的功能编号a-h:\n")/*调用删除函数*/;break;
		case 'd': Modify();fflush(stdin);printf("成功执行d\n\n请输入所需操作的功能编号a-h:\n")/*调用修改函数*/;break;
		case 'e': DisplayAll();fflush(stdin);printf("成功执行e\n\n请输入所需操作的功能编号a-h:\n")/*调用显示所有数据函数*/; break;
		case 'f': /**/;break;
		case 'g': flag = true;printf("成功退出\n")/*退出程序*/;break;
		case 'h': Menu();/*调用菜单函数*/;break;
		default : printf("请输入正确功能编号a-h\n");/*提示错误*/break;
		}
		if(flag)
			break;
	}
	Fin();
	return 0;
}
