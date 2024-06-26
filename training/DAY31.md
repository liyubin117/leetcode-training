# day31

## 第三十一天| 贪心法 455 376 53

### 455 分发饼干 easy
```
假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。
对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。
```
思路：尽量用大饼干满足胃口大的小孩，对小孩胃口和饼干都排序后，外层倒序遍历小孩胃口，内层倒序判断当前饼干是否不小于当前小孩胃口，若是则说明符合条件，结果加1，再移动当前饼干和小孩到前一个，反之则只移动小孩到前一个
```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int result = 0, index = s.length - 1;
        for (int i = g.length - 1; i >= 0; i--) {
            if (index >=0 && s[index] >= g[i]) {
                index--;
                result++;
            }
        }
        return result;
    }
}
```

### 376 摆动序列 middle
```
如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为 摆动序列 。第一个差（如果存在的话）可能是正数或负数。仅有一个元素或者含两个不等元素的序列也视作摆动序列。

例如， [1, 7, 4, 9, 2, 5] 是一个 摆动序列 ，因为差值 (6, -3, 5, -7, 3) 是正负交替出现的。

相反，[1, 4, 7, 2, 5] 和 [1, 7, 4, 5, 5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。
子序列 可以通过从原始序列中删除一些（也可以不删除）元素来获得，剩下的元素保持其原始顺序。

给你一个整数数组 nums ，返回 nums 中作为 摆动序列 的 最长子序列的长度 。

输入：nums = [1,7,4,9,2,5]
输出：6
解释：整个序列均为摆动序列，各元素之间的差值为 (6, -3, 5, -7, 3) 。

输入：nums = [1,17,5,10,13,15,10,5,16,8]
输出：7
解释：这个序列包含几个长度为 7 摆动序列。
其中一个是 [1, 17, 10, 13, 10, 16, 8] ，各元素之间的差值为 (16, -7, 3, -3, 6, -8) 。

输入：nums = [1,2,3,4,5,6,7,8,9]
输出：2
```
思路：
- 遇到摆动时记录+1，遇到坡度或左边的平坡时直接忽略。起始值是1，curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0说明是坡要加1
    - 在某点的prediff与curdiff一正一负则说明遇到了摆动
    - 遇到平坡时记录右侧节点，也可以记录左侧节点，然后忽略平坡的其他节点
    - 首尾元素，首元素默认就是有坡的，因此起始值是1。从第二个元素开始，尾元素curdiff=0，prediff大于0或小于0说明是坡要加1
    - 只在坡度发生变化时更新prediff，即坡刚开始的坡度，防止单调平坡影响计算。
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length <= 1) return nums.length;
        //当前差值
        int curDiff = 0;
        //上一个差值
        int preDiff = 0;
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            //得到当前差值
            curDiff = nums[i] - nums[i - 1];
            //如果当前差值和上一个差值为一正一负
            //等于0的情况表示初始时的preDiff
            if ((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0)) {
                count++;
                preDiff = curDiff;
            }
        }
        return count;
    }
}
```

### 53 最大子数组和 middle
```
给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

子数组是数组中的一个连续部分。

输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```
思路：因为旧的总和若是负数只会拖累新的总和，因此当之前的求和为负时，直接选择当前数为新起点，否则直接加，然后更新最大结果
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int sum = -1;
        int result = Integer.MIN_VALUE;
        for (int num: nums) {
            if (sum < 0) sum = num;
            else sum += num;
            result = Math.max(result, sum);
        }
        return result;
    }
}
```
