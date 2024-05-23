# day59

## 第五十九天| 单调栈 503 42

### 503 下一个更大的元素2 middle
```
给定一个循环数组 nums （ nums[nums.length - 1] 的下一个元素是 nums[0] ），返回 nums 中每个元素的 下一个更大元素 。
数字 x 的 下一个更大的元素 是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1 。

输入: nums = [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```
思路：
- 单调栈中保存的是下标，从栈底到栈顶的下标在数组 nums 中对应的值是单调不升的。每次我们移动到数组中的一个新的位置 i，我们就将当前单调栈中所有对应值小于 nums[i] 的下标弹出单调栈，这些值的下一个更大元素即为 nums[i]（证明很简单：如果有更靠前的更大元素，那么这些位置将被提前弹出栈）。随后我们将位置 i 入栈。
- 注意到只遍历一次序列是不够的，例如序列 [2,3,1]，最后单调栈中将剩余 [3,1]，其中元素 [1] 的下一个更大元素还是不知道的。 一个朴素的思想是，我们可以把这个循环数组「拉直」，即复制该序列的前 n−1 个元素拼接在原序列的后面。这样我们就可以将这个新序列当作普通序列，用上文的方法来处理。 而在本题中，我们不需要显性地将该循环数组「拉直」，而只需要在处理时对下标取模即可。
```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        Arrays.fill(res, -1);
        Deque<Integer> stack = new LinkedList<Integer>();
        for (int i = 0; i < n * 2 - 1; i++) {
            while (!stack.isEmpty() && nums[i % n] > nums[stack.peek()]) {
                res[stack.pop()] = nums[i % n];
            }
            stack.push(i % n);
        }
        return res;
    }
}
```
### 42 接雨水 hard
```
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```
思路：找到每一个柱子左、右第一个更高的柱子，某凹槽的水量面积=(min(左边第一个更高的柱子，右边第一个更高的柱子)-当前凹槽高度) * (右边第一个更高柱子索引-左边第一个更高柱子索引-1)。可通过单调栈求第一个更高柱子，单调栈存数组索引，从栈顶到栈底单调递增。当遇到当前元素比栈顶元素高时，当前元素是栈顶元素右边第一个更高柱子，第二个栈顶元素是栈顶元素左边第一个更高柱子。注意当前元素若等于栈顶元素，可以直接push到栈，也可抛出栈顶再push新的，不影响最终结果，前者更简洁建议用，因为直接push后，栈顶和第二个栈顶相同，后续出栈时左右更高柱子的最小值-当前栈顶=0，因此面积是0，不影响最终面积。
```java
class Solution {
    public int trap(int[] height) {
        int size = height.length;
        if (size <= 2)
            return 0;

        Stack<Integer> stack = new Stack<Integer>();
        stack.push(0);

        int sum = 0;
        for (int index = 1; index < size; index++) {
            while (!stack.isEmpty() && height[index] > height[stack.peek()]) {
                int mid = stack.pop(); // 栈顶元素是中间凹槽
                if (!stack.isEmpty()) {
                    int left = stack.peek(); // 第二个栈顶元素是凹槽左边第一个更大柱子
                    int h = Math.min(height[left], height[index]) - height[mid]; // 当前元素是凹槽右边第一个更大柱子，最小值决定水面高度，减去凹槽高度即水的高度
                    int w = index - left - 1;
                    int hold = h * w;
                    if (hold > 0)
                        sum += hold;
                }
            }
            stack.push(index);
        }
        return sum;
    }
}
```