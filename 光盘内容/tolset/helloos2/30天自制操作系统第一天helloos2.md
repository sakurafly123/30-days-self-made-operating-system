# helloos2
## 准备工作
- 虚拟机（VMware-workstation)
- 编辑器（vs code)
- 编译器（nask)
- 文章使用的程序源代码：https://download.csdn.net/download/zl18206208825/11243484
## 编辑代码
````
; hello-os
; TAB=4

; 以下这段是标准FAT12格式软盘专用的代码

		DB		0xeb, 0x4e, 0x90
		DB		"HELLOIPL"		; 启动区的名称可以是任意的字符串
		DW		512				; 每个扇区（sector)的大小（必须为512字节）
		DB		1				; 簇（cluster)的大小（必须为1个扇区）
		DW		1				; FAT12的起始位置（一般从第一个扇区开始）
		DB		2				; FAT的个数（必须为2)
		DW		224				; 根目录的大小（一般设成224项）
		DW		2880			; 该磁盘的大小（必须是2880扇区）
		DB		0xf0			; 磁盘的种类（必须是0xf0）
		DW		9				; FAT的长度（必须是9扇区）
		DW		18				; 1个磁道（track)有几个扇区（必须是18）
		DW		2				; 磁头数（必须是2）
		DD		0				; 不使用分区，必须是0
        DD      2880            ; 重写一次磁盘大小
		DB		0,0,0x29		; 意义不明，固定
		DD		0xffffffff		; (可能是)卷标号码
		DB		"HELLO-OS   "	; 磁盘的名称（11字节）
		DB		"FAT12   "		; 磁盘格式名称
		RESB	18				; 先空出18字节

; 程序主体

		DB		0xb8, 0x00, 0x00, 0x8e, 0xd0, 0xbc, 0x00, 0x7c
		DB		0x8e, 0xd8, 0x8e, 0xc0, 0xbe, 0x74, 0x7c, 0x8a
		DB		0x04, 0x83, 0xc6, 0x01, 0x3c, 0x00, 0x74, 0x09
		DB		0xb4, 0x0e, 0xbb, 0x0f, 0x00, 0xcd, 0x10, 0xeb
		DB		0xee, 0xf4, 0xeb, 0xfd

; 信息显示部分
		
		DB		0x0a, 0x0a		;两个换行
		DB		"hello, world"
		DB		0x0a			;换行
		DB		0

		RESB	0x1fe-$			; 填写0x00,直到 0x001fe

		DB		0x55, 0xaa

; 以下是启动区以外部分的输出

		DB		0xf0, 0xff, 0xff, 0x00, 0x00, 0x00, 0x00, 0x00
		RESB	4600
		DB		0xf0, 0xff, 0xff, 0x00, 0x00, 0x00, 0x00, 0x00
		RESB	1469432

````
以上代码保存为helloos.nas并存放在helloos2文件夹内。
## 编译与运行
- 在tolset文件夹中新建子文件夹helloos2；
- 复制 tolset 文件夹中子文件夹z_new_w中，!cons_9x.bat和!cons_nt.bat文件到helloos2文件夹内；
- 打开记事本输入以下代码，并另存为asm.bat且放在helloos2文件夹内:
````
..\z_tools\nask.exe helloos.nas helloos.img
````
- 同上，打开记本输入以下代码，并保存为run.bat且放在helloos2文件夹内:
````
copy helloos.img ..\z_tools\qemu\fdimage0.bin
..\z_tools\make.exe -C ..\z_tools\qemu
````
- 以上，文件都准备好了，下面开始运行。
	1. 点击运行!cons_nt.bat，出现如下界面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200207194847971.png)
	2. 在打开的命令行程序中输入 asm 并回车（这里的nask.exe win7的还可用，win10完全不兼容，如果有高手看到的话，请@阿龙_LAMW我也想学会啊！！！）并生成helloos.img文件：
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20200207195444911.png)
	3. 接着输入 run 并回车输出以下界面：
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20200207195729960.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3psMTgyMDYyMDg4MjU=,size_16,color_FFFFFF,t_70)
- 接下来使用VMware-Workstation运行。
	1. 创建虚拟机
	 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200207200421249.png)
	 因为大多所以就在这里一一发了，直接上链接：https://blog.csdn.net/zl18206208825/article/details/104118226
	2.运行虚拟机，会输出以下界面
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20200207200848587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3psMTgyMDYyMDg4MjU=,size_16,color_FFFFFF,t_70)
	好了，以上是**30天自制操作系统第1天helloos2**,请大家多多支持！！！
