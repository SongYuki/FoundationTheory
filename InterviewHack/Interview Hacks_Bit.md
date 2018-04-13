# Interview Hacks

标签（空格分隔）： Bit&Byte

---
chapter 1 data structure
Q：将整数的某一位置一(set bit)或清零(clear bit)
A：这道题的解决方案分两部分：
 1. 先定义一个宏（macro），这个宏就实现一个功能：返回二进制整数，把第n位设为1（其他位为0）.
 2. 根据需要，对需要操作的整数与宏的结果进行或操作。
```C
//way 1
#define SET_BIT(n)  (0x01<<(n))
int setBit(int n,int value)
{
    value |= SET_BIT(n); //OR操作，设第n位为1，其他位不变
}

//way 2
setBit(int n,int value)
{
    value = value | (0x01<<n);//把宏直接写在代码里
}
//-END OF SET BIT-
```
清零的操作也分两部分：
 1. 使用~SET_BIT(n)，结果是除了第n位是零，其他位都置为1
 2. 进行与操作
```C
//Clear Bit
ClearBit(int n,int value)
{
    //先将事先准备好的SET_BIT()结果反转，使得第n位是0其它位是1
    //然后进行与操作
    value &= ~SET_BIT(n);
}//-END OF CLEAR BIT-
```

Q：对一个整数乘7的最快的方法是什么
A：二进制操作，位操作来完成*8(左移三位)，然后自减一次。int X;X = X<<3-X;
Q：对于现代CPU而言，加减乘除哪个操作最昂贵？由于微处理器的内核实现不尽相同，这个问题可以变得很复杂，不过一般而言：

 - 除法操作大概是乘法操作的10倍时间
 - 乘法操作需要加法，减法或比较操作的四倍时间
 - 取绝对值操作是加减法操作的二倍时间
 - 其他操作，包括exp,sqrt,sin/cos/tan/arctan大约是加减法时间的50-100倍之多
 
 
 Q：能否用一行C表达式来判断整数x是否是2的n次方
 A：这是一个需要借助位操作来完成的问题。
 2的N=[1-n]次方用二进制表述有何特点呢？x = 0x10,0x100,0x1000,ox1000 0000...(也就是第N位是1，其他位是0)
 x-1恰好反过来 ： x-1 = 0x01 0x011,0x0111,0x111 1111...
 如果x*(x-1)结果就是全零，放到宏里一句话可写为：
```C
    #define is_power_of_two(x) (x)?(!(x&(x-1))):0
```

Q：怎么判断一个整数中或一个数组中有多少位或多少元素为1
A：利用位操作的问题。让x与x-1进行与操作，会造成1个bit被unset为零，放到循环中循环的次数就是整数二进制表述时被设置的位的数量
```C
    //x is the integer or a specific array element
    int cout = 0;
    while(x){
        //只要x不为0就继续循环
        x &= (x-1); //bit-AND cause 1 bit to unset(0).
        ++count;//keep track of number of 1s set.
    }
    //返回的count值即为1的位数
```

Q：如何实现htonl()函数
A：htonl()指的是hostlong to network(byte-order)long，反之为ntohl()。对应的short类型的转换为htons()与ntohs()。通常在i386架构中，主机字节顺序(host byte order)为最小有效字节优先（Least Significant Byte First）;而在TCP/IP网络中为最大有效字节优先（Most Significant Byte First），两者的区别我们称之为大端（Big Endian）和小端（Little Endian）.在小端模式中，低位字节放在低地址，高位字节放在高地址；在大端模式中，低位字节放在高地址，高位字节放在低地址。
```C
//htonl()其实就是从大端到小端之间互转的一个实现
//BYTE-shifting(2-way shift,not rotary shift)
#define my_htonl(LB)\
    (((uint32)(LB)&0xff000000)>>24)|\
    (((uint32)(LB)&0x00ff0000)>>8)|\
    (((uint32)(LB)&0x0000ff00)<<8)|\
    (((uint32)(LB)&0x000000ff)<<24)
```

Q：如何判断字节顺序是大端还是小端
A：大端（BE）：高字节在低位，小端（LE）：低字节在低位。从数据结构的角度上，设计一个union数据结构，定义一个双字节整数s和一个双字节字符数组c[].
如果s被初始化为：0x0102,只要检验c[0]与c[1]，就可以做出判断了。**选对数据结构并加以充分利用，则事半而功倍**。
```C
    //define a union data structure
    union{
        short s;
        char c[sizeof(short)];//Assuming 2B short
    }un;
    un.s = 0x0102;
    if(un.c[0]==1&&un.c[1]==2){
        //Big-Endian
    }else if(un.c[0]=2&&un.c[1]==1){
        //Little-Endian
    }else{
        //Error or Unknown byte order.
    }//-END-
```