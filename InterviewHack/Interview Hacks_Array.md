# Interview Hacks

标签（空格分隔）： Array

---

chapter 1 data structure
1.2 数组
Q：如何在一个排好序的数组中打印出唯一的unique数组元素
A：对于已经排好序的数组，打印unique的数组元素无非就是比较相邻的两个元素
```C
//assume it's a char * array
char * unique_array(char *s,int size)
{
    int i=1,c=1;
    for(c=1;c<=size;++c){
        //Only proceed when it's unique
        if(s[c]!=s[i-1]){
            s[i]=s[c];
            i++;
        }
    }
    s[i]='\0';//NULL terminate the string
    return (s);
}//-END-
```

Q：如何在一个字符串数组中(char* array)滤掉所有匹配的字符
A：在一个数组中找到匹配的字符，只需要循环一次而已
```C
//filter_array()
//s为字符串数组，size标识了数组的长度，待匹配字符为tgt
char * filter_array(char *s,int size,char tgt)
{
    int i=0,c;
    //loop：将数组中的每个字符元素与tgt比较
    for(c=0;c<size;c++){
        //如果当前数组下标c指向元素不等于tgt，则赋值给下标为i指向的元素
        //i实际上指向了新的结果数组（这个做法很smart不适用多余内存）
        if(s[c]!=tgt){
            s[i]=s[c];
            i++;
        }
    }//end-of-loop
    s[i]='\0';
    return(s);//返回的s为匹配完毕后的数组
}//-END-
```

Q：给你一个二维数组，如何以螺旋的方式打印出数组内容
A：边界效应（垂直转弯处）
```C
    void print_spiral(int** arr,int size)
    {
        int i,j,k,middle;
        for(i=size-1,j=0;i>0;i--,j++){
            for(k=j;k<i;k++)//first-row up to 2nd to the last colum
                printf("%d",arr[j][k]);
            
            for(k=j;k<i;k++)//last-col up to 2nd to the currently last row
                printf("%d",arr[k][i]);
                
            for(k=i;k>j;k--)//last-row up to 2nd to the currently first colum
                printf("%d",arr[i][k]);
                
            for(k=i;k>j;k--)//first-col up to 2nd to currently first row
                printf("%d",arr[k][j]);
        }
        printf("\n");
    }//end of print_spiral()
```

Q：如何给一个二维数组分配内存
A：这是一个关于二维数组内存申请的问题
```C
    //使用calloc()，假设有二维数组，尺寸为height*width
    //allocate height number of pointers to each row
    Array = (int**)calloc(Height,sizeof(int *));
    
    //allocate all space needed
    *(Array)=(int*)calloc(Width*Height,sizeof(int));
```


