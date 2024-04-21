本章讲解MPI的6个基本函数，掌握了这六个基本函数，就能开始写一个MPI程序了。考虑到读者可能有人擅长C++，有人擅长Fortran。因此在讲这些函数接口时，我会将两种语言的接口和参数类型都附上。如果只是应用MPI处理科学计算的问题，很多参数不用深究其意义，甚至不需要记忆，只需要在写或读代码的时候随时查手册即可。这6个基本函数基本可以实现大部分功能，因此有必要对这6个函数有比较准确的理解。

## 1.MPI\_Init

任何MPI程序都应该首先调用该函数。 此函数不必深究，只需在MPI程序开始时调用即可（必须保证程序中第一个调用的MPI函数是这个函数）。

```cpp
call MPI_INIT() # Fortran MPI_Init(&argc, &argv) //C++ & C
```

Fortran版本调用时不用加任何参数，而C和C++需要将main函数里的两个参数传进去，因此在写main函数的主程序时，应该加上这两个形参。

```cpp
int main(int *argc,char* argv[]) { MPI_Init(&argc,&argv); }
```

## 2.MPI\_Finalize

任何MPI程序结束时，都需要调用该函数。切记Fortran在调用MPI\_Finalize的时候，需要加个参数ierr来接收返回的值，否则计算结果可能会出问题甚至编译报错。在Fortran中ierr为integer型变量。 该函数同第一个函数，都不必深究，只需要求格式去写即可。

```cpp
call MPI_Finalize(ierr) # Fortran MPI_Finalize() //C++
```

## 3.MPI\_COMM\_RANK

```cpp
call MPI_COMM_RANK(comm, rank) int MPI_Comm_Rank(MPI_Comm comm, int *rank)
```

该函数是获得当前进程的进程标识，如进程0在执行该函数时，可以获得返回值0。可以看出该函数接口有两个参数，前者为进程所在的通信域，后者为返回的进程号。通信域可以理解为给进程分组，比如有0-5这六个进程。可以通过定义通信域，来将比如\[0,1,5\]这三个进程分为一组，这样就可以针对该组进行“组”操作，比如规约之类的操作。这类概念会在之后的MPI进阶一章中讲解。MPI\_COMM\_WORLD是MPI已经预定义好的通信域，是一个包含所有进程的通信域，目前只需要用该通信域即可。

在调用该函数时，需要先定义一个整型变量如myid，不需要赋值。将该变量传入函数中，会将该进程号存入myid变量中并返回。

比如，让进程0输出Hello，让进程1输出Hi就可以写成如下方式。

```fortran
Program main use mpi implicit none integer :: myid MPI_INIT() call MPI_COMM_RANK(MPI_COMM_WOLRD,myid) if (myid==0) then print *, "Hello!" end if if (myid==1) print *, "Hi!" end if MPI_FINALIZE() end Program
```

C和C++版本如下

```cpp
#include "mpi.h" int main(int *argc,char* argv[]) { int myid; MPI_Init(&argc,&argv); MPI_Comm_Rank(MPI_COMM_WORLD,&myid); if(myid==0) { printf("Hello!"); } if(myid==1) { printf("Hi!"); } MPI_Finalize(); }
```

## 4.MPI\_COMM\_SIZE

该函数是获取该通信域内的总进程数，如果通信域为MP\_COMM\_WORLD，即获取总进程数，使用方法和MPI\_COMM\_RANK相近。

```cpp
MPI_COMM_SIZE(comm, size) int MPI_Comm_Size(MPI_Comm, int *size)
```

## 5.MPI\_SEND

该函数为发送函数，用于进程间发送消息，如进程0计算得到的结果A，需要传给进程1，就需要调用该函数。

```cpp
call MPI_SEND(buf, count, datatype, dest, tag, comm) int MPI_Send(type* buf, int count, MPI_Datatype, int dest, int tag, MPI_Comm comm)
```

该函数参数过多，不过这些参数都很有必要存在。

这些参数均为传入的参数，其中buf为你需要传递的数据的起始地址，比如你要传递一个数组A，长度是5，则buf为数组A的首地址。count即为长度，从首地址之后count个变量。datatype为变量类型，注意该位置的变量类型是MPI预定义的变量类型，比如需要传递的是C++的int型，则在此处需要传入的参数是MPI\_INT，其余同理。dest为接收的进程号，即被传递信息进程的进程号。tag为信息标志，同为整型变量，发送和接收需要tag一致，这将可以区分同一目的地的不同消息。比如进程0给进程1分别发送了数据A和数据B，tag可分别定义成0和1，这样在进程1接收时同样设置tag0和1去接收，避免接收混乱。

## 6.MPI\_RECV

该函数为MPI的接收函数，需要和MPI\_SEND成对出现。

```cpp
call MPI_RECV(buf, count, datatype, source, tag, comm，status) int MPI_Recv(type* buf, int count, MPI_Datatype, int source, int tag, MPI_Comm comm, MPI_Status *status)
```

参数和MPI\_SEND大体相同，不同的是source这一参数，这一参数标明从哪个进程接收消息。最后多一个用于返回状态信息的参数status。

在C和C++中，status的变量类型为MPI\_Status，分别有三个域，可以通过status.MPI\_SOURCE，status.MPI\_TAG和status.MPI\_ERROR的方式调用这三个信息。这三个信息分别返回的值是所收到数据发送源的进程号，该消息的tag值和接收操作的错误代码。

在Fortran中，status的变量类型为长度是MPI\_STATUS\_SIZE的整形数组。通过status(MPI\_SOURCE)，status(MPI\_TAG)和status(MPI\_ERROR)来调用。

SEND和RECV需要成对出现，若两进程需要相互发送消息时，对调用的顺序也有要求，不然可能会出现死锁或内存溢出等比较严重的问题，具体在之后的对等模式这一章中详细介绍。

## Example

发送和接收这两个函数参数过多，初学可能看不懂部分参数的意义以及使用方法，在学了这六个函数之后，再来看本系列[第一章](https://zhuanlan.zhihu.com/p/356295181)提到的一个简单的例子，这个例子就把这六个函数都使用上了。

```cpp
//第一章提到的案例，具体描述可以回看第一章 MPI_Init(&argc,&argv); MPI_Comm_rank(MPI_COMM_WORLD,&myid); //得到的变量myid即为当前的进程号 //假设要求和的数组为A={[1,1,1,1],[2,2,2,2]} if(myid==0) { memset(A,1,sizeof(int)); //将数组A全赋值为1 } else if (myid==1) { memset(A,2,sizeof(int)); //将数组A全赋值为2 } //以上部分是将数组的两行分别存储到进程0和进程1上 for(int i=0;i<4;i++) { s=s+A[i]; } if(myid==1) { MPI_Send(s,1,MPI_INT,0,99,MPI_COMM_WORLD); //将求和结果s发送到进程0 } if(myid==0) { MPI_Recv(s1,1,MPI_INT,1,99,MPI_COMM_WORLD,&status); //用s1这个变量来存储从进程1发送来的求和结果 s=s+s1; } printf("%d",&s); MPI_Finalize();
```

## 总结

这一章主要介绍了MPI的最基本的六大接口，理解了这六个函数接口，就可以写一个最基本的MPI并行程序了，下一章详细讲解如何写一个MPI的基本程序，通过编程实践，将会进一步加深对这些函数接口的理解。

## 参考资料

都志辉. 高性能计算并行编程技术:MPI并行程序设计\[M\]. 清华大学出版社, 2001.

## 本系列目录如下

## 版权声明

写该教程的初衷是帮助初学者入门。因此本文欢迎转载或引用，但请标明转载或引用并说明原文出处，附上本文的链接，并私信告知我。未经允许不可用以商业用途。