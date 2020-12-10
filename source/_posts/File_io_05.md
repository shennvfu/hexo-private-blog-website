---
title: File_io_05
date: 2020-09-13 16:32:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	誓扫匈奴不顾身，五千貂锦丧胡尘。
	可怜无定河边骨，犹是深闺梦里人。

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

# 一、标准IO

#### 	  1)第一组:

	​	 对一个字符操作：

	​						读取:  fgetc

	​						写入：fputc

	练习：使用fgetc和fputc实现两个文件的拷贝





#### 	2)第二组：

	​	对一行字符进行操作：

	​	man 3 ...

	​				  读取:fgets   char *fgets(char *s, int size, FILE *stream);

	​				  返回值：

	​							读取失败：

	​							NULL： 可能是读取到了文件的末尾，也有可能是读取错误。（ferror  feof）

	​							读取成功：返回第一个形参s

	​					形参一：

	​				  	s：存放读取到的数据的缓存区，s存放的是缓存区的首地址	

	​                 形参二：

	​					size：设置读取字符数据的字节大小，为了避免读取的数据大于缓存区的大小。

	​				    设置读取的数据为 10个字节

	​							文件中的数据为：hello\nworld ,fgets读取到\n也会结束读取

	​							文件中的数据为：hello worlddfgjasdgj\n  fgets读满（10-1）个字符再停止

	​				形参三：

	​					stream：想要读取的文件对应文件的流指针



	​                  写入:fputs   int fputs(const char *s, FILE *stream);

	​                 返回值：

	​							成功： 非负数

	​                            失败： EOF	

	​                  形参一：s     要写入数据的对应缓存区（这个缓存区的首地址，指向的内存是存放要写入的数据）

	​				 形参二：stream   文件流指针

	​			练习：使用fgets和fputs实现两个文件的拷贝



#### 第三组：对指定大小的字符进行操作

	读取：fread       size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
			
			返回值：
					成功：返回读取到的数据块的个数（把读取到的数据分成块）
					失败：返回小于数据块的个数   （把读取到的数据分成块）
			形参一：
					ptr：存放读取到的数据的缓存区的首地址
			形参二：
					size
						每个数据块的大小（字节）
			形参三：
					nmemb每一个数据块的个数
					注：因为fread设置读取的方式是数据块和数据块的大小来确定的，所以读取到的数据的大小一般是
					块的个数 * 块的大小
			形参四：
					stream：文件流指针
					char buf[100];
					fread(buf, 1, 100, FILE *stream);
					考虑：
							一个文件里面只有88个数据，但是你的fread设置了以上的读取方式。很明显数据不够读
							fread回去读的，但是返回值为0
							
					例子：创建一个文件，这个文件里面只有10个字符
							fread探讨一下其返回值
							
					关于fread函数返回值（块的个数理解）
						只有读取到数据大小等于块的大小，才是一个块，小于块的大小不算一个块
						
					如果特殊情况：以及知道文件中的数据的字节数，可以采取 块大小为1，块的个数为数据的字节数


				
	写入：fwrite      size_t fwrite(const void *ptr, size_t size, size_t nmemb,FILE *stream);

				 形参一：
				 		成功：
				 		失败：

	关于fread fwrite返回值情况
			
				fread(buf, 20, 5, FILE *stream);
				fwrite(buf,20, 5, FILE *stream);
	返回值为5：
				则读取写入成功
	小于五：
    			fread：读取错误，或者读取到了文件末尾
	    			fwrite：写入失败

	   lseek:
	   			off_t lseek(int fd, off_t offset, int whence);
	   			返回值”
	   				偏移成功，返回当前读写位置到文件头的偏移（来计算文件的大小）
	   				lseek(int fd, 0, SEEK_END);


	​	
		  fseek:
		  			int fseek(FILE *stream, long offset, int whence);
		  			
		  			返回值：
		  					失败：-1
		  					成功：0
	  					
	  偏移量=ftell(文件流指针)
---



# 二、目录检索（目录IO）





```
1）了解目录IO的接口：
					opendir  打开指定的目录    注：目录项（目录内的基本单位）
					
					readdir  读取指定的目录内的目录项 （调用一次只能读取一个目录项，后面需要循环读取才能全部遍历到）
					closedir
					
					
2)
 	man 3 opendir
 	
 		#include <sys/types.h>
        #include <dirent.h>

       DIR *opendir(const char *name);
       
      int closedir(DIR *dirp);

       
       opendir返回值：
       		打开目录成功：返回打开目录对应目录流指针
       		打开目录失败：NULL
       		
       形参：
       		name 指定打开目录的路径 
       		
       closedir:
       		返回值：
       			关闭成功： 0
       			关闭失败： -1
       	形参：
       		dirp：opendir打开目录成功返回的目录流指针
       示例：
       		#include <stdio.h>
       		#include <sys/types.h>
       		#include <dirent.h>
       		
       		int main()
       		{
       			DIR * dp = opendir("./");
       			if(dp == NULL)
       			{
       				perror("opendir ");
       				return -1;
       			}
       			else
       			{
       				printf("打开目录成功！\n");
       			}
       			
       			int ret = closedir(dp);
       			if(ret == -1)
       			{
       				perror("closedir");
       				return -1;
       			}
       			else
       			{
       				printf("关闭目录成功！\n");
       			}
       			return 0;
       		}
       

3) man 3 readdir
	
	头文件：
		#include <dirent.h>

    struct dirent *readdir(DIR *dirp);
    
    返回值：
    		读取成功：返回读取到的目录项指针，这个指针变量指向一个结构体struct dirent 
    		读取失败：NULL (读取到目录尾端返回NULL)
    形参：
    	dirp:opendir打开目录成功对应的首目录流指针
   
   读到的目录项存放再readdir返回值目录项指针指向的结构体struct dirent （找出struct dirent结构体的类型才能知道你读取到了什么目录项信息 ）
   	      struct dirent 
   	      {
               ino_t          d_ino;       /* inode number */ 读取到的目录项的索引号
               off_t          d_off;       /* not an offset; see NOTES */该目录到首目录项的偏移量
               unsigned short d_reclen;    /* length of this record */ 目录项的大小（文件名的长度字节）
               unsigned char  d_type;      /* type of file; not supported 目录项的类型
                                              by all filesystem types */
               char           d_name[256]; /* filename */ 目录项名字
           };
           
           
          关于文件类型的枚举定义在dirent.h
          
          enum
          {
                    DT_UNKNOWN = 0,   //未知类型
                # define DT_UNKNOWN	DT_UNKNOWN
                    DT_FIFO = 1,      //管道文件
                # define DT_FIFO	DT_FIFO
                    DT_CHR = 2,       //字符设备
                # define DT_CHR		DT_CHR
                    DT_DIR = 4,       //目录
                # define DT_DIR		DT_DIR
                    DT_BLK = 6,       //块设备
                # define DT_BLK		DT_BLK
                    DT_REG = 8,      //普通文件
                # define DT_REG		DT_REG
                    DT_LNK = 10,     //链接文件
                # define DT_LNK		DT_LNK
                    DT_SOCK = 12,    //套接字文件
                # define DT_SOCK	DT_SOCK
                    DT_WHT = 14
                # define DT_WHT		DT_WHT
          };
```

	练习：使用目录检索接口。遍历打印指定目录内的目录项名字

```
思路：
	先打开目录
	读取目录（循环读取，打印遍历到的目录项名字）
	关闭目录
	
	#include <stdio.h>
	#include <dirent.h>
	#include <errno.h>
	
	int main()
	{
		DIR * dp = opendir("./a");
		if(dp == NULL)
		{
            perror("opendir ");
            return -1;
		}
		
		struct dirent * eq = (struct dirent *)malloc(sizeof(struct dirent));
		if(eq == NULL)
		{
			perror("malloc eq");
			return -1;
		}
		while(1)
		{
			eq = readdir(dp);
			if(eq == NULL)
			{
				break;
			}
			else
			{
				printf("%s\n",eq->d_name);
			}
		
		}
	
		closedir(dp);
		return 0;
	}
```



	思路：符合多级目录遍历

```
	int Find_Dir(char * path)
	{
		opendir(path)
		while(1)
		{
			readdir();
			if（目录项指针是否为NULL）
			{
				break;
			}
			
			if(检索到的路径是否是.和.. eq->d_name[0] == '.')
			{
				continue;
			}
			
			if(如果是目录)  path = a/..../b
			{	在打开这个目录(需要把一级目录和二级目录用字符串拼接一起)
				判断上级路径中最后一个字符是否是/,如果有/则拼接的时候不需假/
				if(find_path[strlen(find_path)-1] == '/')
                {
                sprintf(buf,"%s%s",find_path,eq->d_name);//先拼接路径
                }
                else
                {
                sprintf(buf,"%s/%s",find_path,eq->d_name);//先拼接路径
                }

				Find_Dir(buf.find_file);
			}
			if(如果是文件)
			{  
				if(strcmp(指定文件名字，d_name) == 0)
				{
					if(find_path[strlen(find_path)-1] == '/')
                	{
                	sprintf(buf,"%s%s",find_path,eq->d_name);//先拼接路径
                	}
                	else
                	{
                   	 	sprintf(buf,"%s/%s",find_path,eq->d_name);//先拼接路径
                	}
					
			   		把之前走过的路径个当前文件名拼接
					printf("打印文件名字");
				}
			}
		}
	}
	
	搜索指定后缀名文件：
			截取的是读取的到eq->d_name中.后面的字读数据
			
            bool check_str(char * str,char * data);

            bool check_str(char * str,char * data)
            {
                char * p = str+(strlen(str)-strlen(data)-1);//直接把.对应的字符地址赋值到字符指针变量p
                if(strcmp(p,data) == 0)
                {
                	return ture;
                }
                else
                {
                	return flase;
                }
            }
```



```
第一阶段项目 -- 电子相册

	知识点：数据结构 + 文件IO + makefile + 动态库静态库 + 开发板（图片显示、触摸屏、字体显示）

	功能要求（必）：
			1）使用检索图片，把检索到的图片图片名与路径存放在链表中
			2）检索完成字体显示 （图片数量）
			3）触摸屏控制图片切换（点击上下张切换，或者滑动切换）
			4）每个界面使用缓存显示。
			5）至少三个界面（主界面-->浏览图片界面--->退出界面）

	可附加：
			滑动解锁
			实时显示时间
			图片放大缩小功能
			图片的名字（英文名字）
			显示图片的缩略图 （把原本的图片缩小显示）
			
	做项目的时间：至下周一
	
				验收时间，下周一下午

```


			
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


	#include <stdio.h>
	#include <errno.h>
	#include <sys/types.h>
	#include <sys/mman.h>
	#include <sys/stat.h>
	#include <sys/ioctl.h>
	#include <unistd.h>
	#include <fcntl.h>
	#include <linux/fb.h>
	#include <string.h>

	#define LCD_DEV_PATH      "/dev/fb0"
	#define TOUCH_DEV_PATH    "/dev/input/event0"
	#define CACHE_PATH_1      "/root/photo/bmpimages/one.bmp"
	#define CACHE_PATH_2      "/root/photo/bmpimages/five.bmp"
	#define OPEN_BMP_FLAG     O_RDONLY
	#define OPEN_TOUCH_FLAG   O_RDONLY
	#define OPEN_LCD_FLAG     O_RDWR
	#define OPEN_FLAG         O_RDWR
	#define BMP_OFFSET        54
	#define MAP_LENGTH        800*480*4
	#define MAP_CACHE_LENGTH  800*1440*4
	#define BMP_SIZE          800*480*3
	#define MAP_OFFSET        0
	#define INIT_CACHE        0
	#define UPDATE_CACHE      1
	#define CACHE_PATH_LENGTH 256 

	struct fb_fix_screeninfo f_info; //存放获取到的显示信息
	struct fb_var_screeninfo v_info; //存放获取到的可以修改的信息

	struct files_info
	{
		//文件描述符
	  int lcd_fd,touch_fd,cache_bmp_fd_1,cache_bmp_fd_2;  //全局变量
	  int * mmap_lcd_star;
	  int mmap_size;
	  char cache_bmp_1[BMP_SIZE],cache_bmp_2[BMP_SIZE]; //存放两个缓存区的像素点的空间
	  char cache_path_1[CACHE_PATH_LENGTH],cache_path_2[CACHE_PATH_LENGTH]; //定义专门存放缓存图片的路径
	  int skip;
	  int bmp_w,bmp_h;
	  int black_color;
	  int touch_x,touch_y;
	  int x,y;
	  //char r,g,b;
	  //int rgb;
	  //int x,y;
	  struct input_event touch; //定义输入子系统提供的结构体变量存放读取到的触摸屏信息
	  //struct fb_fix_screeninfo f_info; //存放获取到的显示信息
	}FI;

	int Init_Info();
	int Free_Close();
	int Display_Bmp();
	int Cache_Init(int cache_select,char * cache_path_1,char * cache_path_2);//如果是第一次初始化则把默认的图片映射进缓存区,如果不是第一次则是更新缓存区中的图片像素
	int Dis_Bmp(char * bmp_path);
	int Cache_Display_Bmp(char * bmp_path);

	int Init_Info()
	{
		FI.lcd_fd = open(LCD_DEV_PATH,OPEN_LCD_FLAG);
		if(FI.lcd_fd == -1)
		{
			perror("open lcd:");

			return -1;
		}

		int io_ret_1 = ioctl(FI.lcd_fd,FBIOGET_FSCREENINFO,&f_info);
		int io_ret_2 = ioctl(FI.lcd_fd,FBIOGET_VSCREENINFO,&v_info);
		if(io_ret_1 == -1 || io_ret_2 == -1)
		{
			perror("ioctl lcd");

			return -1;
		}

		printf("显存:%d\n", f_info.smem_len);
		printf("可见区w:%d---h:%d,虚拟区w:%d---h:%d\n", v_info.xres,v_info.yres,v_info.xres_virtual,v_info.yres_virtual);
		FI.mmap_size = v_info.xres_virtual*v_info.yres_virtual*4;

		FI.mmap_lcd_star = (int *)mmap(NULL,MAP_CACHE_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,FI.lcd_fd,MAP_OFFSET);
		if(FI.mmap_lcd_star == MAP_FAILED) //if(FI.mmap_lcd_star == (void *)-1) 
		{
			perror("mmap lcd:");

			return -1;
		}

		// int io_ret_1 = ioctl(FI.lcd_fd,FBIOGET_FSCREENINFO,&f_info);
		// int io_ret_2 = ioctl(FI.lcd_fd,FBIOGET_VSCREENINFO,&v_info);
		// if(io_ret_1 == -1 || io_ret_2 == -1)
		// {
		// 	perror("ioctl lcd");

		// 	return -1;
		// }

		// printf("显存:%d\n", f_info.smem_len);
		// printf("可见区w:%d---h:%d,虚拟区w:%d---h:%d\n", v_info.xres,v_info.yres,v_info.xres_virtual,v_info.yres_virtual);

		return 0;
	}

	int Free_Close()
	{
		close(FI.lcd_fd);
		//munmap(FI.mmap_lcd_star,MAP_CACHE_LENGTH);
		munmap(FI.mmap_lcd_star,FI.mmap_size);

		return 0;
	}

	int Cache_Init(int cache_select,char * cache_path_1,char * cache_path_2)
	{
		if(cache_select == INIT_CACHE) //初始化缓存
		{
			strcpy(FI.cache_path_1,CACHE_PATH_1);
			strcpy(FI.cache_path_2,CACHE_PATH_2);
		}

		if(cache_select == UPDATE_CACHE) //更新缓存
		{
			if(cache_path_1 == NULL || cache_path_2 == NULL)
			{
				printf("无效的路径!!请输入正确的图片路径!!\n");

				return -1;
			}

			strcpy(FI.cache_path_1,cache_path_1);
			strcpy(FI.cache_path_2,cache_path_2);

		}
		//打开要缓存的图片
		FI.cache_bmp_fd_1 = open(FI.cache_path_1,OPEN_FLAG);
		FI.cache_bmp_fd_2 = open(FI.cache_path_2,OPEN_FLAG);
		if(FI.cache_bmp_fd_1 == -1 || FI.cache_bmp_fd_2 == -1)
		{
			perror("open bmp");

			return -1;
		}

		lseek(FI.cache_bmp_fd_1,BMP_OFFSET,SEEK_SET);
		lseek(FI.cache_bmp_fd_2,BMP_OFFSET,SEEK_SET);
		read(FI.cache_bmp_fd_1,FI.cache_bmp_1,BMP_SIZE);
		read(FI.cache_bmp_fd_2,FI.cache_bmp_2,BMP_SIZE);

		int *new_lcd_star = FI.mmap_lcd_star+(800*480);

		for(FI.y = 0; FI.y < 480; FI.y++)
		{
			for(FI.x = 0; FI.x < 800; FI.x++)
			{
				// *(new_lcd_star+800*(479-FI.y-1)+(FI.x)) = FI.cache_bmp_1[3*((800*FI.y)+(FI.x))] << 0 |
				//                                           FI.cache_bmp_1[(3*((800*FI.y)+(FI.x))+1)] << 8 |
				//                                           FI.cache_bmp_1[(3*((800*FI.y)+(FI.x))+2)] << 16;
				*(new_lcd_star+800*(479-FI.y)+(FI.x)) = FI.cache_bmp_1[3*((800*FI.y)+(FI.x))] << 0 |
				                                          FI.cache_bmp_1[(3*((800*FI.y)+(FI.x))+1)] << 8 |
				                                          FI.cache_bmp_1[(3*((800*FI.y)+(FI.x))+2)] << 16;

			}
		}

		new_lcd_star = FI.mmap_lcd_star+((800*480)*2);

		for(FI.y = 0; FI.y < 480; FI.y++)
		{
			for(FI.x = 0; FI.x < 800; FI.x++)
			{
				*(new_lcd_star+800*(479-FI.y-1)+(FI.x)) = FI.cache_bmp_2[3*((800*FI.y)+(FI.x))] << 0 |
				                                          FI.cache_bmp_2[(3*((800*FI.y)+(FI.x))+1)] << 8 |
				                                          FI.cache_bmp_2[(3*((800*FI.y)+(FI.x))+2)] << 16;
			}
		}

		//关闭缓存图片文件
		close(FI.cache_bmp_fd_1);
		close(FI.cache_bmp_fd_2);

		return 0;
	}


	int Dis_Bmp(char * bmp_path)
	{
		//疑问，这里显示的像素点是放在第一个房间里面，需不要要考虑可见区域的偏移量
		//先打开要显示的图片
		FI.cache_bmp_fd_1 = open(bmp_path,OPEN_FLAG);
		if(FI.cache_bmp_fd_1 == -1)
		{
			perror("open no cache bmp");

			return -1;
		}

		memset(FI.cache_bmp_1,0,BMP_SIZE);
		//memset(FI.cache_bmp_1,0,sizeof(cache_bmp_1));
		lseek(FI.cache_bmp_fd_1,BMP_OFFSET,SEEK_SET);
		read(FI.cache_bmp_fd_1,FI.cache_bmp_1,BMP_SIZE);

		for(FI.y = 0; FI.y < 480; FI.y++)
		{
			for(FI.x =0; FI.x < 800; FI.x++)
			{
				*(FI.mmap_lcd_star+800*(479-FI.y)+(FI.x)) == FI.cache_bmp_1[3*(800*FI.y)+(FI.x)] << 0 |
				                                             FI.cache_bmp_1[(3*(800*FI.y)+(FI.x))+1] << 8 |
				                                             FI.cache_bmp_1[(3*(800*FI.y)+(FI.x))+2] << 16;
			}
		}

		v_info.xoffset = 0;
		v_info.yoffset = 0; //设置数据值，没有生效

		ioctl(FI.lcd_fd,FB_ACTIVATE_NOW,&v_info); //设置使能生效
		ioctl(FI.lcd_fd,FBIOPAN_DISPLAY,&v_info); //扫描显示缓存区中的画面

		close(FI.cache_bmp_fd_1);

		return 0;
	}

	int Cache_Display_Bmp(char * bmp_path)
	{
		if(strcmp(bmp_path,CACHE_PATH_1) == 0) //判断bmp_path是否在第一个房间
		{
			v_info.xoffset = 0;
			v_info.yoffset = 480;
			ioctl(FI.lcd_fd,FB_ACTIVATE_NOW,&v_info); //设置使能生效
			ioctl(FI.lcd_fd,FBIOPAN_DISPLAY,&v_info); //扫描显示缓存区中的画面
		}
		else if(strcmp(bmp_path,CACHE_PATH_2) == 0)
		{
			v_info.xoffset = 0;
			v_info.yoffset = 960;
			ioctl(FI.lcd_fd,FB_ACTIVATE_NOW,&v_info); //设置使能生效
			ioctl(FI.lcd_fd,FBIOPAN_DISPLAY,&v_info); //扫描显示缓存区中的画面
		}
		else
		{
			Dis_Bmp(bmp_path);
		}

		return 0;
	}

	int Display_Bmp()
	{
		FI.bmp_fd_1 = open(CACHE_PATH_1,OPEN_BMP_FLAG);
		FI.bmp_fd_2 = open(CACHE_PATH_2,OPEN_BMP_FLAG);
		if(FI.bmp_fd_1 == -1 || FI.bmp_fd_2 == -1)
		{
			perror("open bmp:");

			return -1;
		}

		char buf1[MALLOC_RGB],buf2[MALLOC_RGB];
		//char *RGB_BUF = (char *)malloc(MALLOC_RGB*(sizeof(char)));
		//char * RGB_BUF = (char *)malloc(malloc_length*(sizeof(char)));

		lseek(FI.bmp_fd_1,54,SEEK_SET);
		read(FI.bmp_fd_1,buf1,MALLOC_RGB);
		lseek(FI.bmp_fd_2,54,SEEK_SET);
		read(FI.bmp_fd_2,buf2,MALLOC_RGB);

		//把图片中的像素先刷进预留的后面两个缓存区当中
		int x,y;
		int *new_lcd_star = FI.mmap_lcd_star+(800*480);
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800; x++)
			{
				*(new_lcd_star+800*(479-y)+x) = buf1[3*(800*y+x)] << 0 | buf1[(3*(800*y+x))+1] << 8 | buf1[(3*(800*y+x))+2] << 16;
			}
		}

		new_lcd_star = FI.mmap_lcd_star+(800*480)*2;
		for(y = 0; y < 480; y++)
		{
			for(x = 0; x < 800; x++)
			{
				*(new_lcd_star+800*(479-y)+x) = buf2[3*(800*y+x)] << 0 | buf2[(3*(800*y+x))+1] << 8 | buf2[(3*(800*y+x))+2] << 16;
			}
		}

		//使用ioctl设置可见区到虚拟区的偏移量，来控制要显示的区域
		/*
	   int io_ret = ioctl(I.lcd_fd,FBIOGET_VSCREENINFO,&v_info);
	   if(io_ret == -1)
	   {
	     perror("ioctl lcd");
	     return -1;
	   }
		*/

		while(1)
		{
			v_info.xoffset = 0;
			v_info.yoffset = 480; //设置数据值，没有生效
			//设置使能生效
			ioctl(FI.lcd_fd,FB_ACTIVATE_NOW,&v_info);
			ioctl(FI.lcd_fd,FBIOPAN_DISPLAY,&v_info); //扫描显示缓存区中的画面
			sleep(1);
			v_info.xoffset = 0;
			v_info.yoffset = 960; //设置数据值，没有生效
			//设置使能生效
			ioctl(FI.lcd_fd,FB_ACTIVATE_NOW,&v_info);
			ioctl(FI.lcd_fd,FBIOPAN_DISPLAY,&v_info); //扫描显示缓存区中的画面
			sleep(1);
		}

		close(FI.bmp_fd_1);
		close(FI.bmp_fd_2);

		return 0;
	}

	int main(int argc, char const *argv[])
	{
		Init_Info();

		Cache_Init(0,NULL,NULL);
		while(1)
		{
			Cache_Display_Bmp(CACHE_PATH_1);
			sleep(1);
			Cache_Display_Bmp(CACHE_PATH_2);
			sleep(1);
		}

		//Display_Bmp();

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

	#define LCD_DEV_PATH  "/dev/fb0"
	#define DZK_FILE_PATH "/root/dzk16.Dzk"
	#define MAP_LENGTH 800*480*4
	#define MAP_OFFSET 0
	#define OPEN_FILE_FLAG O_RDWR

	struct files_info
	{
		int lcd_fd,dzk_fd;
		int dzk_size;
		int *lcd_star;
		int dzk_offset;
		int color;
		int *dzk_star;
	}FI;

	int Init_Info();
	int Free_Close();
	int Dis_Font(const char *font,int site_x,int site_y,int color);
	int Draw_Point(int site_x,int site_y,int color);
	int Draw_Chinese(const char * font,int site_x,int site_y,int color);

	int Init_Info()
	{
		//打开 LCD 汉字库
		FI.lcd_fd = open(LCD_DEV_PATH,OPEN_FILE_FLAG);
		FI.dzk_fd = open(DZK_FILE_PATH,OPEN_FILE_FLAG);
		if(FI.lcd_fd == -1 || FI.dzk_fd == -1)
		{
			perror("open lcd dzk:");

			return -1;
		}

		FI.dzk_size = lseek(FI.dzk_fd,0,SEEK_END); //求出汉字库的大小，才能根据他的大小进行申请映射空间
		lseek(FI.dzk_fd,0,SEEK_SET); //重置偏移量 偏移到首部位置
		//映射 LCD 和 DZK
		FI.lcd_star = (int *)mmap(NULL,MAP_LENGTH,PROT_READ | PROT_WRITE,MAP_SHARED,FI.lcd_fd,MAP_OFFSET);
		FI.dzk_star = (int *)mmap(NULL,FI.dzk_size,PROT_READ | PROT_WRITE,MAP_SHARED,FI.dzk_fd,MAP_OFFSET);
		if(FI.lcd_star == MAP_FAILED || FI.dzk_star == MAP_FAILED)
		{
			perror("mmap lcd or mmap dzk:");

			return -1;
		}

		return 0;
	}

	int Free_Close()
	{
		close(FI.lcd_fd);
		close(FI.dzk_fd);

		munmap(FI.lcd_star,MAP_LENGTH);
		munmap(FI.dzk_star,FI.dzk_size);

		return 0;
	}

	int Dis_Font(const char * font,int site_x,int site_y,int color) //font存放对应要显示的汉字
	{
		//先判断指定的坐标是否可用
		if(site_x+16 > 799) //判断 x
		{
			//回到行首
			site_x = 0;
			if(site_y+16 > 479)
			{
				printf("指定的坐标有误!!无法显示!!\n");

				return -1;
			}
			else
			{
				site_y+=16;
			}
		}

		//先让lcd的映射指针跳到对应要显示的左上角的坐标位置
		//int * new_lcd_p = FI.lcd_star+(800*where_y)+where_x;
		FI.dzk_offset = (94*(fontl[0]-1-0xa0)+(fontl[1]-1-0xa0))*32;
		char * new_dzk_p = FI.dzk_star+dzk_offset; //定义一个新的指针变量存放dzk字库的映射指针
		//求出要显示的汉字在字库中的偏移量
	  	//根据偏移量把映射指针跳到要显示的汉字对应的位置
	  	int x,y,z;
	  	char type;//定义一个字符变量存放每次判断的字符汉字库数据

	  	for(y = 0; y < 16; y++)
	  	{
	  		for(x = 0; x < 2; x++)
	  		{
	  			type = *(new_dzk_p+2*y+x); //获取字库中一个字节的汉字模
	  			for(z = 0; z < 8; z++)
	  			{
	  				if(type & (0x80 >> z))
	  				{
	  					//*(new_lcd_p+800*y+(8*x+z)) = 0x0000ff00;
	  					Draw_Point(site_x+(8*x+z),(site_y+y),color);
	  				}
	  				else
	  				{
	  					Draw_Point(site_x+(8*x+z),(site_y+y),0x00000000);
	  				}
	  			}
	  		}
	  	}

	  	return 0;
	}

	int Draw_Point(int site_x,int site_y,int color)
	{
		if(site_x > 799 || site_y > 479)
		{
			printf("指定的坐标有误!!无法显示!!\n");

			return -1;
		}

		*(FI.lcd_star+800*site_y+site_x) = color;

		return 0;
	}

	int Draw_Chinese(const char * fonts,int site_x,int site_y,int color)
	{
		//循环截取每一个汉字
		const char * new_f = fonts;
		int n;
		for(n = 0; n < (strlen(fonts)/2); n++)
		{
			Dis_Font(new_f,site_x,site_y,color);
			new_f += 2;
			site_x += 18;
		}

		return 0;
	}

	int main(int argc, char const *argv[])
	{
		Init_Info();

		//  Dis_Fontl("啊",400,240);//这里的啊要和字库的中文编码格式要一致（GBK）

		Draw_Chinese("你好中国!!",400,240,0xf0f0f0);

		Free_Close();


		return 0;
	}


	#include "stdio.h"
	#include "stdlib.h"
	#include "errno.h"
	#include "string.h"
	#include "dirent.h"

	//定义最大的路径名为200 不能超过200
	#define PATH_LENGTH 200

	int Find_File(const char *find_path,const char *find_file); //find_path:要检索的一级路径  find_file:要检索的文件

	int Find_File(const char *find_path,const char *find_file)
	{
		if(find_path == NULL || find_file == NULL)
		{
			printf("指定的目标参数无效!!不合法!!请输入正确的目标参数!!\n");

			return -1;
		}

		//第一次调用的时候打开一级目录
		DIR *dp = opendir(find_path); //指定要打开的目录
		if(dp == NULL)
		{
			perror("opendir");

			return -1;
		}

		//struct dirent * dir = (struct dirent*)malloc(sizeof(struct dirent));
		struct dirent dir;
		struct dirent *dir_pointer = &dir;
		//if(dir == NULL)
		// if(dir_pointer == NULL)
		// {
		// 	perror("malloc:");

		// 	return -1;
		// }

		//定义存放拼接好的目录路径
		char buf[PATH_LENGTH];
		while(1)
		{
			//memset(buf,0,sizeof(buf));
			memset(buf,0,PATH_LENGTH);
			//dir = readdir(dp);
			dir_pointer = readdir(dp);
			//if(dir == NULL)
			if(dir_pointer == NULL)
			{
				//perror("readdir:");
				break;
			}
			//判断检索到的是不是当前路径和上一级路径
			//if(dir->d_name[0] == '.') //.和..在Linux文件目录结构中都是在最前面的
			if(dir_pointer->d_name[0] == '.') //.和..在Linux文件目录结构中都是在最前面的
			{
				continue;
			}
			//判断读取到的目录项的类型  目录项:一个文件夹里面的文件和子文件夹都叫目录项
			//if(dir->d_type == DT_DIR) //判断读取到的是否是目录
			if(dir_pointer->d_type == DT_DIR) //判断读取到的是否是目录
			{
				if(find_path[strlen(find_path)-1] == '/')
				{
					//sprintf(buf,"%s%s",find_path,dir->d_name); //先拼接路径
					//strcat(buf,dir->d_name);
					strcat(buf,dir_pointer->d_name);//strcat只能拼接一次
				}
				else
				{
					//sprintf(buf,"%s/%s",find_path,dir->d_name); //先拼接路径
					//strcat(buf,dir->d_name);
					strcat(buf,dir_pointer->d_name);
				}

				//printf("%s\n", buf);

				//再调用自身打开
				//Find_File(buf,find_file);
				Find_File(buf,find_file);
			}

			//if(dir->d_type == DT_REG) // 判断是不是文件
			if(dir_pointer->d_type == DT_REG) // 判断是不是文件
			{
				//判断检索到的文件名字是否是要寻找的
				//if(strcmp(find_file,dir->d_name) == 0)
				//  if(eq->d_name[strlen(find_path)-2] == '.' && eq->d_name[strlen(find_path)-1] == 'c')   // find_path[strlen(find_path)-2] 最后一个字符 最后二个字符
	    		//  printf("--- --- %c\n",eq->d_name[strlen(eq->d_name)-1]);
	    		//if(dir->d_name[strlen(dir->d_name)-1] == 'c' && dir->d_name[strlen(dir->d_name)-2] == '.')
	    		//char * p = find_path+(strlen(find_path)-strlen(find_file)-1); //直接把.对应的字符地址赋值到字符指针变量p
	    		//if(strcmp(find_file,dir->d_name) == 0)
	    		//if(dir->d_name[strlen(dir->d_name)-4] == '.' && dir->d_name[strlen(dir->d_name)-3] == 'bmp')
	    		//if(dir->d_name[strlen(dir->d_name)-4] == '.' && dir->d_name[strlen(dir->d_name)-3] == 'b' && dir->d_name[strlen(dir->d_name)-2] == 'm' && dir->d_name[strlen(dir->d_name)-1] == 'p')
	    		if(dir_pointer->d_name[strlen(dir_pointer->d_name)-4] == '.' && dir_pointer->d_name[strlen(dir_pointer->d_name)-3] == 'b' && dir_pointer->d_name[strlen(dir_pointer->d_name)-2] == 'm' && dir_pointer->d_name[strlen(dir_pointer->d_name)-1] == 'p')
				{
					//printf("----%c\n", dir->d_name[strlen(find_path)-1]);
					if(find_path[strlen(find_path)-1] == '/')
					{
						//sprintf(buf,"%s%s",find_path,dir->d_name); //先拼接路径
						//strcat(buf,dir->d_name);
						strcat(buf,dir_pointer->d_name); //strcat只能拼接一次
					}
					else
					{
						//sprintf(buf,"%s/%s",find_path,dir->d_name); //先拼接路径
						//strcat(buf,dir->d_name);
						strcat(buf,dir_pointer->d_name);
					}

					printf("%s\n", buf);
				}
			}

		}

		//free(dir);
		//free(dir_pointer);
		closedir(dp);

		return 0;
	}

	int main(int argc, char const *argv[])
	{
		if(argc !=3)
		{
			printf("输入的指令有误!!无法搜索数据!!\n");

			return -1;
		}
		else
		{
			Find_File(argv[1],argv[2]);
			//Find_File(*(argv+1),*(argv+2));
		}


		return 0;
	}


## 省钱利器：
![](/hexo-private-blog-website/images/淘宝客2.jpg)
![](/hexo-private-blog-website/images/淘宝客3.jpg)
![](/hexo-private-blog-website/images/淘宝客4.jpg)

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


