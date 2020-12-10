---
title: File_io_02
date: 2020-08-28 13:53:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	新月曲如眉，未有团圆意。
	红豆不堪看，满眼相思泪。

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


# 一、Linux下的图像化处理（显示图像）

	1）了解BMP （24）位

​![](/hexo-private-blog-website/images/bmp.png)


```
①、获取bmp 24位图片文件（这个格式的文件有自己存放像素点的格式），把图片放到开发板
	24位：bmp图片里面的每一个像素点占24位（R G B）
	在开发板上全屏显示BMP 24位 图片 （图片的分辨率：800*480 ，大小为：800*480*3+54 = 1152054）
	
②、
	思路：
		LCD分辨率（像素点 4个字节）的特点
		BMP分辨率（像素点 3个字节）的特点
		
	代码流程：
			open lcd  one.bmp
			mmap lcd
			跳过54个字节的读写位置
			read图片的像素点
			
			循环
			{
				映射指针 = 图片的像素点
			}
			
			close lcd  one.bmp
			munmap();
```

注：

​	放大显示:

​				放大两倍：

​								同一个像素点赋值给映射指针变量2次

​				缩小两倍：

​								像素点赋值给映射指针变量，赋值一个跳过1个

----

二、显示任意大小的bmp

```
1:如果显示的bmp 24位图片的宽度（w*3）不是4的倍数，有没有对显示造成影响？
	显示失真：
			Cpu读取数据的时候是4个字节一次一次的读取的，在存放bmp图片的时候，会把图片的宽度（字节数）
			补到是4的倍数为止，比如：宽度w为799
			则 补数 = 4-（799*3）%4
			
			
		系统给bmp宽度的补数：4-（11 * 3）%4 （每一行的补数），补的数据不是像素点（不需要赋值给映射指针）
2：
	如果有补数:
			那么申请存放读取到的数据的空间大小：(w*h*3)+(补数*h)
			
	每刷完一行像素点，记得跳掉系统补的数据 数组下标+补数
```

三、汉字显示

​	

```
1：
	汉字很多，没办法一个一个汉字封装对应的显示接口，需要用一个取模软件给全部的汉字取一个汉字库（取模）
	只用TS4取汉字库：
			第一步：选择编码格式：GB2312
			第二步：选择每个字节在库中的大小（分辨率--位为单位）16X16 = 256个位 = 32个字节（正方形）
				  注意： 因为是GB2312，在编码的环境那么每个汉字2个字节
			第三步：字库格式：DZK
			
			第四步：创建
			
2：
在dzk库里面，，每个汉字由 区码（汉字前一个字节的数据）和位码组成（汉字后一个字节的数据），
	char a[2] = "啊"
	偏移量 = (94*(a[0]-1- 0xa0)+(a[1]-1-0xa0))*32                  (18X18*8)
	
3:
	代码思路：
		打开汉字库 dzk16.DZK (可读可写的方式打开)
		映射dzk：char * p = mmap (映射空间的大小需要用lseek计算dzk文件的大小)
		计算要显示的汉字在字库中的偏移量
		根据每次获取到的一个字节的数据循环判断是否要刷像素
			
```

作业：

​		实现开发板上显示一行文字。


	作业：
	实现开发板上显示一行文字。
	#include <stdio.h>
	#include <errno.h>
	#include <stdlib.h>
	#include <sys/stat.h>
	#include <sys/types.h>
	#include <sys/mman.h>
	#include <fcntl.h>
	#include <unistd.h>
	#include <string.h>

	#define LCD_DEV_PATH  "/dev/fb0"
	#define DZK_FILE_PATH "/root/dzk16.DZK"
	#define MAP_LENGTH 800*480*4
	#define MAP_OFFSET 0
	#define OPEN_FILE_FLAG O_RDWR


	struct file_info
	{
	  int  lcd_fd;
	  int  dzk_fd;
	  int  dzk_size;
	  int  *lcd_star;
	  int  dzk_offset;
	  char *dzk_star;
	}FI;

	int Init();
	int Free();
	int Dis_Font(const char * font);
	int Dis_Many_Font(const char * font);

	int Init()
	{
	  //打开LCD 汉字库
	  FI.lcd_fd = open(LCD_DEV_PATH,OPEN_FILE_FLAG);
	  FI.dzk_fd = open(DZK_FILE_PATH,OPEN_FILE_FLAG);
	  if(FI.lcd_fd == -1 || FI.dzk_fd == -1)
	  {
	    perror("open file ");
	    return -1;
	  }

	  FI.dzk_size = lseek(FI.dzk_fd,0,SEEK_END);//求出汉字库的大小，才能根据他的大小进行申请映射空间
	  lseek(I.dzk_fd,0,SEEK_SET);//重置偏移量
	  //映射lcd和dzk
	  FI.lcd_star = (int *)mmap(NULL,MAP_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,FI.lcd_fd,MAP_OFFSET);
	  FI.dzk_star  = (char *)mmap(NULL,FI.dzk_size,PROT_READ | PROT_WRITE,MAP_SHARED,FI.dzk_fd,MAP_OFFSET);
	  if(FI.lcd_star == MAP_FAILED || FI.dzk_star == MAP_FAILED)
	  {
	    perror("mmap lcd or mmap dzk ");
	    return -1;
	  }

	  return 0;
	}

	int Free()
	{
	  close(FI.lcd_fd);
	  close(FI.dzk_fd);
	  munmap(FI.lcd_star,MAP_LENGTH);
	  munmap(FI.dzk_star,FI.dzk_size);

	  return 0;
	}

	int Dis_Font(const char * font)//font存放对应要显示的汉字
	{
	  FI.dzk_offset = (94*(font[0]-1-0xa0)+(font[1]-1-0xa0))*32; //求出要显示的汉字在字库中的偏移量
	  char * new_p = FI.dzk_star+FI.dzk_offset;//定义一个新的指针变量存放dzk字库的映射指针
	  //求出要显示的汉字在字库中的偏移量
	  //根据偏移量把映射指针跳到要显示的汉字对应的位置
	  int x,y,z;
	  char type;//定义一个字符变量存放每次判断的字符汉字库数据
	  for(y = 0; y < 16; y++)
	  {
	    for(x=0; x < 2; x++)
	    {
	      type = *(new_p+2*y+x);//获取字库中一个字节的汉字模
	      for(z = 0; z < 8; z++)
	      {
	        if(type &(0x80 >> z))
	        {
	          *(FI.lcd_star+800*y+(8*x+z)) = 0x000000ff;
	        }
	        else
	        {
	          *(FI.lcd_star+800*y+(8*x+z)) = 0xffffffff;
	        }
	      }
	    }
	  }

	  return 0;
	}

	int Dis_Many_Font(const char * font) //font存放对应要显示的汉字
	{
	  int font_count = 0;
	  int *tmp_font = FI.lcd_star;
	  while(font[font_count] != '\0')
	  {
	    FI.dzk_offset = (94*(font[font_count]-1-0xa0)+(font[font_count+1]-1-0xa0))*32; //求出要显示的汉字在字库中的偏移量

	    char * new_p = FI.dzk_star+FI.dzk_offset;//定义一个新的指针变量存放dzk字库的映射指针

	    int x,y,z;
	    char type; //定义一个字符变量存放每次判断的字符汉字库数据
	    for(y = 0; y < 16; y++)
	    {
	      for(x = 0; x < 2; x++)
	      {
	        type = *(new_p+2*y+x); //获取字库中一个字节的汉字模
	        for(z = 0; z < 8; z++)
	        {
	          if(type & (0x80 >> z))
	          {
	            *(tmp_font+800*y+(8*x+z)) == 0x000000ff;
	          }
	          else
	          {
	            *(tmp_font+800*y+(8*x+z)) == 0xffffffff;
	          }
	        }
	      }
	    }

	    tmp_font += 16; // 指向下一个存放像素的位置
	    font_count += 2; //GB2312编码格式中一个汉字占用 2个字节
	  }

	  return 0;
	}
	 

	int main()
	{
	  char *font = "万壑有声含晚籁，数峰无语立斜阳。棠梨叶落胭脂色，荞麦花开白雪香。";

	  Init();

	  Dis_Many_Font(font);

	  //Dis_Font("啊");

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

	#define lcd_path "/dev/fb0"
	#define dzk_path "/root/font/font32_32.Dzk"
	#define lcd_length 800*480*4
	#define font_size 32

	struct 
	{
		int lcd_fd, dzk_fd;
		int *lcd_p;
		char *dzk_p;
		int dzk_length;
		int dzk_offset;
	}info;

	int Init()
	{
		info.lcd_fd = open(lcd_path, O_RDWR);	//鎵撳紑鏂囦欢
		info.dzk_fd = open(dzk_path, O_RDWR);
		if(info.lcd_fd == -1 || info.dzk_fd == -1)
		{
			perror("lcd/dzk");
			return -1;
		}
		//鏄犲皠
		info.lcd_p = (int *)mmap(NULL, lcd_length, PROT_READ | PROT_WRITE,\
									MAP_SHARED, info.lcd_fd, 0);
									
		
		info.dzk_length = lseek(info.dzk_fd, 0, SEEK_END);
		lseek(info.dzk_fd, 0, SEEK_SET);
		info.dzk_p = (char *)mmap(NULL, info.dzk_length, PROT_READ | PROT_WRITE,\
									MAP_SHARED, info.dzk_fd, 0);
		//鍦ㄦ殏鏃朵笉鍐欐槸鍚︽垚						
		return 0;
	}

	int Free_File()
	{
		close(info.lcd_fd);
		close(info.dzk_fd);
		munmap(info.lcd_p, lcd_length);
		munmap(info.dzk_p, info.dzk_length);
		
		return 0;
	}

	//备份一个字
	// int Display_font(const char *font)
	// {
		// info.dzk_offset = (94*(font[0]-1-0xa0)+(font[1]-1-0xa0))*32;	//姹夊瓧鍋忕Щ閲?
		
		// char *p = info.dzk_p + info.dzk_offset;	//鎸囧悜鍋忕Щ澶?
		
		// int x, y, z;
		// char type;	//瀛樻斁瀛楄妭
		// for(y = 0; y < 16; y ++)
		// {
			// for(x = 0; x < 2; x ++)
			// {
				// type = *(p + 2 * y + x);
				// for(z = 0; z < 8; z ++)
				// {
					// if(type & (0x80 >> z))
						// *(info.lcd_p + 800 * y + 8 * x + z) = 0x000000ff;
					// else
						// *(info.lcd_p + 800 * y + 8 * x + z) = 0x00000000;
				// }
			// }
		// }
		
		// return 0;
	// }

	//备份多个字显示
	// int Display_more_font(const char *font)
	// {
		// int font_num = 0;
		// int n=20;
		// int *tmp_p = info.lcd_p;
		// while(n--)
		// {
			// font_num = 0;
			// while(font[font_num] != '\0')
			// {
				
				// info.dzk_offset = (94*(font[font_num]-1-0xa0)+(font[font_num+1]-1-0xa0))*(font_size*font_size/8);	//字在字库偏移量
				
				// char *p = info.dzk_p + info.dzk_offset;	//字所在地址
				
				// int x, y, z;
				// char type;	//存放一个字节
				// for(y = 0; y < font_size; y ++)
				// {
					// for(x = 0; x < font_size/8; x ++)
					// {
						// type = *(p + font_size/8 * y + x);
						// for(z = 0; z < 8; z ++)
						// {
							// if(type & (0x80 >> z))
								// *(tmp_p + 800 * y + 8 * x + z) = 0x000000ff;
							// else
								// *(tmp_p + 800 * y + 8 * x + z) = 0x00000000;
						// }
					// }
				// }
				// sleep(1);
				// tmp_p += 32;
				// font_num += 2;
			// }
		// }
		// return 0;
	// }

	//准备做字移动显示
	int Display_move_font(const char *font)
	{
		int font_num = 0;
		int n=20;
		int *tmp_p = info.lcd_p;
		while(n--)
		{
			int count = 2;
			font_num = 0;
			while(font[font_num] != '\0')
			{
				
				info.dzk_offset = (94*(font[font_num]-1-0xa0)+(font[font_num+1]-1-0xa0))*(font_size*font_size/8);	//字在字库偏移量
				
				char *p = info.dzk_p + info.dzk_offset;	//字所在地址
				
				int x, y, z;
				char type;	//存放一个字节
				for(y = 0; y < font_size; y ++)
				{
					for(x = 0; x < font_size/8; x ++)
					{
						type = *(p + font_size/8 * y + x);
						for(z = 0; z < 8; z ++)
						{
							if(type & (0x80 >> z))
								*(tmp_p + 800 * y + 8 * x + z) = 0x000000ff;
							else
								*(tmp_p + 800 * y + 8 * x + z) = 0x00000000;
						}
					}
				}
				if(font_num == 800/32-1)
				{
					font_num = count;
					count += 2;
					tmp_p = info.lcd_p;
					sleep(1);				
				}
				else
				{
					tmp_p += 32;
					font_num += 2;
				}
				
			}
		}
		return 0;
	}

	int main()
	{
		char *font = "一生一世一双人，半醉半醒半浮生；愿无岁月可回首，且以深情共白头！";//
		Init();
		//Display_font(font);
		// Display_more_font(font);
		Display_move_font(font);
		Free_File();
		return 0;
	}

### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客26.jpg)
![](/hexo-private-blog-website/images/淘宝客27.jpg)
![](/hexo-private-blog-website/images/淘宝客28.jpg)

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