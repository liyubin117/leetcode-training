# day4

## 24 两两交换链表中的节点 middle
思路：递归，抽象成三个节点，head nextNode newNode，三节点的交换是非常简单的
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode nextNode = head.next;
        ListNode newNode = swapPairs(nextNode.next);
        nextNode.next = head;
        head.next = newNode;
        return nextNode;
    }
}
```

## 19 删除链表的倒数第N个节点 middle
思路：快慢指针
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummyHead = new ListNode(), fast = dummyHead, slow = dummyHead;
        dummyHead.next = head;
        while (fast != null && n-- >= 0) {
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

## 160 相交链表 easy
思路：哈希表
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Set<ListNode> hash = new HashSet<>();
        ListNode curA = headA, curB = headB;
        while (curA != null) {
            hash.add(curA);
            curA = curA.next;
        }
        while (curB != null) {
            if (hash.contains(curB)) return curB;
            curB = curB.next;
        }
        return null;
    }
}
```

## 142 环形链表2 middle
思路：快指针一次跑两下，慢指针跑一下，相遇后慢指针回到head，然后同时一次跑一下，相遇即为环入口
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head, slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                slow = head;
                while (fast != slow) {
                    fast = fast.next;
                    slow = slow.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```