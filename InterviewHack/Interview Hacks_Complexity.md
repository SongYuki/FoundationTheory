# Interview Hacks

标签： Complexity

---
2.2 算法复杂性

Q：实现一个排序算法，要求其算法复杂性优于O(N*N).

A：比较排序算法(comparative sorting)，注定其性能不会优于O(nlogn).

```C
qsort((void *)array,strlen(array),sizeof(array[0],CmpFunc));

int CmpFunc(const void *a_,const void *b_){
    Const char *a = (const char *)a_;
    Const char *b = (const char *)b_;
    Return (strcmp(a,b)<0?1:0);
}
```


Q：能不能聊一下排序算法复杂性的问题？

A：算法类型包括七大类型：交换(exchange)，选择(selection)，插入(insertion)，合并(merge)，分布(distribution)，并行(concurrent)，混合(hybrid)。
在排序算法中，通常具有O(nlogn)或更低的计算复杂度的算法被认为是好的算法(good behavior)；而具有O(n^2)或更好计算复杂度的算法会被认为是差的算法。
