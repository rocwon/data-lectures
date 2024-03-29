# Lecture 1: 数据模型

在计算机科学中，数据（Data）是指以二进制形式存在于存储介质上的字节（Byte）序列。数据承载了信息（Information），而从信息中则可以提炼出知识（Knowledge）。这个讲义只关注数据的形态和存储方式，不包括数据分析乃至知识工程相关的内容。

从存储器的角度看，每个byte是中性的，不需要去理解其意义，它只是占据了一个存储单元。它关注的焦点是怎么快速地将一个byte写到指定的位置，又如何从指定位置读出来，读写的过程既要速度快又要不出错。

但是计算机不能以byte为单位管理数据，粒度太细了。存储介质大抵分为两类，一类是临时性的，掉电以后数据就消失了，叫做内存（Memory）。根据它的特性，也称作RAM（Random Access Memory，随机访问存储器）；一类是持久性的，写入以后不依赖于供电也会存在，现在最常见的就是硬盘（Hard Disk）。

操作系统的通行做法是以文件（File）为单位管理磁盘上的数据，以进程（Process）为单位管理内存中的数据。文件就是一系列逻辑上有关系的byte的集合，本质上它还是byte序列，只不过这些bytes有了严格的边界。File是对数据的宏观抽象（Abstract），让离散的、无意义的字节都找到了自己的组织，作为一个整体，具备了初步的意义（比如我们说这个文件是一份简历）。在计算机科学里，无处不抽象，在Windows中，File还包含了更广阔的范围，比如串口（Series Port），网络（Network）都被当作文件对待。

内存里的bytes被以进程为单位隔离起来，进程之间是不允许互相访问对方的内存空间的，跨进程的越界访问是一种危险的行为，有可能取得毫无意义的数据，有可能会导致进程崩溃。所以，编程的时候，进程间通信（IPC）是一件麻烦的事情，只能通过共享内存（Shared Memory）、命名管道（Named Pipe）或者网络套接字（Socket）进行数据交换。数据在自己的进程空间里，也被划分为不同的区域进行管理（比如Stack, Heap）。因此，文件系统、进程管理和内存管理都是操作系统最基础的功能，操作系统课程的大多数篇幅都在讨论这几个主题。

内存里每个区域中的bytes又按照不同的数据类型或者复杂的数据结构进行分组（比如这连续的4个byte表示一个整数），按照它们在内存里的地址（位置）去读写。同样，文件里的内容也是如此，要按照某种规则去解释这些bytes，比如同样是2个byte，可以将其理解为一个short / int-16 形式的整数，也可以将其理解为是一个Unicode编码的汉字。这是一种从可理解性出发的、对数据的微观抽象，是围绕类型（Type）和编码（ Encoding）两个角度进行的。

如果把视角切换到应用的角度看，为了数据处理的方便，在宏观和微观结构之间，可能还需要增加一个抽象层次，它将最小的有意义的数据单元组织起来，构成某一种结构形态，我们称之为数据模型（Data Model）。数据模型是一种逻辑结构，针对不同的数据模型，数据处理的方式不同，虽然从操作系统的角度看起来都是在调用文件系统的create / remove, open / close, write / read / seek等几个系统调用。

当然，不是所有的数据都具有这种逻辑结构的。我们将不具有特定逻辑结构的数据称之为非结构化数据（Unstructured Data），具有特定形态结构的数据称之为结构化数据（Structured Data），介于这二者之间的形态称之为半结构化数据（Semi-Structured Data）。这里所谓的“特定形态”，就是具有高度组织和整齐的、格式化的存储形式，便于修改和搜索。说得更严谨一点就是：数据是否能满足或者转换为用关系代数的理论描述。

## 1.1 非结构化数据

