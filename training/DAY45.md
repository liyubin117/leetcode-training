# day45

## 第四十五天| 动态规划 70 322 279

### 70 爬楼梯 easy
```
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
```
思路：动态规划，完全背包，排列，物品1 2，背包容量n
```java
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for (int j = 1; j <= n; j++) {
            for (int i = 1; i <= 2; i++) {
                if (j >= i) {
                    dp[j] += dp[j - i];
                }
            }
        }
        return dp[n];
    }
}
```
### 322 零钱兑换 middle
```
给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。
计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。
你可以认为每种硬币的数量是无限的。

输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```
思路：自顶向下DP
- 状态：目标金额amount
- 选择：会导致状态变化，从coins中选择
- 函数定义：凑出总金额amount，至少需求coinChange(coins, amount)枚硬币
- base case: amount==0时需要0枚，<0时不可能凑出
```java
//存在重复子问题，O(2^N) O(1)
class Solution {
    public int coinChange(int[] coins, int amount) {
        //base case
        if (amount == 0) return 0;
        if (amount < 0) return -1;

        int res = Integer.MAX_VALUE;
        for (int coin: coins) {
            int subProblem = coinChange(coins, amount - coin); //子问题的结果
            if (subProblem == -1) continue; //子问题无解则直接忽略
            res = Math.min(res, subProblem + 1); //在子问题中找最优解然后加1
        }
        return res == Integer.MAX_VALUE ? -1 : res;
    }
}
//备忘录优化 O(N*K) O(N)
class Solution {
  int[] memo; //备忘录
  public int coinChange(int[] coins, int amount) {
    memo = new int[amount + 1];
    Arrays.fill(memo, -111);
    return dp(coins, amount);
  }
  private int dp(int[] coins, int amount) {
    //base case
    if (amount == 0) return 0;
    if (amount < 0) return -1;

    if (memo[amount] != -111) return memo[amount]; //查备忘录防止重复计算

    int res = Integer.MAX_VALUE;
    for (int coin: coins) {
      int subProblem = dp(coins, amount - coin); //子问题的结果
      if (subProblem == -1) continue; //子问题无解则直接忽略
      res = Math.min(res, subProblem + 1); //在子问题中找最优解然后加1
    }
    memo[amount] = res == Integer.MAX_VALUE ? -1 : res; //更新备忘录
    return memo[amount];
  }
}
```
**思路：自底向上DP，迭代。完全背包，组合**
- dp数组定义：dp[j] 装满容量j的背包的最少元素个数
- 递推公式：dp[j] = min(dp[j], dp[j - coins[i]] + 1) 由一维数组左方推出，不放硬币i的最小元素个数+1后再与dp[i]取最小值
- dp数组初始化：dp[0] = 0，非零下标的元素初始化成amount+1或整数最大值防止影响min计算
- 遍历顺序：背包正序，因为是组合，先遍历背包或物品都行
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        for (int i = 0; i < coins.length; i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```
### 279 完全平方数 middle
```
给你一个整数 n ，返回 和为 n 的完全平方数的最少数量 。
完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```
思路：转化成动态规划完全背包问题，组合，与322零钱兑换类似。n是背包容量，物品是完全平方数1 4 9...
- dp数组定义：dp[j]装满容量为j的背包的最少元素个数
- 递推公式：dp[j] = min(dp[j], dp[j - i * i] + 1)
- dp数组初始化：dp[0] = 0, 非零下标的元素初始化成整数最大值防止影响min计算
- 遍历顺序：背包正序，组合可先遍历背包或物品都行。由于0并非可能的物品，因此从1开始遍历物品，然后是4 9...直到等于n
```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for (int i = 1; i * i <= n; i++) {
            for (int j = i * i; j <= n; j++) {
                dp[j] = Math.min(dp[j], dp[j - i * i] + 1);
            }
        }
        return dp[n];
    }
}
```