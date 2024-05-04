# day38

## 第三十八天| 动态规划 509 70 746

### 509 斐波那契数 easy
```
斐波那契数 （通常用 F(n) 表示）形成的序列称为 斐波那契数列 。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：

F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
给定 n ，请计算 F(n) 。
```
思路：递归或动态规划
- 确定dp[i]含义：dp[i]是第i个斐波那契数
- 递推公式：dp[i] = dp[i - 1] + dp[i - 2]
- dp数组初始化：dp[0] = 0, dp[1] = 1
- 遍历顺序：从前向后
```java
//暴力递归，O(2^N)，对本例来说效率低，存在重复计算
class Solution {
  public int fib(int n) {
    if (n == 0 || n == 1) return n;
    return fib(n - 1) + fib(n - 2);
  }
}
//自顶向下DP，备忘录优化，消除重叠子问题 时间复杂度O(N) 空间复杂度O(N)
class Solution {
    public int fib(int n) {
        int[] dp = new int[n + 1]; //备忘录
        return func(dp, n);
    }
    private int func(int[] dp, int N) {
        if (N == 0 || N == 1) return N; //base case
        if (dp[N] != 0) return dp[N]; //若备忘录已经计算过则直接返回不重复计算
        dp[N] = func(dp, N - 1) + func(dp, N - 2);
        return dp[N];
    }
}
//自底向上DP 时间复杂度O(N) 空间复杂度O(N)
class Solution {
    public int fib(int n) {
        if (n == 0 || n == 1) return n;
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}
//优化后的自底向上DP 时间复杂度O(N) 空间复杂度O(1)
class Solution {
    public int fib(int n) {
        if (n == 0 || n == 1) return n;
        int pre = 0, cur = 1, newCur;
        for (int i = 2; i <= n; i++) {
            newCur = pre + cur;
            pre = cur;
            cur = newCur;
        }
        return cur;
    }
}
```
### 70 爬楼梯 easy
```
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
```
思路：考虑最后一步可能跨了一级台阶，也可能跨了两级台阶，因此只要得到这两种情况的爬楼梯方法，加起来就是当前台阶数的。
- 动态规划
- 递归，会超时
```java
//动态规划
class Solution {
  public int climbStairs(int n) {
    int p = 0, q = 0, r = 1;
    for (int i = 1; i <= n; ++i) {
      p = q;
      q = r;
      r = p + q;
    }
    return r;
  }
}
//递归
class Solution {
  public int climbStairs(int n) {
    if (n <= 2) return n;
    return climbStairs(n - 1) + climbStairs(n - 2);
  }
}
```
### 746 使用最小花费爬楼梯 easy
```
给你一个整数数组 cost ，其中 cost[i] 是从楼梯第 i 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。
你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。
请你计算并返回达到楼梯顶部的最低花费。

输入：cost = [10,15,20]
输出：15
解释：你将从下标为 1 的台阶开始。
- 支付 15 ，向上爬两个台阶，到达楼梯顶部。
总花费为 15 。
```
思路：
- dp[i]定义：i是到第几级台阶，dp[i]是对应的最小消耗，比如一个高度3的台阶，到达楼顶的最小消耗是dp[3]，相应地dp数组长度是4
- 递推公式：dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2])
- dp数组初始化：题意是第一步不花费，往上跳才开始花费当前台阶往上跳所用的费用，而一开始可以站在0或1，此时并不消耗，因此都要初始化为0
- 遍历顺序：从前往后
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int len = cost.length;
        int[] dp = new int[len + 1];

        // 从下标为 0 或下标为 1 的台阶开始，因此支付费用为0
        dp[0] = 0;
        dp[1] = 0;

        // 计算到达每一层台阶的最小费用
        for (int i = 2; i <= len; i++) {
            dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }

        return dp[len];
    }
}
```