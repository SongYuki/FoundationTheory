# Interview Hacks

标签（空格分隔）： String

---

chapter 1 data structure
1.2 字符串
Q：实现自己的atoi()函数
A：这个题问的是如何把ASCII码转换成整数（Integer）类型，比如将字符串“12345”转换成数字12345.至少要知道ASCII的“0”对应哪个数字，才能顺利的把0-9中的任何一个转换成数字类型。此外，这个问题需要考虑的边界条件——正负数一个都不能少。
```C
char *a(int i);
Int len = strlen(a);
Int i=0,j=0,sign=2;//2 stands for '+'
If(a[0]=='-'){
    sign=1;
    ++j;
}else if(a[0]=='+'){
    sign=2;
    ++j;
}
while(j<len){
    i=10*i+(a[j]-48);//48 = 0x30 which is ASCII '0'
    ++j;
}
if(sign==1)
    i*=-1;
//-END-
//NOTE:a-z:0x41-5a(65-90); A-Z:0x61-7a(97-122)
```




