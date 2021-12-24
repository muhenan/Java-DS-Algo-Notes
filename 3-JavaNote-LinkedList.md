[toc]

# LinkedList 链表

通过 LinkedNode 连起来

```java
public class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
```

无法高效获取长度，无法根据偏移快速访问元素，是链表的两个劣势。然而面试的时候经常碰见诸如获取倒数第k个元素，获取中间位置的元素，判断链表是否存在环，判断环的长度等和长度与位置有关的问题。这些问题都可以通过灵活运用双指针来解决。

**Tips：双指针并不是固定的公式，而是一种思维方式~**

链表是空节点，或者有一个值和一个指向下一个链表的指针，因此很多链表问题可以用**递归**来处理。

递归的一个重要思想就是：**相信函数**。



## 160. Intersection of Two Linked Lists (Easy) 链表交点

链表经典题，链表交点！

例如以下示例中 A 和 B 两个链表相交于 c1：

```
A:          a1 → a2
                    ↘
                      c1 → c2 → c3
                    ↗
B:    b1 → b2 → b3
```

要求时间复杂度为 O(N)，空间复杂度为 O(1)。如果不存在交点则返回 null。

设 **A** 的长度为 **a + c**，**B** 的长度为 **b + c**，其中 c 为尾部公共部分长度，可知 **a + c + b = b + c + a**。

**数学思想！**

当访问 A 链表的指针访问到链表尾部时，令它从链表 B 的头部开始访问链表 B；同样地，当访问 B 链表的指针访问到链表尾部时，令它从链表 A 的头部开始访问链表 A。这样就能控制访问 A 和 B 两个链表的指针能同时访问到交点。

如果不存在交点，那么 a + b = b + a，以下实现代码中 l1 和 l2 会同时为 null，从而退出循环。

也就是说当不想交时，我们也认为是相交于 null

```
A:          a1 → a2
                    ↘
                      null
                    ↗
B:    b1 → b2 → b3
```

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode l1 = headA, l2 = headB;
    while (l1 != l2) {
        l1 = (l1 == null) ? headB : l1.next;
        l2 = (l2 == null) ? headA : l2.next;
    }
    return l1;
}
```

如果是是否相交，就是另外一个题了，有两种解法：

- 把第一个链表的结尾连接到第二个链表的开头，看第二个链表是否存在环；
- 或者直接比较两个链表的最后一个节点是否相同。



## 206. Reverse Linked List (Easy) 翻转

**方法一：递归，递归思想在链表相关的东西中至关重要！！！**

链表的东西，可以多想想递归，因为链表这个东西本身就是递归的。

递归的思想就是，我只做一点，把剩下的东西交给函数再去做。

```java
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode next = head.next;
        ListNode newHead = reverseList(head.next);
        next.next = head;
        head.next = null;
        return newHead;
    }
```

这里注意 next 和 newHead 那些东西

**方法二：迭代，原地翻转**

```java
    public ListNode reverseList2(ListNode head) {
        ListNode pre = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
```

这里注意是让 head 走到了最后，最后返回的有效的是 pre 指针

这里也是用到了**双指针**的思想，这里就两个指针，next 就是个纪录



## 21. Merge Two Sorted Lists (Easy) 递归和迭代（循环）两种！

最简单的，直接递归，相信函数。

```java
// recursion Time: O(m+n) Space: O(m+n)
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    if (list1 == null) return list2;
    if (list2 == null) return list1;
    if (list1.val < list2.val) {
        list1.next = mergeTwoLists(list1.next, list2);
        return list1;
    } else {
        list2.next = mergeTwoLists(list2.next, list1);
        return list2;
    }
}
```

**时间复杂度 O(m+n) 空间复杂度 O(m+n)**

递归的思路虽然非常简单，但是函数调用栈会占用额外的内存空间，递归调用 mergeTwoLists 函数时需要消耗栈空间，栈空间的大小取决于递归调用的深度。结束递归调用时 mergeTwoLists 函数最多调用 n+m 次，因此空间复杂度为 O(n+m)

```java
// iteration Time: O(m+n) Space: O(1)
    public ListNode mergeTwoLists2(ListNode list1, ListNode list2) {
        ListNode head = new ListNode(-1, null);
        ListNode pre = head;
        while (!(list1 == null && list2 == null)) {
            if (list1 == null) {
                pre.next = list2;
                return head.next;
            }
            if (list2 == null) {
                pre.next = list1;
                return head.next;
            }
            if (list1.val < list2.val) {
                pre.next = list1;
                list1 = list1.next;
            } else {
                pre.next = list2;
                list2 = list2.next;
            }
            pre = pre.next;
        }
        return head.next;
    }
```

**时间复杂度 O(m+n) 空间复杂度 O(1)**

迭代其实也是非常非常简单的思想，最简单的思想就是迭代（循环）的思想，但实际实现的时候要注意一些细节：

* head 弄一个最头部的哑节点，这样最头部这里就不用伤脑筋了，直接用实在的数据结构解决了

* pre 记录前一个
* 每一个怎么往下走
* （写代码小技巧：可以先不考虑特殊情况，先按最理想的情况写，然后之后再修修补补考虑特殊情况）



## 83. Remove Duplicates from Sorted List (Easy) 递归，迭代

**递归**

```java
    // recursion
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) return head;
        head.next = deleteDuplicates(head.next);
        if (head.val == head.next.val) {
            return head.next;
        }
        else {
            return head;
        }
    }
```

相信函数即可，从第二个开始弄，弄完了以后和第一个比一下，看看怎么样，重的话删掉一个。

时空都是 O(n)

**迭代**

```java
    // iteration 2
    public ListNode deleteDuplicates3(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode left = head;
        ListNode right = head.next;
        while (right != null) {
            if (left.val != right.val) {
                left = left.next;
                right = right.next;
            } else {
                right = right.next;
                left.next = right;
            }
        }
        return head;
    }
```

这里的迭代，具体操作就是**双指针**，左右指针，往下走，特殊处理重复的！



## 19. Remove Nth Node From End of List (Medium) 双指针 快慢指针

倒数第几个，标准哑节点，快慢指针。

1. **哑结点**的巧妙应用
   * 链表删除添加等问题，在操作第一个或最后一个时
   * 常出现**找不到前一个**或者**找不到后一个**的问题
   * 这个添加哑结点，就是保证了一定会有前一个
   * 所以这里就要利用哑结点提供的这个**一致条件**（都有前一个） 
   * 找要删除的结点的前一个 

2. 一遍遍历，双指针，快慢指针
   * 通过**快慢指针**的距离找到要删除的结点的前一个
     * 让两个指针保持一个距离，没错，刚好是 k 距离
   * 然后正常删除
   * 返回哑结点的next

```java
// Two pointers
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(-1, head);
        ListNode left = dummy;
        ListNode right = dummy;
        while (n-- > 0) {
            right = right.next;
        }
        while (right.next != null) {
            left = left.next;
            right = right.next;
        }
        left.next = left.next.next;
        return dummy.next;
    }
```

