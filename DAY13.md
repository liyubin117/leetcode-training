# day13

## 第十三天| 栈与队列 239 347

### 239 滑动窗口最大值 hard
```
给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回 滑动窗口中的最大值 。
```
https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html

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

### 347 前K个高频元素 middle
```
给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。
```
https://programmercarl.com/%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E6%80%BB%E7%BB%93.html

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