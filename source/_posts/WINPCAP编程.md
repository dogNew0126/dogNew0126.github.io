---
title: WINPCAP编程
date: 2022-05-28 10:33:30
tags:
  - 计算机网络
categories: 
  - 计算机网络
index_img: https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F106%20-%20kSLZvm9.png
description: 计算机网络上机实验二：WINPCAP编程
---

# WINPCAP编程

## Winpcap简介

Winpcap是UNIX下的libpcap移植到windows下的产物,他是一个free and open source的项目。Winpcap工作于驱动(Driver)层，所以能以很高的效率进行网络操作。

其提供了以下强大的功能

+ 捕获原始的数据包
+ 设置fliter，只捕获自己感兴趣的数据包
+ 方便地把捕获的数据包输出到文件和从文件输入
+ 发送原始的数据包
+ 统计网络流量

## 实验目的

+ 了解WINPCAP的架构

+ 学习WINPCAP编程

## 实验原理

WinPcap是一个基于Win32平台的，用于捕获网络数据包并进行分析的开源库. 

大多数网络应用程序通过被广泛使用的操作系统元件来访问网络，比如sockets。 这是一种简单的实现方式，因为操作系统已经妥善处理了底层具体实现细节（比如协议处理，封装数据包等等），并且提供了一个与读写文件类似的，令人熟悉的接口。 

然而，有些时候，这种“简单的方式”并不能满足任务的需求，因为有些应用程序需要直接访问网络中的数据包。也就是说，那些应用程序需要访问原始数据包，即没有被操作系统利用网络协议处理过的数据包。 

WinPcap产生的目的，就是为Win32应用程序提供这种访问方式； WinPcap提供了以下功能 

1. 捕获原始数据包，无论它是发往某台机器的，还是在其他设备（共享媒介）上进行交换的 

2. 在数据包发送给某应用程序前，根据用户指定的规则过滤数据包 

3. 将原始数据包通过网络发送出去 

4. 收集并统计网络流量信息 

以上这些功能需要借助安装在Win32内核中的网络设备驱动程序才能实现，再加上几个动态链接库DLL。 

所有这些功能都能通过一个强大的编程接口来表现出来,易于开发，并能在不同的操作系统上使用。这本手册的主要目标是在一些程序范例的帮助下，叙述这些编程接口的使用。 

WinPcap可以被用来制作网络分析、监控工具。一些基于WinPcap的典型应用有： 

1. 网络与协议分析器 (network and protocol analyzers) 

2. 网络监视器 (network monitors) 

3. 网络流量记录器 (traffic loggers) 

4. 网络流量发生器 (traffic generators) 

5. 用户级网桥及路由 (user-level bridges and routers) 

6. 网络入侵检测系统 (network intrusion detection systems (NIDS)) 

7. 网络扫描器 (network scanners) 

8. 安全工具 (security tools) 

## WINPCAP环境配置

1. 打开VS新建一个空项目并添加一个空的源文件

2. 点击菜单栏 项目->项目属性

3. 选择C/C++->预处理器，在预处理器定义中添加**WIN32**、WPCAP和HAVE_REMOTE三个宏定义，然后点击 应用；


![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F82%20-%20wN7ZhzX.png)

4. 选择链接器->输入，在附加依赖项添加wpcap.lib、ws2_32.lib、Packet.lib三个库，然后点击应用；

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F84%20-%20jtgejz6.png)

​		![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F83%20-%20RkAqJcE.png)

5. 选择VC++目录，添加WpdPack文件夹中的包含目录(include目录)，然后点击应用

​		![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F85%20-%20kdVqLVx.png)

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F86%20-%20JAHJo0c.png)

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F87%20-%20pqYx6Gm.png)

6. 同样，添加WpdPack文件夹中的库目录(Lib目录)，然后点击应用

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F88%20-%20BIYU1Eh.png)

7. 选择C/C++->语言，将 符合模式 设置为 否，点击确定

8. 将解决方案平台设置为x64

​		![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F89%20-%20EB6Bj3o.png)

9. 环境配置到此结束。注意，使用C语言开发，因此新建.c文件而不是.cpp文件。

10. 测试配置是否完成，复制以下代码到项目中运行测试。

    ```c
    #define WIN32      // 必须加这条，否则vs不会自动识别
    #include <stdio.h>
    #include <pcap.h>
    #pragma comment(lib, "wpcap.lib")
    
    main()
    {
    	pcap_if_t* alldevs, * d;
    	int i = 0;
    	char errbuf[PCAP_ERRBUF_SIZE];
    	if (pcap_findalldevs_ex(PCAP_SRC_IF_STRING, NULL,
    		&alldevs, errbuf) == -1)
    	{
    		fprintf(stderr, "Error in pcap_findalldevs_ex: %s\n",
    			errbuf);
    		exit(1);
    	}
    	for (d = alldevs; d != NULL; d = d->next)
    	{
    		printf("%d.%s", ++i, d->name);
    		if (d->description)
    		{
    			printf("(%s)\n", d->description);
    		}
    		else {
    			printf("(No description available)\n");
    		}
    	}
    	if (i == 0)
    	{
    		printf("\nNo interfaces found! Make sure Winpcap is installed.\n");
    		return;
    	}
    	// 释放设备
    	pcap_freealldevs(alldevs);
    }
    ```

## WINPCAP基本开发流程

+ WinPcap开发网络应用程序的大致流程：首先获取主机安装的网络设备列表，然后选择并激活某个网络设备，封装或过滤数据包，发送或解析数据包。

​	![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwinpcap%2F90%20-%20omkINVh.png)

### 获取设备列表

WinPcap提供了`pcap_findalldevs_ex()`函数来实现获取设备列表的功能，这个函数返回一个`pcap_if`结构的链表，每个这样的结构都包含了一个适配器的详细信息。值得注意的是，数据域`name`和`description`表示一个适配器名称和一个可以让人们理解的描述。下面获取适配器列表，并在屏幕上显示出来，如果没有找到适配器，将打印错误信息。

```C
#include <stdio.h>
#include "pcap.h"
#define PCAP_SRC_IF_STRING   "rpcap://"
#define PCAP_SRC_FILE_STRING   "file://"

main()
{
	pcap_if_t *alldevs;
	pcap_if_t *d;
	int i = 0;
	char errbuf[PCAP_ERRBUF_SIZE];

	/* 获取本地机器设备列表 */
	if (pcap_findalldevs_ex(PCAP_SRC_IF_STRING, NULL /* auth is not needed */, &alldevs, errbuf) == -1)
	{
		fprintf(stderr, "Error in pcap_findalldevs_ex: %s\n", errbuf);
		exit(1);
	}

	/* 打印列表 */
	for (d = alldevs; d != NULL; d = d->next)
	{
		printf("%d. %s", ++i, d->name);
		if (d->description)
			printf(" (%s)\n", d->description);
		else
			printf(" (No description available)\n");
	}

	if (i == 0)
	{
		printf("\nNo interfaces found! Make sure WinPcap is installed.\n");
		return;
	}
	getchar();
	/* 不再需要设备列表了，释放它 */
	pcap_freealldevs(alldevs);
}
```

有关这段代码的一些说明

pcap_findalldevs_ex() 是 WinPcap 提供的用以查找主机网络适配器功能的函数，它是 pcap_findalldevs() 函数的升级版，现在基本都选择使用 ex 版本，它不仅能查找本地的设备，还能查找远程的设备，关于此函数的定义如下

```C
// 函数原型：
int pcap_findalldevs_ex(char* source,  struct pcap_rmtauth *auth,  pcap_if_t** alldevs,   char* errbuf);
/* 参数说明
* source：指定是本地适配器还是远程适配器
* auth：身份验证信息，可以为NULL
* alldevs：存放获取的适配器数据，如果查找失败，其值为NULL
* errbuf：存放查找失败的相关错误信息
*/
// 返回值：成功返回0，失败返回-1
```

### 获取已安装设备的高级信息

参数`alldevs`的类型是`pcap_if_t`类型的二级指针，这里需要说明，`pcap_findalldevs_ex()`函数再获取到设备信息之后会将详细信息存储在一个结构体`pcap_if_t`中，他的定义如下：

```C
struct pcap_if_t
{
	struct pcap_if_t *next;		// 如果不为空，就指向下一个元素
	char *name;					// 设备名称
	char *description;			// 设备描述
	struct pcap_addr *addresses;// 接口地址列表
	bpf_u_int32 flags;			// 标志位，标志是否 loopback 设备
}
```

从中可以看出，我们需要建立一个`pcap_if_t`类型的指针来存储调用函数获取到的设备信息，然后直接通过指针就可以访问指定设备的相关信息。

同样，从`pcap_if_t`结构体的定义中可以看到，在结构体里面有定义了一个名为`pcap_addr`的结构体，用来存储接口地址列表，`pcap_addr`的定义如下：

```C
struct pcap_addr
{
	struct pcap_addr *next;			// 如果不为空，则指向下一个元素
	struct sockaddr *addr;			// 接口IP地址
	struct sockaddr *netmask;		// 接口网络掩码
	struct sockaddr *broadaddr;		// 接口广播地址
	struct sockaddr *dstaddr;		// 接口 P2P 目的地址
}
```

另外，函数`pcap_findalldevs_ex()`还能返回远程适配器信息和一个位于所给的本地文件夹的pcap文件列表。

下面的程序使用了`ifprint()`函数来打印出`pcap_if`结构体中的所有内容。程序对每一个由`pcap_findalldevs_ex()`函数返回的`pcap_if`，都调用`ifprint()`函数来打印。

```C
#define _CRT_SECURE_NO_WARNINGS

#include "pcap.h"
#include <stdio.h>
#define PCAP_SRC_IF_STRING   "rpcap://"
#define PCAP_SRC_FILE_STRING   "file://"

#ifndef WIN32
#include <sys/socket.h>
#include <netinet/in.h>
#else
#include <winsock.h>
#endif


// 函数原型
void ifprint(pcap_if_t* d);
char* iptos(u_long in);


int main()
{
	pcap_if_t* alldevs;
	pcap_if_t* d;
	char errbuf[PCAP_ERRBUF_SIZE + 1];
	char source[PCAP_ERRBUF_SIZE + 1];

	printf("Enter the device you want to list:\n"
		"rpcap://              ==> lists interfaces in the local machine\n"
		"rpcap://hostname:port ==> lists interfaces in a remote machine\n"
		"                          (rpcapd daemon must be up and running\n"
		"                           and it must accept 'null' authentication)\n"
		"file://foldername     ==> lists all pcap files in the give folder\n\n"
		"Enter your choice: ");

	fgets(source, PCAP_ERRBUF_SIZE, stdin);
	source[PCAP_ERRBUF_SIZE] = '\0';

	/* 获得接口列表 */
	if (pcap_findalldevs_ex(source, NULL, &alldevs, errbuf) == -1)
	{
		fprintf(stderr, "Error in pcap_findalldevs: %s\n", errbuf);
		exit(1);
	}

	/* 扫描列表并打印每一项 */
	for (d = alldevs; d; d = d->next)
	{
		ifprint(d);
	}

	pcap_freealldevs(alldevs);
	getchar();
	return 1;
}


/* 打印所有可用信息 */
void ifprint(pcap_if_t* d)
{
	pcap_addr_t* a;

	/* 设备名(Name) */
	printf("%s\n", d->name);

	/* 设备描述(Description) */
	if (d->description)
		printf("\tDescription: %s\n", d->description);

	/* Loopback Address*/
	printf("\tLoopback: %s\n", (d->flags & PCAP_IF_LOOPBACK) ? "yes" : "no");

	/* IP addresses */
	for (a = d->addresses; a; a = a->next) {
		printf("\tAddress Family: #%d\n", a->addr->sa_family);

		switch (a->addr->sa_family)
		{
		case AF_INET:
			printf("\tAddress Family Name: AF_INET\n");
			if (a->addr)
				printf("\tAddress: %s\n", iptos(((struct sockaddr_in*)a->addr)->sin_addr.s_addr));
			if (a->netmask)
				printf("\tNetmask: %s\n", iptos(((struct sockaddr_in*)a->netmask)->sin_addr.s_addr));
			if (a->broadaddr)
				printf("\tBroadcast Address: %s\n", iptos(((struct sockaddr_in*)a->broadaddr)->sin_addr.s_addr));
			if (a->dstaddr)
				printf("\tDestination Address: %s\n", iptos(((struct sockaddr_in*)a->dstaddr)->sin_addr.s_addr));
			break;

		default:
			printf("\tAddress Family Name: Unknown\n");
			break;
		}
	}
	printf("\n");
}



/* 将数字类型的IP地址转换成字符串类型的 */
#define IPTOSBUFFERS    12
char* iptos(u_long in)
{
	static char output[IPTOSBUFFERS][3 * 4 + 3 + 1];
	static short which;
	u_char* p;

	p = (u_char*)&in;
	which = (which + 1 == IPTOSBUFFERS ? 0 : which + 1);
	sprintf(output[which], "%d.%d.%d.%d", p[0], p[1], p[2], p[3]);
	return output[which];
}
```



当我们完成了设备列表的使用，我们要调用`pcap_freealldevs()`函数将其占用的内存资源释放。

### 打开适配器并捕获数据包

现在，我们已经知道如何获取适配器的信息了，那我们就开始一项更具意义的工作，打开适配器并捕获数据包。编写一个程序，将每一个通过适配器的数据包打印出来。
打开设备的函数是 `pcap_open()`。下面是参数 `snaplen`,` flags` 和 `to_ms` 的解释说明

+ `snaplen`：制定要捕获数据包中的哪些部分。 在一些操作系统中 (比如 xBSD 和 Win32)， 驱动可以被配置成只捕获数据包的初始化部分： 这样可以减少应用程序间复制数据的量，从而提高捕获效率。本例中，将值定为65535，它比能遇到的最大的MTU还要大，因此总能收到完整的数据包。

+ `flags`：最最重要的flag是用来指示适配器是否要被设置成混杂模式。 一般情况下，适配器只接收发给它自己的数据包， 而那些在其他机器之间通讯的数据包，将会被丢弃。 相反，如果适配器是混杂模式，那么不管这个数据包是不是发给我的，我都会去捕获。也就是说，我会去捕获所有的数据包。 这意味着在一个共享媒介(比如总线型以太网)，WinPcap能捕获其他主机的所有的数据包。 大多数用于数据捕获的应用程序都会将适配器设置成混杂模式，所以在下面的范例中，使用混杂模式。

+ `to_ms`：指定读取数据的超时时间，以毫秒计(1s=1000ms)。在适配器上进行读取操作(比如用 pcap_dispatch() 或 pcap_next_ex()) 都会在 to_ms 毫秒时间内响应，即使在网络上没有可用的数据包。 在统计模式下，to_ms 还可以用来定义统计的时间间隔。 将 to_ms 设置为0意味着没有超时，那么如果没有数据包到达的话，读操作将永远不会返回。 如果设置成-1，则情况恰好相反，无论有没有数据包到达，读操作都会立即返回。

```C
#include "pcap.h"
#define PCAP_SRC_IF_STRING   "rpcap://"
#define PCAP_SRC_FILE_STRING   "file://"
#define 	PCAP_OPENFLAG_PROMISCUOUS   1
//Defines if the adapter has to go in promiscuous mode.
#define 	PCAP_OPENFLAG_DATATX_UDP   2
//Defines if the data trasfer(in case of a remote capture) has to be done with UDP protocol.
#define 	PCAP_OPENFLAG_NOCAPTURE_RPCAP   4
//Defines if the remote probe will capture its own generated traffic.
#define 	PCAP_OPENFLAG_NOCAPTURE_LOCAL   8
//Defines if the local adapter will capture its own generated traffic.
#define 	PCAP_OPENFLAG_MAX_RESPONSIVENESS   16
//This flag configures the adapter for maximum responsiveness.

/* packet handler 函数原型 */
void packet_handler(u_char *param, const struct pcap_pkthdr *header, const u_char *pkt_data);

main()
{
	pcap_if_t *alldevs;
	pcap_if_t *d;
	int inum;
	int i = 0;
	pcap_t *adhandle;
	char errbuf[PCAP_ERRBUF_SIZE];

	/* 获取本机设备列表 */
	if (pcap_findalldevs_ex(PCAP_SRC_IF_STRING, NULL, &alldevs, errbuf) == -1)
	{
		fprintf(stderr, "Error in pcap_findalldevs: %s\n", errbuf);
		exit(1);
	}

	/* 打印列表 */
	for (d = alldevs; d; d = d->next)
	{
		printf("%d. %s", ++i, d->name);
		if (d->description)
			printf(" (%s)\n", d->description);
		else
			printf(" (No description available)\n");
	}

	if (i == 0)
	{
		printf("\nNo interfaces found! Make sure WinPcap is installed.\n");
		return -1;
	}

	printf("Enter the interface number (1-%d):", i);
	scanf("%d", &inum);

	if (inum < 1 || inum > i)
	{
		printf("\nInterface number out of range.\n");
		/* 释放设备列表 */
		pcap_freealldevs(alldevs);
		return -1;
	}

	/* 跳转到选中的适配器 */
	for (d = alldevs, i = 0; i< inum - 1; d = d->next, i++);

	/* 打开设备 */
	if ((adhandle = pcap_open(d->name,          // 设备名
		65536,            // 65535保证能捕获到不同数据链路层上的每个数据包的全部内容
		PCAP_OPENFLAG_PROMISCUOUS,    // 混杂模式
		1000,             // 读取超时时间
		NULL,             // 远程机器验证
		errbuf            // 错误缓冲池
		)) == NULL)
	{
		fprintf(stderr, "\nUnable to open the adapter. %s is not supported by WinPcap\n", d->name);
		/* 释放设备列表 */
		pcap_freealldevs(alldevs);
		return -1;
	}

	printf("\nlistening on %s...\n", d->description);

	/* 释放设备列表 */
	pcap_freealldevs(alldevs);

	/* 开始捕获 */
	pcap_loop(adhandle, 0, packet_handler, NULL);

	return 0;
}


/* 每次捕获到数据包时，libpcap都会自动调用这个回调函数 */
void packet_handler(u_char *param, const struct pcap_pkthdr *header, const u_char *pkt_data)
{
	struct tm *ltime;
	char timestr[16];
	time_t local_tv_sec;

	/* 将时间戳转换成可识别的格式 */
	local_tv_sec = header->ts.tv_sec;
	ltime = localtime(&local_tv_sec);
	strftime(timestr, sizeof timestr, "%H:%M:%S", ltime);

	printf("%s,%.6d len:%d\n", timestr, header->ts.tv_usec, header->len);

}
```

适配器被打开，捕获工作就可以用 `pcap_dispatch()` 或 `pcap_loop()` 进行。 这两个函数非常的相似，区别就是 `pcap_ dispatch()` 当超时时间到了(timeout expires)就返回 (尽管不能保证) ，而 `pcap_loop()` 不会因此而返回，只有当 cnt 数据包被捕获，所以，`pcap_loop()` 会在一小段时间内，阻塞网络的利用。`pcap_loop()` 对于这个简单的范例来说，可以满足需求，不过， `pcap_dispatch()` 函数一般用于比较复杂的程序中。

这两个函数都有一个 回调 参数， `packet_handler`指向一个可以接收数据包的函数。 这个函数会在收到每个新的数据包并收到一个通用状态时被libpcap所调用 ( 与函数 `pcap_loop()` 和 `pcap_dispatch()` 中的 user 参数相似)，数据包的首部一般有一些诸如时间戳，数据包长度的信息，还有包含了协议首部的实际数据。 注意：冗余校验码CRC不再支持，因为帧到达适配器，并经过校验确认以后，适配器就会将CRC删除，与此同时，大部分适配器会直接丢弃CRC错误的数据包，所以，WinPcap没法捕获到它们。

上面的程序将每一个数据包的时间戳和长度从 `pcap_pkthdr` 的首部解析出来，并打印在屏幕上。

### 不用回调方法捕获数据包

下面范例程序所实现的功能和效果和上一讲的非常相似（打开适配器并捕获数据包），但本讲将用`pcap_next_ex()`函数代替上面的`pcap_loop()`函数。

`pcap_loop()`函数是基于毁掉的原理来进行数据捕获，这是一种精妙的方法，并且在某些场合中，它是一种很好的选择。然而，处理回调有时候并不适用。他会增加程序的复杂度，特别是在拥有多线程的C++程序中。

可以通过直接调用`pcap_next_ex()`函数来获得一个数据包。只有当编程人员使用了`pcap_next_ex()`函数才能收到数据包。

这个函数的参数和捕获回调函数的参数是一样的。它包含一个网络适配器的描述符和两个可以初始化和返回给用户的指针(一个指向`pcap_pkthdr`结构体，另一个指向数据包数据的缓冲)。

在下面的程序中，会再次用到上一讲中的有关回调方面的代码，只是我们将它放入了main()函数，之后调用pcap_next_ex()函数。

```C
#define _CRT_SECURE_NO_WARNINGS
#include "pcap.h"
#define PCAP_SRC_IF_STRING   "rpcap://"
#define PCAP_SRC_FILE_STRING   "file://"
#define 	PCAP_OPENFLAG_PROMISCUOUS   1
//Defines if the adapter has to go in promiscuous mode.
#define 	PCAP_OPENFLAG_DATATX_UDP   2
//Defines if the data trasfer(in case of a remote capture) has to be done with UDP protocol.
#define 	PCAP_OPENFLAG_NOCAPTURE_RPCAP   4
//Defines if the remote probe will capture its own generated traffic.
#define 	PCAP_OPENFLAG_NOCAPTURE_LOCAL   8
//Defines if the local adapter will capture its own generated traffic.
#define 	PCAP_OPENFLAG_MAX_RESPONSIVENESS   16
//This flag configures the adapter for maximum responsiveness.

/* packet handler 函数原型 */
void packet_handler(u_char* param, const struct pcap_pkthdr* header, const u_char* pkt_data);

main()
{
	pcap_if_t* alldevs;
	pcap_if_t* d;
	int inum;
	int i = 0;
	pcap_t* adhandle;
	char errbuf[PCAP_ERRBUF_SIZE];

	/* 获取本机设备列表 */
	if (pcap_findalldevs_ex(PCAP_SRC_IF_STRING, NULL, &alldevs, errbuf) == -1)
	{
		fprintf(stderr, "Error in pcap_findalldevs: %s\n", errbuf);
		exit(1);
	}

	/* 打印列表 */
	for (d = alldevs; d; d = d->next)
	{
		printf("%d. %s", ++i, d->name);
		if (d->description)
			printf(" (%s)\n", d->description);
		else
			printf(" (No description available)\n");
	}

	if (i == 0)
	{
		printf("\nNo interfaces found! Make sure WinPcap is installed.\n");
		return -1;
	}

	printf("Enter the interface number (1-%d):", i);
	scanf("%d", &inum);

	if (inum < 1 || inum > i)
	{
		printf("\nInterface number out of range.\n");
		/* 释放设备列表 */
		pcap_freealldevs(alldevs);
		return -1;
	}

	/* 跳转到选中的适配器 */
	for (d = alldevs, i = 0; i < inum - 1; d = d->next, i++);

	/* 打开设备 */
	if ((adhandle = pcap_open(d->name,          // 设备名
		65536,            // 65535保证能捕获到不同数据链路层上的每个数据包的全部内容
		PCAP_OPENFLAG_PROMISCUOUS,    // 混杂模式
		1000,             // 读取超时时间
		NULL,             // 远程机器验证
		errbuf            // 错误缓冲池
	)) == NULL)
	{
		fprintf(stderr, "\nUnable to open the adapter. %s is not supported by WinPcap\n", d->name);
		/* 释放设备列表 */
		pcap_freealldevs(alldevs);
		return -1;
	}

	printf("\nlistening on %s...\n", d->description);

	/* 释放设备列表 */
	pcap_freealldevs(alldevs);

	int res;
	struct tm* ltime;
	char timestr[16];
	struct pcap_pkthdr* header;
	const u_char* pkt_data;
	time_t local_tv_sec;

	/* 获取数据包 */
	while ((res = pcap_next_ex(adhandle, &header, &pkt_data)) >= 0) {

		if (res == 0)
			/* 超时时间到 */
			continue;

		/* 将时间戳转换成可识别的格式 */
		local_tv_sec = header->ts.tv_sec;
		ltime = localtime(&local_tv_sec);
		strftime(timestr, sizeof timestr, "%H:%M:%S", ltime);

		printf("%s,%.6d len:%d\n", timestr, header->ts.tv_usec, header->len);
	}

	if (res == -1) {
		printf("Error reading the packets: %s\n", pcap_geterr(adhandle));
		return -1;
	}

	return 0;

	return 0;
}
```

为什么我们要用`pcap_next_ex()`代替以前的`pcap_next()`?因为`pcap_next()`有一些不好的地方。首先，他效率低下，尽管它隐藏了回调的方式，但它依然依赖于函数`pcap_dispatch()`。第二，它不能检测到文件末尾这个状态(EOF)，因此，如果数据包是从文件读取来的，那么它就不那么有用了。

值得注意的是，pcap_next_ex()在成功，超时，出错或EOF的情况下，会返回不同的值。

### 分析数据包

捕获的数据包的协议首部，打印一些网络上传输的UDP数据的信息。

```c
#define _CRT_SECURE_NO_WARNINGS
#include "pcap.h"
#define PCAP_SRC_IF_STRING   "rpcap://"
#define PCAP_SRC_FILE_STRING   "file://"
#define 	PCAP_OPENFLAG_PROMISCUOUS   1
//Defines if the adapter has to go in promiscuous mode.
#define 	PCAP_OPENFLAG_DATATX_UDP   2
//Defines if the data trasfer(in case of a remote capture) has to be done with UDP protocol.
#define 	PCAP_OPENFLAG_NOCAPTURE_RPCAP   4
//Defines if the remote probe will capture its own generated traffic.
#define 	PCAP_OPENFLAG_NOCAPTURE_LOCAL   8
//Defines if the local adapter will capture its own generated traffic.
#define 	PCAP_OPENFLAG_MAX_RESPONSIVENESS   16
//This flag configures the adapter for maximum responsiveness.

#include "pcap.h"

/* 4字节的IP地址 */
typedef struct ip_address {
	u_char byte1;
	u_char byte2;
	u_char byte3;
	u_char byte4;
}ip_address;

/* IPv4 首部 */
typedef struct ip_header {
	u_char  ver_ihl;        // 版本 (4 bits) + 首部长度 (4 bits)
	u_char  tos;            // 服务类型(Type of service) 
	u_short tlen;           // 总长(Total length) 
	u_short identification; // 标识(Identification)
	u_short flags_fo;       // 标志位(Flags) (3 bits) + 段偏移量(Fragment offset) (13 bits)
	u_char  ttl;            // 存活时间(Time to live)
	u_char  proto;          // 协议(Protocol)
	u_short crc;            // 首部校验和(Header checksum)
	ip_address  saddr;      // 源地址(Source address)
	ip_address  daddr;      // 目的地址(Destination address)
	u_int   op_pad;         // 选项与填充(Option + Padding)
}ip_header;

/* UDP 首部*/
typedef struct udp_header {
	u_short sport;          // 源端口(Source port)
	u_short dport;          // 目的端口(Destination port)
	u_short len;            // UDP数据包长度(Datagram length)
	u_short crc;            // 校验和(Checksum)
}udp_header;

/* 回调函数原型 */
void packet_handler(u_char* param, const struct pcap_pkthdr* header, const u_char* pkt_data);


main()
{
	pcap_if_t* alldevs;
	pcap_if_t* d;
	int inum;
	int i = 0;
	pcap_t* adhandle;
	char errbuf[PCAP_ERRBUF_SIZE];
	u_int netmask;
	char packet_filter[] = "ip and udp";
	struct bpf_program fcode;

	/* 获得设备列表 */
	if (pcap_findalldevs_ex(PCAP_SRC_IF_STRING, NULL, &alldevs, errbuf) == -1)
	{
		fprintf(stderr, "Error in pcap_findalldevs: %s\n", errbuf);
		exit(1);
	}

	/* 打印列表 */
	for (d = alldevs; d; d = d->next)
	{
		printf("%d. %s", ++i, d->name);
		if (d->description)
			printf(" (%s)\n", d->description);
		else
			printf(" (No description available)\n");
	}

	if (i == 0)
	{
		printf("\nNo interfaces found! Make sure WinPcap is installed.\n");
		return -1;
	}

	printf("Enter the interface number (1-%d):", i);
	scanf("%d", &inum);

	if (inum < 1 || inum > i)
	{
		printf("\nInterface number out of range.\n");
		/* 释放设备列表 */
		pcap_freealldevs(alldevs);
		return -1;
	}

	/* 跳转到已选设备 */
	for (d = alldevs, i = 0; i < inum - 1; d = d->next, i++);

	/* 打开适配器 */
	if ((adhandle = pcap_open(d->name,  // 设备名
		65536,     // 要捕捉的数据包的部分 
		// 65535保证能捕获到不同数据链路层上的每个数据包的全部内容
		PCAP_OPENFLAG_PROMISCUOUS,         // 混杂模式
		1000,      // 读取超时时间
		NULL,      // 远程机器验证
		errbuf     // 错误缓冲池
	)) == NULL)
	{
		fprintf(stderr, "\nUnable to open the adapter. %s is not supported by WinPcap\n");
		/* 释放设备列表 */
		pcap_freealldevs(alldevs);
		return -1;
	}

	/* 检查数据链路层，为了简单，我们只考虑以太网 */
	if (pcap_datalink(adhandle) != DLT_EN10MB)
	{
		fprintf(stderr, "\nThis program works only on Ethernet networks.\n");
		/* 释放设备列表 */
		pcap_freealldevs(alldevs);
		return -1;
	}

	if (d->addresses != NULL)
		/* 获得接口第一个地址的掩码 */
		netmask = ((struct sockaddr_in*)(d->addresses->netmask))->sin_addr.S_un.S_addr;
	else
		/* 如果接口没有地址，那么我们假设一个C类的掩码 */
		netmask = 0xffffff;


	//编译过滤器
	if (pcap_compile(adhandle, &fcode, packet_filter, 1, netmask) < 0)
	{
		fprintf(stderr, "\nUnable to compile the packet filter. Check the syntax.\n");
		/* 释放设备列表 */
		pcap_freealldevs(alldevs);
		return -1;
	}

	//设置过滤器
	if (pcap_setfilter(adhandle, &fcode) < 0)
	{
		fprintf(stderr, "\nError setting the filter.\n");
		/* 释放设备列表 */
		pcap_freealldevs(alldevs);
		return -1;
	}

	printf("\nlistening on %s...\n", d->description);

	/* 释放设备列表 */
	pcap_freealldevs(alldevs);

	/* 开始捕捉 */
	pcap_loop(adhandle, 0, packet_handler, NULL);

	return 0;
}

/* 回调函数，当收到每一个数据包时会被libpcap所调用 */
void packet_handler(u_char* param, const struct pcap_pkthdr* header, const u_char* pkt_data)
{
	struct tm* ltime;
	char timestr[16];
	ip_header* ih;
	udp_header* uh;
	u_int ip_len;
	u_short sport, dport;
	time_t local_tv_sec;

	/* 将时间戳转换成可识别的格式 */
	local_tv_sec = header->ts.tv_sec;
	ltime = localtime(&local_tv_sec);
	strftime(timestr, sizeof timestr, "%H:%M:%S", ltime);

	/* 打印数据包的时间戳和长度 */
	printf("%s.%.6d len:%d ", timestr, header->ts.tv_usec, header->len);

	/* 获得IP数据包头部的位置 */
	ih = (ip_header*)(pkt_data +
		14); //以太网头部长度

	/* 获得UDP首部的位置 */
	ip_len = (ih->ver_ihl & 0xf) * 4;
	uh = (udp_header*)((u_char*)ih + ip_len);

	/* 将网络字节序列转换成主机字节序列 */
	sport = ntohs(uh->sport);
	dport = ntohs(uh->dport);

	/* 打印IP地址和UDP端口 */
	printf("%d.%d.%d.%d.%d -> %d.%d.%d.%d.%d\n",
		ih->saddr.byte1,
		ih->saddr.byte2,
		ih->saddr.byte3,
		ih->saddr.byte4,
		sport,
		ih->daddr.byte1,
		ih->daddr.byte2,
		ih->daddr.byte3,
		ih->daddr.byte4,
		dport);
}
```

首先，将过滤器设置成"ip and udp"。在这种方式下，`packet_handler`只会收到基于IPv4的UDP数据包；这将简化解析过程，提高程序的效率。其次，创建了用于描述IP首部和UDP首部的结构体。这些结构体中的各种数据会被packet_handler()合理地定位。在开始捕捉前，使用了pcap_datalink() 对MAC层进行了检测，以确保在处理一个以太网络，确保MAC首部是14位的。IP数据包的首部就位于MAC首部的后面，将从IP数据包的首部解析到源IP地址和目的IP地址。

处理UDP的首部有一些复杂，因为IP数据包的首部的长度并不是固定的，可以通过IP数据包的length域来得到它的长度。一旦知道了UDP首部的位置就能解析到源端口和目的端口。

被解析出来的值被打印在屏幕上，形式上图所示，每一行分别代表一个数据包。

## Winpcap的一些基本功能的实现

### 捕获数据包

1. 枚举所有可用的设备`pcap_findalldevs_ex()`

2. 通过名字打开一个设备`pcap_open()`

   在这里可以打开一个文件，只是在打开这个文件之前需要通过`pcap_createsrcstr`创建相应的`name string`

3. 设置`Filter[pcap_compile,pcap_setfilter]`（可选）

4. 捕获数据

   有几种捕获数据的方法(捕获数据的数据都是最原始的数据包，即包含数据链路层的数据头)

   + 是以回调的方式`[pcap_loop,pcap_dispatch]`

   这两种方法基本相同，底层收集数据包，当满足一定的条件(timeout或者缓冲区满)，就会调用回调函数，把收集到的原始数据包s,交给用户。他们返回的数据缓冲区包含多个包。

   + `pcap_next_ex()`的方式

   每当一个包到达以后，`pcap_next_ex`就会返回，返回的数据缓冲区里只包含一个包

### 发送包

Winpcap中有发送单个包和发送多个包的方法。这里只说发送单个包。

1. 通过名字打开一个设备`[pcap_open]`
2. 自己构造一个原始数据包（这个数据包会不经过任何处理就发送出去，所以必须把包中的各个字段设置好。另外这个数据包是包含数据链路层报头的）
3. 使用`pcap_sendpacket()`发送数据包

### 统计网络流量

1. 通过名字打开一个设备`[pcap_open]`

   通过read_timeout来设置统计的时间间隔

2. 设置`filter[pcap_compile,pcap_setfilter]`(可选)

3. 设置设备的模式为统计模式`[pcap_setmode(MODE_STAT);]`	

4. 开始统计，`pcap_loop/pcap_dispatch()`

5. 在回调函数中的参数中就包含了统计信息。	

## 习题与思考题

1、WINPCAP是否能实现服务质量的控制？

答：不能。WinPcap可以独立地通过主机协议发送和接受数据，如同TCP-IP。 这就意味着WinPcap不能阻止、过滤或操纵同一机器上的其他应用程序的通讯： 它仅仅能简单地"监视"在网络上传输的数据包。所以，它不能提供类似网络流量控制、服务质量调度和个人防火墙之类的支持，因而不能实现服务质量的控制。

# tips

使用winpcap踩了大坑。

Npcap是基于Winpcap和Libpcap的，Winpcap已多年无人维护，其官网也推荐Windows XP之后的用户转移到Npcap上。Npcap基于WINPCAP，Winpcap基于libcap，并做出了诸多改进。

后面都是用Npcap开发了。
