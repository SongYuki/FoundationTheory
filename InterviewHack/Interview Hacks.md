# Interview Hacks

---

要想了解从学校过渡到工业界的跨度，不妨来看个简单的例子：
使用递归的代码看起来非常简短（虽然逻辑解读上可能更复杂），而且应用的地方很多，比如著名的链表反转(reversing a link-list).但是，递归会占用更多的内存（context saving），其效率也相对更差（调用前的内存准备，占用和调用后的内存释放），在面对长链表的情况下堆栈溢出也是潜在问题。在业界的真实应用中，单纯的递归几乎是不存在的，取而代之的是循环（不占用额外的存储，堆栈溢出的危险有效降低etc）或递归嵌套循环（时刻监视内存的使用及是否有内存泄漏等风险）

chapter 1 data structure
解决问题的不二法门是数据结构+算法，好的数据结构再配上高效的算法几乎可以无往而不利。
1.1 链表
Q：如何翻转一个单向链表（头变尾，尾变头）
```C
    //Recursively递归算法,效率不高，每一次调用自己都产生大量额外内存占用和清除。对于长链表，发生堆栈溢出是绝对有可能的，更优质的解决方案是采用非递归
    reverse_ll(struct node ** hashref){
        struct node *first,last;
        if(*hashref==NULL)return -1;//boundry condition
        first = *hashref;
        rest = first->next;
        if(rest==NULL)return;
        
        reverse_ll(&rest);
        
        first->next->next=first;//reversion happens.反转
        first->next=NULL;
    }//-END-
```

```C
    //Non-Recursively 使用一个相当简单的循环，只用一个临时指针在循环中调整指针的指向，将头变尾，尾变头。在工业界，大多数面试官期待的是这种基于循环的答案
    Node *p=head;//h points to head of the linklist.指向链表头
    if(p==NULL)
        return NULL;
    link *h;
    h=p;
    if(p->next)
        p=p->next;
    h-next=NULL;//h is now an isolated node which will be the tail node eventually.h现在是孤立结点，最终会成为尾结点
    while(p!=NULL){
        link *t=p->next;//tmp node.
        p->next=h;//reverse the linkpage
        h=p;//moving one node forward
        p=t;//moving forward
    }
    return h;//h points to the new Head.h指向新表头
    //-END-
```
Q:为什么递归有风险？
A:有可能出现堆栈溢出，特别是对于大数据集或过度使用嵌套递归的情况。

Q:如何反向打印一个链表（从尾到头）

```C
    //Recursively
    rev_ll(Node *h){
        if(!h)
            return -1;
        else{
            rev_ll(h->next);
            print(h->data);
            }
    }
```

