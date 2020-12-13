---
title: Data_structure_02
date: 2020-10-13 12:11:18
comments: true
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	人生到处知何似，应似飞鸿踏雪泥。
	泥上偶然留指爪，鸿飞那复计东西。

[Linux命令手册][Linux]

[Linux]: https://man.linuxde.net/

[阿里云][aliyun]

[aliyun]: http://www.aliyun.com

[菜鸟教程][Runoob]

[Runoob]: http://www.runoob.com/

[百度][Baidu]

[Baidu]: http://www.baidu.com

[Google][Google]

[Google]: http://google.com.hk


# 一、顺序表

---

**1）在顺序表的头部添加数据**

​		`思路：	tmp_num的初始值是有效长度-1，循环到0结束`

​			

```
for(int tmp_num=SQ_L->num-1; tmp_num>=0; tmp_num--)
		SQ_L->DATA[tmp_num+1] = SQ_L->DATA[tmp_num];
SQ_L->DATA[0] = add_data;
```

---



**2）在顺序表的尾部添加数据**

​	**`思路：直接使用当前有效长度做下标赋值要添加的数据`**

```
SQ_L->DATA[SQ_L->num] = add_data;
```

---



# 二、单向链表

---

#### 1）链表的特点：

​				

```
1：链表是由结构体组成的，但是每一个结构体的地址是不连续的。（每一个结构体称为结点）
2：可以动态定义链表的长度
3：操作数据的时候不需要和数组一样遍历循环数组下标
```

​				

#### 2）单向链表的形状

---



`单向链表是由多个结构体链接组成的:`

![](/hexo-private-blog-website/images/数据结构2.png)

---





`每个节点的组成（每个结构体）：`

```
普通结点的组成：
    数据域：对应结点存放的数据
    指针域：存放下一个结点的地址
头结点可以这样组成：
 	数据域：一些符合每一个结点的属性信息，统计链表的长度
 	指针域：存放下一个结点的地址,第一次创建出来先指向空 head->next = NULL;
```



![](/hexo-private-blog-website/images/数据结构3.png)

#### 3）单向链表的创建

---



​		**注意：只需要先创建链表头（头结点），后续想要添加数据的时候在创建新结点，使其链接即可。**

​		`初始化单向链表头结点时，其指针域指向NULL，head->next = NULL;`

```c
typedef struct Link_List
{
	int data;//数据域
	struct Link_List * next; //指针域 link

}L_L,*P_LL;

P_LL Creat_List_Node();
P_LL Creat_List_Node()//创建普通结点
{
    P_LL node = (P_LL)malloc(sizeof(L_L));
    if(head == NULL)
    {
        perror("malloc:");
        return NULL;
    }
    
    node->next = NULL;//(*head).next = NULL;
 
    return node;
}


int main()
{
    P_LL head = Creat_List_Node();
    if(head == NULL)
    {
        printf("创建链表失败！\n");
        return -1;
    }
    else
    {
         printf("创建链表成功！\n");
    }
    
    free(head);
    return 0;
}
```

-----



**`练习：`**

​		**`创建一个关于存放国际信息链表，需要的数据类型：国籍、电话、年龄、ID,人员数的头结点。`**

```c
#define M 10
struct  //定义存放头结点的结构体类型+变量
{
    /*。。特殊属性信息可存放在这里。。*/
    char buf_c[M];
    int list_length;
	struct Link_List * head;
}Head;

typedef struct Link_List//普通的结点类型
{
	int data;//数据域 /*。。特殊属性信息也可存放在这里。。*/
	unsigned int id;
	unsigned int age;/*。。。。*/
	struct Link_List * next; //指针域 link

}L_L,*P_LL;

P_LL Creat_Head_Node();
P_LL Creat_Head_Node() //帮你创建头结点
{
	//给头结点申请空间
	Head.head = (P_LL)malloc(sizeof(L_L));
	if(Head.head == NULL)
	{
		perror("malloc:");
		return -1;
	}
	
	strcpy(Head.buf_c,"美国");
	Head.list_length = 0;
	Head.head->next = NULL;
	
	return (Head.head);
}
```

----

**`练习：`**

​		**`尝试封装一个给链表添加结点的函数（尾插入数据）`**

----



#### 4）尾插加入结点

---

`注意：`

​		`如果单向不循环链表中一个结点的指针域为NULL，该节点一定是最后的结点或者是头结点`

```c
int Tail_Add_Node();

int Tail_Add_Node()
{	
	//保留head存放的数据依然是头结点的地址
	P_LL tmp_node = Head.head;
	while(1)//第一步:先得到链表中最后的结点
	{
		if(tmp_node->next == NULL)
			break;
		tmp_node = tmp_node->next;
	}
    /*创建要加入新结点*/
    P_LL add_node = Creat_List_Node();
    /*if判断是否为空*/
    printf("请输入要添加的ID：");
    scanf("%d",&(add_node->id));
	tmp_node->next = add_node;//连接

    
	return 0;
}

int main()
{
    
    //创建一个表
   P_LL head =  Creat_Head_Node();
    if(head == NULL)
    {
        printf("创建头结点失败！\n");
        return -1;
    }
    else
    {
        printf("创建头节点成功！\n");
    }
    
    Tail_Add_Node();//才能添加结点
    
    printf("%d\n",head->next->id);
    
    return 0;
}
```



#### 5）头插加入结点

```c
int Head_Add_Node()
int Head_Add_Node()
{
 	/*创建要加入新结点*/
    P_LL add_node = Creat_List_Node();
    /*if判断是否为空*/
    printf("请输入要添加的ID：");
    scanf("%d",&(add_node->id));
    /*连接*/
    add_node->next = head->next;//连右手
    head->next = add_node;//连左手

	return 0;
}
```

----



#### 6）指定结点的位置删除

----

![](/hexo-private-blog-website/images/数据结构4.png)



```c
int Del_Where_Node(P_LL del_node);
int Del_Where_Node(P_LL del_node)
{
    P_LL tmp_node = Head.head;
    /*添加判断链表是否有数据，没有数据则不需要删除*/
	if(del_node == NULL)
	{
		printf("非法操作，删除的结点为空！\n");
		return -1;
	}
	else
	{
		while(1)//第一步:先查找表中是否有要删除的结点
		{	
			//不仅判断是否有要删除的结点，而且如果有还获取到了删除的结点的前一个结点
			if(tmp_node->next == del_node)
				break;
			tmp_node = tmp_node->next;
            //把tmp_node的下一个结点的位置赋值给自己，这时候tmp_node保存的就是它下一个结点的位置
		}
        
        tmp_node->next = del_node->next;
        del_node->next = NULL;
        free(del_node);
        return 0;
	}
}
```



**`练习：`**

​		**`1：特殊头节点的数据域完善`**

​		**`2： 添加判断链表是否有数据，没有数据则不需要删除`**





**作业：**

​	**封装实现单向链表的循环遍历、移动结点的函数。**


	作业：
	封装实现单向链表的循环遍历、移动结点的函数。
	#include "stdio.h"
	#include "stdlib.h"
	#include "errno.h"
	#include "string.h"
	#define LINK_LIST_MAX 10

	typedef long DATA_TYPE;

	int flag = 0; // 判断删除后是否需要释放空间: 0 需要,非 0 不需要,移动时删除不需要

	struct Link_List_out // 定义存放头结点的结构体类型
	{
		char country[LINK_LIST_MAX];
		int list_length;
		struct Link_List *head; //特殊属性的信息也可以存放在这里 公共的信息 比如国家等
	}Head; //定义结构体变量

	typedef struct Link_List
	{
		int data; //数据域 一些特殊属性的私人的信息也可以存放在这里 如密码资料等等
		unsigned int id;
		unsigned int age;
		DATA_TYPE tel;
		DATA_TYPE people;
		struct Link_List *next; //指针域 link

	}L_L,*LL_P;

	LL_P My_Link_List_Node(); // 创建普通结点
	LL_P My_Head_List_Node(); // 创建头结点
	int Tail_Add_Node(); // 在链表尾部插入结点
	int Head_Add_Node(); // 在链表头部插入结点
	int Del_Site_Node(LL_P del_node); // 指定结点的位置删除结点
	int Display_Link_List_Node(); //遍历循环打印结点
	int Move_Site_Node(LL_P before_node,LL_P after_node); // 移动结点before_node到after_node后面
	int Site_Add_Node(LL_P add_node,LL_P after_node);

	LL_P My_Head_List_Node()
	{
		Head.head = (LL_P)malloc(sizeof(L_L)); //给头结点申请内存空间
		if(Head.head == NULL)
		{
			perror("malloc");

			return NULL;
		}

		char *pointer = "美国";
		strcpy(Head.country,pointer);
		Head.list_length = 0;
		Head.head->next = NULL; // 指针域 存放下一个结点的地址  避免野指针

		return (Head.head);
	}

	LL_P My_Link_List_Node() //创建普通结点
	{
		LL_P head_node = (LL_P)malloc(sizeof(L_L));
		if(head_node == NULL)
		{
			perror("malloc");

			return NULL; //返回空地址(*void) 0
		}

		head_node->next = NULL; //(*head_node).next = NULL;

		return head_node;
	}

	int Tail_Add_Node() // 尾插法
	{
		LL_P tmp_node = Head.head; //保留head存放的数据依然是头结点的地址  防止头结点地址被修改
		while(1) //第一步:先得到链表中最后的结点
		{
			if(tmp_node->next == NULL)
				break;
			tmp_node = tmp_node->next; //结构体指针变量存放下一个结点的地址
		}
		LL_P add_node = My_Link_List_Node(); //创建要加入的新的结点
		printf("请输入要添加的ID:");
		scanf("%d", &(add_node->id));

		tmp_node->next = add_node; //连接操作

		return 0;
	}

	int Head_Add_Node() //头插法
	{
		LL_P tmp_node = Head.head; //保留head存放的数据依然是头结点的地址  防止头结点地址被修改
		LL_P add_node = My_Link_List_Node();
		printf("请输入要添加的ID:");
		scanf("%d", &(add_node->id));

		add_node->next = tmp_node->next; //连接操作 连右手
		tmp_node->next = add_node; // 连左手

		return 0;
	}

	int Del_Site_Node(LL_P del_node)
	{
		LL_P tmp_node = Head.head; //保留head存放的数据依然是头结点的地址  防止头结点地址被修改
		if(del_node == NULL)
		{
			printf("非法操作,删除的结点为空!!\n");

			return -1;
		}
		else
		{
			while(1) //第一步:先查找表中是否有要删除的结点
			{	//不仅判断是否有要删除的结点,而且如果有还获取到了删除的结点的前一个结点
				if(tmp_node->next == del_node) //不仅判断是否有要删除的结点,而且如果有还获取到了删除的结点的前一个结点
					break;
				tmp_node = tmp_node->next; //结构体指针变量存放下一个结点的地址
			}

			tmp_node->next = del_node->next;
			del_node->next = NULL;

			if(!flag)
				free(del_node);

			return 0;
		}

	}

	int Display_Link_List_Node() //循环遍历打印结点
	{
		LL_P tmp_node = Head.head;
		if(tmp_node->next == NULL) //判断除了表头以外是否有其它结点
		{
			printf("链表为空!!\n");

			return 0;
		}
		else if(tmp_node == NULL)
		{
			printf("非法操作!!\n");

			return -1;
		}

		while(1) //循环遍历打印出包括头结点的id元素数据
		{
			printf("%d ", tmp_node->id);
			if(tmp_node->next == NULL) //遍历打印直到结点存放的地址为空地址为止,相当于遍历整个链表
				break;
			tmp_node = tmp_node->next; //结构体指针变量存放下一个结点的地址

		}

		printf("\n");

		return 0;
	}

	int Site_Add_Node(LL_P add_node,LL_P after_node)
	{
		add_node->next = after_node->next; //先连接后面结点地址
		after_node->next = add_node; //再存放给前面自己的地址

		return 0;
	}

	int Move_Site_Node(LL_P before_node,LL_P after_node) //移动结点before_node到after_node后面
	{
		if(before_node == NULL && after_node == NULL) //判断是否为非法输入
		{
			printf("非法操作!!\n");

			return -1;
		}
		flag = 1; //限制删除时不需要释放before_node的内存空间
		Del_Site_Node(before_node); //删除移动第一个结点
		flag=0;
		Site_Add_Node(before_node,after_node); //往指定的位置添加结点

		return 0;
	}

	int main(int argc, char const *argv[])
	{
		LL_P head_node_ret = My_Head_List_Node(); // 创建一个头结点
		if(head_node_ret == NULL)
		{
			printf("创建头结点失败!!\n");

			return -1;
		}
		else
		{
			printf("创建头结点成功!!\n");
		}

		Head_Add_Node(); //头部插入结点 添加结点
		Head_Add_Node();
		Head_Add_Node();

		Display_Link_List_Node(); //循环遍历

		Move_Site_Node(head_node_ret->next,head_node_ret->next->next); //移动结点

		Display_Link_List_Node();


		//free(head_node_ret);

		return 0;
	}


### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客10.jpg)
![](/hexo-private-blog-website/images/淘宝客11.jpg)
![](/hexo-private-blog-website/images/淘宝客12.jpg)

### 内存分布：
![](/hexo-private-blog-website/images/内存分布.bmp)
![](/hexo-private-blog-website/images/内存分布.png)
### 指针数组:
![](/hexo-private-blog-website/images/整型指针数组.bmp)
![](/hexo-private-blog-website/images/字符指针数组.bmp)
### 二维数组:
![](/hexo-private-blog-website/images/二维数组.png)
![](/hexo-private-blog-website/images/二维数组1.png)
### 支付宝打赏:
![](/hexo-private-blog-website/images/alipay.jpg)
### 微信打赏:
![](/hexo-private-blog-website/images/wechat.jpg)
### 财付通打赏：
![qq.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggwd0rvjj32ai2lxdrm.jpg)
