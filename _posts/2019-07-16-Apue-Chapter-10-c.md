---
layout: post
title:  "[Apue]Chapter 10 - c"
subtitle: "信号"
date:   2019-07-16 00:59:32 +0800
categories: [apue]
---

# 信号集

信号集（signal set）是一个能表示多个信号的数据类型。

一般而言，不能用整型量中的一位代表一种信号，也就是说，不能用一个整型变量表示信号集

<br><br>

**POSIX.1定义数据类型 sigset_t，它可以包含一个信号集，并且定义了下列 5个处理信号集的函数。**

![](/images/Apue/APUE_10/APUE_10_9.png)

函数 sigemptyset 初始化由 set 指向的信号集，清除其中所有信号。

函数 sigfillset 初始化由 set 指向的信号集，使其包括所有信号。

函数 sigaddset 将一个信号添加到已有的信号集中。

函数 sigdelset 则从信号集中删除一个信号。

函数 sigismember 判断给定的信号（signo 参数），是否时信号集中（set 参数）的一个成员。

<br>

**注意：**

**（1）**所有应用程序在使用信号集前，要对该信号集调用 sigemptyset 或 sigfillset 一次。

这是因为 C编译程序 把 不赋初值的外部变量和静态变量都初始化为0。一旦初始化了一个信号集，以后就可以在该信号集中操作（增、删）特定的信号。

<br>

**（2）**所有以信号集作为参数的函数，信号集参数通常以指针的形式传递。

<br><br>

# 函数 sigprocmask

调用函数 sigprocmask 可以检测 或 更改，或 同时进行检测和更改进程的信号屏蔽字。

![](/images/Apue/APUE_10/APUE_10_10.png)

**参数：**

**（1）**参数 oset

如果它是非空指针，那么进程的当前信号屏蔽字通过 oset 返回。

<br>

**（2）**参数 set

a，如果 set 参数是一个非空指针，那么 how 参数表示如何修改当前信号屏蔽字。下图说明了 how 参数可选的值。

![](/images/Apue/APUE_10/APUE_10_11.png)

SIG_BLOCK 是或操作，而SIG_SETMASK 则是赋值操作。注意，不能阻塞 SIGKILL 和 SIGSTOP 信
号。

<br>

b，如果 set 参数是一个空指针，那么不改变该进程的信号屏蔽字， how 参数的值也没有意义。

<br>

------

<br>

在调用 sigprocmask 函数后，如果有任何（未决的、不再阻塞的）信号，那么在 sigprocmask 函数返回前，至少要把其中的一个信号发送给该进程。

sigprocmask 函数 仅为单线程定义的。处理多线程进程中的信号屏蔽字，需要使用另一个函数。

<br><br>

# 函数 sigpending

sigpending 函数返回一个信号集，对于调用进程而言，其中的各个信号是阻塞的、不能传送的。因此，该信号当前是未决的。该信号集通过 set 参数返回。

![](/images/Apue/APUE_10/APUE_10_12.png)

<br><br>

# 函数 sigaction

sigaction 函数的功能是：检查 或 修改 （或 检查并修改）与指定信号相关联的处理动作。

![](/images/Apue/APUE_10/APUE_10_13.png)

参数 signo 是要检测或修改其具体动作的信号编号。

如果 act 指针参数非空，那么就要修改信号的动作。

如果 oact 指针参数非空，那么系统经由 oact 指针返回该信号的上一个动作。 

<br><br>

**参数 act 和 参数 oact 使用的结构内容，如下：**

![](/images/Apue/APUE_10/APUE_10_14.png)





























































































