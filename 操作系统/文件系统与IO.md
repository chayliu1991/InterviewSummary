# 硬链接与软链接

硬链接是指将该文件名直接指向一个另一个文件的 inode，当删除原文件后，由于该文件名了解该文件 inode，所以不会受到影响。每次创建硬链接会使 inode 中记录的引用计数 +1，当引用计数减少到0，则释放该 inode。硬链接是不能跨文件系统建立的。

创建软链接会真正的分配一个 inode 存储指向原文件的文件名，也就是读取该 inode，则直接跳转到原文件的目录下进行查找原文件从而进行操作。所以，软链接是可以跨文件系统建立的。类似 windows 的快捷方式，给文件创建一个快速的访问路径，它依赖于原文件。

删除源文件后硬链接还可以访问源文件数据，软链接失效。原因：硬链接与源文件共用同一个inode，删除源文件后只是减少了inode的一个链接数，硬链接文件还可以继续访问源文件数据。而软链接是通过源文件路径来访问数据，但是源文件已经删除，所以路径访问不到，无法获取源文件数据。

# 磁盘调度算法

## 先来先服务(FCFS)

按照磁盘请求的顺序进行调度。

优点是公平和简单。缺点也很明显，因为未对寻道做任何优化，使平均寻道时间可能较长。

## 最短寻道时间优先(SSTF)

优先调度与当前磁头所在磁道距离最近的磁道。

虽然平均寻道时间比较低，但是不够公平。如果新到达的磁道请求总是比一个在等待的磁道请求近，那么在等待的磁道请求会一直等待下去，也就是出现饥饿现象。具体来说，两端的磁道请求更容易出现饥饿现象。

## 电梯扫描算法(SCAN)

电梯总是保持一个方向运行，直到该方向没有请求为止，然后改变运行方向。

电梯算法（扫描算法）和电梯的运行过程类似，总是按一个方向来进行磁盘调度，直到该方向上没有未完成的磁盘请求，然后改变方向。

# 常见IO模型

- 同步阻塞 IO（Blocking IO）：用户线程发起IO读/写操作之后，线程阻塞，直到可以开始处理数据
- 同步非阻塞I O（Non-blocking IO）：发起 IO 请求之后可以立即返回，如果没有就绪的数据，需要不断地发起 IO 请求直到数据就绪；不断重复请求消耗了大量的 CPU 资源
- IO 多路复用：是指单个进程/线程就可以同时处理多个IO请求
- 异步 IO（Asynchronous IO）：用户线程发出 IO 请求之后，继续执行，由内核进行数据的读取并放在用户指定的缓冲区内，在 IO 完成之后通知用户线程直接使用

# 中断

中断的处理过程：

- 保护现场：将当前执行程序的相关数据保存在寄存器中，然后入栈
- 开中断：以便执行中断时能响应较高级别的中断请求
- 中断处理
- 关中断：保证恢复现场时不被新中断打扰
- 恢复现场：从堆栈中按序取出程序数据，恢复中断前的执行状态

中断和轮询的区别：

- 轮询：CPU 对特定设备轮流询问。中断：通过特定事件提醒 CPU
- 轮询：效率低等待时间长，CPU 利用率不高。中断：容易遗漏问题，CPU 利用率不高





