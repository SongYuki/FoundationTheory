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

Q:如何反转一个字符串(string)(要求复杂度为O(N/2))
A:以字符串数组为例，将第一与倒数第一置换，第二与倒数第二置换，一直到自己（subscript=脚注，下标）与自己相等则停止置换


Q：如何翻转双向链表（双向链表翻转意义何在呢）
A：直接用循环，期间只需要做两件事情，头尾置换，向前指针和向后指针的互换

```C
Node *PCurrent = pHead,*pTemp;
while(pCurrent){
    pTemp=pCurrent->next;
    pCurrent->next=pCurrent->prev;
    pCurrent->prev=pTemp;
    pHead=pCurrent;
    pCurrent=pTemp;
}
//pHead will point to the newly reversed head by now.
//-END-
```

Q：在一个升序排列的双向链表中，如何插入一个节点
A：考察的是对边界条件的检查，这是一个优秀的程序员必备的能力。先想象一下可能在哪些地方插入一个节点：头部，中间，尾部。
```C
//assume we have a struct
Struct Node{
    Int data;
    Node *prev;
    Node *next;
};

//pHead,pCur,nn(new node),ppCur(pre-pCur)
if(pHead==NULL)return(0);
pCur=pHead;
while(pCur){
    if(pCur!=pHead){
        ppCur=pCur->prev;//keep track of prev node.
    }
    if(pCur->data >= nn->data){
        if(pCur==pHead){
            //insert at the head
            pHead = nn;
            nn->prev = NULL;
            nn->next = pCur;
            pCur->prev = nn;
        }else{//insert at non head.
            if(pCur->next==NULL){//insert at the tail.
                nn->next=NULL;
                pCur->next=nn;
                nn->prev=pCur;
            }else{//insert somewhere in the middle.
                nn->next = pCur;
                pCur->prev=nn;
                ppCur->next=nn;
                nn->prev=ppCur;
            }
            return(pHead);//return head of double-linklist.
        }
    }else{//keep going!
        pCur=pCur->next;
    }
}//end of while()
```


Q：在双向链表中，如何删除一个节点
A：Need to consider all boundary conditions
```C
void del(node *n){
    if(!n)return;
    if(n->prev){//Treat one direction
        n->prev->next=n->next;
    }else{
        n->next->prev=NULL;//head deletion
        head = n->next;//reassign list head
    }
    
    if(n->next){//Treat the other direction
        n->next->prev=n->prev;
    }else{
        n->prev->next=NULL;//delete at tail
    }
    delete(n);
}//END of del()
```

Q：在单链表中如何发现loop（环）
A：发现是否有环的基本算法就是从表头出发，两个指针在循环中以不同的步幅前进，其中p1步幅为1，p2步幅为2，如果在某次迭代中指向同一点，则说明出现环loop.
```C
Node *p1,*p2,*head;
p1=p2=head;
do{
    p1=p1->next;
    p2=p2->next->next;
}while(p1!=p2&&p1->next!=NULL&&p2->next!=NULL&&p2->next->next!=NULL)
    
if(p1->next==NULL||p2->next==NULL||p2->next->next==NULL){
    printf("None loop found!\n");
}else if(p1==p2&&p1!=NULL){
    printf("Loop found!\n");
}//-END-
//
```

