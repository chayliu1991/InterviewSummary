# linux 的重要命令

-  readelf 命令 查看 ELF 文件的详细信息
- objdump命令 查看目标文件或者可执行的目标文件，可以显示反汇编代码
- ar 命令 用于操作高度结构化的存档文件(.a)
- ldd 用于打印程序或者库文件所依赖的共享库列表，ldd 不是一个可执行程序，而只是一个 shell 脚本
- lsof 是一个列出当前系统打开文件的工具
- netstat 命令用于显示各种网络相关信息
- ss 命令可以用来获取 socket 统计信息，它可以显示和 netstat 类似的内容。但ss的优势在于它能够显示更多更详细的有关TCP和连接状态的信息，而且比 netstat 更快速更高效

# C/CPP

## free 为什么不指定大小

操作系统中有一个记录空闲内存地址的链表。当操作系统收到程序的申请时，就会遍历该链表，然后就寻找第一个空间大于所申请空间的堆结点，然后就将该结点从空闲结点链表中删除，并将该结点的空间分配给程序。

malloc() 申请的空间实际分了两个不同性质的空间。一个就是用来记录管理信息的空间，另外一个就是可用空间了。而用来记录管理信息的实际上是一个结构体。

