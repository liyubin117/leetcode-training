# day3

## 第三天|链表 203 707 206

### 203 移除链表元素  

https://programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html

思路：用虚拟头节点，方便判断下一个节点

复杂度：时间O(N) 空间O(1)
```java
public ListNode removeElements(ListNode head, int val) {
    ListNode dummyHead = new ListNode(-1), cur = dummyHead;
    dummyHead.next = head;
    while (cur != null && cur.next != null) {
        if (cur.next.val == val) cur.next = cur.next.next;
        else cur = cur.next;
    }
    return dummyHead.next;
}
```

### 707 设计链表

https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html

思路：用虚拟头节点

```java
//单链表
class ListNode {
    int val;
    ListNode next;
    ListNode(){}
    ListNode(int val) {
        this.val=val;
    }
}
class MyLinkedList {
    //size存储链表元素的个数
    int size;
    //虚拟头结点
    ListNode head;

    //初始化链表
    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }

    //获取第index个节点的数值，注意index是从0开始的，第0个节点就是头结点
    public int get(int index) {
        //如果index非法，返回-1
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode currentNode = head;
        //包含一个虚拟头节点，所以查找第 index+1 个节点
        for (int i = 0; i <= index; i++) {
            currentNode = currentNode.next;
        }
        return currentNode.val;
    }

    //在链表最前面插入一个节点，等价于在第0个元素前添加
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    //在链表的最后插入一个节点，等价于在(末尾+1)个元素前添加
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    // 在第 index 个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果 index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果 index 大于链表的长度，则返回空
    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        if (index < 0) {
            index = 0;
        }
        size++;
        //找到要插入节点的前驱
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        ListNode toAdd = new ListNode(val);
        toAdd.next = pred.next;
        pred.next = toAdd;
    }

    //删除第index个节点
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        size--;
        if (index == 0) {
            head = head.next;
	    return;
        }
        ListNode pred = head;
        for (int i = 0; i < index ; i++) {
            pred = pred.next;
        }
        pred.next = pred.next.next;
    }
}

//双链表
class ListNode{
    int val;
    ListNode next,prev;
    ListNode() {};
    ListNode(int val){
        this.val = val;
    }
}


class MyLinkedList {  

    //记录链表中元素的数量
    int size;
    //记录链表的虚拟头结点和尾结点
    ListNode head,tail;
    
    public MyLinkedList() {
        //初始化操作
        this.size = 0;
        this.head = new ListNode(0);
        this.tail = new ListNode(0);
        //这一步非常关键，否则在加入头结点的操作中会出现null.next的错误！！！
        head.next=tail;
        tail.prev=head;
    }
    
    public int get(int index) {
        //判断index是否有效
        if(index<0 || index>=size){
            return -1;
        }
        ListNode cur = this.head;
        //判断是哪一边遍历时间更短
        if(index >= size / 2){
            //tail开始
            cur = tail;
            for(int i=0; i< size-index; i++){
                cur = cur.prev;
            }
        }else{
            for(int i=0; i<= index; i++){
                cur = cur.next; 
            }
        }
        return cur.val;
    }
    
    public void addAtHead(int val) {
        //等价于在第0个元素前添加
        addAtIndex(0,val);
    }
    
    public void addAtTail(int val) {
        //等价于在最后一个元素(null)前添加
        addAtIndex(size,val);
    }
    
    public void addAtIndex(int index, int val) {
        //index大于链表长度
        if(index>size){
            return;
        }
        //index小于0
        if(index<0){
            index = 0;
        }
        size++;
        //找到前驱
        ListNode pre = this.head;
        for(int i=0; i<index; i++){
            pre = pre.next;
        }
        //新建结点
        ListNode newNode = new ListNode(val);
        newNode.next = pre.next;
        pre.next.prev = newNode;
        newNode.prev = pre;
        pre.next = newNode;
        
    }
    
    public void deleteAtIndex(int index) {
        //判断索引是否有效
        if(index<0 || index>=size){
            return;
        }
        //删除操作
        size--;
        ListNode pre = this.head;
        for(int i=0; i<index; i++){
            pre = pre.next;
        }
        pre.next.next.prev = pre;
        pre.next = pre.next.next;
    }
}
```

### 206 反转链表

https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html 

思路：双指针分别记录上一个节点和当前节点

复杂度：时间O(N) 空间O(1)
```java
public ListNode reverseList(ListNode head) {
    ListNode pre = null, cur = head;
    while (cur != null) {
        ListNode tmp = cur.next;
        cur.next = pre;
        pre = cur;
        cur = tmp;
    }
    return pre;
}
```