# day60

## 第六十天| 单调栈 84

### 84 柱状图中最大的矩形 hard
```
给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
求在该柱状图中，能够勾勒出来的矩形的最大面积。

输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```
思路：与42接雨水类似，只是接雨水求柱子外部，本题求柱子内部最大面积。
- 先确定宽度，找左、右第一个比它矮的，确定扩展的边界，高度即当前柱子的高度。单调栈是栈顶到栈底单调递减
- 数组两侧加0，是为了触发while判断即不符合单调递减栈的情况。若是一个递减数组是不可能触发结果的，则两边置0则可以对每个元素都触发，且能防止对空栈操作，而且求面积至少需要三个元素。
```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int[] newHeight = new int[heights.length + 2];
        System.arraycopy(heights, 0, newHeight, 1, heights.length);
        newHeight[heights.length+1] = 0;
        newHeight[0] = 0;

        Stack<Integer> stack = new Stack<>();
        stack.push(0);

        int res = 0;
        for (int i = 1; i < newHeight.length; i++) {
            while (!stack.isEmpty() && newHeight[i] < newHeight[stack.peek()]) {
                int mid = stack.pop();
                int w = i - stack.peek() - 1;
                int h = newHeight[mid];
                res = Math.max(res, w * h);
            }
            stack.push(i);

        }
        return res;
    }
}
```