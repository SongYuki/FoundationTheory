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

Q：实现一个函数完成如下功能：输入一个字符串参数，检查其是否为整型，如果是则返回整数值。
A：先处理符号的问题，然后处理循环，在循环中判断是否有边界条件发生
```C
    int str2int(const char *str)
    {
        int value=0;
        int sign=(str[0]=='-')?-1:1;//正负数判断
        int i = (str[0]=='-')?1:0;//decide starting index
        char ch;
        
        while(ch=str[i++])
        {
            if((ch>='0')&&(ch<='9'))
                value = value*10+(ch-'0');
            else
                return 0;//non-integer
        }
        return value*sign;
    }//-END-
```

Q：实现你的itoa()
A： 1. 利用sprintf(s,"%d",i);
    2. 把每个数字转换为ASCII类型，然后转储为char *array.
    3. 递归调用putchar()(利用putchar('0'+current_digit);)


 Q：用且只用putchar()来打印整数，并且最好不使用额外的存储
 A：这个问题很适合用递归来解决，一个整数能有几个字节呢（1，2，4，8）
```C
    //recusion is one way(since integer is only 16 or 32 or 64 bits long)
    void putlong(long ln)
    {
        if(ln==0)return;
        if(ln<0){
            putchar('-');//print - sign first
            putlong(abs(ln)); //print as positive number
            return;
        }
        putlong(ln/10);//print higher digits
        //print last digit(mod),'0'+will convert to ASCII
        putchar('0'+ln%10);
    }//-END-
```
 
 Q：把一串连续的字符转换为大写
 A：关键是知道A-Z,a-z对应的ASCII码
```C
    //A ASCII is 65, a ASCII is 97 difference 32
    void ToUpper(char *s)
    {
        while(*S!=0)
        {
            *S = (*S>='a'&&*S<='z')?(*S-'a'+'A'):*S;
            //or islower(*S){*S=*S-'A'+'a';}....
            S++;
        }
    }//-END-
```

Q：如何反转一串数字
A：可以分别使用递归和循环
```C
    void reversenum(int i)
    {
        if(i==0) return(0);
        printf("%d",i%10);
        reversenum((int)i/10);//print all remaining digits
    }//-END OF RECURSIVE
    
    #include<stdio.h>
    int main()
    {
        int n,reverse = 0;
        printf("Enter a number to reverse\n");
        scanf("%d",&n);
        
        while(n!=0){
            reverse = reverse*10;
            reverse = reverse + n%10;
            n = n/10;
        }
        
        printf("Reverse of entered number is = %d\n",reverse);
        return 0;
    }//-END OF LOOP-
```

Q：如何将一句话中的每个单词翻转
A：算法复杂性为O(1.5*N):
 1. 从字符串尾部开始向前遍历
 2. 当发现空格或tab，找出此空格与后面空格间的单词，反转
 3. 继续向前直到到达串头