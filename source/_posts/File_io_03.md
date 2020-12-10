---
title: File_io_03
date: 2020-09-01 17:32:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	望门投止思张俭，忍死须臾待杜根。
	我自横刀向天笑，去留肝胆两昆仑。

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


# 一作业：

​	

```
显示一行汉字：
		1：每个汉字的分辨率：16X16
		2：每次显示的汉字的左上角坐标（x+=18 y不变）
		3：x+=18 大于 799 需要换行显示（y+=18  x=0）
		
		4: y+=18 大于 479 不能显示（指定坐标有误）
		
代码思路：
		已完成的代码接口：显示一个汉字（从x=0，y=0的位置开始显示）
	
未完成的接口：
		把之前显示汉字接口改成能在任意位置显示的接口
		
		
		
		
```

# 二、Linux下触摸屏应用程序编码

```
1、常见的设备分类：
		输入设备：触摸屏、键盘、麦克风...
		输出设备：耳机、显示器。。。
		
2、Linux下，是怎么在内核层统一控制管理输入设备？
		使用一个系统机制（输入子系统）来控制管理Linux下所有输入设备。
		LCD    /dev/fb0
		touch  /dev/input/event0
		
		输入子系统提供一个结构体类型给应用层存放读取到的触摸信息（x,y压力值）
		找出结构体的类型：
						/usr/include/linux/input.h ,里面有一个结构体类型
						struct input_event;
						
						struct input_event {
                                                struct timeval time; //系统提供的时间结构体
                                                __u16 type; unsigned short
                                                __u16 code;
                                                __s32 value; signed long 或者 signed int
                                            };
                                            
          type:
       分类：输入设备的类型 触摸屏：EV_ABS (宏定义 数字) 要type等于EV_ABS时，截取到的信息才是触摸屏的
       	  code：
			输入设备产生的信息分类：
								x：ABS_X    y:ABS_Y  压力值:type==EV_KEY code == BTN_TOUCH
								
		  value:存放具体的数据值：
代码流程：
		目的：获取到每次触碰的平面坐标。拿去判断即可。
		已知：每次触摸的平面坐标会被触摸屏驱动存放在/dev/input/event0。
		
    int Get_Touch_Xy()
    {
      int touch_fd = open(TOUCH_DEV_PATH,OPEN_FLAG);
      if(touch_fd == -1)
      {
        perror("open touch");
        return -1;
      }

      int x,y;//定义存放触摸屏平面坐标的变量
      while(1)
      {
        read(touch_fd,&touch,sizeof(touch)); //readn读取的时特殊的设备结点文件，会堵塞读取
        //先在结构体里面赛选x坐标
        if(touch.type == EV_ABS && touch.code == ABS_X) x= touch.value;//先判断是不是触摸屏设备 && 在判断产生的信息是不是X坐标
        if(touch.type == EV_ABS && touch.code == ABS_Y) y= touch.value;
		
		if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value == 0)
        {
        	printf("x:%d----y:%d\n",x,y);//输出的是离开触摸屏那一时刻的x y 
        }

      }

      close(touch_fd);

      return 0;
    }
```

2）判断手是否离开触摸屏

```
//先判断是否是按钮的输入设备，再判断是不是触摸屏的按钮，再判断压力值是否为0（touch.value == 0）
if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value == 0)


注:
	如果触摸屏驱动设置的坐标范围和显示器分辨率的大小不一样
		触摸屏坐标范围：x:1024  y:600 
		分辨率：  x:800   y:480
		
	例子：
		用的是黑色的板子：
				读取到的xy坐标为 touch_x  touch_y x:1024  y:600 
				
							touch_x/1024 :x坐标再1024中占的百分比
							800*touch_x/1024:根据百分比求出800比例的x
							480*touch_y/600：根据百分比求出480比例的y
							
```

练习：

​	1）

​		把纵方向屏幕分层三个部分，一开始全屏白色，触碰每一个格子（手没松开显示 R G B），手松开每个格子显示	白色

​		封装：

​				一个任意位置，任意颜色的接口（大小固定）`一个任意位置，显示白色的接口（大小固定的）`

​				一个触摸屏坐标读取的接口（加判断手是否松开）

​			`松开的时候判断读取到的坐标范围，再来决定要在哪个格子上刷颜色`

初始化：

​				打开文件的次数：1024 - 3

​				映射的次数



2）

​	实现十字校准线





作业：

​		滑动的木板（固定水平面滑动）

​		总结



下周：标准IO + 目录IO


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
	//定义输入子系统提供的结构体变量存放读取到的触摸屏信息

	struct inf
	{
	  int lcd_fd,touch_fd;
	  int * mmap_lcd_star;
	  int black_color;
	  int touch_x,touch_y;
	  struct input_event touch;
	}I;

	int Init();
	int Free();
	int Get_Touch_Xy();
	int Dis_Color(int where_x,int where_y,int w,int h,int color);//显示任意大小任意颜色矩形快

	int Init()
	{
	  I.lcd_fd   = open(LCD_DEV_PATH,OPEN_LCD_FLAG);
	  I.touch_fd = open(TOUCH_DEV_PATH,OPEN_TOUCH_FLAG);
	  if(I.lcd_fd == -1 || I.touch_fd == -1)
	  {
	    perror("open dev file ");
	    return -1;
	  }

	  I.mmap_lcd_star = (int *)mmap(NULL,MAP_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,I.lcd_fd,MAP_OFFSET);
	  if(I.mmap_lcd_star == MAP_FAILED)
	  {
	    perror("mmap lcd");
	    return -1;
	  }

	  I.black_color = 0x00ffffff;

	  Dis_Color(0,0,800,480,0xffffff);

	  return 0;
	}


	int Free()
	{
	  close(I.lcd_fd);
	  close(I.touch_fd);
	  munmap(I.mmap_lcd_star,MAP_LENGTH);

	  return 0;
	}


	int Get_Touch_Xy()
	{
	  while(1)
	  {
	    read(I.touch_fd,&(I.touch),sizeof(I.touch));
	    if(I.touch.type == EV_ABS && I.touch.code == ABS_X) I.touch_x = I.touch.value;
	    if(I.touch.type == EV_ABS && I.touch.code == ABS_Y) I.touch_y = I.touch.value;

	    if(I.touch.type == EV_KEY && I.touch.code == BTN_TOUCH && I.touch.value != 0)
	    {
	        if(I.touch_x > 0 && I.touch_x <266)//红色
	          Dis_Color(0,0,266,480,0xff0000);

	        if(I.touch_x > 260 && I.touch_x <532)//绿色
	          Dis_Color(266,0,266,480,0x00ff00);

	        if(I.touch_x > 532 && I.touch_x <800)//蓝色
	          Dis_Color(532,0,266,480,0x0000ff);
	    }

	    if(I.touch.type == EV_KEY && I.touch.code == BTN_TOUCH && I.touch.value == 0)
	    {
	      if(I.touch_x > 0 && I.touch_x <266)//白色
	        Dis_Color(0,0,266,480,I.black_color);

	      if(I.touch_x > 260 && I.touch_x <532)//白色
	        Dis_Color(266,0,266,480,I.black_color);

	      if(I.touch_x > 532 && I.touch_x <800)//白色
	        Dis_Color(532,0,266,480,I.black_color);
	    }

	  }

	  return 0;
	}


	int Dis_Color(int where_x,int where_y,int w,int h,int color)
	{
	  int * new_p = I.mmap_lcd_star+(800*where_y+where_x);

	  int x,y;

	  for(y=0; y<h; y++)
	  {
	    for(x=0; x<w; x++)
	    {
	      *(new_p+800*y+x) = color;
	    }
	  }

	  return 0;
	}


	int main()
	{
	  Init();

	  Get_Touch_Xy();

	  Free();

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
	//定义输入子系统提供的结构体变量存放读取到的触摸屏信息

	struct inf
	{
	  int lcd_fd,touch_fd;
	  int * mmap_lcd_star;
	  int black_color;
	  int touch_x,touch_y;
	  struct input_event touch;
	}I;

	int Init();
	int Free();
	int Get_Touch_Xy();
	int Dis_Corss(int where_x,int where_y);//显示任意大小任意颜色矩形快

	int Init()
	{
	  I.lcd_fd   = open(LCD_DEV_PATH,OPEN_LCD_FLAG);
	  I.touch_fd = open(TOUCH_DEV_PATH,OPEN_TOUCH_FLAG);
	  if(I.lcd_fd == -1 || I.touch_fd == -1)
	  {
	    perror("open dev file ");
	    return -1;
	  }

	  I.mmap_lcd_star = (int *)mmap(NULL,MAP_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,I.lcd_fd,MAP_OFFSET);
	  if(I.mmap_lcd_star == MAP_FAILED)
	  {
	    perror("mmap lcd");
	    return -1;
	  }

	  I.black_color = 0x00ffffff;

	  return 0;
	}


	int Free()
	{
	  close(I.lcd_fd);
	  close(I.touch_fd);
	  munmap(I.mmap_lcd_star,MAP_LENGTH);

	  return 0;
	}


	int Get_Touch_Xy()
	{
	  while(1)
	  {
	    read(I.touch_fd,&(I.touch),sizeof(I.touch));
	    if(I.touch.type == EV_ABS && I.touch.code == ABS_X) I.touch_x = I.touch.value;
	    if(I.touch.type == EV_ABS && I.touch.code == ABS_Y) I.touch_y = I.touch.value;


	    if(I.touch_x - 50 <0  || I.touch_x + 50 > 799 || I.touch_y - 50 <0 || I.touch_y + 50 > 479 )
	      continue;

	    Dis_Corss(I.touch_x,I.touch_y);

	  }

	  return 0;
	}


	int Dis_Corss(int where_x,int where_y)
	{
	  int x,y;
	  /*
	  for(y=0; y<480; y++)
	  {
	    for(x=0; x<800; x++)
	    {
	      if(x==I.touch_x || y==I.touch_y)
	      {
	        *(I.mmap_lcd_star+800*y+x) = 0xff0000;
	      }
	      else
	      {
	        *(I.mmap_lcd_star+800*y+x) = I.black_color;
	      }

	    }
	  }
	*/


	  for(y=0; y<480; y++)
	  {
	    for(x=I.touch_x-50; x<I.touch_x+50; x++)
	    {
	      if(x==I.touch_x || y==I.touch_y)
	      {
	        *(I.mmap_lcd_star+800*y+x) = 0xff0000;
	      }
	      else
	      {
	        *(I.mmap_lcd_star+800*y+x) = I.black_color;
	      }
	    }
	  }

	  for(y=I.touch_y-50; y<I.touch_y+50; y++)
	  {
	    for(x=0; x<800; x++)
	    {
	      if(x==I.touch_x || y==I.touch_y)
	      {
	        *(I.mmap_lcd_star+800*y+x) = 0xff0000;
	      }
	      else
	      {
	        *(I.mmap_lcd_star+800*y+x) = I.black_color;
	      }
	    }
	  }


	  return 0;
	}


	int main()
	{
	  Init();

	  Get_Touch_Xy();

	  Free();

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
	#include <linux/input.h>

	#define lcd_path "/dev/fb0"
	#define touch_path "/dev/input/event0"

	int Display_W(int fd, int *mmap_p);

	struct input_event touch;

	int Display_W(int fd, int *mmap_p)
	{	
		lseek(fd, 0, SEEK_SET);
		int i, j;
		for(i = 0; i < 480; i++)
			for(j = 0; j < 800; j++)
				*(mmap_p + 800 * i + j) = 0x00ffffff;
	}

	int Display_R(int fd, int *mmap_p)
	{	
		int i, j;
		for(i = 0; i < 480; i++)
			for(j = 0; j < 800/3; j++)
				*(mmap_p + 800 * i + j) = 0x00ff0000;
	}

	int Display_G(int fd, int *mmap_p)
	{	
		mmap_p = mmap_p + 800/3+1;
		int i, j;
		for(i = 0; i < 480; i++)
			for(j = 0; j < 800/3; j++)
				*(mmap_p + 800 * i + j) = 0x0000ff00;
	}

	int Display_B(int fd, int *mmap_p)
	{	
		mmap_p = mmap_p + 2*800/3+1;
		int i, j;
		for(i = 0; i < 480; i++)
			for(j = 0; j < 800/3; j++)
				*(mmap_p + 800 * i + j) = 0x000000ff;
	}

	int Get_Touch_Xy()
	{
		int fd = open("/dev/fb0", O_RDWR);
		int *mmap_p = (int *)mmap(NULL, 800*480*4, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
		int touch_fd = open(touch_path, O_RDWR);
		int *tem_map;
		
		int x, y;
		while(1)
		{
			tem_map = mmap_p;
			read(touch_fd, &touch, sizeof(touch));
			if(touch.type == EV_ABS && touch.code == ABS_X) x = touch.value;
			if(touch.type == EV_ABS && touch.code == ABS_Y) y = touch.value;
			if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value != 0)
			{
				printf("ddd\n");
				if(800*x/1024 < 800/3) Display_R(fd, tem_map);
				if(800*x/1024 > 800/3&&800*x/1024 < 2*800/3) Display_G(fd, tem_map);
				if(800*x/1024 > 2*800/3&&800*x/1024 < 800) Display_B(fd, tem_map);
			}
			if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value == 0)	//弹出来才给白色
			{
				Display_W(fd, tem_map);
			}
		
		}
		close(touch_fd);
		close(fd);
		munmap(mmap_p, 800*480*4);
		return 0;
	}

	int Display_line(int x, int y, int *mmap_p,int color)
	{
		int i;
		for(i = 0; i < 480; i++)	//纵	
			*(mmap_p + 800 * i + x) = color;
		for(i = 0; i < 800; i++)	//横
			*(mmap_p + 800 * y + i) = color;
	}

	int Move_line()
	{
		int fd = open("/dev/fb0", O_RDWR);
		int *mmap_p = (int *)mmap(NULL, 800*480*4, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
		int touch_fd = open(touch_path, O_RDWR);
		int x, y;
		int x_tm, y_tm;
		int flag = 0;
		Display_W(fd, mmap_p);
		while(1)
		{
			read(touch_fd, &touch, sizeof(touch));
			if(touch.type == EV_ABS && touch.code == ABS_X) 
			{
				x = touch.value;
				x = 800*x/1024;
			}
			if(touch.type == EV_ABS && touch.code == ABS_Y) 
			{
				y = touch.value;
				y = 480*y/600;
			}
			
			if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value != 0)
			{
				flag =1;			
			}
			printf("%d---%d\n",x,y);
			
			if(flag) Display_line(x, y, mmap_p, 0x000000ff);	
			if(x_tm != x || y_tm != y)Display_line(x_tm, y_tm, mmap_p, 0x00ffffff);
			
			printf("%d---%d\n",x,y);
			
			if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value == 0)	//弹出来才给白色
			{
				Display_W(fd, mmap_p);
				flag=0;
			}

			x_tm = x;
			y_tm = y;
		}
	}

	int main()
	{
		//Get_Touch_Xy();
		Move_line();
	}



	#include <stdio.h>
	#include <errno.h>
	#include <stdlib.h>
	#include <sys/stat.h>
	#include <sys/types.h>
	#include <sys/mman.h>
	#include <fcntl.h>
	#include <unistd.h>
	#include <linux/input.h>

	#define lcd_path "/dev/fb0"
	#define touch_path "/dev/input/event0"

	struct input_event touch;

	int clear(int fd, int *mmap_p)	//清白屏
	{	
		lseek(fd, 0, SEEK_SET);
		int i, j;
		for(i = 0; i < 480; i++)
			for(j = 0; j < 800; j++)
				*(mmap_p + 800 * i + j) = 0x00ffffff;
	}

	int display_block(int x, int *mmap_p, int color)
	{
		int *tmp_map = mmap_p;	//暂时映射首地址
		tmp_map += (480/2-50/2-1)*800 + x;	//跳到指定的地方显示图形，相对手触位置左偏移了35
		
		if(tmp_map > mmap_p+214*800+800-70)	//右边界
		{
			tmp_map = mmap_p+214*800+799-70;	//超出范围则固定左地址不变
		}
		if(tmp_map < mmap_p+214*800)	//左边界
		{
			tmp_map = mmap_p+214*800;	//超出范围则固定右地址不变
		}
		
		int i, j;
		for(i = 0; i < 50; i++)	//方块显示
		{
			for(j = 0; j < 70; j++)
				*(tmp_map + 800 * i + j ) = color;
		}
	}

	int Move_block()
	{
		int fd = open("/dev/fb0", O_RDWR);	//屏幕初始化
		int *mmap_p = (int *)mmap(NULL, 800*480*4, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
		int touch_fd = open(touch_path, O_RDWR);
		
		int x=0, y=0;	//存放触点x，y值
		int x_tm; //存放上一个触点x值，以便清除上一个方块移动痕迹
		int flag = 0;	//标志是否开始移动
		
		clear(fd, mmap_p);	//清白屏
		display_block(0, mmap_p, 0x000000ff);	//显示初始方块点
		
		int i;
		for(i = 0; i < 800; i++)	//上下边界线
		{	
			*(mmap_p +  i + (480/2-50/2-2)*800) = 0x00000000;//上线
			*(mmap_p +  i + (480/2+50/2-1)*800) = 0x00000000;//下线
		}
		
		while(1)
		{
			read(touch_fd, &touch, sizeof(touch));	//读触屏结点文件
			if(touch.type == EV_ABS && touch.code == ABS_X) //取x值转换成800*480分辨率
			{
				x = touch.value;
				x = 800*x/1024;
				
			}
			if(touch.type == EV_ABS && touch.code == ABS_Y) 
			{
				y = touch.value;
				y = 480*y/600;
			}
			
			if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value != 0) //判断触屏压力值
			{
				if(y > (480/2-50/2-2) && y < (480/2+50/2-1) && x < 70)	//判断开始时触点是否在方块内，若在才能移动
				{
					display_block(0, mmap_p, 0x00ffffff);	//清除初始方块
					flag =1;	//启动移动标志位
				}			
						
			}
			printf("%d---%d\n",x,y);
			
			if(flag) display_block(x-35, mmap_p, 0x000000ff);	//移动方块
			if(x_tm != x ) display_block(x_tm-35, mmap_p, 0x00ffffff);	//判断是否需要清除痕迹
			
			printf("%d---%d\n",x,y);
			
			if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value == 0)	//弹出来
			{
				display_block(x-35, mmap_p, 0x00ffffff);	//清除最后松手的方块
				display_block(0, mmap_p, 0x000000ff);	//方块自动回归初始位置
				flag=0;	//关闭移动
			}

			x_tm = x;	//保存上一个触点x值
		}
	}

	int main()
	{
		Move_block();
	}


	#include <stdio.h>
	#include <errno.h>
	#include <stdlib.h>
	#include <sys/stat.h>
	#include <sys/types.h>
	#include <sys/mman.h>
	#include <fcntl.h>
	#include <unistd.h>
	#include <linux/input.h>

	#define lcd_path "/dev/fb0"
	#define touch_path "/dev/input/event0"

	struct input_event touch;

	int clear(int fd, int *mmap_p)	//清白屏
	{	
		lseek(fd, 0, SEEK_SET);
		int i, j;
		for(i = 0; i < 480; i++)
			for(j = 0; j < 800; j++)
				*(mmap_p + 800 * i + j) = 0x00ffffff;
	}

	int display_block(int x, int *mmap_p, int color)
	{
		int *tmp_map = mmap_p;	//暂时映射首地址
		tmp_map += (480/2-50/2-1)*800 + x;	//跳到指定的地方显示图形，相对手触位置左偏移了35
		
		if(tmp_map > mmap_p+214*800+800-70)	//右边界
		{
			tmp_map = mmap_p+214*800+799-70;	//超出范围则固定左地址不变
		}
		if(tmp_map < mmap_p+214*800)	//左边界
		{
			tmp_map = mmap_p+214*800;	//超出范围则固定右地址不变
		}
		
		int i, j;
		for(i = 0; i < 50; i++)	//方块显示
		{
			for(j = 0; j < 70; j++)
				*(tmp_map + 800 * i + j ) = color;
		}
	}

	int Move_block()
	{
		int fd = open("/dev/fb0", O_RDWR);	//屏幕初始化
		int *mmap_p = (int *)mmap(NULL, 800*480*4, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
		int touch_fd = open(touch_path, O_RDWR);
		
		int x=0, y=0;	//存放触点x，y值
		int x_tm; //存放上一个触点x值，以便清除上一个方块移动痕迹
		int flag = 0;	//标志是否开始移动
		
		clear(fd, mmap_p);	//清白屏
		display_block(0, mmap_p, 0x000000ff);	//显示初始方块点
		
		int i;
		for(i = 0; i < 800; i++)	//上下边界线
		{	
			*(mmap_p +  i + (480/2-50/2-2)*800) = 0x00000000;//上线
			*(mmap_p +  i + (480/2+50/2-1)*800) = 0x00000000;//下线
		}
		
		while(1)
		{
			read(touch_fd, &touch, sizeof(touch));	//读触屏结点文件
			if(touch.type == EV_ABS && touch.code == ABS_X) //取x值转换成800*480分辨率
			{
				x = touch.value;
				x = 800*x/1024;
				
			}
			if(touch.type == EV_ABS && touch.code == ABS_Y) 
			{
				y = touch.value;
				y = 480*y/600;
			}
			
			if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value != 0) //判断触屏压力值
			{
				if(y > (480/2-50/2-2) && y < (480/2+50/2-1) && x < 70)	//判断开始时触点是否在方块内，若在才能移动
				{
					display_block(0, mmap_p, 0x00ffffff);	//清除初始方块
					flag =1;	//启动移动标志位
				}			
						
			}
			printf("%d---%d\n",x,y);
			
			if(flag) display_block(x-35, mmap_p, 0x000000ff);	//移动方块
			if(x_tm != x ) display_block(x_tm-35, mmap_p, 0x00ffffff);	//判断是否需要清除痕迹
			
			printf("%d---%d\n",x,y);
			
			if(touch.type == EV_KEY && touch.code == BTN_TOUCH && touch.value == 0)	//弹出来
			{
				display_block(x-35, mmap_p, 0x00ffffff);	//清除最后松手的方块
				display_block(0, mmap_p, 0x000000ff);	//方块自动回归初始位置
				flag=0;	//关闭移动
			}

			x_tm = x;	//保存上一个触点x值
		}
	}

	int main()
	{
		Move_block();
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


	#define LCD_FILE  "/dev/fb0"
	#define LED_LENGTH  800*480*4
	#define TOUCH_PATH  "/dev/input/event0"

	struct inif
	{
		int lcd_fd,touch_fd;
		int * lcd_star;
		struct input_event touch;
		int w,h;
		
	}I;

	int init();
	int close_file();
	int dis_where(int w_x,int w_y,int color);

	int init()
	{
	    I.lcd_fd = open(LCD_FILE,O_RDWR);
		 if(I.lcd_fd == -1)
		 {
			 perror("open lcd:");
			 return -1;
		 }
		I.lcd_star = (int *)mmap(NULL,LED_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,I.lcd_fd,0);
		 if(I.lcd_star == MAP_FAILED)
		 {
			 perror("mmap:");
			 return -1;
		 }
		
		
		int i;
		for(i=0;i<800*480;i++)
		{
			*(I.lcd_star+i) = 0x00ffffff;
		}
		
		
		int *new_star = I.lcd_star + 800*213;
		int *new_star_1 = I.lcd_star +800*266;
		
		int w,h;
		
			for(w=0;w<800;w++)   //(214,0)
			{
				*(new_star + w) = 0x00000000;
			}
				
		   for(h=0;h<800;h++)   //(266,0)
			{
				*(new_star_1 + h) = 0x00000000;
			}
		
		
		
		
		
		return 0;
	}

	int close_file()
	{
		
		close(I.lcd_fd);
		close(I.touch_fd);
		munmap(I.lcd_star,LED_LENGTH);	
		return 0;
	}

	int dis_where(int w_x,int w_y,int color) //指定位置映射（小长方体体积固定）
	{
		//初始小方块位置
		int * new_star = I.lcd_star + 800*w_y + w_x;
		//小方块定义为高50，长100  (215,0)
		int w,h;
		
		for(h=0;h<50;h++)
		{
			for(w=0;w<100;w++)
				*(new_star+800*h+w) = color;
		}
		
		return 0;
	}

	int touch_box()
	{
		
		I.touch_fd = open(TOUCH_PATH,O_RDONLY);
		 if(I.touch_fd == -1)
		 {
			 perror("open touch :");
			 return -1;
		 }
		int x,y,x_tep,y_tep;
		int flag;
		dis_where(0,215,0x00ff00ff);
		while(1)
		{
			
			read(I.touch_fd,&I.touch,sizeof(I.touch));
			
			if(I.touch.type == EV_ABS && I.touch.code == ABS_X) 
		   {
			   x=I.touch.value;
		       x = 800*x/1024;
		   }
		   if(I.touch.type == EV_ABS && I.touch.code == ABS_Y) 
		   {
			   y=I.touch.value;
			   y = 480*y/600;
		   }
			//按下了屏幕则把初始位置画为白色
			if(I.touch.type == EV_KEY && I.touch.code == BTN_TOUCH && I.touch.value !=0 && y>213 && y<266 && (x-50)>0 && (x+50)<800)
			{
				dis_where(0,215,0x00ffffff);
				flag = 1;
				
				printf("x=%d  y=%d\n",x,y);
				
			}
			printf("x=%d  y=%d\n",x,y);
			if(flag&& y>213 && y<266 && (x-50)>0 && (x+50)<800)
			{
				dis_where(x-50,215,0x00ff00ff);
				//printf("x=%d  y=%d\n",x,y);
			}
			if(x+50>800)
				dis_where(0,215,0x00ff00ff);
			
			if((x_tep != x || y_tep != y)&&flag==1)
			{
				dis_where(x_tep-50,215,0x00ffffff);//与原来的X，Y不同时
				
			}
			//抬起了屏幕
			if(I.touch.type == EV_KEY && I.touch.code == BTN_TOUCH && I.touch.value ==0||y>266||y<213)
			{
				 dis_where(x-50,215,0x00ffffff);
				 printf("%d\n",__LINE__);
				 dis_where(0,215,0x00ff00ff);
				 printf("x-50=%d\n",x-50);
				
				 flag = 0;
			}
			
			x_tep = x;
			y_tep = y;
			
		}
		
		return 0;
	}




	int main()
	{
		
		init();
		//dis_where(0,215,0x00ff00ff);
		touch_box();
		close_file();
		
		return 0;
	}


## 省钱利器：
![](/hexo-private-blog-website/images/淘宝客6.jpg)
![](/hexo-private-blog-website/images/淘宝客7.jpg)
![](/hexo-private-blog-website/images/淘宝客8.jpg)

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