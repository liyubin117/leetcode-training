# day53

## 第五十三天| 动态规划 1143 1035 53

### 1143 最长公共子序列 middle
```
给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。
一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。
两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。

输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```
思路：因为是比较两个数组，所以用二维dp数组。与718问题类似，但是因为多了不连续的限制，所以即使i-1、j-1下标元素不相等时，也要比较text1[0,i)和text2[0,j-1)、text1[0,i-1)和text2[0,j)
- dp数组定义：dp[i][j] 以i-1下标结尾的text1和以j-1下标结尾的text2的最长公共子序列的长度
- 递推公式：text1第i-1下标元素和text2第j-1下标元素若相等则dp[i][j]=dp[i-1][j-1]+1，不相等则模拟删除当前i-1下标元素（即text1[0,i-1)和text2[0,j)）、模拟删除当前j-1下标元素（text1[0,i)和text2[0,j-1)），取较大值
- dp数组初始化：dp[i][0]表示以i-1下标结尾的text1和以-1下标结尾的text2的最长公共子序列，无意义，置0，同理dp[0][j]也是，其他下标的情况在后续运算中会覆盖掉，因此为方便都可以置0
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int[][] dp = new int[text1.length() + 1][text2.length() + 1];
        for (int i = 1; i <= text1.length(); i++) {
            for (int j = 1; j <= text2.length(); j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[text1.length()][text2.length()];
    }
}
```
### 1035 不相交的线 middle
```
在两条独立的水平线上按给定的顺序写下 nums1 和 nums2 中的整数。
现在，可以绘制一些连接两个数字 nums1[i] 和 nums2[j] 的直线，这些直线需要同时满足：
* nums1[i] == nums2[j]
* 且绘制的直线不与任何其他连线（非水平线）相交。
请注意，连线即使在端点也不能相交：每个数字只能属于一条连线。
以这种方法绘制线条，并返回可以绘制的最大连线数。

输入：nums1 = [1,4,2], nums2 = [1,2,4]
输出：2
解释：可以画出两条不交叉的线，如上图所示。 
但无法画出第三条不相交的直线，因为从 nums1[1]=4 到 nums2[2]=4 的直线将与从 nums1[2]=2 到 nums2[1]=2 的直线相交。
```
思路：等价为1143问题，求最长公共子序列，这样连的线不会相交且是最长的连接线
```java
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int[][] dp = new int[nums1.length + 1][nums2.length + 1];
        for (int i = 1; i <= nums1.length; i++) {
            for (int j = 1; j <= nums2.length; j++) {
                if (nums1[i - 1] == nums2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[nums1.length][nums2.length];
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
思路：最大子数组即最大连续子序列
- 动态规划
    - dp数组定义：dp[i] 以i下标元素为结尾的最大子数组的和
    - 递推公式：求和，要么延续即dp[i-1]+nums[i]，要么不延续即nums[i]，较大值即dp[i]
    - dp数组初始化：dp[0]=nums[0]
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        int res = nums[0];
        for (int i = 1; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
            if (dp[i] > res) res = dp[i];
        }
        return res;
    }
}
```
- 贪心法：只加正的旧sum。sum += num，当sum<0时若与num相加必然变小，因此就不加了，且题意要求连续，那么直接置sum为num，否则就相加。
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