---
title: File_io_04
date: 2020-09-10 13:53:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	云母屏风烛影深,长河渐落晓星沉.
	嫦娥应悔偷灵药，碧海青天夜夜心。

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



# 	一、作业

```
一：长方形由触摸屏控制滑动

	1：y: 400~450    x:0~800 颜色:自选
	
	2：接口分析
	
		1：专门读取滑动的坐标 int Get_Touch_Yx();
		2：专门显示长方形    int Draw_Rgb(int touch_x);
		
		int Get_Touch_Yx()
		{
			int x,y;
			while(1)
			{
				read(touch_fd,&(touch),sizeof(touch));
				if(touch.type == EV_ABS && touch.code == ABS_X) x = touch.value;
				if(touch.type == EV_ABS && touch.code == ABS_Y) y = touch.value;
				
				if(y>400 && y<450)
				{
					Draw_Rgb(y)
				}
			
			}
		
			return 0;
		}
		
		
		int Draw_Rgb(int touch_x)
		{	
			int y,x;
			for(y=400; y<450; y++)
			{
				for(x=0; x<800; x++)
				{
					//判断木板外边
						//刷红色
					else
						//刷木板颜色
				}
			}
		
			return 0;
		}
		
		
```

# 二、双缓冲刷图

Frame buffer  :提供了关于显示设备的两个结构体，存放再/usr/include/linux/fb.h

缓存刷图：

​				帧缓存驱动提供了3块显示的缓存区（显存），之前一直用的是偏移量为0的第一块。



第一个：里面的数据信息可修改

	struct fb_var_screeninfo {
		__u32 xres;			/* visible resolution		*/
		__u32 yres;
		__u32 xres_virtual;		/* virtual resolution		*/
		__u32 yres_virtual;
		__u32 xoffset;			/* offset from virtual to visible */
		__u32 yoffset;			/* resolution			*/
	__u32 bits_per_pixel;		/* guess what			*/
	__u32 grayscale;		/* 0 = color, 1 = grayscale,	*/
					/* >1 = FOURCC			*/
	struct fb_bitfield red;		/* bitfield in fb mem if true color, */
	struct fb_bitfield green;	/* else only length is significant */
	struct fb_bitfield blue;
	struct fb_bitfield transp;	/* transparency			*/	
	
	__u32 nonstd;			/* != 0 Non standard pixel format */
	
	__u32 activate;			/* see FB_ACTIVATE_*		*/
	
	__u32 height;			/* height of picture in mm    */
	__u32 width;			/* width of picture in mm     */
	
	__u32 accel_flags;		/* (OBSOLETE) see fb_info.flags */
	
	-----------------------------------------------------------------------
	关于双缓存，是需要关注 
		xoffset :可见区域x坐标的偏移量
		yoffset ：可见区域y坐标的偏移量
		
	注：可见区域默认是比全局区域小（虚拟区域）


二、使用文件IO中ioctl函数获取开发板显示设备的信息，这些信息存放再上面的结构体里面



​	1)man 2  ioctl :专门用来控制获取设备的IO通道中流向的信息

	 #include <sys/ioctl.h>
	
	   int ioctl(int fd, unsigned long request, ...);
	   
	   fd:先要控制IO信息的设备文件描述符
	   request：控制命令（请求信号--宏定义：未知的，不可记得。只能用得多就知道得多，设备对应宏定义在对应的设    备头文件里面）
	   
	   后面的参数：最后只有一个，这个参数根据控制命令来设定
	   
	   思路：
	   	int lcd_fd = open("/dev/fb0",O_RDWR);
	   	if(lcd == -1)
	   	{
	   		perror("open lcd");
	   		return -1;
	   	}
	   	
	   	ioctl(lcd_fd,能获取帧缓存驱动固定信息的宏定义，);
	   	struct fb_fix_screeninfo f_info;
	   	ioctl(lcd_fd,FBIOGET_FSCREENINFO,&f_info); //调用完就获取到了显示的基本不可改的信息
	   	printf("%d\n",f_info.smem_len);
	   	
	   	//因为你是通过ioctl获取IO输出信息，需要用一个缓存区取存放
	   	缓存区的选择：类型选择：都知道FBIOGET_FSCREENINFO是获取fb_fix_screeninfo结构体整个成员信息，肯定定义fb_fix_screeninfo类型去存储
练习：根据获取显存大小的方法，获取fb_var_screeninfo中  可见区以及虚拟区的高度和宽度

```
关于双缓冲刷图的思路：

		1：由三个显示的缓存区，第一个默认直接把像素点赋值给映射指针就会立马显示
		
		2：
			在初始化的时候，自定义两个缓存区固定指定两图片
			
		注：可见区如果不是对应的缓存区域，该缓存区域的像素点是不会显示的。
		
		试验：
				使用后面两个房间来显示，前面的房间先不管
			
```



2)整合双缓存刷图的接口调用流程

```
1：函数的分类,函数形参
	int  Init();
		打开lcd
		ioctl:获取LCD硬件信息
		映射LCD
	
	int  Free();
	注：特殊情况，缓存区中缓存的图片都没有要显示的，那么只能正常刷图
	int Dis_Cache_Bmp(char * bmp_path);
		1) 判断bmp_path是否有缓存
		2）如果缓存区里面没有，则调用Dis_Bmp正常显示
		3）如果缓存区里面有，则设置可见区的偏移量来显示缓存图片
	
	
	int Dis_Bmp(char * bmp_path);
	int Cache_Init(int cache_select);提前更新缓存区  （因为初始化的时候要缓存指定的两张图片）
	判断是否要更换缓存区中的图片
	cache_select:0 默认缓存指定的图片  1：更换缓存的图片

初步的调用顺序：
	int main()
	{
		Init();
		Cache_Init();
		Dis_Cache_Bmp();
		Free();
		return 0;
	}
	

2：通用变量的分类
	
	#define BMP_SIZE 800*480*3
	struct inf
	{
		//文件描述符
		int lcd_fd;
		int cache_bmp_fd_1,cache_bmp_fd_2;
		int *mmap_lcd_star;
		char * cache_bmp_1[BMP_SIZE],cache_bmp_2[BMP_SIZE]; //存放两个缓存区的像素点的空间(read)
	}I;





```

三、标准IO （stdio.h）

（在基于系统IO的基础上封装的C库提供的接口，只要你的系统安装了C标准库，就可调用标准IO接口）

 1)man  3 fopen

```
头文件：#include <stdio.h>

FILE *fopen(const char *path, const char *mode);
返回值：
		FILE *
		成功：返回对应文件的文件流指针
		失败：NULL（设置错误码）
		
path：是要打开的文件的路径
mode：
		r   :只读方式打开，但是文件必须存在
		r+  :读写方式打开，但是文件必须存在
		w   :只写方式打开，文件存在这清空数据，不存在则创建
		w+  :读写方式打开，文件存在这清空数据，不存在则创建
		a   :只写的方式打开，如果文件存在则把数据写入文件数据尾部，如果不存在则创建
		a+  :读写的方式打开，如果文件存在则把数据写入文件数据尾部，如果不存在则创建
练习：
		打开一个文件，以只写的方式打开。
		
		#include <stdio.h>
		int main()
		{
			FILE * f_p = fopen("./data.txt","w");
			if(f_p == NULL)
			{
				perror("fopen ");
				return -1;
			}
			
            int ret = fputc('S',f_p);
            if(ret == EOF)
            {
            perror("fputc...");
            return -1;
            }
            else
            {
            printf("%d---%c\n",ret,ret);
            }

			fclose(f_p);
			return 0;
        }
```



	​	  2)第一组：

	​				对文件：写一个字符数据，读取一个读取数据

	​				 读取： 

	​								 int fgetc(FILE *stream);

	​								 int getc(FILE *stream);

	​				 返回值：

	​								成功：返回读取到的一个字符的数据

	​                                失败： EOF   (-1)

	​											读取失败

	​                                            读取到文件的末尾

	​				 写入：  

	​							  int fputc(int c, FILE *stream);

	​							 返回值：

	​											成功：返回写入到的字符数据

	​											失败：EOF (-1)

	 							 int putc(int c,  FILE *stream);



	​							  返回值：

	​											成功：返回写入到的字符数据

	​											失败：EOF (-1)

	​                            形参一：

	​                                      c：存放要写入文件中的一个字符的数据	

	​                           形参二：

	​									stream：想要写入的文件对应的文件流指针（*解引用后获取的是文件描述符）

	​				fputc的示例：

	​							 	往标准输入写入一个字符的数据			

```
#include <stdio.h>
#include <errno.h>
int main()
{
    int ret = fputc('S',stdout);
    if(ret == EOF)
    {
       perror("fputc...");
       return -1;
    }
    else
    {
    	printf("%d---%c\n",ret,ret);
    }
	return 0;
}
```

	作业：

	​		学习flseek,ferror,feof接口						

	 明天：

	​		目录IO

	​		布置文件IO阶段项目


作业：
学习flseek,ferror,feof接口
① flseek 
头文件  #include <stdio.h>
        函数原型：int fseek(FILE *stream, long offset, int whence);
函数功能：指定当前文件位置的偏移量
形参一：文件流指针，打开文件返回的文件指针。
形参二：相对起始位置的偏移量。
形参三：偏移起始位置，SEEK_SET：文件起始位置，SEEK_CUR:文件当前位置，SEEK_END：文件末尾位置。
返回值：函数执行成功返回0，失败返回-1.
 The  rewind()  function  returns no value.  Upon successful completion,
       fgetpos(), fseek(), fsetpos() return 0, and ftell() returns the current
       offset.   Otherwise,  -1  is  returned and errno is set to indicate the
       error.
② ferror
   头文件：#include <stdio.h>
   函数原型：int ferror(FILE *stream);
函数功能： 测试一个文件是否遇到了某种错误。
函数 ferror  测试  stream  指向的流中的错误标记，如果已设置就返回非零值。错误标记只能用函数 clearerr 重置。
形参一：打开文件返回的文件指针。
返回值：遇到错误返回真，否则返回假。
这些函数不应当失败，它们不设置外部变量 errno 。(但是，如果 fileno  检测到它的参数不是有效的流，它必须返回 -1，并且将 errno 设置为 EBADF 。)
③ feof
   头文件：#include <stdio.h>
   函数原型：int feof(FILE *stream);
函数功能：判断一个文件指针是否达到文件末尾
函数  feof  测试 tests the end-of-file indicator for the stream pointed
to by stream 指向的流中的文件结束标记，如果已设置就返回非零值。文件结束标记只能用函数 clearerr 清除。
形参一：打开文件返回的文件指针
返回值：到达文件末尾返回真，否则返回假。
#include "stdio.h"
#include "errno.h"
#include "unistd.h"
#include "sys/types.h"
#include "sys/stat.h"
#include "fcntl.h"

int main(int argc, char const *argv[])
{
	FILE *fp = fopen("./file.txt","a+");
	if(fp == NULL)
	{
		perror("fopen file:");

		return -1;
	}

	int ret = fputc('s',fp); //往文件中输入一个字符
	if(ret == EOF)
	{
		perror("fputc char:");

		return -1;
	}
	else
	{
		printf("%d---%c\n", ret,ret);
	}

	//一直读 读完跳出循环
	fseek(fp,0,SEEK_SET); //设置文件偏移量
	char char_data;
	while(1)
	{
		char char_data = fgetc(fp); //从文件当中获取一个字符
		if(char_data == EOF && feof(fp)) //判断是否到达文件末尾
		//if(char_data == EOF || feof(fp)) //判断是否到达文件末尾
		//if(char_data == EOF)
		{
			printf("读取失败或者无数据可读取或者已到达文件末尾无法读取!!\n");

			return -1;
		}
		if(feof(fp)) //判断是否到达文件末尾
		{
			printf("已读到文件末尾!!无法再继续读取!!\n");

			return -1;
		}
		else if(ferror(fp)) //判断是否遇到错误
		{
			perror("fgetc()");

			break;
		}
		else
		{
			fputc(char_data,stdout);
		}
	}

	fclose(fp);

	return 0;
}


#include <stdio.h>
#include <errno.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <sys/mman.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>
#include <linux/input.h>


#define LCD_FILE    	"/dev/fb0"
#define TOUCH_PATH  	"/dev/input/event0"
#define OPEN_LCD_FLAG 	O_RDWR
#define OPEN_TOUCH_FLAG O_RDONLY
#define LCD_LENGTH  	800*480*4
#define MAP_OFFSET		0

struct files_info
{
	int lcd_fd,touch_fd;
	int * lcd_star;
	struct input_event touch;
	int touch_x,touch_y;
	
}FI;

int Init_Info();
int Close_File();
int Dis_Block(int site_x,int site_y,int color);
int Move_Block();

int Init_Info()
{
    FI.lcd_fd = open(LCD_FILE,OPEN_LCD_FLAG);
    FI.touch_fd = open(TOUCH_PATH,OPEN_TOUCH_FLAG);
	if(FI.lcd_fd == -1 || FI.touch_fd == -1)
	{
		perror("open dev:");
		return -1;
	}
	FI.lcd_star = (int *)mmap(NULL,LCD_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,FI.lcd_fd,MAP_OFFSET);
	if(FI.lcd_star == MAP_FAILED)
	{
		perror("mmap lcd");

		return -1;
	}
	
	
	int i;
	for(i=0;i<800*480;i++)
	{
		*(FI.lcd_star+i) = 0x00ffffff;
	}
	
	
	// int *new_star = I.lcd_star + 800*400;
	// int *new_star_1 = I.lcd_star + 800*455;

	// int w,h;

	// for(w=0;w<800;w++)
	// {
	// 	*(new_star + w) = 0x00000000;
	// }
		
 //   for(h=0;h<800;h++)
	// {
	// 	*(new_star_1 + h) = 0x00000000;
	// }
	
	
	return 0;
}

int Close_File()
{
	
	close(FI.lcd_fd);
	close(FI.touch_fd);

	munmap(FI.lcd_star,LCD_LENGTH);

	return 0;
}

int Dis_Block(int site_x,int site_y,int color) //指定位置映射（小长方体体积固定）
{
	//初始小方块位置
	int * new_star = FI.lcd_star + 800*site_y + site_x;
	//小方块定义为高50，长100
	int w,h;
	
	for(h = 0; h < 50; h++)
	{
		for(w = 0; w < 100; w++)
		{
			*(new_star + 800*h+w) = color;
		}
	}
	
	return 0;
}

int Move_Block()
{
	int *new_star = FI.lcd_star + 800*400;
	int *new_star_1 = FI.lcd_star + 800*455;

	//int w,h;
	int x;

	for(x = 0;x < 800; x++)
	{
		*(new_star + x) = 0x00000000;
	}
		
   for(x = 0;x < 800; x++)
	{
		*(new_star_1 + x) = 0x00000000;
	}
	
	// I.touch_fd = open(TOUCH_PATH,O_RDONLY);
	//  if(I.touch_fd == -1)
	//  {
	// 	 perror("open touch :");
	// 	 return -1;
	//  }
	int x_tmp,y_tmp;
	int flag;
	Dis_Block(0,402,0x00ff00ff);
	while(1)
	{
		
		read(FI.touch_fd,&(FI.touch),sizeof(FI.touch));
		
		if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X) 
	   {
		   FI.touch_x=FI.touch.value;
	       FI.touch_x = 800*FI.touch_x/1024;
	   }
	   if(FI.touch.type == EV_ABS && FI.touch.code == ABS_Y) 
	   {
		   FI.touch_y = FI.touch.value;
		   FI.touch_y = 480*FI.touch_y/600;
	   }
		//按下了屏幕则把初始位置画为白色
		if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value !=0 && FI.touch_y>400 && FI.touch_y<450 && (FI.touch_x-50)>0 && (FI.touch_x+50)<800)
		{
			Dis_Block(0,402,0x00ffffff);
			flag = 1;
			
			//printf("x=%d  y=%d\n",x,y);
			
		}
		//printf("x=%d  y=%d\n",x,y);
		if(flag && FI.touch_y>400 && FI.touch_y<450 && (FI.touch_x-50)>0 && (FI.touch_x+50)<800)
		{
			Dis_Block(FI.touch_x-50,402,0x00ff00ff);
			//printf("x=%d  y=%d\n",x,y);
		}
		if(FI.touch_x+50>800)
			Dis_Block(0,402,0x00ff00ff);
		
		if((x_tmp != FI.touch_x || y_tmp != FI.touch_y) && flag==1)
		{
			Dis_Block(x_tmp-50,402,0x00ffffff);//与原来的X，Y不同时
			
		}
		//抬起了屏幕
		if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value ==0 || FI.touch_y < 400 || FI.touch_y > 450)
		{
			Dis_Block(FI.touch_x-50,402,0x00ffffff);
			//printf("%d\n",__LINE__);
			Dis_Block(0,402,0x00ff00ff);
			printf("x-50=%d\n",x-50);
			
			flag = 0;
		}
		
		x_tmp = FI.touch_x;
		y_tmp = FI.touch_y;
		
	}
	
	return 0;
}




int main()
{
	
	Init_Info();
	//Dis_Block(0,215,0x00ff00ff);
	Move_Block();

	Close_File();
	
	return 0;
}





	#include <stdio.h>
	#include <errno.h>
	#include <stdlib.h>
	#include <sys/stat.h>
	#include <sys/types.h>
	#include <sys/mman.h>
	#include <fcntl.h>
	#include <unistd.h>
	#include <string.h>
	#include <linux/input.h>


	#define LCD_DEV_PATH    "/dev/fb0"
	#define TOUCH_DEV_PATH  "/dev/input/event0"
	#define OPEN_FLAG        O_RDWR
	#define OPEN_BMP_FLAG    O_RDONLY
	#define MAP_LENGTH       800*480*4
	#define BMP_LENGTH       800*480*3
	#define MAP_OFFSET       0
	#define BMP_OFFSET       54


	struct files_info
	{
		int lcd_fd,touch_fd,bmp_fd_1,bmp_fd_2,bmp_fd_3;
		int *mmap_lcd_star;
		char *bmp_star;
		struct input_event touch;
	}FI;

	typedef struct double_link_list
	{
		int data;
		struct double_link_list *next,*prev;
	}DL,*P_DL;


	int Init_Info();
	int Free_Close();
	P_DL Cread_Head();
	int Cread_Node(P_DL head,int fd);
	int Display_Bmp(int fd);
	int Get_Touch_Xy(P_DL head);

	//双向循环链表头结点
	P_DL Cread_Head()
	{
		//创建结构体指针空间
		P_DL head = (P_DL)malloc(sizeof(DL));
		if(head == NULL)
		{
			perror("malloc:");

			return NULL;
		}

		head->next = head->prev = head;
		// head->next = head;
		// head->prev = head;

		return head;
	}

	//创建普通结点
	int Cread_Node(P_DL head,int fd)
	{
		if(head == NULL)
		{
			printf("头结点的地址为空!!\n");

			return -1;
		}
		else
		{
			P_DL add_node = (P_DL)malloc(sizeof(DL));
			if(add_node == NULL)
			{
				//printf("新添加的结点地址为空!!添加失败!!\n");
				perror("malloc:");

				return -1;
			}
			add_node->data = fd;
			add_node->prev = head->prev; //尾插法
			add_node->next = head;
			head->prev->next = add_node;
			head->prev = add_node;
		}

		return 0;
	}

	int Init_Info() //只需要初始化一次的变量
	{
		FI.lcd_fd = open(LCD_DEV_PATH,OPEN_FLAG); //打开lcd
		FI.touch_fd = open(TOUCH_DEV_PATH,OPEN_FLAG); //打开触摸屏
		if(FI.lcd_fd == -1 || FI.touch_fd == -1)
		{
			perror("open dev:");

			return -1;
		}

		//映射LCD
		FI.mmap_lcd_star = (int *)mmap(NULL,MAP_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,FI.lcd_fd,MAP_OFFSET);
		if(FI.mmap_lcd_star == MAP_FAILED)
		{
			perror("mmap lcd:");

			return -1;
		}

		//初始化让屏幕变全黑
		int n;
		for(n = 0; n < 800*480; n++)
		{
			*(FI.mmap_lcd_star+n) = 0x00000000;
		}

		FI.bmp_fd_1 = open("/root/photo/bmpimages/one.bmp",OPEN_BMP_FLAG); //打开图片
		if(FI.bmp_fd_1 == -1)
		{
			perror("open bmp1:");

			return -1;
		}
		FI.bmp_fd_2 = open("/root/photo/bmpimages/two.bmp",OPEN_BMP_FLAG); //打开图片
		if(FI.bmp_fd_2 == -1)
		{
			perror("open bmp2:");

			return -1;
		}
		FI.bmp_fd_3 = open("/root/photo/bmpimages/three.bmp",OPEN_BMP_FLAG); //打开图片
		if(FI.bmp_fd_3 == -1)
		{
			perror("open bmp3:");

			return -1;
		}
		FI.bmp_star = (char *)malloc(BMP_LENGTH*(sizeof(char)));
		if(FI.bmp_star == NULL)
		{
			perror("malloc bmp:");

			return -1;
		}

		return 0;
	}

	//关闭文件
	int Free_Close()
	{
		close(FI.lcd_fd);
		close(FI.bmp_fd_1);
		close(FI.bmp_fd_2);
		close(FI.bmp_fd_3);

		munmap(FI.mmap_lcd_star,MAP_LENGTH);

		free(FI.bmp_star);

		return 0;
	}

	//映射图片函数
	int Display_Bmp(int fd) //这里形参为图片的路径
	{
		// FI.bmp_fd_1 = open("/root/photo/bmpimages/one.bmp",OPEN_BMP_FLAG); //打开图片
		// FI.bmp_fd_2 = open("/root/photo/bmpimages/two.bmp",OPEN_BMP_FLAG); //打开图片
		// FI.bmp_fd_3 = open("/root/photo/bmpimages/three.bmp",OPEN_BMP_FLAG); //打开图片
		// if(FI.bmp_fd_1 == -1 || FI.bmp_fd_2 == -1 || FI.bmp_fd_3 == -1)
		// {
		// 	perror("open bmp:");

		// 	return -1;
		// }
		

		// FI.bmp_star = (char *)malloc(BMP_LENGTH*(sizeof(char)));
		// if(FI.bmp_star == NULL)
		// {
		// 	perror("malloc bmp:");

		// 	return -1;
		// }

		lseek(fd,BMP_OFFSET,SEEK_SET); //偏移前54个字节再读取（bmp图片前54位不是数据位）
		read(fd,FI.bmp_star,BMP_LENGTH); //读取图片的像素信息并存入bmp_star指针变量中
		//循环映射赋值
		int x,y;
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800; x++)
			{
				*(FI.mmap_lcd_star+800*(479-y)+x) = FI.bmp_star[3*(800*y+x)] << 0 | FI.bmp_star[(3*(800*y+x))+1] << 8 | FI.bmp_star[(3*(800*y+x))+2] << 16;
			}
			usleep(500); //单位为毫秒
		}

		return 0;
	}

	//触摸屏函数
	int Get_Touch_Xy(P_DL head)
	{
		int x,y,x1,y1,x2,y2;
		int flag=0,flag1=0;
		P_DL tmp_node = head;
		while(1)
		{
			read(FI.touch,&(FI.touch),sizeof(FI.touch)); //读取触摸屏接收的信息并且存入系统提供的结构体当中
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X) //判断触摸屏读取的值是否为 x
			{
				x = FI.touch.value;
				x = 800*x/1024; //按比例转换为800*480内的像素坐标 该触摸屏原来的像素为1024*600
			}
			if(FI.touch.type == EV_ABS && FI.touch.code == ASB_Y) // 判断触摸屏读取的值是否为 y
			{
				y = FI.touch.value;
				y = 480*y/600; //按比例转换为800*480内的像素坐标 该触摸屏原来的像素为1024*600
			}
			//手按下去的时候记录x的坐标
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value != 0)
			{
				x1 = x;
				flag = 1;

				//printf("%d\n", __LINE___);
				//printf("x=%d y=%d\n", x,y);
			}
			//手离开触摸屏时再记录一次x的坐标
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value == 0)
			{
				x2 = x;
				flag1 = 1;
				//printf("%d\n", __LINE___);
				//printf("x=%d y=%d\n", x,y);
			}
			if(flag && flag1)
			{
				if(x2 > x1)
				{
					if(tmp_node->next == head) //如果下一个为头结点，则变为头结点
					{
						tmp_node = head;
					}
				

				tmp_node = tmp_node->next;
				Display_Bmp(tmp_node->data); //如果右滑则为第一张图片
				//tmp_node = tmp_node->prev;
				flag = flag1 = 0;
				}
				else if(x2 < x1)
				{
					if(tmp_node->prev == head) //如果上一个为头结点，则变为头结点
					{
						tmp_node = head;
					}

				tmp_node = tmp_node->prev;
				Display_Bmp(tmp_node->data); //左滑第二张图片
				//tmp_node = tmp_node->prev;
				flag = flag1 = 0;
				}
				else
				{
					flag = flag1 = 0;
					continue;
				}
			}
		}

		return 0;
	}

	int main(int argc, char const *argv[])
	{
		Init_Info();

		P_DL head = Cread_Head();
		Cread_Node(head,FI.bmp_fd_1);
		Cread_Node(head,FI.bmp_fd_2);
		Cread_Node(head,FI.bmp_fd_3);

		//Display_Bmp("/root/photo/bmpimages/one.bmp");

		Get_Touch_Xy(head);

		Free_Close();

		return 0;
	}


	#include <stdio.h>
	#include <errno.h>
	#include <stdlib.h>
	#include <sys/stat.h>
	#include <sys/types.h>
	#include <sys/mman.h>
	#include <fcntl.h>
	#include <unistd.h>
	#include <string.h>
	#include <linux/input.h>

	#define LCD_DEV_PATH    "/dev/fb0"
	#define TOUCH_DEV_PATH  "/dev/input/event0"
	#define OPEN_TOUCH_FLAG O_RDONLY
	#define OPEN_LCD_FLAG   O_RDWR
	#define MAP_LENGTH      800*480*4
	#define MAP_OFFSET      0

	struct files_info
	{
	  int lcd_fd,touch_fd;
	  int * mmap_lcd_star;
	  int black_color;
	  int touch_x,touch_y;
	  //int x,y;
	  struct input_event touch; //定义输入子系统提供的结构体变量存放读取到的触摸屏信息
	}FI;

	int Init_Info();
	int Free_Close();
	int Get_Touch_Xy();
	//int Display_W();
	int Clean();
	int Display_R();
	int Display_G();
	int Display_B();
	int Display_Block();
	int Move_Block();

	int Init_Info()
	{
		FI.lcd_fd = open(LCD_DEV_PATH,OPEN_LCD_FLAG);
		FI.touch_fd = open(TOUCH_DEV_PATH,OPEN_TOUCH_FLAG);
		if(FI.lcd_fd == -1 || FI.touch_fd == -1)
		{
			perror("open lcd or touch:");

			return -1;
		}

		FI.mmap_lcd_star = (int *)mmap(NULL,MAP_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,FI.lcd_fd,MAP_OFFSET);
		if(FI.mmap_lcd_star == MAP_FAILED)
		{
			perror("mmap lcd:");

			return -1;
		}

		FI.black_color = 0x00ffffff;

		return 0;
	}

	int Free_Close()
	{
		close(FI.lcd_fd);
		close(FI.touch_fd);

		munmap(FI.mmap_lcd_star,MAP_LENGTH);

		return 0;
	}

	//int Display_W()
	int Clean() //清屏幕 显示白色
	{
		lseek(FI.lcd_fd,0,SEEK_SET);
		int x,y;
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800; x++)
			{
				*(FI.mmap_lcd_star+800*y+x) = FI.black_color;
			}
		}

		return 0;
	}

	int Display_R()
	{
		int x,y;
		for( y = 0; y < 480; y++)
		{
			for(x = 0; x < 800/3; x++)
			{
				*(FI.mmap_lcd_star+800*y+x) = 0x00ff0000;
			}
		}

		return 0;
	}

	int Display_G()
	{
		int new_lcd_star = FI.mmap_lcd_star+800/3+1;
		int x,y;
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800/3; x++)
			{
				*(new_lcd_star+800*y+x) = 0x0000ff00;
			}
		}

		return 0;
	}

	int Display_B()
	{
		int new_lcd_star = FI.mmap_lcd_star+2*800/3+1;
		int x,y;
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800/3; x++)
			{
				*(new_lcd_star+800*y+x) = 0x000000ff;
			}
		}

		return 0;
	}

	int Display_Block(int site_x,int color)
	{
		int new_lcd_star = FI.mmap_lcd_star; //防止映射的首地址发生改变
		new_lcd_star += (480/2-50/2-1)*800 + site_x; //跳到指定的地方显示图形  相对手触位置左偏移了35

		if(new_lcd_star > FI.mmap_lcd_star+214*800+800-70) //右边界
		{
			new_lcd_star = FI.mmap_lcd_star+214*800+799-70; //超出范围则固定左地址不变
		}
		if(new_lcd_star < FI.mmap_lcd_star+214*800) //左边界
		{
			new_lcd_star = FI.mmap_lcd_star+214*800; //超出范围则固定右地址不变
		}

		int x,y;
		for(y = 0; y < 50; y++) //方块显示
		{
			for(x = 0; x < 70; x++)
			{
				*(new_lcd_star + 800*y+x) = color;
			}
		}

		return 0;
	}

	int Get_Touch_Xy()
	{
		while(1)
		{
			read(FI.touch_fd,&(FI.touch),sizeof(FI.touch));
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X)
				FI.touch_x = FI.touch.value;
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_Y)
				FI.touch_y = FI.touch.value;
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value != 0)
			{
				//printf("ddd\n");
				if(800*FI.touch_x/1024 > 0 && 800*FI.touch_x/1024 < 800/3)
					Display_R();
				if(800*FI.touch_x/1024 > 800/3 && 800*FI.touch_x/1024 < 2*800/3)
					Display_G();
				if(800*FI.touch_x/1024 > 2*800/3 && 800*FI.touch_x/1024 < 800)
					Display_B();

			}
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value == 0) //手松开时让屏幕显示白色
			{
				// if(800*FI.touch_x/1024 > 0 && 800*FI.touch_x/1024 < 800/3)
				// 	Display_W();
				// if(800*FI.touch_x/1024 > 800/3 && 800*FI.touch_x/1024 < 2*800/3)
				// 	Display_W();
				// if(800*FI.touch_x/1024 > 2*800/3 && 800*FI.touch_x/1024 < 800)
				// 	Display_W();
				//Display_W();
				Clean();
			}
		}

		return 0;
	}


	int Display_Line(int site_x,int site_y,int color)
	{
		int n;
		for(n = 0; n < 800; n++) //横
		{
			*(FI.mmap_lcd_star+800*site_y+n) = color;
		}
		for(n = 0; n < 480; n++) // 纵
		{
			*(FI.mmap_lcd_star+800*n+site_x) = color;
		}

		return 0;
	}

	int Move_Line()
	{
		int x_tm,y_tm;
		int flag = 0;
		Display_W();
		while(1)
		{
			read(FI.touch_fd,&(FI.touch),sizeof(FI.touch));
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X)
			{
				FI.touch_x = FI.touch.value;
				FI.touch_x = 800*FI.touch_x/1024;
			}
			if(FI.touch.type == EV_ABS &&FI.touch.code == ABS_Y)
			{
				FI.touch_y = FI.touch.value;
				Fi.touch_y = 480*FI.touch_y/600;
			}
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value != 0)
			{
				//flag = 1;
				Display_Line(FI.touch_x,FI.touch_y,0x000000ff);
			}
			//printf("%d----%d\n", x,y);

			// if(flag)
			// {
			// 	Display_Line(FI.touch_x,FI.touch_y,0x000000ff);
			// }
			if(x_tm != FI.touch_x || y_tm != FI.touch_y)
			{
				Display_Line(x_tm,y_tm,0x00ffffff);
			}

			//printf("%d----%d\n", x,y);

			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value == 0)
			{
				Display_W();
				//flag = 0;
			}

			x_tm = FI.touch_x;
			y_tm = FI.touch_y;

		}
	}

	int Move_Block()
	{
		int x_tm; //存放上一个触点 x 的值，以便清除上一个方块移动的痕迹
		int flag = 0; // 标志是否开始移动

		Clean(); //清白屏
		Display_Block(0,0x000000ff); //显示初始方块点

		int n;
		for(n = 0; n < 800; n++) //上下边界线
		{
			*(FI.mmap_lcd_star + n + (480/2-50/2-2)*800) = 0x00000000; //上线
			*(FI.mmap_lcd_star + n + (480/2-50/2-1)*800) = 0x00000000; // 下线
		}

		while(1)
		{
			read(FI.touch_fd,&(FI.touch),sizeof(FI.touch)); //读触屏结构体文件
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X) //取触屏点x转换成800*480的分辨率
			{
				FI.touch_x = FI.touch.value;
				FI.touch_x = 800*FI.touch_x/1024;
			}
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_Y) //取触屏点 y转换成800*480的分辨率
			{
				FI.touch_y = FI.touch.value;
				FI.touch_y = 480*FI.touch_y/600;
			}

			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH FI.touch.value != 0) //判断触屏压力值 手是否按下
			{ 
				if(FI.touch_y > (480/2-50/2-2) && FI.touch_y < (480/2+50/2-1) && FI.touch_x < 70) //判断开始时触点是否在方块内，若在才能移动
				{
					Display_Block(0,FI.mmap_lcd_star,0x00ffffff); //清除初始方块
					flag = 1; //启动移动标志位
					//Display_Block(FI.touch_x-35,0x000000ff); //移动方块
				}
			}
			//printf("%d---%d\n", FI.touch_x,FI.touch_y);

			if(flag)
			{
				Display_Block(FI.touch_x-35,0x000000ff); //移动方块
			}
			if(x_tm != FI.touch_x)
			{
				Display_Block(x_tm-35,0x00ffffff); //判断是否需要清除移动痕迹
			}

			//printf("%d----%d\n", FI.touch_x,FI.touch_y);

			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value == 0) //判断手是否松开了触摸屏 松开了则滑块需要返回起始位置
			{
				Display_Block(x-35,0x00ffffff); //清除最后松手的方块
				Display_Block(0,0x000000ff); // 方块自动回归到初始位置
				flag = 0; //关闭移动
			}

			x_tm = FI.touch_x; //保存上一个触点 x的值
		}

		return 0;
	}

	int main(int argc, char const *argv[])
	{
		Init_Info();
		//Get_Touch_Xy();

		//Move_Line();

		Move_Block();

		Free_Close();

		return 0;
	}


	#include <stdio.h>
	#include <errno.h>
	#include <stdlib.h>
	#include <sys/stat.h>
	#include <sys/types.h>
	#include <sys/mman.h>
	#include <fcntl.h>
	#include <unistd.h>
	#include <string.h>
	#include <linux/input.h>

	#define LCD_DEV_PATH    "/dev/fb0"
	#define TOUCH_DEV_PATH  "/dev/input/event0"
	#define OPEN_BMP_FLAG   O_RDONLY
	#define OPEN_TOUCH_FLAG O_RDONLY
	#define OPEN_LCD_FLAG   O_RDWR
	#define BMP_OFFSET      54
	#define MAP_LENGTH      800*480*4
	#define MALLOC_RGB      800*400*3
	#define MAP_OFFSET      0

	struct files_info
	{
	  int lcd_fd,touch_fd,bmp_fd;  //全局变量
	  int * mmap_lcd_star;
	  int skip;
	  int bmp_w,bmp_h;
	  int black_color;
	  int touch_x,touch_y;
	  char r,g,b;
	  int rgb;
	  //int x,y;
	  struct input_event touch; //定义输入子系统提供的结构体变量存放读取到的触摸屏信息
	}FI;

	int Init_Info();
	int Free_Close();
	int Get_Touch_Xy();
	int Get_Touch_X_Y();
	//int Display_W();
	int Clean();
	int Display_R();
	int Display_G();
	int Display_B();
	int Display_Block();
	int Move_Block();
	int Display_Site(int site_x,int site_y,int color); //指定位置映射（小长方体体积固定）
	int Display_Bmp(char *bmp_path,int site_x,int site_y); //形参只需传指定刷图的位置，图片的w和h不需要传，可以通过read读取
	int Dis_Corss(int site_x,int site_y); //显示任意大小任意颜色矩形快
	int Move_Sliding_Block();


	int Init_Info()
	{
		FI.lcd_fd = open(LCD_DEV_PATH,OPEN_LCD_FLAG);
		FI.touch_fd = open(TOUCH_DEV_PATH,OPEN_TOUCH_FLAG);
		if(FI.lcd_fd == -1 || FI.touch_fd == -1)
		{
			perror("open lcd or touch:");

			return -1;
		}

		FI.mmap_lcd_star = (int *)mmap(NULL,MAP_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,FI.lcd_fd,MAP_OFFSET);
		if(FI.mmap_lcd_star == MAP_FAILED)
		{
			perror("mmap lcd:");

			return -1;
		}

		// int n;
		// for(n = 0; n < MAP_LENGTH; n++)
		// {
		// 	*(FI.mmap_lcd_star+n) = 0x00ffffff;
		// }

		// int *new_lcd_star = FI.mmap_lcd_star + 800*213;
		// int *new_lcd_star_1 = FI.mmap_lcd_star + 800*266;

		// int n,m;
		// for(n = 0; n < 800; n++) //(214,0)
		// {
		// 	*(new_lcd_star + n) = 0x00000000;
		// }
		// for(m = 0; m < 800; m++) //(266,0)
		// {
		// 	*(new_lcd_star_1 + m) = 0x00000000;
		// }

		FI.black_color = 0x00ffffff;

		return 0;
	}

	int Free_Close()
	{
		close(FI.lcd_fd);
		close(FI.touch_fd);

		munmap(FI.mmap_lcd_star,MAP_LENGTH);

		return 0;
	}

	//int Display_W()
	int Clean() //清屏幕 显示白色
	{
		lseek(FI.lcd_fd,0,SEEK_SET);
		int x,y;
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800; x++)
			{
				*(FI.mmap_lcd_star+800*y+x) = FI.black_color;
			}
		}

		return 0;
	}

	int Display_R()
	{
		int x,y;
		for( y = 0; y < 480; y++)
		{
			for(x = 0; x < 800/3; x++)
			{
				*(FI.mmap_lcd_star+800*y+x) = 0x00ff0000;
			}
		}

		return 0;
	}

	int Display_G()
	{
		int *new_lcd_star = FI.mmap_lcd_star+800/3+1;
		int x,y;
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800/3; x++)
			{
				*(new_lcd_star+800*y+x) = 0x0000ff00;
			}
		}

		return 0;
	}

	int Display_B()
	{
		int *new_lcd_star = FI.mmap_lcd_star+2*800/3+1;
		int x,y;
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800/3; x++)
			{
				*(new_lcd_star+800*y+x) = 0x000000ff;
			}
		}

		return 0;
	}

	int Display_Block(int site_x,int color)
	{
		int *new_lcd_star = FI.mmap_lcd_star; //防止映射的首地址发生改变
		new_lcd_star += (480/2-50/2-1)*800 + site_x; //跳到指定的地方显示图形  相对手触位置左偏移了35

		if(new_lcd_star > FI.mmap_lcd_star+214*800+800-70) //右边界
		{
			new_lcd_star = FI.mmap_lcd_star+214*800+799-70; //超出范围则固定左地址不变
		}
		if(new_lcd_star < FI.mmap_lcd_star+214*800) //左边界
		{
			new_lcd_star = FI.mmap_lcd_star+214*800; //超出范围则固定右地址不变
		}

		int x,y;
		for(y = 0; y < 50; y++) //方块显示
		{
			for(x = 0; x < 70; x++)
			{
				*(new_lcd_star + 800*y+x) = color;
			}
		}

		return 0;
	}

	int Display_Bmp(char *bmp_path, int site_x,int site_y)
	{
		 /*根据形参中的int where_x,int where_y来确定映射指针开始赋值像素的位置*/
		int *new_lcd_star = FI.mmap_lcd_star+(800*site_y+site_x);
		//int bmp_fd = open(bmp_path,OPEN_BMP_FLAG);
		FI.bmp_fd = open(bmp_path,OPEN_BMP_FLAG);
		if(FI.bmp_fd == -1)
		{
			perror("open bmp:");

			return -1;
		}

		lseek(FI.bmp_fd,18,SEEK_SET); //跳到w对应的字节偏移的位置
		read(FI.bmp_fd,&(FI.bmp_w),4); //先读取54头字节中的图片的w和h
		read(FI.bmp_fd,&(FI.bmp_h),4); //先读取54头字节中的图片的w和h
		//printf("bmp的w:%d---h:%d\n",I->bmp_w,I->bmp_h);
		//判断图片的宽度的字节数是不是4的倍数
		if(((FI.bmp_w)*3)%4 != 0)
		{
			//求补数
			FI.skip = 4-(((FI.bmp_w)*3)%4);
		}
		else
		{
			FI.skip = 0;
		}

		//图片文件先跳过前54个字节
		lseek(FI.bmp_fd,BMP_OFFSET,SEEK_SET);
		int malloc_length = (FI.bmp_w*FI.bmp_h*3)+(FI.skip*FI.bmp_h);
		char * RGB_BUF = (char *)malloc(malloc_length*(sizeof(char)));


		if(RGB_BUF == NULL)
		{
			perror("malloc rgb:");

			return -1;
		}

		//读取像素点
		read(FI.bmp_fd,RGB_BUF,malloc_length);

		//循环有规律的把像素点赋值给映射指针
		int x,y,z=0;
		for(y = 0; y < FI.bmp_h; y++)
		{
			for(x = 0; x < FI.bmp_w; x++,z+=3)
			{
				// *(new_p+800*(FI.bmp_h-1-y)+x) = RGB_BUF[(3*(FI.bmp_w*y+x))]<<0  | RGB_BUF[(3*(FI.bmp_w*y+x))+1]<<8 | RGB_BUF[(3*(FI.bmp_w*y+x))+2]<<16;
				*(new_lcd_star+800*(FI.bmp_h-1-y)+x) = RGB_BUF[z] << 0 | RGB_BUF[z+1] << 8 || RGB_BUF[z+2] << 16;
			}
			z+=FI.skip; // 这里刚刚结束了一行颜色的显示，想要跳系统的补的的数据
		}

		return 0;
	}

	int Get_Image_Touch()
	{
		while(1)
		{
			read(FI.touch_fd,&(FI.touch),sizeof(FI.touch));
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X)
			{
				FI.touch_x = FI.touch.value;
				FI.touch_x = 800*FI.touch_x/1024;
			}
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_Y)
			{
				FI.touch_y = FI.touch.value;
				FI.touch_y = 480*FI.touch_y/600;
			}
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value != 0)
			{
				if(FI.touch_x-1 < FI.touch_x)
				{
					Display_Bmp("/root/photo/bmpimages/",0,0);
				}
				else if(FI.touch_x+1 > FI.touch_x)
				{
					Display_Bmp("/root/photo/bmpimages/",0,0);
				}
			}
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value == 0)
			{
				Display_Bmp("/root/photo/bmpimages/",0,0);
			}
		}
		
		return 0;
	}

	int Get_Touch_Xy()
	{
		while(1)
		{
			read(FI.touch_fd,&(FI.touch),sizeof(FI.touch));
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X)
			{
				FI.touch_x = FI.touch.value;
			}
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_Y)
			{
				FI.touch_y = FI.touch.value;
			}
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value != 0)
			{
				//printf("ddd\n");
				if(800*FI.touch_x/1024 > 0 && 800*FI.touch_x/1024 < 800/3)
					Display_R();
				if(800*FI.touch_x/1024 > 800/3 && 800*FI.touch_x/1024 < 2*800/3)
					Display_G();
				if(800*FI.touch_x/1024 > 2*800/3 && 800*FI.touch_x/1024 < 800)
					Display_B();

			}
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value == 0) //手松开时让屏幕显示白色
			{
				// if(800*FI.touch_x/1024 > 0 && 800*FI.touch_x/1024 < 800/3)
				// 	Display_W();
				// if(800*FI.touch_x/1024 > 800/3 && 800*FI.touch_x/1024 < 2*800/3)
				// 	Display_W();
				// if(800*FI.touch_x/1024 > 2*800/3 && 800*FI.touch_x/1024 < 800)
				// 	Display_W();
				//Display_W();
				Clean();
			}
		}

		return 0;
	}

	int Get_Touch_X_Y()
	{
		while(1)
		{
			read(FI.touch_fd,&(FI.touch),sizeof(FI.touch));
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X)
			{
				FI.touch_x = FI.touch.value;
				FI.touch_x = 800*FI.touch_x/1240;
			}
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_Y)
			{
				FI.touch_y = FI.touch.value;
				FI.touch_y = 480*FI.touch_y/600;
			}
			//if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value != 0)
			if(FI.touch_x - 50 < 0 || FI.touch_x + 50 > 799 || FI.touch_y - 50 < 0 || FI.touch_y + 50 > 479)
				continue;
			Dis_Corss(FI.touch_x,FI.touch_y);

		}

		return 0;
	}

	int Dis_Corss(int site_x,int site_y)
	{
		int x,y;
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800; x++)
			{
				if(x == FI.touch_x || y = FI.touch_y)
				{
					*(FI.mmap_lcd_star+800*y+x) = 0xff0000;
				}
				else
				{
					*(FI.mmap_lcd_star+800*y+x) = FI.black_color;
				}
			}
		}


		return 0;
	}

	int Dis_Corss(int site_x,int site_y)
	{
		int x,y;
		for(y = 0; y < 480; y++)
		{
			for(x == FI.touch_x - 50; x < FI.touch_x + 50; x ++)
			{
				if(x == FI.touch_x || y == FI.touch_y)
				{
					*(FI.mmap_lcd_star+800*y+x) = 0xff0000;
				}
				else
				{
					*(FI.mmap_lcd_star+800*y+x) = FI.black_color;
				}
			}
		}

		for(x = 0; x < 800; x++)
		{
			for(y = FI.touch_y - 50; y < FI.touch_y + 50; y++)
			{
				if(x == FI.touch_x || y == FI.touch_y)
				{
					*(FI.mmap_lcd_star+800*y+x) = 0xff0000;
				}
				else
				{
					*(FI.mmap_lcd_star+800*y+x) = FI.black_color;
				}
			}
		}

		return 0;
	}

	// 	for(y = FI.touch_y - 50; y < FI.touch_y + 50; y++)
	// 	{
	// 		for(x = 0; x < 800; x++)
	// 		{
	// 			if(x == FI.touch_x || y == FI.touch_y)
	// 			{
	// 				*(FI.mmap_lcd_star+800*y+x) = 0xff0000;
	// 			}
	// 			else
	// 			{
	// 				*(FI.mmap_lcd_star+800*y+x) = FI.black_color;
	// 			}
	// 		}
	// 	}

	// 	return 0;
	// }


	int Display_Line(int site_x,int site_y,int color)
	{
		int n;
		for(n = 0; n < 800; n++) //横
		{
			*(FI.mmap_lcd_star+800*site_y+n) = color;
		}
		for(n = 0; n < 480; n++) // 纵
		{
			*(FI.mmap_lcd_star+800*n+site_x) = color;
		}

		return 0;
	}

	int Move_Line()
	{
		int x_tm,y_tm;
		int flag = 0;
		Display_W();
		while(1)
		{
			read(FI.touch_fd,&(FI.touch),sizeof(FI.touch));
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X)
			{
				FI.touch_x = FI.touch.value;
				FI.touch_x = 800*FI.touch_x/1024;
			}
			if(FI.touch.type == EV_ABS &&FI.touch.code == ABS_Y)
			{
				FI.touch_y = FI.touch.value;
				Fi.touch_y = 480*FI.touch_y/600;
			}
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value != 0)
			{
				//flag = 1;
				Display_Line(FI.touch_x,FI.touch_y,0x000000ff);
			}
			//printf("%d----%d\n", x,y);

			// if(flag)
			// {
			// 	Display_Line(FI.touch_x,FI.touch_y,0x000000ff);
			// }
			if(x_tm != FI.touch_x || y_tm != FI.touch_y)
			{
				Display_Line(x_tm,y_tm,0x00ffffff);
			}

			//printf("%d----%d\n", x,y);

			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value == 0)
			{
				Display_W();
				//flag = 0;
			}

			x_tm = FI.touch_x;
			y_tm = FI.touch_y;

		}
	}

	int Move_Block()
	{
		int x_tm; //存放上一个触点 x 的值，以便清除上一个方块移动的痕迹
		int flag = 0; // 标志是否开始移动

		Clean(); //清白屏
		Display_Block(0,0x000000ff); //显示初始方块点

		int n;
		for(n = 0; n < 800; n++) //上下边界线
		{
			*(FI.mmap_lcd_star + n + (480/2-50/2-2)*800) = 0x00000000; //上线
			*(FI.mmap_lcd_star + n + (480/2-50/2-1)*800) = 0x00000000; // 下线
		}

		while(1)
		{
			read(FI.touch_fd,&(FI.touch),sizeof(FI.touch)); //读触屏结构体文件
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X) //取触屏点x转换成800*480的分辨率
			{
				FI.touch_x = FI.touch.value;
				FI.touch_x = 800*FI.touch_x/1024;
			}
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_Y) //取触屏点 y转换成800*480的分辨率
			{
				FI.touch_y = FI.touch.value;
				FI.touch_y = 480*FI.touch_y/600;
			}

			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH FI.touch.value != 0) //判断触屏压力值 手是否按下
			{ 
				if(FI.touch_y > (480/2-50/2-2) && FI.touch_y < (480/2+50/2-1) && FI.touch_x < 70) //判断开始时触点是否在方块内，若在才能移动
				{
					Display_Block(0,FI.mmap_lcd_star,0x00ffffff); //清除初始方块
					flag = 1; //启动移动标志位
					//Display_Block(FI.touch_x-35,0x000000ff); //移动方块
				}
			}
			//printf("%d---%d\n", FI.touch_x,FI.touch_y);

			if(flag)
			{
				Display_Block(FI.touch_x-35,0x000000ff); //移动方块
			}
			if(x_tm != FI.touch_x)
			{
				Display_Block(x_tm-35,0x00ffffff); //判断是否需要清除移动痕迹
			}

			//printf("%d----%d\n", FI.touch_x,FI.touch_y);

			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value == 0) //判断手是否松开了触摸屏 松开了则滑块需要返回起始位置
			{
				Display_Block(x-35,0x00ffffff); //清除最后松手的方块
				Display_Block(0,0x000000ff); // 方块自动回归到初始位置
				flag = 0; //关闭移动
			}

			x_tm = FI.touch_x; //保存上一个触点 x的值
		}

		return 0;
	}

	int Display_Site(int site_x,int site_y,int color); //指定位置映射（小长方体体积固定）
	{
		//初始小方块的位置
		int *new_lcd_star = FI.mmap_lcd_star + 800*site_y+site_x;
		//小方块定义的宽和长
		int w,h;
		for(h = 0; h < 50; h++)
		{
			for(w = 0; w < 100; w++)
			{
				*(new_lcd_star+800*h+w) = color;
			}
		}

		return 0;
	}

	int Move_Sliding_Block()
	{
		int x_tm,y_tm;
		int flag;
		Display_Site(0,215,0x00ff00ff);
		while(1)
		{
			read(FI.touch_fd,&(FI.touch),sizeof(FI.touch));
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_X)
			{
				FI.touch_x = FI.touch.value;
				FI.touch_x = 800*FI.touch_x/1024;
			}
			if(FI.touch.type == EV_ABS && FI.touch.code == ABS_Y)
			{
				FI.touch_y = FI.touch.value;
				FI.touch_y = 480*FI.touch_y/600;
			}
			//按下屏幕则把初始位置画为白色
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value != 0)
			{
				if(FI.touch_y > 213 && FI.touch_y < 266 && (FI.touch_x - 50) > 0 && (FI.touch_x + 50) < 800)
				{
					Display_Site(0,215,0x00ffffff);
					flag = 1;

					printf("x=%d y=%d\n", FI.touch_x,FI.touch_y);
				}
			}
			printf("x=%d y=%d\n", FI.touch_x,FI.touch_y);
			//if(flag && FI.touch_y>213 && FI.touch_y<266 && (FI.touch_x - 50) > 0 && (FI.touch_x + 50) < 800)
			if(flag)
			{
				if(FI.touch_y > 213 && FI.touch_y < 266 && (FI.touch_x - 50) > 0 && (FI.touch_x + 50) < 800)
				{
					Display_Site(FI.touch_x - 50,215,0x00ff00ff);

					//printf("x=%d y=%d\n", FI.touch_x,FI.touch_y);
				}
			}
			if(FI.touch_x + 50 > 800)
			{
				Display_Site(0,215,0x00ff00ff)
			}
			if((x_tm != FI.touch_x || y_tm != FI.touch_y) && flag == 1)
			{
				Display_Site(x_tm - 50,215,0x00ffffff); //与原来的x,y不同时 清除滑块轨迹
			}
			//手松开时
			if(FI.touch.type == EV_KEY && FI.touch.code == BTN_TOUCH && FI.touch.value == 0 || FI.touch_y > 266 || FI.touch_y < 213)
			{
				Display_Site(FI.touch_x - 50;215,0x00ffffff);
				//printf("%d\n",__LINE__);
				Display_Site(0,215,0x00ff00ff);
				//printf("x - 50=%d\n", FI.touch_x - 50);

				flag = 0;
			}

			x_tm = FI.touch_x;
			y_tm = FI.touch_y;
		}

		return 0;
	}

	int main(int argc, char const *argv[])
	{
		Init_Info();
		//Get_Touch_Xy();

		//Move_Line();
		//Display_Bmp("/root/photo/bmpimages/",0,0);

		//Move_Block();

		Free_Close();

		return 0;
	}

## 省钱利器：
![](/hexo-private-blog-website/images/淘宝客29.jpg)
![](/hexo-private-blog-website/images/淘宝客30.jpg)
![](/hexo-private-blog-website/images/淘宝客31.jpg)


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
![wechat.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggvv58jjj30u010sgnq.jpg)
### 财付通打赏：
![qq.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggwd0rvjj32ai2lxdrm.jpg)
