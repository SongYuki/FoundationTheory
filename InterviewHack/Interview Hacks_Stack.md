# Interview Hacks

标签（空格分隔）： Stack

---
Q：能否用一个数组来封装两个堆栈，其中一个从头部开始增长，另一个从尾部开始增长，实现一个程序PUSH（X,S）可以把元素X推入堆栈S中，S为前面提到的两个堆栈之一，注意所有必要的边界和错误检查。
A：程序实现的逻辑要点如下：
 1. 两个堆栈都可以看到整个数组(array visible to both stacks)。
 2. 每次在调用PUSH()时，一个堆栈顶总是在增长（数组下标），而另一个的栈顶一定要总是在减少。
 3. 在s1.top==s2.top的时候，栈堆满(throws an exception)
 4. 以下代码仅仅实现了push 功能，还有pop,print等堆栈功能未实现。
 5. 示例只给出了整数类型的数组，如果使用C++STL能支持任意数据类型么。
```C
#include<stdio.h>
#define SIZE 16

int myarray[SIZE];
int top1 = -1;//堆栈1的栈顶
int top2 = SIZE;//堆栈2的栈顶

//Functions to push data
void push_stack1(int data)
{
    if(top1<top2-1){
        myarray[++top1]=data;
    }else{
        printf("Stack Full!Cannot Push\n"); //top1=top2栈满
    }
}
void push_stack2(int data)
{
    if(top1<top2-1){
        myarray[--top2]=data;
    }else{
        printf("Stack Full!Cannot Push\n");//top1=top2栈满
    }
}

int main()
{
    int i;
    printf("We can push a total of 10 values\n");
    //Number of elements pushed in stack 1 & stack 2 are both 8
    for(i=1;i<=8;++i){
        push_stack1(i);//[1-8]
        push_stack2(17-i);//[16-9]
        printf("Value Pushed in Stack 1 is %d,Stack2 is %d\n",i,17-i);
    }
    
    //Pushing on Stack Full
    printf("Pushing Value in Stack 1 is %d\n",17);
    push_stack1(17);
    return 0;
}
```
 
Q：何为堆栈溢出，如何检测。
A：堆栈溢出最常见最常见的表现形式就是page-fault错误。通俗的说，就是分配的内存空间（page）不够了，再用就越界了，于是系统报错(fault)。造成page-fault的一种可能是递归函数出现了无穷递归(infinite recursion)。
    大多数操作系统并没有做运行时(run-time)的堆栈溢出检测。如果要实现溢出检测，最简单的方法是调用sigaltstack()来获取(get)或设置(set)备用（alternate）信号堆栈的内容。
    当一个程序有可能会用光它的标准堆栈(standard stack)时，建立一个备用信号堆栈就显得很有意义。比如，在大多数linux系统中，堆栈向上或向下伸展到与自下而上生长的heap相碰撞的时候，或者是堆栈用光(exhausted)了的时候，内核会向该进程发送SIGSEGV（segmentation fault）信号，而备用信号堆栈是唯一能捕捉到这个信号的机制。
```C
/* stack_t 是描述备用信号堆栈的数据结构
   SIGSTKSZ 是系统定义的一个宏：足够大空间的备用信号栈
   Signalstack() 告诉系统来使用给信号栈分配的内存空间
*/
stack_t ss;
ss.ss_sp = malloc(SIGSTKSZ);
if(ss.ss_sp==NULL)/*Handle error*/;
ss.ss_size = SIGSTKSZ;
ss.ss_flags = 0;
if(sigaltstack(&ss,NULL)==-1);//Handle error
```

Q：什么是内存泄漏，如何侦测。
A：memory leak是计算机编程中永恒的话题。内存泄漏是资源泄漏的一种，当计算机程序不正确地管理和使用内存分配的时候就会发生内存泄漏。最常见的内存泄漏是，一个对象已经被分配了内存空间，但是运行代码中没有任何地方对其发生索引（使用），则该种现象就是内存泄漏。在更多的情况下，内存泄漏的发生是由于软件的使用时间增长(software aging)而造成（加重）的。
    内存泄漏在编程语言中极为常见，特别是那些没有内置垃圾收集机制的语言，例如C和C++，如不能对动态分配的内存进行及时回收管理，则内存泄漏通常会随之发生。即便是在java/android类OO语言中，尽管内置垃圾收集功能，内存泄漏依然会发生，比如：
 1. 可变静态域及集合vs 常量(Mutable static fields and collections vs Constants)
 2. 线程负载变量(thread-load variables)
 3. 循环引用(Circular references)
 4. JNI内存泄漏(JNI memory leaks)
    以上几种情形，如果没有明确地手工回收已分配内存，java的垃圾收集功能是不会自动工作的。解决方案就是，或者谨慎编程，或者使用内存分析工具检测，分析内存使用情况。
    帮助内存检测的工具，cmalloc,LeakAnalyzer,valgrind,purify,sentinet和Plumbr(Java)等。在嵌入式系统中，可以使用matrace,dmalloc,memwatch等。另外，寄存器eip/ebp对于检测内存溢出或泄漏也相当有用。

Q：在下面的操作之后，"a"的值是什么。
A：



Q：有如下的结构s:
A：



Q：strcpy()和memcpy()哪一个更快
A：


Q：如何实现你的fibonacci（0，1，1，2，3，5，8，13，21......）函数
A：