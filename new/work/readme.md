# V4L2

流程：

打开设备->检查属性和设置属性->设置帧格式->设置一种输入输出方法->循环获取数据->关闭设备。

打开设备文件：

```
int fd=open(”/dev/video0″,O_RDWR);
```

取得设备的属性：

```
int ioctl(int fd, VIDIOC_QUERYCAP, struct v4l2_capability* argp);
```

看看设备具有什么功能，比如是否具有视频输入，或者音频输入输出等。

设置和获取视频的制式和帧格式：

```
int ioctl(int fd, VIDIOC_S_FMT, struct v4l2_fmtdesc* argp);		/* 设置捕获视频的格式 */
int ioctl(int fd, VIDIOC_G_FMT, struct v4l2_fmtdesc* argp);		/* 获取设置支持的视频格式 */
int ioctl(int fd, VIDIOC_ENUM_FMT, struct v4l2_fmtdesc* argp);  /* 查询所有支持的格式*/       
```

制式包括 PAL，NTSC，帧的格式个包括宽度和高度等。

图像的缩放：

```
int ioctl(int fd, VIDIOC_CROPCAP, struct v4l2_crop* argp);  /* 图像的剪裁和缩放*/        
```

申请和管理缓冲区：

````
int ioctl(int fd, VIDIOC_REQBUFS, struct v4l2_requestbuffers* argp);  /* 向驱动提出申请内存的请求 */    
````

向驱动申请帧缓冲，一般不超过5个。

应用程序和设备之间的数据交换一共有三种方式：直接 red/write，内存映射，和用户指针。这里列举的是针对用户映射。将申请到的帧缓冲映射(mmap)到用户空间，这样就可以直接操作采集到的帧了，而不必去复制。

帧缓冲全部入队列：

```
int ioctl(int fd, VIDIOC_QBUF, struct v4l2_bufer* argp);  /* 将空闲的内存加入可捕获视频的队列 */ 

struct v4l2_buffer {
	__u32			index;  //缓存编号
	__u32			type;   //视频捕获模式
	__u32			bytesused;  //缓存已使用空间大小
	__u32			flags; //缓存当前状态
/*
	flags 为缓存当前状态（常见值有 V4L2_BUF_FLAG_MAPPED | V4L2_BUF_FLAG_QUEUED | V4L2_BUF_FLAG_DONE，分别代表当前缓存已经映射、缓存可以采集数据、缓存可以提取数据）
*/
	__u32			field;
	struct timeval		timestamp;
	struct v4l2_timecode	timecode;
	__u32			sequence; //@ 队列中的序号

	/* memory location */
	__u32			memory;
	union {
		__u32           offset; //@ 缓冲帧地址，支队 MMAP 有效
		unsigned long   userptr;
		struct v4l2_plane *planes;
		__s32		fd;
	} m;
	__u32			length;  //@ 缓冲帧长度
	__u32			reserved2;
	__u32			reserved;
};
```

以便存放采集到的数据。

开始和停止视频的采集：

```
int ioctl(int fd, VIDIOC_STREAMOFF);  /* 关闭视频流 */ 
```

出队列以取得已采集数据的帧缓冲：

```
int ioctl(int fd, VIDIOC_DQBUF, struct v4l2_bufer* argp);  /* 将已经捕获好视频的内存拉出已捕获视频的队列 */  
```

将缓冲重新入队列尾：

```
int ioctl(int fd, VIDIOC_QBUF, struct v4l2_bufer* argp);  /* 将空闲的内存加入可捕获视频的队列 */ 
```

这样可以循环采集。

停止视频的采集：

```
int ioctl(int fd, VIDIOC_STREAMON);  /* 打开视频流 */ 
```

关闭设备：

```
close(fd);
```

