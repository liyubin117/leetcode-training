# day52

## 第五十二天| 动态规划 300 674 718

### 300 最长递增子序列 middle
```
给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。
子序列 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```
思路：动态规划，时间复杂度O(N^2)，空间复杂度O(N)
- dp数组定义：dp[i] 以nums[i]结尾的最长子序列的长度
- 转移方程： 设 j∈[0,i)，考虑每轮计算新 dp[i] 时，遍历 [0,i) 列表区间
    - 当 nums[i]>nums[j] 时，最长递增子序列长度为 dp[j]+1
    - 当 nums[i]<=nums[j] 时，非递增，跳过
    - 遍历 j 时，每轮执行 dp[i]=max(dp[i],dp[j]+1)
- 初始状态：dp[i] 所有元素置 1，含义是每个元素都至少可以单独成为子序列，此时长度都为 1。
- 返回值：返回 dp 列表最大值，即可得到全局最长上升子序列长度。注意不是最后一个dp元素，因为以最后一个元素结尾的子串不一定具有最长的子序列长度，比如[10,9,2,5,3,7,101,6]以6结尾那么最长递增子序列是[2,5,6]长度是3，整个的最长递增子序列是[2,5,7,101]长度是4
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums.length == 0) return 0;
        int[] dp = new int[nums.length];
        int res = 0;
        Arrays.fill(dp, 1);
        for(int i = 0; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if(nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```
- 二分查找
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tails = new int[nums.length];
        int res = 0;
        for(int num : nums) {
            int i = 0, j = res;
            while(i < j) {
                int m = (i + j) / 2;
                if(tails[m] < num) i = m + 1;
                else j = m;
            }
            tails[i] = num;
            if(res == j) res++;
        }
        return res;
    }
}
```
### 674 最长连续递增子序列 easy
```
给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。
连续递增的子序列 可以由两个下标 l 和 r（l < r）确定，如果对于每个 l <= i < r，都有 nums[i] < nums[i + 1] ，那么子序列 [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] 就是连续递增子序列。

输入：nums = [1,3,5,4,7]
输出：3
解释：最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为 5 和 7 在原数组里被 4 隔开。 
```
思路：相较于300问题，多了连续的限制，其实更简单了
- 动态规划。dp[i]以i为结尾的最长连续递增子序列的最大长度，递推公式：当前一个元素小于当前元素时dp[i-1]+1即dp[i]
```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int[] dp = new int[nums.length];
        int res = 1;
        Arrays.fill(dp, 1);
        for (int i = 1; i < nums.length; i++) {
            if (nums[i - 1] < nums[i]) dp[i] = dp[i - 1] + 1;
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```
- 两个变量存储全局最大长度、当前最大长度
```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int res = 1, tmp = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > nums[i-1]) tmp++;
            else tmp = 1;
            res = Math.max(res, tmp);
        }
        return res;
    }
}
```
### 718 最长重复子数组 middle
```
给两个整数数组 nums1 和 nums2 ，返回 两个数组中 公共的 、长度最长的子数组的长度 。

输入：nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
输出：3
解释：长度最长的公共子数组是 [3,2,1] 。
```
思路：子数组和子序列是不同，子数组是连续的。用二维dp数组记录两个数组比较的状态
- dp数组定义：dp[i][j] 以i-1下标结尾的nums1和以j-1下标结尾的nums2的最长重复子数组的长度，之所以-1是为了方便初始化
- 递推公式：if (nums1[i-1] == nums2[j-1]) dp[i][j] = dp[i-1][j-1] + 1
- dp数组初始化：第一行、第一列无意义，因此dp[i][0] dp[0][j]都初始化为0，其他下标的初始化成多少都可以，因为后续会被覆盖，因此都初始化成0
- 遍历顺序：先遍历nums1还是nums2都行
```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int[][] dp = new int[nums1.length + 1][nums2.length + 1];
        int res = 0;
        for (int i = 1; i <= nums1.length; i++) {
            for (int j = 1; j <= nums2.length; j++) {
                if (nums1[i - 1] == nums2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                res = Math.max(res, dp[i][j]);
            }
        }
        return res;
    }
}

class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int[][] dp = new int[nums1.length + 1][nums2.length + 1];
        int res = 0;
        for (int i = 1; i <= nums1.length; i++) {
            for (int j = 1; j <= nums2.length; j++) {
                if (nums1[i - 1] == nums2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                if (dp[i][j] > res) res = dp[i][j];
            }
        }
        return res;
    }
}
```