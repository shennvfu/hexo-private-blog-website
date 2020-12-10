---
title: File_io_01
date: 2020-08-27 16:32:18
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

# 一、练习

1）获取一份未知数据的文件，让你统计文件中字母的个数。



```

#define READ_LENGTH 1

struct file_data
{
	int fd;
    char read_data;//存放读取到的字符数据
	unsigned int char_sum;//统计字目个数
	unsigned int read_ret;//read函数的返回值
	unsigned int close_ret;//close函数的返回值
}FD = {-1,'\0',0,0,0};

int main()
{
	FD.fd = open("./data.txt",O_RDONLY);//打开想要统计的文件
	if(FD.fd == -1)
	{
		perror("open data.txt");
		return -1;
	}
	
	while(1)//循环读取数据，每次只读一个字节
	{
		FD.read_ret = read(FD.fd,&(FD.read_data),READ_LENGTH);
		if(FD.read_ret == -1)//判断是否读取成功
		{
			perror("read data:");
			return -1;
		}
		else if(FD.read_ret == 0)//是否读取到了文件的末尾
		{
			printf("字母个数统计完成，总数为%d\n",FD.char_sum);
			break;
		}
		else//还没统计完
		{
			//判断读取到的字符数据是否是字母
			if((FD.read_data >= 65 && FD.read_data <= 90) || ((FD.read_data >= 97 && FD.read_data <= 122))
			{
				FD.char_sum++;
			}
		}
		
	
	}
	
	FD.close_ret = close(FD.fd);
	if(FD.close_ret == -1)
	{
		perror("close data.txt");
		return -1;
	}
	
	return 0;
}



```

练习：

​		文件结束符是长什么样的（打印出来）



##### 2）lseek（控制文件的读写位置）

```
1）
	注意：
		1 默认文件一开始打开，读写位置是在文件的首部
		2 在一份程序中，多次的read和write (注意当前读写位置在哪)
		
2）
	man 2 lseek
	头文件：
	 	#include <sys/types.h>
        #include <unistd.h>
        
    原型：
    	off_t lseek(int fd, off_t offset, int whence);
		
		返回值：
			偏移读写位置失败：
						-1
			偏移成功：返回当前读写位置到文件首部的偏移量（字节数）
		形参：
			1：
				fd 文件描述符
			
			2：
				offset：想要的偏移量（字节数）
			
			3：
				whence：从哪里开始偏移
				
				从文件头         SEEK_SET
				从当前读写位置    SEEK_CUR
				从文件尾部        SEEK_END  (当从文件尾部偏移时，偏移量可以是负数)
```





2）获取一份未知数据的文件，让你统计文件中“你”的个数。

​	字符和中文混在一起读取的情况只有一种：第一个字符数据时ascii里面的

​	如果第一个字符数据不是中文字符：当前读写位置需要退2个字节重新读取

​	注：

​		如果读取到最后，读取的数据不是3个字节（不是中文数据），不需要统计判断，之前退出循环。

​													

3）使用文件IO，实现文件拷贝。

```
一、
	两个文件：
			file_a.txt:要拷贝的
			file_b.txt:
						如果存在则清空  O_TRUNC
						如果不存在则创建 O_CREAT
	数据的操作：
			固定每次读取的字节数：
							 先固定读取10个字节 file_a
							 再固定写10个字节   file_b
			注：
				如果a文件的数据大小不是10的倍数，肯定最后一次读取时读取不了10个字节的，需要该write第三个形参

```



# 二、 系统IO与ARM开发板的程序编程

​	1)开发板LCD参数：

​						分辨率：800*480（最多能显示800X480个像素点： A R G B），一个像素点4个字节

​						像素点的表示（十六进制数表示）：0x00FF0000 (红色)

 2）

​		只需把像素点写入LCD对应的设备文件内即可。

​		LCD:/dev/fb0(开发板的路径)



```
第一步：打开LCD
第二步：
	  把800*480个像素点写入LCD
第三步：
	  关闭LCD
	  
	  #define LCD_DEV_PATH "/dev/fb0"
	  #define RGB_LENGTH   800*480
	  int lcd_fd = open(LCD_DEV_PATH,O_WRONLY);
	  if(lcd_fd == -1)
	  {
	  	perror("open lcd");
	  	return -1;
	  }
	  
	  int buf[RGB_LENGTH];
	  int color = 0x00ff0000;
	  int num;
	  for(num=0; num<RGB_LENGTH; num++)
	  	buf[RGB_LENGTH] = color;
	  
	  write(lcd_fd,buf,RGB_LENGTH*4);
	  
	  close(lcd_fd);
```



​	练习：

​			显示任意位置任意大小的矩形颜色块

```
1）
	求出可以指定LCD中任意像素点的位置的通项公式：800*y+x
	
2)
	先把读写位置偏移到要写像素点的位置
	再x:50 y:50的位置显示颜色块
	lseek(lcd_fd,800*50+50,SEEK_SET);
	write
	
	
3)封装函数
	//定义两个形参：w h:长和宽 两个形参指定显示矩形的左上角的坐标
	int Display_Where_Color(int w,int h,int X,int Y)
	{
		if((w<=0 || w> 800) && (y<=0) || (y>480)) //显示的范围大于LCD范围
		{
			printf("参数有误！\n");
			return -1;
		}
		
		int lcd_fd = open(LCD_DEV_PATH,O_WRONLY);
		if(lcd_fd == -1)
		{
			perror("open lcd");
			return -1;
		}
		
		int buf = 0x0000ff00;
		int n;

			
		//先把读写位置偏移到要写像素点的位置
		//第一次偏移
		int lcd_offset = 800*Y+X;
		lseek(lcd_fd,lcd_offset*4,SEEK_SET);
		int x,y;
		for(y=0; y<h; y++)
		{
			for(x=0; x<w; x++)
			{
				write(lcd_fd,buf,4);
			}
			//刷完一行，需要跳到下一行
			lseek(lcd_fd,(800-w)*4,SEEK_CUR);
		}
	
		close(lcd_fd);
		return 0;
	}
	

```

4）使用内存映射函数显示颜色

​		man 2 mmap

   内存映射： 把文件映射进内存

​			内存： 运行内存（映射的是设备文件，对应的内存有设备而定，fb0---》集显）

​			映射：两个元素集合内，各个元素具有一一对应的关系。



​			集合1：被映射文件 fb0(存放的是像素点)

​			集合2：内存（显存 ---放像素点<只要像素点刷进显存就会里面显示>） 



​		头文件：

​					#include <sys/mman.h>

​		原型：

				void *mmap(void *addr, size_t length, int prot, int flags,
	              int fd, off_t offset);
	              
	    返回值类型：
	    		万能指针：void * (考虑到可以通过返回的指针变量赋值任意类型的数据)
	    返回值：
	    		On success, mmap() returns a pointer to the mapped area.  On error, the
	       		value MAP_FAILED (that is, (void *) -1) is returned, 
	       		映射成功：
	       				返回指向映射空间的首地址的指针变量
	       		映射失败：
	       				返回MAP_FAILED或者（void *）-1
	    形参一：
	    		void *addr：设置映射空间的首地址，设置NULL，让系统决定首地址
	    形参二：
	    		length：设置映射空间的大小 800*480*4
	    形参三：
	    		prot：映射空间的权限  选可读可写
	    		   PROT_EXEC  Pages may be executed.
	
	               PROT_READ  Pages may be read.
	
	               PROT_WRITE Pages may be written.
	
	               PROT_NONE  Pages may not be accessed
	    形参四：
	    	映射空间的属性
	    	flags：
	    		 MAP_SHARED：
	    		 			在给指针变量赋值数据时，这些数据被拷贝一份，同时回写到源文件（被映射的文件），
	    		 			源文件里面的数据会共享给其他进程.
	
	    		 
				 MAP_PRIVATE：
							在给指针变量赋值数据时，这些数据不会被拷贝一份，同时不会回写到源文件（被映射的文件），
	    		 			源文件里面的数据不会共享给其他进程.
	    		 			
	    形参五：
	    		fd：想要映射的文件对应的文件描述符（想映射，先打开） （设置的是MAP_SHARED，打开文件的方式不能和共享属性冲突）
	   
	    形参六：
	    		offset：被映射的文件中一开始的映射位置的偏移量，一般设置为0，从文件首部开始映射
		
		
		代码思路：
				打开fb0
				映射fb0
				给返回指针赋值像素点
				释放映射空间
				关闭fb0
				
	#include <stdio.h>
	#include <sys/stat.h>
	#include <sys/types.h>
	#include <fcntl.h>
	#include <unistd.h>
	#define LCD_DEV_PATH "/dev/fb0"
	#define MAP_LENGTH 800*480*4
	#define MAP_OFFSET 0
	
	int main()
	{
		//打开fb0
		int lcd_fd = open(LCD_DEV_PATH,O_RDWR);
		if(lcd_fd == -1)
		{
			perror("open lcd");
			return -1;
		}
		
		int * mmap_star = (int *)mmap(NULL,
	                            MAP_LENGTH,
	                            PROT_READ | PROT_WRITE,
	                            MAP_SHARED,
	                            lcd_fd,
	                            MAP_OFFSET);
	     if(mmap_star == MAP_FAILED) //if(mmap_star == (void *)-1)
	     {
	     	perror("mmap lcd");
	     	return -1;
	     }
	     
	     //通过指针赋值像素点
	     int x,y;
	     for(y=0; y<480; y++)
	     {
	     	for(x=0; x<800; x++)
	     	{
	     		*(mmap_star+800*y+x) = 0x000000ff;
	     	}
	     }
	     
	     int ret = munmap(mmap_star,MAP_LENGTH);
	     if(ret == -1)
	     {
	     	perror("munmap lcd");
	     	return -1;
	     }
	     
		return 0;
	}
				
	    		
作业：

​		1：了解 bmp 24 位 图片 （格式：一个的像素点多少字节，前54个字节（头信息有哪些？））


	作业：
	1：了解 bmp 24 位 图片 （格式：一个的像素点多少字节，前54个字节（头信息有哪些？））
	BMP文件格式，又称为Bitmap（位图）或是DIB(Device-Independent Device，设备无关位图)，是Windows系统中广泛使用的图像文件格式。由于它可以不作任何变换地保存图像像素域的数据，因此成为我们取得RAW数据的重要来源。Windows的图形用户界面（graphical user interfaces）也在它的内建图像子系统GDI中对BMP格式提供了支持。
	BMP是一种与硬件设备无关的图像文件格式
		 头文件：提供文件的格式、大小、到图像数据的偏移量等信息，14个字节
	    位图信息头：提供图像数据的尺寸、位平面数、压缩方式、颜色索引等信息，40个字节
		 颜色表（调色板）：（不用）
	    位图数据：就是图像数据，从左到右、从上到下存储，字节长度由图像长宽尺寸决定
		bmp24位图像，R、G、B三种颜色各用8个bit来表示，也就是一个像素点是3个字节，位图信息头后面
	是位图数据，位图文件从文件头开始偏移54个字节就是位图数据。

	BMP简介
	BMP是一种与硬件设备无关的图像文件格式，使用非常广。BMP是Windows环境中交换与图有关的数据的一种标准，在Windows环境中运行的图形图像软件都支持BMP图像格式。
	相对来讲，BMP格式比较简单，它只包含两个重要参数：编码格式（Encoding）和像素位数（bpp, bit-per-pixel）。

	典型的BMP图像文件由四部分组成：
	1：文件头，它包含BMP图像文件的类型、内容尺寸和起始偏移量等信息；
	2：图像参数，它包含图像的宽、高、压缩方法，以及颜色定义等信息；
	3：调色板，可选部分，bpp较小的位图需要调色板；有些位图，比如24bpp（真彩色）图就不需要调色板；
	4：位图数据，这部分的内容因位图实际像素位数和编码格式而不同，在真彩位图中直接使用RGB真彩色值；而有调色板的位图则使用调色板中颜色索引值。

	BMP是一种与硬件设备无关的图像文件格式，使用非常广。它采用位映射存储格式，除了图像深度可选以外，不采用其他任何压缩，因此，BMP文件所占用的空间很大。BMP文件的图像深度可选lbit、4bit、8bit及24bit（我们这里使用24bit）。由于BMP文件格式是Windows环境中交换与图有关的数据的一种标准，因此在Windows环境中运行的图形图像软件都支持BMP图像格式。

	1.1  文件头结构定义

	typedef struct tagBITMAPFILEHEADER{

	　　WORD bfType; // 位图文件的类型，必须为BM(1-2字节)

	　　DWORD bfSize; // 位图文件的大小，以字节为单位(3-6字节)

	　　WORD bfReserved1; // 位图文件保留字，必须为0(7-8字节)

	　　WORD bfReserved2; // 位图文件保留字，必须为0(9-10字节)

	　　DWORD bfOffBits; // 位图数据的起始位置，以相对于位图(11-14字节)

	　　// 文件头的偏移量表示，以字节为单位

	　　} BITMAPFILEHEADER;

	1.2  位图信息头结构定义
	BMP位图信息头数据用于说明位图的尺寸等信息。

	typedef struct tagBITMAPINFOHEADER{

	　　DWORD biSize; // 本结构所占用字节数(15-18字节)

	　　LONG biWidth; // 位图的宽度，以像素为单位(19-22字节)

	　　LONG biHeight; // 位图的高度，以像素为单位(23-26字节)

	　　WORD biPlanes; // 目标设备的级别，必须为1(27-28字节)

	　　WORD biBitCount;// 每个像素所需的位数，必须是1(双色),(29-30字节)　// 4(16色)，8(256色)或24(真彩色)之一

	　　DWORD biCompression; // 位图压缩类型，必须是 0(不压缩),(31-34字节)　// 1(BI_RLE8压缩类型)或2(BI_RLE4压缩类型)之一

	　　DWORD biSizeImage; // 位图的大小，以字节为单位(35-38字节)

	　　LONG biXPelsPerMeter; // 位图水平分辨率，每米像素数(39-42字节)

	　　LONG biYPelsPerMeter; // 位图垂直分辨率，每米像素数(43-46字节)

	　　DWORD biClrUsed;// 位图实际使用的颜色表中的颜色数(47-50字节)

	　　DWORD biClrImportant;// 位图显示过程中重要的颜色数(51-54字节)

	　　} BITMAPINFOHEADER;

	1.3  位图数据阵列
	位图数据记录了位图的每一个像素值，记录顺序是在扫描行内是从左到右,扫描行之间是从下到上。位图的一个像素值所占3个字节

	从一个24位图的图片里，读出如下信息：

	424D B634 1800 0000 0000 3600 0000 2800 0000 3003 0000 8802 0000 0100 1800 0000 0000 8034 1800 C40E 0000 C40E 0000 0000 0000 0000 0000 B48D 61B3 8C60

	24位BMP图可分为3个部分：位图文件头、位图信息、位图阵列

	2.1     文件头（14个字节）
	字节

	字节号

	解释说明

	424D

	1-2

	可以转换为字符‘B’、‘M’，是用于识别BMP文件的标志

	B634 1800

	3-6

	存放的是位图文件的大小，计算得文件大小为1586358字节

	0000 0000

	7-10

	保留字节，必须为0

	3600 0000

	11-14

	表示位图阵列相对于文件头的偏移量，计算得54，即位图阵列从第55字节开始

	2.2  位图信息（40个字节）
	字节

	字节号

	解释说明

	2800 0000

	15-18

	位图信息头占用的字节数，把0028转换10进制为40，即占用了40节

	3003 0000

	19-22

	位图的宽度，以像素为单位，计算得位图的宽度为816像素

	8802 0000

	23-26

	位图的高度，也以像素为单位，计算得位图的高度为648像素

	0100

	27-28

	位图的平面数，必须为1

	1800

	29-30

	代表每个像素所需要的位数，很显然我们做的是24为位图，结果为24

	0000 0000

	31-34

	代表位图压缩类型，如果不压缩必须为0

	8034 1800

	35-38

	位图阵列的大小，以字节为大小，计算得1586304字节

	C40E 0000

	39-42

	位图水平分辨率

	C40E 0000

	43-46

	位图垂直分辨率

	0000 0000

	47-50

	位图实际使用的颜色表中的颜色数，24位不需要调色板所以设置为0

	0000 0000

	51-54

	位图显示过程中重要的颜色数，为0表示都重要

	2.3  位图数据阵列
	从第55字节开始，每3个字节表示一个像素，这3个字节依次表示该像素的红、绿、蓝亮度分量值。


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
![wechat.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggvv58jjj30u010sgnq.jpg)
### 财付通打赏：
![qq.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggwd0rvjj32ai2lxdrm.jpg)
