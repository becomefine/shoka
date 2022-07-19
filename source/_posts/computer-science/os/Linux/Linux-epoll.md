---
title: 'linux内核: 深度理解epoll本质'
date: 2022-04-09 01:40:58
categories:
 - [计算机科学,OS,Linux]
tags: 
 - 操作系统
 - Linux
 - epoll
---

# 深度理解epoll本质

[转载来源](https://bbs.gameres.com/thread_842984_1_1.html)

## 网卡接收数据

1、网卡收到网线传来的数据
2、硬件电路的传输
3、将数据写入到内存中的某个地址上
![计算机结构图](/assets/os/epoll1.jpg)

## 如何知道接收了数据

中断: 计算机执行程序时，会有优先级的需求。例如当计算机收到断电信号时，它应立即去保存数据，保存数据的程序具有较高的优先级。

一般而言，由硬件产生的信号需要cpu立马做出回应，所以它的优先级很高，cpu理应中断掉正在执行的程序，去做出响应，当CPU完成对硬件的响应后，再重新执行用户程序。中断的过程和函数调用差不多，只不过函数调用是事先定好位置，而中断的位置由"信号"决定。

:::info
当网卡把数据写入到内存后，网卡向cpu发出一个中断信号，操作系统便能得知有新数据到来，再通过网卡中断程序去处理数据。
:::

## 进程阻塞为什么不占用cpu资源

阻塞是进程调度的关键一环，指的是进程在等待某事件(如接收到网络数据)发生之前的等待状态，recv、select和epoll都是阻塞方法。

普通的recv接收:
```c
//创建
socketint s = socket(AF_INET, SOCK_STREAM, 0);   
//绑定bind(s, ...)//监听listen(s, ...)//接受客户端连接
int c = accept(s, ...)
//接收客户端数据recv(c, ...);//将数据打印出来printf(...)
```
先创建socket对象，依次调用bind、listen、accept，最后调用recv接收数据。recv是一个阻塞方法，当程序运行到recv时，他会一直等待，直到接收到数据才往下执行。

### 阻塞原理

#### 工作队列

操作系统为了支持多任务，实现了进程调度的功能，会把进程分为"运行"和"等待"等几种状态。运行状态是进程获得cpu使用权，正在执行代码的状态；等待状态是阻塞状态，比如上述程序运行到recv时，程序会从运行状态变为等待状态，接收到数据后又变回运行状态。

下图中的计算机中运行着A、B、C三个程序，进程A执行着上述基础网络程序，一开始，这3个进程都被操作系统的工作队列所引用，处于运行状态，会分时执行。
![工作队列](/assets/os/epoll2.jpg)

#### 等待队列

当进程A执行到创建socket的语句时，操作系统会创建一个由文件系统管理的socket对象，这个socket对象包含了发送缓冲区、接收缓冲区、等待队列等成员。等待队列是个非常重要的结构，指向所有需要等待该socket事件的进程。
![等待队列](/assets/os/epoll3.jpg)
当程序执行到recv时，操作系统会将进程A从工作队列移动到该socket的等待队列中。
![等待队列](/assets/os/epoll4.jpg)
由于工作队列只剩下进程B和C，依据进程调度，cpu会轮流执行这两个进程的程序，不会执行进程A的程序。所以进程A被阻塞，不会往下执行代码，也不会占用cpu资源。

#### 唤醒进程

当socket接收到数据后，操作系统将该socket等待队列上的进程重新放回到工作队列，该进程变成运行状态，继续执行代码，也由于socket的接收缓冲区已经有了数据，recv可以返回接收到的数据。

## 内核接收网络数据全过程

![内核接收数据](/assets/os/epoll5.jpg)
进程在recv阻塞期间，计算机收到对端传送的数据，数据经由网卡传送到内存，然后网卡通过中断信号通知cpu有数据到达，cpu执行中断程序。此处的中断程序主要有两项功能，先将网络数据写入到对应socket的接收缓冲区里面，再唤醒进程A，重新将进程A放入工作队列中

![唤醒进程](/assets/os/epoll6.jpg)

操作系统如何知道网络数据对应哪个socket: 一个socket对应一个端口号，网络数据包中含有ip和端口的信息，内核可以通过端口号快速找到对应的socket。为提高处理速度，操作系统会维护端口号到socket的索引结构，以快速读取。

## 同时监视多个socket的简单方法

服务端需要管理多个客户端连接，而recv只能监视单个socket。epoll的要以是高效的监视多个socket。

假如能够预先传入一个socket列表，如果列表中的socket都没有数据，挂起进程，知道有一个socket收到数据，唤醒进程。

在如下代码中，先准备一个数组(fds)，让fds存放着所有需要监视的socket。然后调用select，如果fds中的所有socket都没有数据，select会阻塞，直到有一个socket接收到数据，select返回，唤醒进程。用户可以遍历fds，通过FD_ISSET判断具体哪个socket收到数据，然后做出处理。

```c
int s = socket(AF_INET, SOCK_STREAM, 0);
bind(s, ...)
listen(s, ...)

int fds;
//存放需要监听的socket

while(1){
    int n = select(..., fds, ...)
    for(int i = 0; i < fds.count; i++ ){
        if(FD_ISSET(fds[i], ...)){
            //fds[i]的数据处理
        }
    }
}

```

### select的流程

![select流程](/assets/os/epoll7.jpg)
select的实现思路很直接，假如程序同时监视如下图的socket1、socket2和socket3，那么在调用select之后，操作系统把进程A分别假如这三个socket的等待队列中。

当一个socket收到数据后，中断程序将唤起进程。
![数据处理](/assets/os/epoll8.jpg)

所谓唤起进程，就是将进程从所有的等待队列中移除，加入到工作队列里面
![唤起进程](/assets/os/epoll9.jpg)

由这些步骤，当进程A被唤醒后，它知道至少一个socket接收了数据。程序只需遍历一遍socket列表，就可以得到就绪的socket。

这种简单方式行之有效，在几乎所有操作系统都有对应的实现。

缺点:
- 每次调用select都需要将进程加入到所有监视socket的等待序列，每次唤醒都需要从每个队列中移除。这里涉及了两次遍历，而且每次都要将整个fds列表传递给内核，有一定的开销。正是因为遍历操作开销大，出于效率的考量，才会规定select的最大监视数量，默认只能监视1024个socket。

- 进程被唤醒后，程序并不知道那些socket收到数据，还需要遍历一次。

## epoll设计思路

epoll是在select出现N年后才被发明，是select和poll的增强版本。epoll通过以下一些措施来改进效率。

### 功能分离
select低效的原因之一是将“维护等待队列”和“阻塞进程”两个步骤合二为一。每次调用select都需要这两步操作，然而大多数应用场景中，需要监视的socket相对固定，并不需要每次都修改。epoll将这两个操作分开，先用epoll_ctl维护等待队列，再调用epoll_wait阻塞进程，显而易见的，效率就能得到提升。
![epoll](/assets/os/epoll10.jpg)

代码如下，先用epoll_create创建一个epoll对象epfd，再通过epoll_ctl将需要监视的socket添加到epfd中，最后调用epoll_wait等待数据。

```c
int s = socket(AF_INET, SOCK_STREAM, 0);
bind(s,...)
listen(s,...)

int epfd = epoll_create(...); 
epoll_ctl(epfd,...);

while(1){
    int n = epoll_wait(...)
    for(接收到数据的cocket){
        //处理
    }
}
```

### 就绪列表

select低效的另一个原因在于程序不知道哪些socket收到数据，只能一个个遍历。如果内核维护一个"就绪列表"，引用收到数据的socket，就能避免遍历。如下图，计算机共有三个socket，收到数据的socket2和socket3被rdlist(就绪列表)所引用。当进程被唤醒后，只要获取rdlist的内容，就能知道哪些socket收到数据。
![epoll](/assets/os/epoll11.jpg)

## epoll的原理和流程

### 创建epoll对象

当某个进程调用epoll_create方法时，内核会创建一个eventpoll对象(也就是程序中epfd所代表的对象)。eventpoll对象也是文件系统中的一员，和socket一样，他也会有等待序列。
![epoll](/assets/os/epoll12.jpg)

创建一个代表该epoll的eventpoll对象是必需的，因为内核要维护"就绪列表"等数据，"就绪列表"可以作为eventpoll的成员。

### 维护监视列表

创建epoll对象后，可以用epoll_ctl添加或删除所要监视的socket。以添加socket为例，如果通过epoll_ctl添加socket1、socket2和socket3的监视，内核会将eventpoll添加到这三个socket的等待队列中。
![epoll](/assets/os/epoll13.jpg)
当socket收到数据后，中断程序会操作eventpoll对象，而不是直接操作进程。

### 接收数据

当socket收到数据后，中断程序会给eventpoll的"就绪列表"添加socket引用。如socket2和socket3收到数据后，中断程序让rdlist引用这两个socket。
![epoll](/assets/os/epoll14.jpg)
eventpoll对象相当于是socket和进程之间的中介，socket的数据接收并不影响进程，而是通过改变eventpoll的就绪列表来改变进程状态。

当程序执行到epoll_wait时，如果rdlist已经引用了socket，那么epoll_wait直接返回，如果rdlist为空，阻塞进程。

### 阻塞和唤醒进程

假设计算机中正在运行进程A和进程B，在某时刻进程A运行到了epoll_wait语句。内核会将进程A放入eventpoll的等待队列中，阻塞进程。
![epoll](/assets/os/epoll15.jpg)

当socket接收到数据，中断程序一方面修改rdlist，另一方面唤醒eventpoll等待队列中的进程，进程A再次进入运行状态。也因为rdlist的存在，进程A可以知道哪些socket发生了变化。
![epoll](/assets/os/epoll16.jpg)

## epoll的实现细节

eventpoll包含了lock、mtx、wq(等待队列)、rdlist等成员。
![epoll](/assets/os/epoll17.jpg)

### 就绪列表的数据结构

就序列表引用着就绪的socket，所以它应能够快速的插入数据。
程序可能随时调用epoll_ctl添加监视socket，也可能随时删除。当删除时，若该socket已经存放在就绪列表中，它也应该被移除。

所以就绪列表应是一种能够快速插入和删除的数据结构。双向列表就是这样一种数据结构，epoll使用双向列表实现就绪队列(rdlist)。

### 索引结构

既然epoll将“维护监视队列”和“进程阻塞”分离，也意味着需要有个数据结构来保存监视的socket。至少要方便的添加和移除，还要便于搜索，以避免重复添加。红黑树是一种自平衡二叉查找树，搜索、插入和删除时间复杂度都是O(log(N))，效率较好。epoll使用了红黑树作为索引结构（对应上图的rbr）。

ps：因为操作系统要兼顾多种功能，以及由更多需要保存的数据，rdlist并非直接引用socket，而是通过epitem间接引用，红黑树的节点也是epitem对象。同样，文件系统也并非直接引用着socket。为方便理解，本文中省略了一些间接结构。