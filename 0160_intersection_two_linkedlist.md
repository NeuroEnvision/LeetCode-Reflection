【知识在输出中获得005】#160 寻找两个单链表的相交结点
[Question](https://leetcode.cn/problems/intersection-of-two-linked-lists/)
#####  思路
1. 最初想法：
	1. 暴力解法：`curA = headA, curB = headB` 两层while loops，遇到curA == curB即找到交点O(m * n) time 
	2. 哈希表：O(m) 时间遍历并将`listA`结点指针储存在哈希表。O(n) 时间遍历并在哈希表中查找`listB`结点指针. O(m + n) time BUT O(m) memory
2. 疑虑：
	1. 这道题不需删除结点，是否还要设立虚拟头结点？不需要
	2. 暴力解法空间ok但超时，哈希表时间ok但需额外空间。*无法实现O(m+n) time, O(1) memory因此转向[题解](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.md)*
3. 题解：关键：**尾端对齐** 两个链表若相交，则形成双头蛇，从相交结点开始共用身体。步骤如下：
	1. 遍历两表得到其各自长度
	2. 计算长度差值 d
	3. 设立两指针`curA = headA, curB = headB` 
	4. 将长度较长的链表指针先后移d步
	5. 将两指针同步后移，若`curA == curB`则找到相交结点 **相等的是指针，而非数值**
	6. 否则循环结束，不相交，返回`null`

##### code 
```cpp
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *curA = headA, *curB = headB;
        int m = 0, n = 0, d = 0;
		while (curA) {m++; curA = curA->next;}  // count listA length
        while (curB) {n++; curB = curB->next;}  // count listB length
        curA = headA, curB = headB;
        if (m >= n) {                           // align lists at the end
            d = m - n;
            while (d--) {curA = curA->next;}
        } else {
            d = n - m;
            while (d--) {curB = curB->next;}
        }
        while (curA){                           // look for intersected node
            if (curA == curB) return curA;
            curA = curA->next;
            curB = curB->next;
        }
        return NULL;                            // not intersected
    }
```
