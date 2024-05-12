# day49

## 第四十九天| 123 188

### 123 买卖股票的最佳时机3 hard
```
给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

输入：prices = [3,3,5,0,0,3,1,4]
输出：6
解释：在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
```
思路：动态规划
- dp数组定义：dp[i][0] dp[i][1] dp[i][2] dp[i][3] dp[i][4]分别表示第i天不操作、第1次持有、第1次不持有、第2次持有、第2次不持有的最大金额
- 递推公式
    - dp[i][0] = dp[i-1][0] 因为不操作所以一直是0
    - dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]) 若是买入导致的第一次持有，则说明前一天是不操作的，即前一天不操作的金额-当天股票价格，也可以简写成-prices[i]
    - dp[i][2] = max(dp[i-1][2], dp[i-1][1] + prices[i]) 若是卖出导致的第一次不持有，则说明前一天是第一次持有的，即前一天持有的金额+当天股票价格
    - dp[i][3] = max(dp[i-1][3], dp[i-1][2] - prices[i]) 若是买入导致的第二次持有，则说明前一天是第一次不持有的
    - dp[i][4] = max(dp[i-1][4], dp[i-1][3] + prices[i]) 若是卖出导致的第二次不持有，则说明前一天是第二次持有的
- dp数组初始化
    - dp[0][0] = 0
    - dp[0][1] = -prices[0]
    - dp[0][2] = 0 同一天买入且卖出
    - dp[0][3] = -prices[0]
    - dp[0][4] = 0 同一天第二次买入且卖出
- 遍历顺序：从左到右
```java
class Solution {
    public int maxProfit(int[] prices) {
        int[][] dp = new int[prices.length][5];
        dp[0][1] = -prices[0];
        dp[0][3] = -prices[0];

        for (int i = 1; i < prices.length; i++) {
            dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] - prices[i]);
            dp[i][2] = Math.max(dp[i-1][2], dp[i-1][1] + prices[i]);
            dp[i][3] = Math.max(dp[i-1][3], dp[i-1][2] - prices[i]);
            dp[i][4] = Math.max(dp[i-1][4], dp[i-1][3] + prices[i]);
        }
        return dp[prices.length - 1][4];
    }
}
```
### 188 买卖股票的最佳时机4 hard
```
给你一个整数数组 prices 和一个整数 k ，其中 prices[i] 是某支给定的股票在第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。也就是说，你最多可以买 k 次，卖 k 次。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```
思路：动态规划，123问题是本题的特例，即k=2的情况。
- dp数组定义：dp[i][j] j的范围是[0,2*k]，j单数是持有，偶数是不持有
- 递推公式：参考123问题
- dp数组初始化：i为0，j单数下标的元素初始化为-prices[0]
- 遍历顺序：从左到右
```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int[][] dp = new int[prices.length][2 * k + 1];
        for (int j = 0; j <= 2 * k - 2; j += 2) {
            dp[0][j + 1] = -prices[0];
        }

        for (int i = 1; i < prices.length; i++) {
            for (int j = 0; j <= 2 * k - 2; j += 2) {
                dp[i][j + 1] = Math.max(dp[i-1][j + 1], dp[i-1][j] - prices[i]);
                dp[i][j + 2] = Math.max(dp[i-1][j + 2], dp[i-1][j + 1] + prices[i]);
            }
        }
        return dp[prices.length - 1][2 * k];
    }
}
```