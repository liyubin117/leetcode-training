# day44

## 第四十四天| 动态规划 518 377

### 518 零钱兑换2 middle
```
给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。
请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。
假设每一种面额的硬币有无限个。 
题目数据保证结果符合 32 位带符号整数。
```
思路：纯完全背包，组合
- dp数组定义：dp[j] 装满容量j的背包的最多方法
- 递推公式：与01背包494目标和问题相同，dp[j] += dp[j - coins[i]]
- dp数组初始化：dp[0] = 1
- 遍历顺序：背包正序，因为是组合，先遍历背包或物品都行
```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int i = 0; i < coins.length; i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
}
```
### 377 组合总和4 middle
```
给你一个由 不同 整数组成的数组 nums ，和一个目标整数 target 。请你从 nums 中找出并返回总和为 target 的元素组合的个数。
题目数据保证答案符合 32 位整数范围。

输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
```
思路：完全背包，排列
- dp数组定义：dp[j] 装满容量j的背包的最多方法
- 递推公式：与01背包494目标和问题相同，dp[j] += dp[j - coins[i]]
- dp数组初始化：dp[0] = 1
- 遍历顺序：背包正序，因为是排列，只能先遍历背包
```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int j = 1; j <= target; j++) {
            for (int i = 0; i < nums.length; i++) {
                if (j >= nums[i]) {
                    dp[j] += dp[j - nums[i]];
                }
            }
        }
        return dp[target];
    }
}
```