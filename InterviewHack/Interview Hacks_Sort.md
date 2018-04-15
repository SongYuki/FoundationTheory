# Interview Hacks

标签（空格分隔）： sort

---

chapter 2 算法与优化

排序

Q：说说你了解的排序算法
A：排序算法按照其特点可分为七类：
 - 并发排序算法(concurrent sort)：比如：biotonic mergesort,odd-even mergesort.
 - 分布排序算法（distribution sort）：常见的分布排序算法有：bucket sort,counting sort,pigeonhole sort,radix sort,flash sort,American flag sort（radix sort的变种，可用来对大的数据集排序，如字符串排序）等。
 - 交换排序算法（exchange sort）：包括bubble sort（效率较低，不甚实用），cocktail sort（双向冒泡排序），odd-even sort,comb sort,genome sort,quick sort等。
 - 混合排序算法(hybrid sort)：混合算法通常是递归类算法在工业界的优化实现（比如，分而治之算法，小而化之算法等，著名的算法如quick sort 和 binary search）。常见的混合排序算法包括：timsort,Jsort,spread sort,block sort,introsort(quick sort + heap sort).
 - 插入排序算法(insertion sort)：常见的有shell sort,library sort,insertion sort.
 - 合并排序算法（merge sort）：常见的有：merge sort及其变种，strand sort等。**merge sort对于基于内存的数组的排序效率通常低于quick sort，而空间复杂度通常又高于heap sort.**
 - 选择排序算法（selection sort）:这类算法用于找到数组或链表中第k个最小值（也可以用于找到最小，最大或中间值）。选择排序算法是最短路径或最近邻居问题中的子问题。常见的有：**selection sort,heap sort,smooth sort,cycle sort,tonurnament sort,cartesian tree sort.**
 
Q：对常见的排序算法进行优势对比
A： - **quick sort**：本质上是分而治之，是comparison sort（比较排序算法）的一种具体实现，其他同类型的比较排序法还包括**heap sort,insertion sort,bubble sort,merge sort,cocktail sort**等至少十几种。
- **bucket sort**：又称作bin sort(bin=bucket，桶的意思)。bucket sort最适合用于数组排序，并且在数组的元素分布相对比较均匀时效率最高。当只用两个buckets来排序的时候，也称为quick sort。bucket sort 是分布排序（distribution sort，指需要通过一些中间数据结构来临时存储待排序元素）的一种，其他此类排序方法有counting sort,radix sort等。
- **merge sort**：在Python，Java，Android中的默认算法都基于timsort，而timsort本身是merge sort+insertion sort的混合体。merge sort的最大特点是高可扩展性，它的最差运行复杂度是O(nlogn).缺点就是空间复杂度为O(2n)，也就是需要O(n)的额外存储空间来完成排序。
- **heap sort**：可以看做是selection sort的高效，进阶版本，通过使用heap数据结构（二叉树的一种特殊表现形式）来完成排序，heap sort的最差运行复杂度是O(nlogn)，而其最差空间复杂度为O(1)。


Q：如何排序一个单链表
A：对于链表的排序，merge sort是最常用的方法。相比之下，quick sort的表现可能会很差，而heap sort则可能完全无法完成排序。
merge sort的算法描述如下：
MergeSort(headRef)
 1. 如果头指针为空或只有一个元素，返回。
 2. 否则，把链表一分为二（也可以一分为多，这里以二为例说明）
```C
    FrontBackSpilt(head,&a,&b);//a and b are two halves
```
 3.对链表a与b分别排序
```C
    MergeSort(a);MergeSort(b);    
```
 4.合并a与b 
```C
MergeSortedLists()
```

```C
/*Link List node*/
struct node
{
    int data;
    struct node* next;
};
/*函数定义声明*/
struct node* MergeSortedLists(struct node *a,struct node *b);
void SpiltIntoHalves(struct node* source,struct node** frontRef,struct node** backRef);

/*sorts the linked list by changing next pointers(not data)*/
void MergeSort(struct node** headRef)
{
    struct node* head = *headRef;
    struct node* a;
    struct node* b;
    
    /*Base case -- length 0 or 1*/
    if((head==NULL)||(head->next==NULL))
    {
        return;
    }
    
    /*Spilt head into 'a' and 'b' sublists(halves)*/
    SpiltIntoHalves(head,&a,&b);
    
    /*Recursively sort the sublists*/
    MergeSort(&a);
    MergeSort(&b);
    
    /*answer = merge the two sorted lists together*/
    *headRef = MergeSortedLists(a,b);
}// end of MergeSort()

//递归程序：对两链表进行递归合并
struct node* MergeSortedLists(struct node* a,struct node* b)
{
    struct node* result = NULL;
    /*Base cases*/
    if(a==NULL)
        return (b);
    else if(b==NULL)
        return (a);
    
    /*按升序排列（递归）*/
    if(a->data <= b->data)
    {
        result = a;
        result->next = MergeSortedLists(a->next,b);
    }
    else
    {
        result = b;
        result->next = MergeSortedLists(a,b->next);
    }
    return (result);
}// end of MergeSortedLists()

//遍历链表，并一分为二
//用两个步幅相差一步的指针，循环中当快指针到链尾时，慢指针正好到链表中央
//通过reference返回快慢两指针:frontRef 与 backRef
void SpiltIntoHalves(struct node* source,struct node** frontRef,struct node** backRef)
{
    struct node* fast;//快指针
    struct node* slow;//慢指针
    
    if(source==NULL||source->next==NULL)
    {
        /*length<2 cases*/
        *frontRef = source;
        *backRef = NULL;
    }
    else
    {
        slow = source;
        fast = source->next;
        
        /* Advance 'fast' two nodes, and advance 'slow' one node*/
        while(fast!=NULL)
        {
            fast = fast->next;
            if(fast!=NULL)
            {
                slow = slow->next;
                fast = fast->next;
            }
        }
        
        /*'slow' is before the midpoint in the list,so spilt it in two at that point.*/
        *frontRef = source;
        *backRef = slow->next;
        slow->next = NULL;
    }
}// end of SpiltIntoHalves
```

Q：如果有1TB的数据需要排序，但只有32GB的内存，请描述你将使用什么样的排序算法处理？
A：分而治之，混合多种算法：
 - 将磁盘上的1TB数据分割为40块（chunks），每份25GB（**要留一些系统空间**）
 - 顺序将每份25GB数据读入内存，使用quick sort算法排序
 - 把排序好的数据（25GB）存放回磁盘
 - 循环40次，现在，所有的40个块都已经各自排序了（剩下的工作就是把它们合并排序）
 - 从40个块中分别读取25GB/40=0.625G 存入内存（40 input buffers）
 - 执行40路合并，并将合并结果临时存储于2GB基于内存的输出缓冲区中。当缓冲区写满2GB时，写入硬盘上最终文件，并清空输出缓冲区；当40个输入缓冲区中任何一个处理完毕时，写入该缓冲区所对应的块中的下一个0.625GB，直到全部处理完成。
 
**优化**：磁盘I/O通常是越少越好（最好完全没有）。40路输入缓冲区，可以先做8路merge sort，把每8个块合并为1路，然后再做5-to-1的合并操作。
**如果有多余的硬件，如何继续优化**：
 
 - 使用并发：如多磁盘（并发I/O提高），多线程，使用异步I/O，使用多台主机集群计算。
 - 提升硬件性能：如更大内存，更高RPM的磁盘，升级为SSD，Flash，使用更多核的CPU。
 - 提高软件性能：比如采用radix sort，压缩文件（提高I/O效率）等。