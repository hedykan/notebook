# 链表反转

## 问题说明
给定单链表的头节点`head`，请反转链表，并返回反转后的链表的头节点。  
```c
// 给定链表节点
struct ListNode {
    int val;
    struct ListNode *next;
};
```

## 问题解析
对于一个序列来说，想要把这个序列反转，最简单的做法就是从尾部开始倒序遍历，然后保存为生成一个新序列，这是在普通序列中最常见也是最容易想到的办法之一。  
但是这里问题给出的是链表，链表有个特点，想要知道这个链表的位置，必须得先从前面的链表获取，那么我们就没有直接从链表位开始往前遍历的方法。  
不过，其实也是有办法的，正向遍历链表的方法其实很简单，就是通过递归，不断的进入链表的next中。  
```c
void traverse(struct ListNode *node) {
    if(node->next == NULL) {
        return;
    }
    // 你可以在这做点什么1
    traverse(node->next);
    // 你可以在这做点什么2
    return;
}
```
我们仔细观察以下，除了固定的遍历操作，在递归结束条件和递归进入点中有两个空位可以给我们操作操作，整点花活。  
仔细分析下第一个空位，如果你在这里做了点什么，会发生什么呢：  
```
traverse(work1, traverse(work2, traverse(work3, traverse(...))))
```
还挺好看，有种数学归纳法的美感，就那个`f(f(f(f(...))))`，其实数学上两者也挺接近的，不过现在的我数学造诣还不够深，就暂时不进行叙述。  
回到上面，遍历的过程其实是没有操作的，只把`work`看作操作的话，上面的操作顺序是`work1->work2->work3->...`，其实就是按照链表的正向顺序进行操作，很容易理解，因为链表的进入顺序就是从第一个开始往后面进入，在进入之前操作，就是顺序操作。  
现在我们来看看第二个位置： 
```
traverse(traverse(traverse(traverse(...), work3), work2), work1)
```
从这里来看操作顺序是`...->work3->work2->work1`，嗯？怎么反过来了，这其实是利用递归回归的特性，在回归前进行操作，最早开始回归的一定是最晚遍历到的那一层，所以，从最晚一层开始进行操作，线性来看就是倒序操作。
哦，这不就是之前序列操作所需的倒序遍历吗，利用这个特性就可以做到和序列相同的事情了。  

## c语言实现
```c
struct ListNode* reverseList(struct ListNode* head){
    // 空链表和尾部直接返回
    if(head == NULL || head->next == NULL) {
        return head;
    }
    // 进入next节点，找到最后节点
    struct ListNode *last = reverseList(head->next);
    // 后续操作，头尾交换位置
    head->next->next = head;
    head->next = NULL;
    return last;
}
```