# day43

## 第四十三天| 动态规划 1049 494 474

### 1049 最后一块石头的重量2 middle
```
有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。

每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

输入：stones = [2,7,4,1,8,1]
输出：1
解释：
组合 2 和 4，得到 2，所以数组转化为 [2,7,1,8,1]，
组合 7 和 8，得到 1，所以数组转化为 [2,1,1,1]，
组合 2 和 1，得到 1，所以数组转化为 [1,1,1]，
组合 1 和 1，得到 0，所以数组转化为 [1]，这就是最优值。
```
思路：把石头分成重量近似相等的两堆相撞后就是最小的可能重量。用动态规划转化成01背包，每个元素只能用一次，其值既是重量也是价值。最终dp[target]是sum/2容量的最大价值，也就是说这一堆石头最多这个重量，而因为sum/2是向下取整，因此另一堆石头重量sum - dp[target]更大，因此最终结果是sum - 2 * dp[target]
- dp数组定义：dp[j]是容量j的背包的最大价值
- 递推公式：在能放物品i时，dp[j] = max(dp[j], dp[j - weights[i]] + values[i])，因为stones[i] == weights[i] == values[i]，因此dp[j] = max(dp[j], dp[j - stones[i]] + stones[i])
- dp数组初始化：初始化成0
- 遍历顺序：物品正序，背包容量倒序
```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int stone: stones) {
            sum += stone;
        }
        int target = sum / 2;
        int[] dp = new int[target + 1];
        for (int i = 0; i < stones.length; i++) {
            for (int j = target; j >= stones[i]; j--) {
                dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
            }
        }
        return sum - 2 * dp[target];
    }
}
```
### 494 目标和 middle
```
给你一个非负整数数组 nums 和一个整数 target 。

向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式 ：

例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。
返回可以通过上述方法构造的、运算结果等于 target 的不同 表达式 的数目。

输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```
思路：用回溯会超时。加法集合add，减法集合minus，根据add + minus = sum, add - minus = target, 因此add = (target + sum) / 2，而target和sum已知，因此加法集合是一个确定的值，若无法整除2则说明凑不出target直接返回0。用动态规划转化成背包01问题，本题所要求的即装满add背包有多少种方法
- dp数组定义：dp[j]装满容量j的背包的方法数
- 递推公式：dp[j] += dp[j - nums[i]]
- dp数组初始化：dp[0] = 1，j非0的初始化成0防止影响累加操作
- 遍历顺序：物品正序，容量倒序
```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) sum += nums[i];

        //如果target的绝对值大于sum，或(target+sum)除以2的余数不为0，那么是没有方案的
        if (Math.abs(target) > sum || (target + sum) % 2 == 1) return 0;

        int bagSize = (target + sum) / 2;
        int[] dp = new int[bagSize + 1];
        dp[0] = 1;

        for (int i = 0; i < nums.length; i++) {
            for (int j = bagSize; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[bagSize];
    }
}
```
### 474 一和零 middle
```
给你一个二进制字符串数组 strs 和两个整数 m 和 n 。
请你找出并返回 strs 的最大子集的长度，该子集中 最多 有 m 个 0 和 n 个 1 。
如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
```
思路：每个元素只取1个或不取，因此是01背包，只是有两个维度：m个0和n个1，因此定义二维dp数组
- dp数组定义：dp[i][j] i个0 j个1的背包所能装的最多元素个数
- 递推公式：单维公式是dp[j] = max(dp[j], dp[j - weights[i]] + values[i])，而此题是二维的，因此dp[i][j] = max(dp[i][j], dp[i-x][j-y] + 1)
- dp数组初始化：dp[0][0] = 0, 非0下标为防止覆盖max也初始化成0
- 遍历顺序：物品正序，容量倒序
```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        //dp[i][j]表示i个0和j个1时的最大子集
        int[][] dp = new int[m + 1][n + 1];
        int oneNum, zeroNum;
        for (String str : strs) {
            oneNum = 0;
            zeroNum = 0;
            for (char ch : str.toCharArray()) {
                if (ch == '0') {
                    zeroNum++;
                } else {
                    oneNum++;
                }
            }
            //倒序遍历
            for (int i = m; i >= zeroNum; i--) {
                for (int j = n; j >= oneNum; j--) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
                }
            }
        }
        return dp[m][n];
    }
}
```