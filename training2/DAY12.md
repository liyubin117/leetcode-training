# day12

## 150 逆波兰表达式求值 middle
栈，若非加减乘除则直接入栈，若是则抛出两个分别为y和x，然后再用此表达式进行计算后入栈。使用map，键是表达式，值是对应的运算
```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        Map<String, BiFunction<Integer, Integer, Integer>> map = new HashMap<>();
        map.put("+", (x, y) -> x + y);
        map.put("-", (x, y) -> x - y);
        map.put("*", (x, y) -> x * y);
        map.put("/", (x, y) -> x / y);
        for (String token: tokens) {
            if (!map.containsKey(token)) stack.push(Integer.valueOf(token));
            else {
                Integer y = stack.pop();
                Integer x = stack.pop();
                stack.push(map.get(token).apply(x, y));
            }
        }
        return stack.pop();
    }
}
```

## 239 滑动窗口最大值 hard
优先级队列，最大堆，元素是二元组(数组值，数组索引)。先初始化第一个窗口的值，再加入新的值到队列，**循环判断当前堆顶元素这是否在窗口中，窗口的范围是[i-k+1, i]，不在则删除堆顶元素**，然后此时的栈顶元素即当前窗口的最大值
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

## 347 前K个高频元素 middle
最小堆，元素是int[]，先用map记录每个元素的出现次数，然后入队列
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num: nums) {
            map.compute(num, (key, value) -> value == null ? 1 : value + 1);
        }
        Queue<int[]> queue = new PriorityQueue<>((p1, p2) -> p1[1] - p2[1]);
        int count = 0;
        for (Map.Entry<Integer, Integer> entry: map.entrySet()) {
            if (count < k) {
                queue.offer(new int[]{entry.getKey(), entry.getValue()});
                count++;
            } else {
                if (queue.peek()[1] < entry.getValue()) {
                    queue.poll();
                    queue.offer(new int[]{entry.getKey(), entry.getValue()});
                }
            }
        }
        int[] result = new int[k];
        int index = 0;
        for (int i = 0; i < k; i++) {
            result[index++] = queue.poll()[0];
        }
        return result;
    }
}
```