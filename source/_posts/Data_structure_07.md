---
title: Data_structure_07
date: 2020-10-19 14:32:18
comments: true
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	五湖春梦扁舟雨，万里秋风两鬓蓬。
	远志出山成小草，神鱼失水困沙虫。

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


# 一、遍历链式栈

```
1：
	什么时候结束遍历
	
2：
	是否只有一个栈结点
    结点结构体：
    typedef struct stack_node
    {
    int data;
    struct stack_node * next;
    }SN,*P_SN;
    栈结构体：
    typedef struct Stack_Ctrl
    {
    struct stack_node * top,*bottom;
    int height;
    }SC,*P_SC;
	
	
	int Dispaly_Stack(P_SC stack)
	{
	
		if(stack == NULL)
		{
			printf("stack为NULL，遍历栈失败！\n");
			return -1;
		}
		else if(stack->top == NULL)
		{
			printf("空栈！\n");
			return 1;
		}
		else
		{
			//是否只有一个栈结点
			printf("栈的高度为%d\n",stack->height);
			if(stack->top == stack->bottom)
			{
				printf("依次为%d\n",stack->top->data);
			}
			else
			{
				
				for(P_SN tmp_node = stack->top;tmp_node!=NULL;tmp_node = tmp_node->next)
					printf("依次为%d\n",tmp_node->data);
			}
		}
		return 0;
	}
```



# 二、链式栈的销毁

```C
1、先释放栈结点的空间（循环）
	P_SN tmp_ndeo = NULL; //备份要free的结点的下一个
	P_SN free_node = stack->top;//这个变量一定是传给free,初始化为栈顶指针
	
	while(free_node != NULL)
	{
		tmp_node = free_node->next;//先备份
		free(free_node);//再free
		free_node = tmp_node//拿备份
	}


2、后面再释放管理栈的空间
	free(stack);
	stack->top = stack->bottom = NULL;
	stack->height = 0;
	stack = NULL;
	
	
int Destory_Stack(P_SC stack)
{
	if(stack == NULL)
	{
		printf("stack为NULL，销毁失败！\n");
		return -1;
	}
	else if(stack->top == NULL)//是否栈为空
	{
		free(stack);
        stack->top = stack->bottom = NULL;
        stack->height = 0;
        stack = NULL;
	}
	else
	{
		P_SN tmp_node = NULL; //备份要free的结点的下一个
        P_SN free_node = stack->top;//这个变量一定是传给free,初始化为栈顶指针

        while(free_node != NULL)
        {
            tmp_node = free_node->next;//先备份
            free(free_node);//再free
            free_node = tmp_node;//拿备份
        }
        
        free(stack);
        stack->top = stack->bottom = NULL;
        stack->height = 0;
        stack = NULL;
	}
	return 0;
}
```



# 三、链式队列

##### 3.1 初始化队列：

```
特点：
	先进先出的逻辑顺序操作数据（也是一条受限制的单向链表）
	基本操作：
			有头结点：入队（尾插）、出队（头删）、遍历、初始化队列
			无头结点：入队（尾插或者头插）、出队（头删或者尾删）
组成:
	管理队列的结构体
	
	队列结点的结构体
	
初始化的时候：
		除了给管理队列的结构体malloc之外，还需要给对头结点malloc
	
```

```

	typedef struct queue_node
	{
		int data;
		struct queue_node * next;
	}QN,*P_QN;
	
管理队列结构体：
	typedef struct queue_ctrl
	{
		struct queue_node * qhead,*qend;
		int length;
	}QC,*P_QC;
	
	P_QC Queue_Init()
	{
		//给队列头结点申请空间
		P_QN q_head = (P_QN)malloc(sizeof(QN));
		q_head->next = NULL;
		//给管理队列的结构体malloc
		P_QC queue = (P_QC)malloc(sizeof(QC));
		if(queue == NULL || q_head == NULL)
		{
			printf("申请队列失败！\n");
			return NULL;
		}
		
		queue->qhead = queue->qend = q_head;
		queue->length = 0;
	
		return queue;
	}
	
	int main()
	{
		P_QC queue = Queue_Init();
		if(queue == NULL)
		{
			printf("初始化队列失败！\n");
			return -1;
		}
		else
		{
			printf("初始化队列成功！\n");
		}
		return 0;
	}
```

##### 	

##### 3.2 入队（单向链表的尾插法）

```
考虑：
	控制队列的结构体指针可不可用

P_QN Create_Queue_Node()
{	
	P_QN queue_node = (P_QN)malloc(sizeof(QN));
	if(queue_node == NULL)
	{
		perror("malloc queue_node");
		return NULL;
	}
	queue_node->next = NULL;
	
	return queue_node;
}


int E_Queue(P_QC queue) //入队函数
{
	if(queue == NULL || queue->q_head == NULL || queue->end == NULL)
	{
		printf("queu为NULL，出队失败！\n");
		return -1;
	}
	P_QN add_node = Create_Queue_Node();//调用封装好的创建队列结点的函数来创建新添加的队列结点
	if(add_node == NULL)
	{
		printf("创建入队结点失败！\n");
		return -1;
	
	}
	else
	{
        printf("请输入要入队的数据：");
        scanf("%d",&(add_node->data));
        queue->qend->next = add_node;//这里需要改
        queue->qend = add_node;
		queue->length++;
        reutrn 0;
	}
}

```

##### 3.3 出队接口封装

---



判断指针的可用性：
if(queue == NULL || queue->q_head == NULL || queue->end == NULL)
{
		printf("queu为NULL，出队失败！\n");
		return -1;
}

要判断是否出队的结点是最后一个，如果是需要把队尾指针指向队列头结点，队列头结点的下一个指向NULL

```
if(queue->length == 1)//如果删除的是最后一个结点，end指针需要指向head结点
{

	queue->qend = queue->qhead;
	queue->qhead->next = NULL;
}
```



```

int G_queue(P_QC queue)
{
	if(queue == NULL || queue->qhead == NULL || queue->qend == NULL)
    {
            printf("queu为NULL，出队失败！\n");
            return -1;
    }
	else if(queue->qhead == queue->qend)//判断队列是否为空
    {
    	printf("队列为空！\n");
    	return -1;
    }
    else
    {
		P_QN del_node = queue->qhead->next;//备份要出队的结点
		printf("出队的数据是：%d\n",del_node->data);
		if(queue->length == 1)//如果删除的是最后一个结点，end指针需要指向head结点
		{

			queue->qend = queue->qhead;
			queue->qhead->next = NULL;
		}
		else
		{
			queue->qhead->next = del_node->next;
			del_node->next = NULL;
		}
		queue->length--;
        free(del_node);
        return 0;
    }
}
```

3.4 遍历队列

```
int Display_Queue(P_QC queue)
{
	if(queue == NULL || queue->qhead == NULL || queue->qend == NULL)
    {
            printf("queu为NULL，出队失败！\n");
            return -1;
    }
	else if(queue->qhead == queue->qend)//判断队列是否为空
    {
    	printf("队列为空！\n");
    	return -1;
    }
    else
    {
    	printf("队列中的数据有：");
    	P_QN tmp_node = queue->qhead->next;
    	while(tmp_node != NULL)
    	{
    		printf("%d ",tmp_node->data);
    		tmp_node = tmp_node->next;
    	}
    	printf("\n");
    }

	return 0;
}
```

---



# 四、文件IO

​	1）概念：

```
	文件：文本文档（文件系统里面）
	IO：
	以文件为参照物：
    输入：往文件里面写入数据
    输出：往文件里面读取数据
	以Linux OS为参照物：
	
	文件IO是一系列的API：
		都是在运行内存内部运行
		系统IO(系统调用函数)： POSIX标准(可移植系统接口)，类UNIX
				IO操作没有缓存的操作
				open: 打开指定的文件
               close：关闭一个已经打开的文件
               read ：往一个已经打开的文件读取数据
               write：往一个已经打开的文件写入数据 
               lseek：控制对文件的读写文件 
               mmap ：把一个文件映射进内存
               ...
		标准IO：C库提供的（标准输入输出库stdio）
				IO操作有缓存的操作（有缓存区）
				fopen fclose fread fwrite flseek ftell fgets fputs ...
				
		如果对与数据较大处理的时候，用标准IO
		
		对硬件设备控制的场景：
			键盘（安装驱动成功后，驱动会在应用层的文件系统里面生成一个设备文件）
			网卡
			显卡
```



2）系统IO接口

​	 	① open函数

​			调用open函数的头文件

		#include <sys/types.h>
	   	#include <sys/stat.h>
	   	#include <fcntl.h>
​			函数原型：	

```
	int open( const  char *pathname, int flags)
	
	
	返回值类型：整形
	返回值： open()return the new file descriptor, or -1  if an error 
	        打开文件成功：打开文件对应的文件描述符 （后面对文件的读写操作多需要文件的文件描述符才可完成）
	        打开文件失败：-1
	        
	形参一：
		pathname ：想要打开的文件的路径 （Linux里面的路径）	
	形参二：
		flags  打开文件的方式	
		以下三种必选一个（must  include  one of the following）
			O_RDONLY 只读
			O_WRONLY 只写
			O_RDWR   可读可写
		------------------------------------------------------------
			O_CREAT | O_EXCL  打开的文件保存时创建该文件，O_CREAT和O_EXCL，O_EXCL会判断文件是否存			 在，如果存在则标准出错
			
			O_NOFOLLOW ：打开的文件如果是（软）链接文件，则打开失败
			O_TRUNC：如果文件存在则清空数据
			O_APPEND：追加写入数据
			
		练习：
			以可读可写的方式打开file.txt,如果不存在则创建，存在则报错
			一个进程中，能最多打开文件多少次（不同一个文件）
			
			总共可以打开1024次文件，（前三个是系统运行程序的时候会默认打开的文件）
				标准输入 0  键盘(open函数是打开对应的LCD驱动安装后生成的设备文件) /dev
				标准输出 1  LCD
				标准出错 2  LCD
        int open(const char *pathname, int flags, mode_t mode);

        形参：mode_t mode：给新创建的文件设置权限（mode-umask）

        umask:系统默认的情况下，创建文件按的权限的掩码（八进制数）
	
```

​	②write函数

```
头文件：
		#include <unistd.h>

函数原型：
	
       ssize_t write(int fd, const void *buf, size_t count);
     返回值：
     	 写入数据成功：On  success,  the  number  of bytes written is returned (zeroindicates nothing was written). 
     	 			返回写入数据的大小（字节为单位）
     	 
       	写入数据失败：-1
       	
    形参一：
    	fd  想要写入数据的文件对应的文件描述符（open打开该文件成功时的返回值）
    	
    形参二：
    	buf：缓存区
    		用来存放想要写入的数据（缓存区需要自己创建）"我的名字" ---> 12个字节
    形参三：
    	count：设置写入数据的大小（字节为单位） 期望值--（要和实际情况相符合）

```

③、read函数

```
头文件：
		#include <unistd.h>
原型：
		ssize_t read(int fd, void *buf, size_t count);
		
		返回值：
				On success, the number of bytes read is returned (zero indicates end of
       file),
       			读取数据成功：返回实际读取到的数据的大小（字节为单位）
       			如果返回0：
       					1：你设置了count为0
       					2：已经读到文件的默认，然后再读一次
       			读取数据失败：
       				-1
       	参数一：fd
       		想要读取数据的文件对应的文件描述符（open打开该文件成功时的返回值）
       		
       形参二：
    	buf：缓存区
    		用来存放想要读取到的数据（缓存区需要自己创建）"我的名字" ---> 12个字节
    							 char *p = (char *)malloc(13*sizoef(char));
    							 char p[13];
    	形参三：
    		count：设置读取数据的大小（字节为单位） 期望值--（要和实际情况相符合）
			
```

​	练习：

​		获取一份未知数据的文件，让你统计文件中字母的个数。

​		获取一份未知数据的文件，让你统计文件中“你”的个数。

# 作业

​	预习二叉树：

​			二叉树中什么叫做度，什么叫做深度，什么叫做满二叉树、完全二叉树，空树

​		    四种遍历方式



汉化man手册


	预习二叉树：
	二叉树中什么叫做度，什么叫做深度，什么叫做满二叉树、完全二叉树，空树
	四种遍历方式
	汉化man手册
	①
	二叉树的度是指树中所以结点的度数的最大值。二叉树的度小于等于2，因为二叉树的定义要求二叉树中任意结点的度数（结点的分支数）小于等于2 。

	二叉树是树形结构中一种特殊的树形结构：二叉树中的每个结点至多有2棵子树（即每个结点的度小于等于2），并且两个子树有左右之分，顺序不可颠倒。在二叉树中还有种特殊的二叉树就是完全二叉树：所有结点中除了叶子结点以外的结点都有两棵子树。如果完全二叉树中只有最底层为叶子结点那么又称为满二叉树。
	二叉树结点的度数指该结点所含子树的个数，二叉树结点子树个数最多的那个结点的度为二叉树的度。

	二叉树的根结点所在的层数为1，根结点的孩子结点所在的层数为2，以此下去。深度是指所有结点中最深的结点所在的层数。
	二叉树是一个连通的无环图，并且每一个顶点的度不大于3。有根二叉树还要满足根结点的度不大于2。有了根结点之后，每个顶点定义了唯一的父结点，和最多2个子结点。然而，没有足够的信息来区分左结点和右结点。如果不考虑连通性，允许图中有多个连通分量，这样的结构叫做森林。

	遍历是对树的一种最基本的运算，所谓遍历二叉树，就是按一定的规则和顺序走遍二叉树的所有结点，使每一个结点都被访问一次，而且只被访问一次。由于二叉树是非线性结构，因此，树的遍历实质上是将二叉树的各个结点转换成为一个线性序列来表示。

	二叉树（Binary tree）是树形结构的一个重要类型。许多实际问题抽象出来的数据结构往往是二叉树形式，即使是一般的树也能简单地转换为二叉树，而且二叉树的存储结构及其算法都较为简单，因此二叉树显得特别重要。二叉树特点是每个结点最多只能有两棵子树，且有左右之分 。

	二叉树是n个有限元素的集合，该集合或者为空、或者由一个称为根（root）的元素及两个不相交的、被分别称为左子树和右子树的二叉树组成，是有序树。当集合为空时，称该二叉树为空二叉树。在二叉树中，一个元素也称作一个结点。

	定义：
	二叉树（binary tree）是指树中节点的度不大于2的有序树，它是一种最简单且最重要的树。二叉树的递归定义为：二叉树是一棵空树，或者是一棵由一个根节点和两棵互不相交的，分别称作根的左子树和右子树组成的非空树；左子树和右子树又同样都是二叉树。

	特殊类型：
	1、满二叉树：如果一棵二叉树只有度为0的结点和度为2的结点，并且度为0的结点在同一层上，则这棵二叉树为满二叉树。
	2、完全二叉树：深度为k，有n个结点的二叉树当且仅当其每一个结点都与深度为k，有n个结点的满二叉树中编号从1到n的结点一一对应时，称为完全二叉树。
	完全二叉树的特点是叶子结点只可能出现在层序最大的两层上，并且某个结点的左分支下子孙的最大层序与右分支下子孙的最大层序相等或大1。
	相关术语：
	①结点：包含一个数据元素及若干指向子树分支的信息。
	②结点的度：一个结点拥有子树的数目称为结点的度。
	③叶子结点：也称为终端结点，没有子树的结点或者度为零的结点。
	④分支结点：也称为非终端结点，度不为零的结点称为非终端结点。
	⑤树的度：树中所有结点的度的最大值。
	⑥结点的层次：从根结点开始，假设根结点为第1层，根结点的子节点为第2层，依此类推，如果某一个结点位于第L层，则其子节点位于第L+1层。
	⑦树的深度：也称为树的高度，树中所有结点的层次最大值称为树的深度。
	⑧有序树：如果树中各棵子树的次序是有先后次序，则称该树为有序树。
	⑨无序树：如果树中各棵子树的次序没有先后次序，则称该树为无序树。
	⑩森林：由m（m≥0）棵互不相交的树构成一片森林。如果把一棵非空的树的根结点删除，则该树就变成了一片森林，森林中的树由原来根结点的各棵子树构成。
	二叉树性质：
	性质1：二叉树的第i层上至多有2i-1（i≥1）个节点。
	性质2：深度为h的二叉树中至多含有2h-1个节点。
	性质3：若在任意一棵二叉树中，有n个叶子节点，有n2个度为2的节点，则必有n0=n2+1。
	性质4：具有n个节点的完全二叉树深为log2x+1（其中x表示不大于n的最大整数）。
	性质5：若对一棵有n个节点的完全二叉树进行顺序编号（1≤i≤n），那么，对于编号为i（i≥1）的节点：
	当i=1时，该节点为根，它无双亲节点。
	当i>1时，该节点的双亲节点的编号为i/2。
	若2i≤n，则有编号为2的左叶子，否则没有左叶子。
	若2+1≤n，则有编号为2i+1的右叶子，否则没有右叶子。
	二叉树遍历：
	遍历是对树的一种最基本的运算，所谓遍历二叉树，就是按一定的规则和顺序走遍二叉树的所有结点，使每一个结点都被访问一次，而且只被访问一次。由于二叉树是非线性结构，因此，树的遍历实质上是将二叉树的各个结点转换成为一个线性序列来表示。
	线索二叉树：

	按照某种遍历方式对二叉树进行遍历，可以把二叉树中所有结点排列为一个线性序列。在该序列中，除第一个结点外，每个结点有且仅有一个直接前驱结点；除最后一个结点外，每个结点有且仅有一个直接后继结点。但是，二叉树中每个结点在这个序列中的直接前驱结点和直接后继结点是什么，二叉树的存储结构中并没有反映出来，只能在对二叉树遍历的动态过程中得到这些信息。为了保留结点在某种遍历序列中直接前驱和直接后继的位置信息，可以利用二叉树的二叉链表存储结构中的那些空指针域来指示。这些指向直接前驱结点和指向直接后继结点的指针被称为线索（thread），加了线索的叉树称为线索二叉树。
	线索二叉树将为二叉树的遍历提供许多遍历。
	二叉树的四种遍历方式：
	二叉树的遍历（traversing binary tree）是指从根结点出发，按照某种次序依次访问二叉树中所有的结点，使得每个结点被访问依次且仅被访问一次。
	四种遍历方式分别为：先序遍历、中序遍历、后序遍历、层序遍历。
	前序遍历
	若树为空，则空操作返回。否则，先访问根节点，然后前序遍历左子树，再前序遍历右子树。（中 左 右）
	中序遍历
	若树为空，则空操作返回。否则，从根节点开始（注意并不是先访问根节点），中序遍历根节点的左子树，然后是访问根节点，最后中序遍历根节点的右子树。（左 中 右）
	后续遍历
	若树为空，则空操作返回。否则，从左到右先叶子后节点的方式遍历访问左右子树，最后访问根节点。（左右中）逆时针型 （左 右 中）
	层序遍历
	若树为空，则空操作返回。否则，从树的第一层，也就是根节点开始访问，从上到下逐层遍历，在同一层中，按从左到右的顺序结点逐个访问。
	②
	1.更新你的下载源目录
	sudo apt-get update
	2.下载并安装manpages-zh
	sudo apt-get install manpages-zh
	3.编辑家目录下的bash配置文件
	sudo vim ~/.bashrc
	4.在最后一行输入：alias cman=‘man -M /usr/share/man/zh_CN’ # 将中文的man命令重命名为cman命令，之后保存并退出编辑
	5.重新运行.bashrc文件
	source ~/.bashrc
	配置完毕，中文帮助手册使用方法示例：
	cman ls

	或者
	下载中文man包
	源码的网址：https://src.fedoraproject.org/repo/pkgs/man-pages-zh-CN
	找到源码包 wget https://src.fedoraproject.org/repo/pkgs/man-pages-zh-CN/manpages-zh-1.5.2.tar.bz2/cab232c7bb49b214c2f7ee44f7f35900/manpages-zh-1.5.2.tar.bz2
	sudo tar -jxvf  manpages-zh-1.5.2.tar.bz2
	./configure --disable-zhtw  #默认安装 
	make && make install
	找到编译安装生成的zh-CN存放路径，然后进入~/.bashrc配置
	编辑家目录下的bash配置文件
	sudo vim ~/.bashrc
	在最后一行输入：alias manc=‘man -M /usr/share/man/zh_CN’ # 将中文的man命令重命名为manc命令，之后保存并退出编辑
	重新运行.bashrc文件
	source ~/.bashrc
	配置完毕，中文帮助手册使用方法示例：
	manc ls


### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客23.jpg)
![](/hexo-private-blog-website/images/淘宝客24.jpg)
![](/hexo-private-blog-website/images/淘宝客25.jpg)


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

