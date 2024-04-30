# day41

## 第四十一天| 动态规划 343 96

### 343 整数拆分 middle
```
给定一个正整数 n ，将其拆分为 k 个 正整数 的和（ k >= 2 ），并使这些整数的乘积最大化。
返回 你可以获得的最大乘积 。
2 <= n <= 58

输入: n = 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```
思路：
- dp数组定义：dp[i]，对i拆分后得到的最大乘积
- 递推公式：dp[i] = max(dp[i], j * (i - j), j * dp[i - j])
    - j * (i - j)将 i 拆分成两个数，再相乘
    - j * dp[i - j]是将 i 拆分成两个以上的数，再相乘
    - 由于每次是固定i然后迭代i-j相乘，因此每次迭代时dp[i]的值会变，取最大值时要考虑到之前迭代的结果
- dp数组初始化：dp[0]、dp[1]是0，dp[2]是1
- 遍历顺序：从小到大
```java
class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n+1];
        dp[2] = 1;
        for(int i = 3; i <= n; i++) {
            for(int j = 1; j <= i-j; j++) {
                dp[i] = Math.max(dp[i], Math.max(j*(i-j), j*dp[i-j]));
            }
        }
        return dp[n];
    }
}
```
### 96 不同的二叉搜索树 middle
```
给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。
1 <= n <= 19

输入：n = 3
输出：5
```
思路：题意是求出不同布局的二叉搜索树种类，节点值大小无所谓
- dp数组定义：dp[i]，i个节点的不同二叉搜索树布局数
- 递推公式：迭代j为头节点，左节点有j-1个，右节点有i-j个，因此dp[i] += dp[j - 1] * dp[i - j]
- dp数组初始化：dp[0]=1 dp[1]=1 dp[2]=2
- 遍历顺序：从小到大
```java
class Solution {
    public int numTrees(int n) {
        if (n < 2) return 1;
        if (n == 2) return 2;
        int[] dp = new int[n + 1];
        dp[0] = 1; dp[1] = 1; dp[2] = 2;
        for (int i = 3; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
}
```