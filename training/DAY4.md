# day4

## 第四天| 链表 24 19 160 142

### 24 两两交换链表中的节点 middle
```
给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。
```
https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html

思路：使用虚拟头节点作为当前节点cur，步骤一是cur指向第二个节点，步骤二是第二个节点指向第一个节点，步骤三是第一个节点指向第三个节点，然后将cur指向第一个节点也是事实此时的第二个节点进行下一次迭代，直到下一个节点、下下一个节点都不为空，因为如果cur后面连两个节点都不够，那就没必要交换了

复杂度：时间O(N) 空间O(1)
```java
//第一种：虚拟头节点，迭代，比较容易想到，建议用这个
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dumyhead = new ListNode(-1); // 设置一个虚拟头结点
        dumyhead.next = head; // 将虚拟头结点指向head，这样方便后面做删除操作
        ListNode cur = dumyhead;
        ListNode temp; // 临时节点，保存两个节点后面的节点
        ListNode firstnode; // 临时节点，保存两个节点之中的第一个节点
        ListNode secondnode; // 临时节点，保存两个节点之中的第二个节点
        while (cur.next != null && cur.next.next != null) {
            temp = cur.next.next.next;
            firstnode = cur.next;
            secondnode = cur.next.next;
            cur.next = secondnode; // 步骤一
            secondnode.next = firstnode; // 步骤二
            firstnode.next = temp; // 步骤三
            cur = firstnode; // cur移动，准备下一轮交换
        }
        return dumyhead.next;
    }
}
//第二种：递归
class Solution {
    public ListNode swapPairs(ListNode head) {
        // base case 退出提交
        if (head == null || head.next == null)
            return head;
        // 获取当前节点的下一个节点
        ListNode next = head.next;
        // 进行递归
        ListNode newNode = swapPairs(next.next);
        // 这里进行交换
        next.next = head;
        head.next = newNode;

        return next;
    }
}
```

### 19 删除链表的倒数第N个节点 middle
```
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。
```
https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html

思路：用虚拟头节点，快慢指针，快指针先跑n+1次，此时快指针指向正数第n个节点，然后快慢指针一起跑直至快指针指向空，此时慢指针指向待删除元素的前一个节点

复杂度：时间O(N) 空间O(1)
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummyHead = new ListNode(-1), slow, fast;
        dummyHead.next = head;
        slow = dummyHead;
        fast = dummyHead;
        while (n-- >= 0) {
            fast = fast.next;
        }
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return dummyHead.next;
    }
}
```

### 160 相交链表 easy
```
给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。
```
https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html

思路：使用HashSet，先把第一个链表的元素都放到set，再遍历第二个链表从set中查找

复杂度：时间O(N) 空间O(N)
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Set<ListNode> set = new HashSet<>();
        ListNode curA = headA, curB = headB;
        while (curA != null) {
            set.add(curA);
            curA = curA.next;
        }
        while (curB != null) {
            if (set.contains(curB)) return curB;
            curB = curB.next;
        }
        return null;
    }
}
```

### 142 环形链表II middle
```
给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。
```
https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html

思路：快慢指针，快指针一次跑两个节点，慢指针一次跑一个节点。如果 fast 和 slow 指针在途中相遇 ，说明这个链表有环，然后头节点和相遇节点每次各走一步，直到相遇，相遇点即为环入口

复杂度：时间O(N) 空间O(1)
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {// 有环
                ListNode index1 = fast;
                ListNode index2 = head;
                // 两个指针，从头结点和相遇结点，各走一步，直到相遇，相遇点即为环入口
                while (index1 != index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            }
        }
        return null;
    }
}
```