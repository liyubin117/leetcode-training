# 数组
## 27 移除元素 easy
快慢指针，快指针指向要保留的元素值，慢指针指向删除元素后的数组的索引。快指针是读指针，慢指针是写指针
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        //双指针
        int fast = 0, slow = 0;
        while (fast < nums.length) {
            if (nums[fast] != val) {
                nums[slow++] = nums[fast];
            }
            fast++;
        }
        return slow;
    }
}
```
## 283 移动零 easy
```
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
请注意 ，必须在不复制数组的情况下原地对数组进行操作。
```
思路：快慢指针，慢指针指向当前已经处理好的序列的尾部，快指针指向待处理序列的头部。快指针不断向右移动，每次快指针指向非零数，则将快慢指针对应的数交换，同时慢指针右移。
- 慢指针左边均为非零数
- 快指针左边直到慢指针处均为零
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int fast = 0, slow = 0, temp;
        while (fast < nums.length) {
            if (nums[fast] != 0) {
                temp = nums[fast];
                nums[fast] = nums[slow];
                nums[slow] = temp;
                slow++;
            }
            fast++;
        }
    }
}
```
## 977 有序数组的平方 easy 
双指针，选较大的那个值逆序放到结果集并移动相应的指针
```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] result = new int[nums.length];
        int k = nums.length - 1;
        for (int l = 0, r = nums.length - 1; l <= r;) {
            int a = nums[l] * nums[l], b = nums[r] * nums[r];
            if (a > b) {
                result[k] = a;
                l++;
            }
            else {
                result[k] = b;
                r--;
            }
            k--;
        }
        return result;
    }
}
```
## 35 搜索插入位置 easy
```
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
请必须使用时间复杂度为 O(log n) 的算法。
```
思路：二分查找。不断用二分法逼近查找第一个大于等于 target 的下标，能找到则是mid，找不到则是二分查找后最终区间的左界
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int l = 0, r = nums.length - 1, mid = 0;
        while (l <= r) {
            mid = l + (r - l) / 2;
            if (nums[mid] < target) l = mid + 1;
            else if (nums[mid] > target) r = mid - 1;
            else return mid;
        }
        return l;
    }
}
```
## 15 三数之和且不可出现重复三元组 middle 
先排序，固定一个点i后双指针left/right，再移动这个点，要注意去重
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) break; // 排序之后如果第一个元素已经大于零，那么无论如何组合都不可能凑成三元组，直接返回结果就可以了
            if (i > 0 && nums[i] == nums[i - 1]) continue; //去重a
            int left = i + 1, right = nums.length - 1, tmp;
            while (left < right) {
                tmp = nums[i] + nums[left] + nums[right];
                if (tmp < 0) left++;
                else if (tmp > 0) right--;
                else {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while (left < right && nums[left] == nums[left + 1]) left++; //去重b
                    while (left < right && nums[right] == nums[right - 1]) right--; //去重c
                    left++;
                    right--;
                }
            }
        }
        return result;
    }
}
```
## 18 四数之和 middle 
在三数之和基础上多了一层for循环，注意有一级剪枝/去重，二级剪枝/去重，且不能过早return结果，剪枝后break即可
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int k = 0; k < nums.length; k++) {
            if (nums[k] >= 0 && nums[k] > target) break;
            if (k > 0 && nums[k] == nums[k - 1]) continue;
            for (int i = k + 1; i < nums.length; i++) {
                if (nums[k] + nums[i] >= 0 && nums[k] + nums[i] > target) break;
                if (i > k + 1 && nums[i] == nums[i - 1]) continue;
                int left = i + 1, right = nums.length - 1;
                while (left < right) {
                    long sum = (long) nums[k] + nums[i] + nums[left] + nums[right];
                    if (sum > target) right--;
                    else if (sum < target) left++;
                    else {
                        result.add(Arrays.asList(nums[k], nums[i], nums[left], nums[right]));
                        while (left < right && nums[left] == nums[left + 1]) left++;
                        while (left < right && nums[right] == nums[right - 1]) right--;
                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
}
```
## 560 和为k的子数组 middle
```
给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的子数组的个数 。
子数组是数组中元素的连续非空序列。
```
思路：
- 暴力枚举，指定start后遍历end，再迭代start
- 前缀和+哈希表，通过遍历数组，计算每个位置的前缀和，并使用一个哈希表来存储每个前缀和出现的次数。对于任意的两个下标i和j（i < j），如果prefixSum[j] - prefixSum[i] = k，即从第i个位置到第j个位置的元素之和等于k，那么说明从第i+1个位置到第j个位置的连续子数组的和为k，也就是若此前获取的前缀和中包含prefix[j]-k，那就说明以j为结尾的子数组有对应键值的次数符合条件
```java
//暴力枚举 O(N^2)
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        for (int start = 0; start < nums.length; ++start) {
            int sum = 0;
            for (int end = start; end < nums.length; ++end) {
                sum += nums[end];
                if (sum == k) {
                    count++;
                }
            }
        }
        return count;
    }
}
//前缀和+哈希表，O(N) pre[i]=pre[i−1]+nums[i]
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0, pre = 0;
        HashMap < Integer, Integer > hash = new HashMap < > ();
        hash.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            pre += nums[i];
            if (hash.containsKey(pre - k)) {
                count += hash.get(pre - k);
            }
            hash.put(pre, hash.getOrDefault(pre, 0) + 1);
        }
        return count;
    }
}
```
## 56 合并区间 middle
```
以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。
```
思路：将列表中的区间按照左端点升序排序。然后将第一个区间加入 merged 数组中，并按顺序依次考虑之后的每个区间：
- 如果当前区间的左端点在数组 merged 中最后一个区间的右端点之后，那么它们不会重合，我们可以直接将这个区间加入数组 merged 的末尾；
- 否则，它们重合，我们需要用当前区间的右端点更新数组 merged 中最后一个区间的右端点，将其置为二者的较大值。
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length == 0) {
            return new int[0][2];
        }
        Arrays.sort(intervals, new Comparator<int[]>() {
            public int compare(int[] interval1, int[] interval2) {
                return interval1[0] - interval2[0];
            }
        });
        List<int[]> merged = new ArrayList<int[]>();
        for (int i = 0; i < intervals.length; ++i) {
            int L = intervals[i][0], R = intervals[i][1];
            if (merged.size() == 0 || merged.get(merged.size() - 1)[1] < L) {
                merged.add(new int[]{L, R});
            } else {
                merged.get(merged.size() - 1)[1] = Math.max(merged.get(merged.size() - 1)[1], R);
            }
        }
        return merged.toArray(new int[merged.size()][]);
    }
}
```
## 11 盛最多水的容器 middle
```
给定一个长度为 n 的整数数组 height 。有 n 条垂线，第 i 条线的两个端点是 (i, 0) 和 (i, height[i]) 。
找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
返回容器可以储存的最大水量。
说明：你不能倾斜容器。
```
思路：双指针

在每个状态下，无论长板或短板向中间收窄一格，都会导致水槽 底边宽度 −1 变短：
- 若向内 移动短板 ，水槽的短板 min(h[i],h[j]) 可能变大，因此下个水槽的面积 可能增大 。
- 若向内 移动长板 ，水槽的短板 min(h[i],h[j]) 不变或变小，因此下个水槽的面积 一定变小 。

因此，初始化双指针分列水槽左右两端，循环每轮将短板向内移动一格，并更新面积最大值，直到两指针相遇时跳出；即可获得最大面积。
```java
class Solution {
    public int maxArea(int[] height) {
        int l = 0, r = height.length - 1, result = 0;
        while (l < r) {
            result = Math.max(result, (r - l) * (height[l] < height[r] ? height[l++] : height[r--]));
        }
        return result;
    }
}
```
## 42 接雨水 hard
```
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
```
思路：双指针，分别指向左右两端，且需要两个值记录左、右边的最大高度，最大高度低的那边的指针可以决定能接多少水，接的量是最大高度-当前列的高度
```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int res = 0;
        // 左右指针：分别指向左右两边界的列
        int left = 0, right = n - 1;
        // 左指针的左边（包括）最大高度、右指针的右边（包括）最大高度
        int leftMax = height[left], rightMax = height[right];
        // 最两边的列存不了水
        left++;
        right--;
        // 向中间靠拢
        while(left <= right){
            leftMax = Math.max(leftMax, height[left]);
            rightMax = Math.max(rightMax, height[right]);
            if(leftMax < rightMax){
                // 左指针的leftMax比右指针的rightMax矮，说明：左指针的右边至少有一个板子 > 左指针左边所有板子。根据水桶效应，保证了左指针当前列的水量决定权在左边
                // 那么可以计算左指针当前列的水量：左边最大高度-当前列高度
                res += leftMax - height[left];
                left++;
            }else{
                // 右边同理
                res += rightMax - height[right];
                right--;
            }
        }
        return res;
    }
}
```

# 矩阵
## 54 螺旋矩阵 middle
```
给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。
```
思路：边界是left right top bottom，i代表横边，j代表纵边，每次迭代完一边后判断是否越界
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        List<Integer> result = new ArrayList<>();
        int left = 0, right = n - 1, top = 0, bottom = m - 1;
        while (true) {
            for (int j = left; j <= right; j++) result.add(matrix[top][j]);
            if(++top > bottom) break;
            for (int i = top; i <= bottom; i++) result.add(matrix[i][right]);
            if(--right < left) break;
            for (int j = right; j >= left; j--) result.add(matrix[bottom][j]);
            if(--bottom < top) break;
            for (int i = bottom; i >= top; i--) result.add(matrix[i][left]);
            if(++left > right) break;
        }
        return result;
    }
}
```
## 59 螺旋矩阵2 middle
思路：边界是left right top bottom，i代表横边，j代表纵边，每次迭代完一边后判断是否越界
```java
class Solution {
    public int[][] generateMatrix(int n) {
        int left = 0, right = n - 1, top = 0, bottom = n - 1, count = 1;
        int[][] result = new int[n][n];
        while (true) {
            for (int j = left; j <= right; j++) result[top][j] = count++;
            if(++top > bottom) break;
            for (int i = top; i <= bottom; i++) result[i][right] = count++;
            if(--right < left) break;
            for (int j = right; j >= left; j--) result[bottom][j] = count++;
            if(--bottom < top) break;
            for (int i = bottom; i >= top; i--) result[i][left] = count++;
            if(++left > right) break;
        }
        return result;
    }
}
```
## 73 矩阵置零 middle
```
给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。
```
思路：用两个标记数组分别记录每一行和每一列是否有零出现，首先遍历该数组一次，如果某个元素为 0，那么就将该元素所在的行和列所对应标记数组的位置置为 true。最后我们再次遍历该数组，用标记数组更新原数组即可
```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean[] rows = new boolean[m], cols = new boolean[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    rows[i] = true;
                    cols[j] = true;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (rows[i] || cols[j]) matrix[i][j] = 0;
            }
        }
    }
}
```
## 48 旋转图像 middle
```
给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。
你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。
```
思路：对于矩阵中第 i 行的第 j 个元素，在旋转后，它出现在倒数第 i 列的第 j 个位置，即[j][rows-1-i]。用翻转操作代替旋转操作，先水平翻转，再对角线翻转
```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // 水平翻转
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < n; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = temp;
            }
        }
        // 主对角线翻转
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
    }
}
```
## 240 搜索二维矩阵2 middle
```
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：
每行的元素从左到右升序排列。
每列的元素从上到下升序排列。
```
思路：暴力查找、二分查找或Z形查找，容易理解且效率高的是二分查找
```java
//二分查找，为简单可直接调用Arrays.binarySearch
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        for (int[] row: matrix) {
            if(Arrays.binarySearch(row, target) >= 0) return true;
        }
        return false;
    }
}
//z形查找
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = matrix.length - 1;
        int line = 0;
        while (row >= 0 && line < matrix[0].length) {
            if (matrix[row][line] == target) return true;
            else if (matrix[row][line] > target) row--; // 得减下，向上检索                
            else line++;
        }
        return false;
    }
}
```

# 链表
## LCR 136 删除链表的节点 easy 
虚拟头节点next指针指向head节点，即可使用统一的方式遍历删除指定节点
```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        //虚拟头节点
        ListNode dummyHead = new ListNode(-1), cur = dummyHead, tmp;
        dummyHead.next = head;
        while (cur != null) {
            tmp = cur.next;
            if (tmp != null && tmp.val == val) {
                cur.next = tmp.next;
            }
            cur = cur.next;
        }
        return dummyHead.next;
    }
}
```
## 707 设计链表 middle 
用虚拟头节点
```java
//单链表
class ListNode {
    int val;
    ListNode next;
    public ListNode(int val) {
        this.val = val;
    }
}
class MyLinkedList {
    ListNode head; // 虚拟头结点
    int size; // 存储有效的链表元素的个数

    public MyLinkedList() {
        this.head = new ListNode(0);
        this.size = 0;
    }

    public int get(int index) {
        if (index < 0 || index >= size) return -1;
        ListNode cur = head;
        for (int i = 0; i <= index; i++) {
            cur = cur.next;
        }
        return cur.val;
    }

    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    public void addAtIndex(int index, int val) {
        if (index < 0 || index > size) return;
        size++;
        ListNode cur = head;
        for (int i = 0; i < index; i++) {
            cur = cur.next;
        }
        ListNode newNode = new ListNode(val);
        newNode.next = cur.next;
        cur.next = newNode;
    }

    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) return;
        size--;
        ListNode cur = head;
        for (int i = 0; i < index; i++) {
            cur = cur.next;
        }
        cur.next = cur.next.next;
    }
}
```
## 19 删除链表倒数第N个节点
快慢指针 构建dummyHead虚拟头节点，快慢指针都指向该dummyHead，先让快指针跑N个节点，慢指针再开始跑，直到快指针到null，此时慢指针指向的就是待删除节点的前一个节点，再让慢指针next指向next.next，再返回虚拟头节点next
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummyNode = new ListNode(0);
        dummyNode.next = head;

        ListNode fastIndex = dummyNode;
        ListNode slowIndex = dummyNode;

        // 只要快慢指针相差 n 个结点即可
        for (int i = 0; i <= n; i++) {
            fastIndex = fastIndex.next;
        }

        while (fastIndex != null) {
            fastIndex = fastIndex.next;
            slowIndex = slowIndex.next;
        }

        // 此时 slowIndex 的位置就是待删除元素的前一个位置。
        slowIndex.next = slowIndex.next.next;
        return dummyNode.next;
    }
}
```
## 141 环形链表 easy
思路：快慢指针
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (slow == fast) return true;
        }
        return false;
    }
}
```
## 142 环形链表2 middle
```
给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
```
思路：快指针两步，慢指针一步，相遇后说明有环，再置慢指针为头结点，快慢指针每次移动一步，相遇点即环的起始点
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {// 有环
               slow = head;
               while (slow != fast) {
                    slow = slow.next;
                    fast = fast.next;
               }
               return slow;
            }
        }
        return null;
    }
}
```
## 234 回文链表 easy
思路：用栈的方式获取倒序查链表的能力，然后迭代出栈按序与链表进行比较。还可使用递归，比较复杂
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        Stack<Integer> stack = new Stack<>();
        ListNode cur = head;
        while (cur != null) {
            stack.push(cur.val);
            cur = cur.next;
        }
        while (!stack.isEmpty()) {
            if (stack.pop() != head.val) return false;
            head = head.next;
        }
        return true;
    }
}
```
## 206 反转链表 easy
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode cur = head, pre = null;
        while (cur != null) {
            ListNode tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
    }
}
```
## 143 重排链表 middle 
使用线性表支持下标的特性，但这个的空间复杂度是O(N)，还可使用寻找链表中点 + 链表逆序 + 合并链表的方式，空间复杂度O(1)
```java
class Solution {
    public void reorderList(ListNode head) {
        List<ListNode> list = new ArrayList<>();
        ListNode cur = head;
        while (cur != null) {
            list.add(cur);
            cur = cur.next;
        }
        int l = 0, r = list.size() - 1;
        while (l < r) {
            list.get(l).next = list.get(r);
            l++;
            if (l == r) break;
            list.get(r).next = list.get(l);
            r--;
        }
        list.get(l).next = null; //此时将实际上的最后一个节点指向null，否则会cycle
    }
}
```
## 148 排序链表 middle
```
给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。
```
思路：
- 第一种：比较直观的想法是用一个list记录遍历后的值然后排序，再依次对链表每个节点赋值，时间效率低 O(N)
- 第二种：归并排序，用递归，时间效率高 O(N*LogN)
```java
//第一种
class Solution {
    public ListNode sortList(ListNode head) {
        List<Integer> list = new ArrayList<>();
        ListNode cur = head;
        while (cur != null) {
            list.add(cur.val);
            cur = cur.next;
        }
        Collections.sort(list);
        cur = head;
        int index = 0;
        while (cur != null) {
            cur.val = list.get(index++);
            cur = cur.next;
        }
        return head;
    }
}
//第二种
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null)
            return head;
        ListNode fast = head.next, slow = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode tmp = slow.next;
        slow.next = null;
        ListNode left = sortList(head);
        ListNode right = sortList(tmp);
        ListNode h = new ListNode(0);
        ListNode res = h;
        while (left != null && right != null) {
            if (left.val < right.val) {
                h.next = left;
                left = left.next;
            } else {
                h.next = right;
                right = right.next;
            }
            h = h.next;
        }
        h.next = left != null ? left : right;
        return res.next;
    }
}
```
## 24 两两交换链表中的节点 middle
思路：
- 迭代：使用虚拟头节点指向head，cur作为虚拟头节点，cur下个节点指向second，second下个节点指向first，first下个节点指向third（tmp），然后置cur为first即此时的第二个字节
- 递归：用 head 表示原始链表的头节点，新的链表的第二个节点，用 newHead 表示新的链表的头节点，原始链表的第二个节点，则原始链表中的其余节点的头节点是 newHead.next。令 head.next = swapPairs(newHead.next)，表示将其余节点进行两两交换，交换后的新的头节点为 head 的下一个节点。然后令 newHead.next = head，即完成了所有节点的交换。最后返回新的链表的头节点 newHead
```java
//迭代
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummyHead = new ListNode(-1), first, second, tmp, cur = dummyHead;
        dummyHead.next = head;
        while (cur.next != null && cur.next.next != null) {
            tmp = cur.next.next.next;
            first = cur.next;
            second = cur.next.next;
            
            cur.next = second;
            second.next = first;
            first.next = tmp;
            cur = first;
        }
        return dummyHead.next;
    }
}
//递归
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode newHead = head.next;
        head.next = swapPairs(newHead.next);
        newHead.next = head;
        return newHead;
    }
}
```
## 237 删除链表中的节点 middle 
由于只给定了要删除的节点node，不能使用遍历的方法，而是将node的值改为下一个节点，再将node下一个节点指向下下一个节点即可
```java
class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```
## 382 链表随机节点 middle
使用线性表
```java
class Solution {
    List<ListNode> list = new ArrayList<>();
    Random random = new Random();

    public Solution(ListNode head) {
        ListNode cur = head;
        while (cur != null) {
            list.add(cur);
            cur = cur.next;
        }
    }
    
    public int getRandom() {
        return list.get(random.nextInt(list.size())).val;
    }
}
```
## 160 相交链表 easy 
使用哈希集合寻找相交的起始节点
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
## 146 LRU缓存 middle
```
请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。
实现 LRUCache 类：
LRUCache(int capacity) 以 正整数 作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字 key 已经存在，则变更其数据值 value ；如果不存在，则向缓存中插入该组 key-value 。如果插入操作导致关键字数量超过 capacity ，则应该 逐出 最久未使用的关键字。
函数 get 和 put 必须以 O(1) 的平均时间复杂度运行。
```
应该用哈希表+双向链表的解法，更能体现考察的目标。get若有则先将节点移到头部再返回其值，put若无该元素则size+1，增加节点到头部，且若超出限制还删掉尾部节点，若有该元素则更新值并移到头部
```java
//哈希表+双向链表
public class LRUCache {
    class DLinkedNode {
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;
        public DLinkedNode() {}
        public DLinkedNode(int _key, int _value) {key = _key; value = _value;}
    }

    private Map<Integer, DLinkedNode> cache = new HashMap<Integer, DLinkedNode>();
    private int size;
    private int capacity;
    private DLinkedNode head, tail;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
        // 使用伪头部和伪尾部节点
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        moveToHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            // 如果 key 不存在，创建一个新的节点
            DLinkedNode newNode = new DLinkedNode(key, value);
            // 添加进哈希表
            cache.put(key, newNode);
            // 添加至双向链表的头部
            addToHead(newNode);
            ++size;
            if (size > capacity) {
                // 如果超出容量，删除双向链表的尾部节点
                DLinkedNode tailNode = tail.prev;
                removeNode(tailNode);
                // 删除哈希表中对应的项
                cache.remove(tailNode.key);
                --size;
            }
        }
        else {
            // 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            node.value = value;
            moveToHead(node);
        }
    }

    private void addToHead(DLinkedNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }

    private void removeNode(DLinkedNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void moveToHead(DLinkedNode node) {
        removeNode(node);
        addToHead(node);
    }
}
//继承内置类LinkedHashMap
class LRUCache extends LinkedHashMap<Integer, Integer>{
    private int capacity;

    public LRUCache(int capacity) {
        super(capacity, 0.75F, true);
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity;
    }
}
```
## 21 合并两个有序链表 easy
```
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
```
思路：递归
- 终止条件：若某节点为空则返回另一个节点
- 单层递归：若l1小于l2，l1下一个节点指向(l1.next, l2)两条链表递归后的头结点，返回l1，反之亦然
```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```
## 2 两数相加 middle
```
给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。
```
思路：同时遍历两个链表，逐位计算它们的和，并与当前位置的进位值相加即sum = n1 + n2 + carry，当前节点的值是sum % 10，新的进位是sum / 10。如果链表遍历结束后，有 carry>0，还需要在答案链表的后面附加一个节点，节点的值为 carry
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = null, tail = null;
        int n1, n2, carry = 0, sum = 0;
        while (l1 != null || l2 != null) {
            n1 = l1 == null ? 0 : l1.val;
            n2 = l2 == null ? 0 : l2.val;
            sum = n1 + n2 + carry;
            ListNode newNode = new ListNode(sum % 10);
            if (head == null) {
                head = tail = newNode;
            } else {
                tail.next = newNode;
                tail = tail.next;
            }
            carry = sum / 10;
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }
        if (carry != 0) tail.next = new ListNode(carry);
        return head;
    }
}
```

# 哈希表
常用于解决多个集合间是否有出现过的元素，结构可以有数组、set、map，当哈希值可控，较少时使用数组，较多时使用set；哈希值不可控时使用map
## 242 有效的字母异位词 easy 
哈希值是固定的即26个字母，构建int[26]，字符串的每个元素-'a'即可作为数组索引，遍历第一个字符串加值，第二个字符串减值，若最终都为0则说明true
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] hash = new int[26];
        for (int i = 0; i < s.length(); i++) {
            hash[s.charAt(i) - 'a']++;
        }
        for (int i = 0; i < t.length(); i++) {
            hash[t.charAt(i) - 'a']--;
        }
        for (int i = 0; i < hash.length; i++) {
            if (hash[i] != 0) return false;
        }
        return true;
    }
}
```
## 349 两个数组的交集 easy 
由于值最大是1000，定义哈希数组int[1001]，一个写，一个读，找到大于0的值，其索引即交集元素
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        int hash[] = new int[1001];
        Set<Integer> sets = new HashSet<>();
        for (int i = 0; i < nums1.length; i++) {
            hash[nums1[i]]++;
        }
        for (int i = 0; i < nums2.length; i++) {
            if (hash[nums2[i]] > 0) sets.add(nums2[i]);
        }
        int results[] = new int[sets.size()], index = 0;
        for (int num : sets) {
            results[index++] = num;
        }
        return results;
    }
}
```
## 454 四数相加 middle 
由于哈希值不可控，使用map结构。两个数组遍历后写入hash，另外两个数组遍历后找-key并将count加上所得的map值
```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> hash = new HashMap<>();
        int count = 0;
        for (int i : nums1) {
            for (int j : nums2) {
                hash.compute(i + j, (key, value) -> value == null ? 1 : value + 1);
            }
        }
        for (int k : nums3) {
            for (int l : nums4) {
                count = count + hash.getOrDefault(-(k + l), 0);
            }
        }
        return count;
    }
}
```
## 2325 解密消息 easy
```
给你字符串 key 和 message ，分别表示一个加密密钥和一段加密消息。解密 message 的步骤如下：

使用 key 中 26 个英文小写字母第一次出现的顺序作为替换表中的字母 顺序 。
将替换表与普通英文字母表对齐，形成对照表。
按照对照表 替换 message 中的每个字母。
空格 ' ' 保持不变。
例如，key = "happy boy"（实际的加密密钥会包含字母表中每个字母 至少一次），据此，可以得到部分对照表（'h' -> 'a'、'a' -> 'b'、'p' -> 'c'、'y' -> 'd'、'b' -> 'e'、'o' -> 'f'）。
返回解密后的消息。
```
思路：使用哈希表，键作为密钥，值作为明文，由于键空间是确定的，所以用数组作为哈希表
```java
class Solution {
    public String decodeMessage(String key, String message) {
        Character[] hash = new Character[26];
        int index = 0;
        StringBuffer sb = new StringBuffer();
        for (Character c: key.toCharArray()) {
            if (c != ' ' && hash[c - 'a'] == null) {
                hash[c - 'a'] = (char)('a' + index);
                index++;
            }
        }
        for (Character c: message.toCharArray()) {
            Character chr = c == ' ' ? ' ' : hash[c - 'a'];
            sb.append(chr);
        }
        return sb.toString();
    }
}
```
## 49 字母异位词分组 middle
```
给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。

字母异位词 是由重新排列源单词的所有字母得到的一个新单词。
```
思路：使用HashMap作为哈希表。由于互为字母异位词的两个字符串包含的字母相同，因此对两个字符串分别进行排序之后得到的字符串一定是相同的，故可以将排序之后的字符串作为哈希表的键。若调用242判断两字串异位词的方法，需要两两相比较，效率很低触发超时
```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            char[] array = str.toCharArray();
            Arrays.sort(array);
            String key = new String(array);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}
```
## 128 最长连续序列 middle
```
给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
请你设计并实现时间复杂度为 O(n) 的算法解决此问题。
```
思路：
- 第一种是哈希表，连续序列的起点即前一个数不存在于哈希表，当循环遍历到后一个数不存在于哈希表时，即到达了此连续序列的终点
- 第二种是排序加遍历（这是自己直接想到的，但排序的时间复杂度是O(N*logN)）
```java
//哈希表
class Solution {
    public int longestConsecutive(int[] nums) {
        int res = 0;    // 记录最长连续序列的长度
        Set<Integer> numSet = new HashSet<>();  // 记录所有的数值
        for(int num: nums){
            numSet.add(num);    // 将数组中的值加入哈希表中
        }
        int seqLen;     // 连续序列的长度
        for(int num: numSet){
            // 如果当前的数是一个连续序列的起点，统计这个连续序列的长度
            if(!numSet.contains(num - 1)){
                seqLen = 1;
                while(numSet.contains(++num))seqLen++;  // 不断查找连续序列，直到num的下一个数不存在于数组中
                res = Math.max(res, seqLen);    // 更新最长连续序列长度
            }
        }
        return res;
    }
}
//排序后依次遍历比较前一个元素，若相等则跳过，多了1则更新临时结果，其他情况则重置临时结果为1，然后比较当前的临时结果与真正结果取最大值
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length < 2) return nums.length;
        Arrays.sort(nums);
        int result = 1, temp = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i - 1]) continue;
            if (nums[i] - nums[i - 1] == 1) temp++;
            else temp = 1;
            result = Math.max(result, temp);
        }
        return result;
    }
}
```
## 75 颜色分类 middle
```
给定一个包含红色、白色和蓝色、共 n 个元素的数组 nums ，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。
必须在不使用库内置的 sort 函数的情况下解决这个问题。
```
思路：由于解空间有限，只有0 1 2，因为用数组作为哈希表，键是元素值，值是出现的次数，然后遍历此哈希表赋值原数组
```java
class Solution {
    public void sortColors(int[] nums) {
        int[] hash = new int[3];
        for (int num: nums) {
            hash[num]++;
        }
        int index = 0;
        for (int i = 0; i < 3; i++) {
            while (hash[i]-- > 0) nums[index++] = i;
        }
    }
}
```

# 字符串
## 344 反转字符串 easy 
双指针
```
public void reverseString(char[] s) {
    int left = 0, right = s.length - 1;
    while (left < right) {
        char tmp = s[left];
        s[left] = s[right];
        s[right] = tmp;
        left++;
        right--;
    }
}
```
## 541 反转字符串2 easy 
注意遍历时是2k递增的，每次处理时要判断end索引值是否够k个元素，若不是则直接取最后一个元素
```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] arr = s.toCharArray();
        for (int i = 0; i < arr.length; i += 2 * k) {
            int start = i;
            int end = Math.min(start + k - 1, arr.length - 1);
            while (start < end) {
                char tmp = arr[start];
                arr[start] = arr[end];
                arr[end] = tmp;
                start++;
                end--;
            }
        }
        return new String(arr);
    }
}
```
## 151 反转字符串里的单词 middle 
比较简单的做法是对字符串进行trim split转换后再倒序添加到新的字符串，但空间复杂度高。还可以用双指针，1.去除首尾以及中间多余空格 2.反转整个字符串 3.反转各个单词
```java
//简单的做法，使用java内置方法
class Solution {
    public String reverseWords(String s) {
        List<String> list = Arrays.asList(s.trim().split("( )+"));
        String result = "";
        for (int i = list.size() - 1; i > 0; i--) {
            result += list.get(i).trim() + " ";
        }
        result += list.get(0).trim();
        return result;
    }
}
//双指针
class Solution {
    public String reverseWords(String s) {
        // System.out.println("ReverseWords.reverseWords2() called with: s = [" + s + "]");
        // 1.去除首尾以及中间多余空格
        StringBuilder sb = removeSpace(s);
        // 2.反转整个字符串
        reverseString(sb, 0, sb.length() - 1);
        // 3.反转各个单词
        reverseEachWord(sb);
        return sb.toString();
    }

    private StringBuilder removeSpace(String s) {
        // System.out.println("ReverseWords.removeSpace() called with: s = [" + s + "]");
        int start = 0;
        int end = s.length() - 1;
        while (s.charAt(start) == ' ') start++;
        while (s.charAt(end) == ' ') end--;
        StringBuilder sb = new StringBuilder();
        while (start <= end) {
            char c = s.charAt(start);
            if (c != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }
            start++;
        }
        // System.out.println("ReverseWords.removeSpace returned: sb = [" + sb + "]");
        return sb;
    }

    /**
     * 反转字符串指定区间[start, end]的字符
     */
    public void reverseString(StringBuilder sb, int start, int end) {
        // System.out.println("ReverseWords.reverseString() called with: sb = [" + sb + "], start = [" + start + "], end = [" + end + "]");
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
        }
        // System.out.println("ReverseWords.reverseString returned: sb = [" + sb + "]");
    }

    private void reverseEachWord(StringBuilder sb) {
        int start = 0;
        int end = 1;
        int n = sb.length();
        while (start < n) {
            while (end < n && sb.charAt(end) != ' ') {
                end++;
            }
            reverseString(sb, start, end - 1);
            start = end + 1;
            end = start + 1;
        }
    }
}
```
## 459 重复的子字符串 easy
可以用kmp算法，但比较复杂，直接归纳出特性然后判断
```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return (s + s).indexOf(s, 1) != s.length();
    }
}
```

# 栈和队列
先进后出，擅长相邻元素的消除
## 232 用两个栈实现队列 easy
```java
class MyQueue {
    Deque<Integer> inStack;
    Deque<Integer> outStack;

    public MyQueue() {
        inStack = new ArrayDeque<>();
        outStack = new ArrayDeque<>();
    }
    
    public void push(int x) {
        inStack.push(x);
    }
    
    public int pop() {
        if (outStack.isEmpty()) {
            in2Out();
        }
        return outStack.pop();
    }
    
    public int peek() {
        if (outStack.isEmpty()) {
            in2Out();
        }
        return outStack.peek();
    }
    
    public boolean empty() {
        return inStack.isEmpty() && outStack.isEmpty();
    }

    private void in2Out() {
        while (!inStack.isEmpty()) {
            outStack.push(inStack.pop());
        }
    }
}
```
## 225 用一个队列模拟栈 easy
```java
class MyStack {
    Queue<Integer> queue;

    public MyStack() {
        queue = new ArrayDeque<>();
    }
    
    public void push(int x) {
        queue.offer(x);
        int size = queue.size();
        while (--size > 0) {
            queue.offer(queue.poll());
        }
    }
    
    public int pop() {
        return queue.poll();
    }
    
    public int top() {
        return queue.peek();
    }
    
    public boolean empty() {
        return queue.isEmpty();
    }
}
```
## 155 最小栈 middle
```
设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

实现 MinStack 类:

MinStack() 初始化堆栈对象。
void push(int val) 将元素val推入堆栈。
void pop() 删除堆栈顶部的元素。
int top() 获取堆栈顶部的元素。
int getMin() 获取堆栈中的最小元素。
```
思路：只需要设计一个数据结构，使得每个元素 a 与其相应的最小值 m 时刻保持一一对应。因此我们可以使用一个辅助栈，与元素栈同步插入与删除，用于存储与每个元素对应的最小值。
- 当一个元素要入栈时，我们取当前辅助栈的栈顶存储的最小值，与当前元素比较得出最小值，将这个最小值插入辅助栈中；
- 当一个元素要出栈时，我们把辅助栈的栈顶元素也一并弹出；
- 在任意一个时刻，栈内元素的最小值就存储在辅助栈的栈顶元素中。
```java
class MinStack {
    Deque<Integer> xStack; //元素栈
    Deque<Integer> minStack; //对应元素的最小栈，与元素栈中的每个元素一一对应

    public MinStack() {
        xStack = new LinkedList<Integer>();
        minStack = new LinkedList<Integer>();
        minStack.push(Integer.MAX_VALUE);
    }
    
    public void push(int x) {
        xStack.push(x);
        minStack.push(Math.min(minStack.peek(), x));
    }
    
    public void pop() {
        xStack.pop();
        minStack.pop();
    }
    
    public int top() {
        return xStack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```
## 20 有效的括号 easy
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (Character c: s.toCharArray()) {
            if (c == '(' || c == '[' || c == '{') stack.push(c);
            else if (c == ')' || c == ']' || c == '}') {
                if (stack.isEmpty()) return false;
                if (c == ')' && stack.pop() != '(') return false;
                if (c == ']' && stack.pop() != '[') return false;
                if (c == '}' && stack.pop() != '{') return false;
            }
        }
        return stack.isEmpty();
    }
}
```
## 1047 删除字符串中的所有相邻重复项 easy
```java
class Solution {
    public String removeDuplicates(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        String result = "";
        for (Character c : s.toCharArray()) {
            if (!stack.isEmpty() && stack.peek().equals(c)) stack.pop();
            else stack.push(c);
        }
        while (!stack.isEmpty()) {
            result += stack.removeLast();
        }
        return result;
    }
}
```
## 150 逆波兰表达式 middle
即后缀表达式，二叉树的后序遍历
```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<String> stack = new ArrayDeque<>();
        Map<String, BiFunction<Integer, Integer, Integer>> map = new HashMap<>();
        map.put("+", (x, y) -> x + y);
        map.put("-", (x, y) -> x - y);
        map.put("*", (x, y) -> x * y);
        map.put("/", (x, y) -> x / y);
        for (String token : tokens) {
            if (!map.containsKey(token)) stack.push(token);
            else {
                Integer y = Integer.valueOf(stack.pop());
                Integer x = Integer.valueOf(stack.pop());
                stack.push(String.valueOf(map.get(token).apply(x, y)));
            }
        }
        return Integer.valueOf(stack.pop());
    }
}
```
## 239 滑动窗口最大值 hard
```
给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。
返回 滑动窗口中的最大值 。
```
思路：使用优先级队列最大堆可以解决，元素是数组值、下标结构，堆顶元素即为当前堆的最大值，先初始化第一个窗口的值，再加入新的值到队列，循环判断当前堆顶元素这是否在窗口中，不在则删除堆顶元素，然后此时的栈顶元素即当前窗口的最大值
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        Queue<int[]> queue = new PriorityQueue<>((p1, p2) -> p1[0] == p2[0] ? p2[1] - p1[1] : p2[0] - p1[0]);
        for (int i = 0; i < k; i++) {
            queue.offer(new int[]{nums[i], i});
        }
        int[] result = new int[nums.length - k + 1];
        int index = 0;
        result[0] = queue.peek()[0];
        for (int i = k; i < nums.length; i++) {
            queue.offer(new int[]{nums[i], i});
            while (queue.peek()[1] < i - k + 1) queue.poll();
            result[++index] = queue.peek()[0];
        }
        return result;
    }
}
```
## 347 前k个高频元素 middle
思路：最小堆PriorityQueue，先统计出所有元素的次数，然后依次加到堆里，入堆前先判断堆的元素个数是否小于k，若小于则直接入堆，不小于则判断若堆顶元素小于要加的元素就先抛出堆顶元素再入堆
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Queue<Map.Entry<Integer, Integer>> queue = new PriorityQueue<>((p1, p2) -> p1.getValue() - p2.getValue());
        Map<Integer, Integer> map = new HashMap<>();
        int[] result = new int[k];

        for (int i: nums) {
            map.put(i, map.getOrDefault(i, 0) + 1);
        }
        for (Map.Entry<Integer, Integer> entry: map.entrySet()) {
            if (queue.size() < k) queue.offer(entry);
            else if (queue.peek().getValue() < entry.getValue()) {
                queue.poll();
                queue.offer(entry);
            }
        }
        for (int i = 0; i < k; i++) {
            result[i] = queue.poll().getKey();
        }
        return result;
    }
}
```

# 滑动窗口
## 3. 无重复字符的最长子串 middle
```
给定一个字符串 s ，请你找出其中不含有重复字符的最长子串的长度。
```
思路：使用滑动窗口
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // 哈希集合，记录每个字符是否出现过
        Set<Character> hash = new HashSet<Character>();
        int n = s.length();
        // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        int end = -1, result = 0;
        for (int begin = 0; begin < n; begin++) {
            if (begin != 0) { // 当左指针不是指向第1个元素时，说明此时已经迭代过一次，发生窗口滑动，哈希表移除前一个字符
                hash.remove(s.charAt(begin - 1));
            }
            while (end + 1 < n && !hash.contains(s.charAt(end + 1))) { //之所以是end + 1是因为窗口向右滑动时，此前的end也加1
                // 不断地移动右指针
                hash.add(s.charAt(end + 1));
                ++end;
            }
            // 第 begin 到 end 个字符是一个无重复字符子串
            result = Math.max(result, end - begin + 1);
        }
        return result;
    }
}
```
## 209 长度最小的子数组 middle
滑动窗口，注意end是窗口的终止位置。具体：每一轮迭代，将 nums[end] 加到 sum ，如果 sum >= s ，则更新子数组的最小长度（此时子数组的长度是 end−start+1），然后将 nums[start] 从 sum 中减去并将 start 右移，直到 sum<s，在此过程中同样更新子数组的最小长度。在每一轮迭代的最后，将 end 右移。
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int begin = 0, end = 0, sum = 0, result = Integer.MAX_VALUE;
        for(; end < nums.length; end++) {
            sum += nums[end];
            while (sum >= target) {
                result = Math.min(result, end - begin + 1);
                sum -= nums[begin++];
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}
```
## 438 找到字符串中所有字母异位词 middle
```
给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

异位词 指由相同字母重排列形成的字符串（包括相同的字符串）。
```
思路：哈希表+滑动窗口，由于要找异位词，滑动窗口长度必然是p字串的长度
```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int sLen = s.length(), pLen = p.length();

        if (sLen < pLen) {
            return new ArrayList<Integer>();
        }

        List<Integer> ans = new ArrayList<Integer>();
        int[] sCount = new int[26];
        int[] pCount = new int[26];
        for (int i = 0; i < pLen; ++i) {
            ++sCount[s.charAt(i) - 'a'];
            ++pCount[p.charAt(i) - 'a'];
        }

        if (Arrays.equals(sCount, pCount)) { //比较第1个窗口：若一样，说明已经找到了第一个异位词，索引是0
            ans.add(0);
        }

        for (int i = 0; i < sLen - pLen; ++i) {
            --sCount[s.charAt(i) - 'a']; //右移窗口，移除前一个窗口的首元素
            ++sCount[s.charAt(i + pLen) - 'a']; //右移窗口，加上前一个窗口的末元素后的元素。因为要找异位词，所以窗口长度必然是pLen

            if (Arrays.equals(sCount, pCount)) { //比较新的窗口，由于此时i代表前一个窗口的首元素，因此这里是i+1
                ans.add(i + 1);
            }
        }

        return ans;
    }
}
```

# 递归
## 144 二叉树的前序遍历 easy
```java
//递归 中左右
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        recurse(root, result);
        return result;
    }

    private void recurse(TreeNode cur, List<Integer> result) {
        if (cur == null) return;
        result.add(cur.val);
        recurse(cur.left, result);
        recurse(cur.right, result);
    }
}
//迭代 中右左，因为栈是先进后出
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode node;
        stack.push(root);
        while (!stack.isEmpty()) {
            node = stack.pop();
            if (node != null) result.add(node.val);
            else continue;
            stack.push(node.right);
            stack.push(node.left);
        }
        return result;
    }
}
```
## 145 二叉树的后序遍历 easy
若用迭代栈，后序遍历是先中左右再双指针进行翻转
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode node;
        stack.push(root);
        while (!stack.isEmpty()) {
            node = stack.pop();
            if (node != null) result.add(node.val);
            else continue;
            stack.push(node.left);
            stack.push(node.right);
        }
        int left = 0, right = result.size() - 1;
        while (left <= right) {
            int temp = result.get(left);
            result.set(left, result.get(right));
            result.set(right, temp);
            left++;
            right--;
        }
        return result;
    }
}
```
## 95 二叉树的中序遍历 easy
若用迭代，循环迭代左侧节点入栈，直到无左侧节点时将当前节点定位到栈的顶端元素即父节点，将其出栈，再将当前节点定位到右孩子节点。直到栈无元素且当前节点为空
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()){
            if (cur != null){
                stack.push(cur);
                cur = cur.left;
            }else{
                cur = stack.pop();
                result.add(cur.val);
                cur = cur.right;
            }
        }
        return result;
    }
}
```
## 102 二叉树的层序遍历 middle
可用递归和迭代队列两种方式实现BFS，递归更容易理解
```java
class Solution {
    private List<List<Integer>> resList = new ArrayList<List<Integer>>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        recurse(root,0);
        //checkFun02(root);
        return resList;
    }

    //BFS--递归方式
    private void recurse(TreeNode root, Integer deep) {
        if (root == null) return;
        deep++;
        if (res.size() < deep) {
            res.add(new ArrayList<Integer>());
        }
        res.get(deep - 1).add(root.val);
        recurse(root.left, deep);
        recurse(root.right, deep);
    }

    //BFS--迭代方式--借助队列
    public void checkFun02(TreeNode node) {
        if (node == null) return;
        Queue<TreeNode> que = new LinkedList<TreeNode>();
        que.offer(node);

        while (!que.isEmpty()) {
            List<Integer> itemList = new ArrayList<Integer>();
            int len = que.size();

            while (len > 0) {
                TreeNode tmpNode = que.poll();
                itemList.add(tmpNode.val);

                if (tmpNode.left != null) que.offer(tmpNode.left);
                if (tmpNode.right != null) que.offer(tmpNode.right);
                len--;
            }

            resList.add(itemList);
        }
    }
}
```
## 226 翻转二叉树 easy
使用前序遍历或后序遍历比较容易。若用中序遍历，recurse(left) swap(node) recurse(left) 
```java
public TreeNode invertTree(TreeNode root) {
    recurse(root);
    return root;
}

private void recurse(TreeNode node) {
    if (node == null) return;
    TreeNode temp = node.left;
    node.left = node.right;
    node.right = temp;
    recurse(node.left);
    recurse(node.right);
}
```
## 101 对称二叉树 easy
只能使用后序遍历，需要先收集孩子的信息再向上一层返回。若外侧、内侧都对应相同则是对称的
```java
public boolean isSymmetric(TreeNode root) {
    return recurse(root.left, root.right);
}
private boolean recurse(TreeNode left, TreeNode right) {
    if (left == null && right != null) return false;
    if (left != null && right == null) return false;
    if (left == null && right == null) return true;
    if (left.val != right.val) return false;
    boolean outer = recurse(left.left, right.right);
    boolean inner = recurse(left.right, right.left);
    if (outer && inner) return true;
    else return false;
}
```
## 104 二叉树的最大深度 easy
后序遍历，找出左右孩子的较大高度+1后即为该节点的高度，根节点的高度是这个二叉树的最大深度。
```java
class Solution {
    int result = 0;
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```
## 111 二叉树的最小深度 easy
后序遍历，根节点的最小高度即这个二叉树的最小深度。若左右孩子有为空的，则找左右孩子较大深度+1作为该节点的高度，若左右孩子都非空，则找左右孩子较小深度+1作为该节点的高度
```java
class Solution {
    int result = 0;
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        int leftDepth = minDepth(root.left);
        int rightDepth = minDepth(root.right);
        if (root.left == null || root.right == null) return Math.max(leftDepth, rightDepth) + 1;
        return Math.min(leftDepth, rightDepth) + 1;
    }
}
```
## 110 平衡二叉树 easy
思路：后序遍历，在求节点高度的过程中，判断左右节点高度差是否大于1，若是则直接返回-1即不符合平衡二叉树，其判断不符合的依据是高度差大于1、左节点已不符合、右节点已不符合
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getHeight(root) != -1 ? true : false;
    }
    private int getHeight(TreeNode node) {
        if (node == null) return 0;
        int leftHeight = getHeight(node.left);
        int rightHeight = getHeight(node.right);
        if (leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1) return -1;
        int height = Math.max(leftHeight, rightHeight) + 1;
        return height;
    }
}
```
## 222 完全二叉树的的节点个数 easy
后序遍历，找出左右孩子的节点个数再加1，即当前节点所在子树的节点个数
```java
class Solution {
    int result = 0;
    public int countNodes(TreeNode root) {
        if (root == null) return 0;
        int left = countNodes(root.left);
        int right = countNodes(root.right);
        return result = left + right + 1;
    }
}
```
## 108 将有序数组转换为搜索二叉树 easy
思路：选择中间位置左边的数字作为根节点，确保中间节点的大小大于左、小于右
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return recurse(nums, 0, nums.length - 1);
    }

    public TreeNode recurse(int[] nums, int left, int right) {
        if (left > right) {
            return null;
        }

        // 总是选择中间位置左边的数字作为根节点
        int mid = left + (right - left) / 2;

        TreeNode root = new TreeNode(nums[mid]);
        root.left = recurse(nums, left, mid - 1);
        root.right = recurse(nums, mid + 1, right);
        return root;
    }
}
```
## 257 二叉树的所有路径 easy
前序遍历+回溯
```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        recurse(root, new ArrayList<String>(), result);
        return result;
    }
    private void recurse(TreeNode node, List<String> path, List<String> result) {
        path.add(String.valueOf(node.val)); //中序遍历
        if (node.left == null && node.right == null) result.add(String.join("->", path)); //遇到叶节点
        if (node.left != null) {
            recurse(node.left, path, result);
            path.remove(path.size() - 1); //回溯
        }
        if (node.right != null) {
            recurse(node.right, path, result);
            path.remove(path.size() - 1); //回溯
        }
    }
}
```
## 404 左叶子之和 easy
后序遍历，当中间节点满足左孩子是左叶节点时，加上左叶节点的值
```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root==null) return 0;
        return sumOfLeftLeaves(root.left)
                + sumOfLeftLeaves(root.right)
                + (root.left!=null && root.left.left==null && root.left.right==null ? root.left.val : 0);
    }
}
```
## 513 找树左下角的值 middle
前中后序遍历都行，因为都是先遍历左节点再遍历右节点。两个全局变量：一个记录最大深度maxDepth，一个记录结果result。终止条件是叶子节点，若此时深度大于最大深度，则更新maxDepth和result
```java
class Solution {
    private int maxDepth = Integer.MIN_VALUE;
    private int result = 0;

    public int findBottomLeftValue(TreeNode root) {
        recurse(root, 1);
        return result;
    }

    private void recurse(TreeNode node, int depth) {
        if (node.left == null && node.right == null && depth > maxDepth) {
            maxDepth = depth;
            result = node.val;
        }
        if (node.left != null) {
            depth++;
            recurse(node.left, depth);
            depth--; //回溯
        }
        if (node.right != null) {
            depth++;
            recurse(node.right, depth);
            depth--; //回溯
        }
    }
}
```
## 112 路径总和 easy
前序遍历，终止条件是叶子节点时的sum等于目标值返回true，注意回溯。原理和257找出所有路径类似
```java
class Solution {
    private int sum = 0;
    private boolean flag = false;
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;
        return recurse(root, targetSum);
    }
    private boolean recurse(TreeNode node, int targetSum) {
        sum += node.val;
        if (node.left == null && node.right == null && sum == targetSum) flag = true;
        if (node.left != null) {
            hasPathSum(node.left, targetSum);
            sum -= node.left.val;
        }
        if (node.right != null) {
            hasPathSum(node.right, targetSum);
            sum -= node.right.val;
        }
        return flag;
    }
}
```
## 106 从中序与后序遍历序列构造二叉树 middle
先从后序最后一个节点确定根节点，再从中序切割出左右，再从后序切割出中，注意区间统一是左闭右开，迭代确定二叉树

第一步：如果数组大小为零的话，说明是空节点了。

第二步：如果不为空，那么取后序数组最后一个元素作为节点元素。

第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点

第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）

第五步：切割后序数组，切成后序左数组和后序右数组

第六步：递归处理左区间和右区间
```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();  // 方便根据数值查找位置
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for (int i = 0; i < inorder.length; i++) { // 用map保存中序序列的数值对应位置
            map.put(inorder[i], i);
        }
        return findNode(inorder, 0, inorder.length, postorder, 0, postorder.length);  // 前闭后开
    }

    public TreeNode findNode(int[] inorder, int inBegin, int inEnd, int[] postorder, int postBegin, int postEnd) {
        // 参数里的范围都是前闭后开
        if (inBegin >= inEnd || postBegin >= postEnd) {  // 不满足左闭右开，说明没有元素，返回空树
            return null;
        }
        int rootIndex = map.get(postorder[postEnd - 1]);  // 找到后序遍历的最后一个元素在中序遍历中的位置
        TreeNode root = new TreeNode(inorder[rootIndex]);  // 构造结点
        int lenOfLeft = rootIndex - inBegin;  // 保存中序左子树个数，用来确定后序数列的个数
        root.left = findNode(inorder, inBegin, rootIndex,
                            postorder, postBegin, postBegin + lenOfLeft); //用左中序、左后序构造左子树
        root.right = findNode(inorder, rootIndex + 1, inEnd,
                            postorder, postBegin + lenOfLeft, postEnd - 1); //用右中序、右后序构造右子树
        return root;
    }
}
```
## 654 最大二叉树 middle
前序遍历
- 确定入参和返回值：参数传入的是存放元素的数组、左闭索引、右开索引，返回该数组构造的二叉树的头结点
- 终止条件：如果传入的数组大小为1，说明遍历到了叶子节点则返回，如果右标小于左标说明没有元素直接返回null
- 单层递归的逻辑：先找到最大值作为根节点，并记录其下标用来分割数组maxIndex，[leftIndex, maxIndex)作为左子树，[maxIndex+1, rightIndex)作为右子树
```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return constructMaximumBinaryTree1(nums, 0, nums.length);
    }

    public TreeNode constructMaximumBinaryTree1(int[] nums, int leftIndex, int rightIndex) {
        if (rightIndex - leftIndex < 1) {// 没有元素了
            return null;
        }
        if (rightIndex - leftIndex == 1) {// 只有一个元素
            return new TreeNode(nums[leftIndex]);
        }
        int maxIndex = leftIndex;// 最大值所在位置
        int maxVal = nums[maxIndex];// 最大值
        for (int i = leftIndex + 1; i < rightIndex; i++) {
            if (nums[i] > maxVal){
                maxVal = nums[i];
                maxIndex = i;
            }
        }
        TreeNode root = new TreeNode(maxVal);
        // 根据maxIndex划分左右子树
        root.left = constructMaximumBinaryTree1(nums, leftIndex, maxIndex);
        root.right = constructMaximumBinaryTree1(nums, maxIndex + 1, rightIndex);
        return root;
    }
}
```
## 617 合并二叉树 easy
前序遍历
```java
public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
    if (root1 == null) return root2;
    if (root2 == null) return root1;
    root1.val += root2.val;
    root1.left = mergeTrees(root1.left, root2.left);
    root1.right = mergeTrees(root1.right, root2.right);
    return root1;
}
```
## 700 二叉搜索树中的搜索 easy
```java
public TreeNode searchBST(TreeNode root, int val) {
    if (root == null || root.val == val) return root;
    TreeNode result = null;
    if (root.left != null && root.val > val) result = searchBST(root.left, val);
    if (root.right != null && root.val < val) result = searchBST(root.right, val);
    return result;
}
```
## 98 验证二叉搜索树 middle
中序遍历。注意第一种方法规定了节点值的范围是[Integer.MIN_VALUE,Integer.MAX_VALUE-1]，max作为遍历到前一个节点时的最大值，若节点中有Integer.MIN_VALUE会判断异常，因为max要设成更小的一个值用Long.MIN_VALUE
```java
//第一种：使用全局最大值进行判断，注意最大值的类型要比节点值的范围大
class Solution {
    private long max = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        boolean left = isValidBST(root.left);
        if (root.val > max) max = root.val;
        else return false;
        boolean right = isValidBST(root.right);
        return left && right;
    }
}
//第二种：遍历中节点时设置前一个节点为当前节点，后续进行比较，更好些，不用担心最大值的类型问题
class Solution {
    private TreeNode pre = null;
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        boolean left = isValidBST(root.left); //左
        if (pre != null && pre.val >= root.val) return false; //中
        pre = root;  //中
        boolean right = isValidBST(root.right); //右
        return left && right;
    }
}
```
## 530 二叉搜索树的最小绝对差
中序遍历
```java
//最简单的方法是中序遍历构造出一个递增的list，然后双指针移动比较得到最小绝对差。但这样性能比较差
class Solution {
    List<Integer> list = new ArrayList<>();
    public int getMinimumDifference(TreeNode root) {
        recurse(root);
        int result = Integer.MAX_VALUE;
        for (int i = 0, j = 1; j < list.size(); i++, j++) {
            result = Math.min(result, Math.abs(list.get(j) - list.get(i)));
        }
        return result;
    }
    private void recurse(TreeNode node) {
        if (node == null) return;
        recurse(node.left);
        list.add(node.val);
        recurse(node.right);
    }
}
//中序遍历过程中记录前一个元素和最小绝对差，初始化时分别为空和最大整数，在中节点比较生成新的最小绝对差，再记录当前节点为上一个节点进行下一次迭代。终止条件是当遍历到空节点时返回一个最大整数以不影响最终结果
class Solution {
    TreeNode pre = null;
    int result = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        if (root == null) return Integer.MAX_VALUE;
        getMinimumDifference(root.left);
        if (pre != null) result = Math.min(Math.abs(root.val - pre.val), result);
        pre = root;
        getMinimumDifference(root.right);
        return result;
    }
}
```
## 501 二叉搜索树中的众数 easy
中序遍历，全局变量：上一个节点pre、结果集list、当前节点的频率count、当前最大的频率maxCount。关键在于count和maxCount的比较
```java
class Solution {
    TreeNode pre = null;
    List<Integer> list = new ArrayList<>();
    int count = 0, maxCount = Integer.MIN_VALUE;
    public int[] findMode(TreeNode root) {
        recurse(root);
        int[] ints = new int[list.size()];
        for (int i = 0; i < list.size();i++) {
            ints[i] = list.get(i);
        }
        return ints;
    }
    private void recurse(TreeNode cur) {
        if (cur == null) return;
        recurse(cur.left); //左
        //中
        if (pre == null) count = 1;
        else if (cur.val == pre.val) count++;
        else count = 1;
        if (count == maxCount) list.add(cur.val); //这样处理就只需要遍历一次二叉树
        if (count > maxCount) { //这样处理就只需要遍历一次二叉树，当发现更多频率的元素时直接清空原有结果集
            list.clear();
            list.add(cur.val);
            maxCount = count;
        }
        pre = cur;
        recurse(cur.right); //右
    }
}
```
## 235 二叉搜索树的公共祖先 middle
本题中的搜索树的值是唯一的，根据二叉搜索树的特性，左右节点应该分别小于、大于最近的公共祖先节点，最近的公共祖先可能不是根节点。当都小于某节点时说明公共祖先在此节点的左子树，反之同理
```java
//迭代法
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while (root != null) {
            if (p.val < root.val && q.val < root.val) root = root.left;
            else if (p.val > root.val && q.val > root.val) root = root.right;
            else break;
        }
        return root;
    }
}
//递归法
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root.val > p.val && root.val > q.val) return lowestCommonAncestor(root.left, p, q);
        if (root.val < p.val && root.val < q.val) return lowestCommonAncestor(root.right, p, q);
        return root;
    }
}
```

# 模拟
## 2899 上一个遍历的整数 easy
```java
public List<Integer> lastVisitedIntegers(int[] nums) {
    List<Integer> seen = new ArrayList<>(), ans = new ArrayList<>();
    int index = 1;
    boolean flag = true;
    for (int num : nums) {
        if (num != -1) {
            seen.add(num);
            flag = false;
            index = 1;
        } else {
            if (flag) index++;
            int pos = seen.size() - index;
            ans.add(pos >= 0 ? seen.get(pos) : -1);
            flag = true;
        }
    }
    return ans;
}
```

# 数学
## 292 Nim游戏 easy
```
你和你的朋友，两个人一起玩 Nim 游戏：

桌子上有一堆石头。
你们轮流进行自己的回合， 你作为先手 。
每一回合，轮到的人拿掉 1 - 3 块石头。
拿掉最后一块石头的人就是获胜者。
假设你们每一步都是最优解。请编写一个函数，来判断你是否可以在给定石头数量为 n 的情况下赢得游戏。如果可以赢，返回 true；否则，返回 false 。
```
思路：如果总的石头数目为 4 的倍数时，因为无论你取多少石头，对方总有对应的取法，你1 2 3，对方3 2 1，让剩余的石头的数目继续为 4 的倍数。对于你或者你的对手取石头时，显然最优的选择是当前己方取完石头后，让剩余的石头的数目为 4 的倍数。假设当前的石头数目为 x，如果 x 为 4 的倍数时，则此时你必然会输掉游戏；如果 x 不为 4 的倍数时，则此时你只需要取走 x % 4 个石头时，则剩余的石头数目必然为 4 的倍数，从而对手会输掉游戏。
```java
class Solution {
    public boolean canWinNim(int n) {
        return n % 4 != 0;
    }
}
```
## 136 只出现一次的数字 easy
```
给你一个 非空 整数数组 nums ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。
```
思路：任何数与0异或是其本身，任何数与自身异或是0，由于只有一个一次出现的数字，因此所有数字异或后即所需的结果
```java
class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;
        for (int num: nums) {
            result ^= num;
        }
        return result;
    }
}
```
## 169 多数元素 easy
```
给定一个大小为 n 的数组 nums ，返回其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。
```
思路：排序后中间元素必为所要的结果
```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[(nums.length - 1) / 2];
    }
}
```
## 287 寻找重复数 middle
```
给定一个包含 n + 1 个整数的数组 nums ，其数字都在 [1, n] 范围内（包括 1 和 n），可知至少存在一个重复的整数。
假设 nums 只有 一个重复的整数 ，返回 这个重复的数 。
你设计的解决方案必须 不修改 数组 nums 且只用常量级 O(1) 的额外空间。
```
思路：能直接想到的就是使用哈希表解决。但也能用快慢指针找环的交点，时间效率高
```java
//快慢指针
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = 0, fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
//哈希表
class Solution {
    public int findDuplicate(int[] nums) {
        Set<Integer> hash = new HashSet<>();
        for (int num: nums) {
            if (hash.contains(num)) {
                return num;
            } else {
                hash.add(num);
            }
        }
        return -1;

    }
}
```