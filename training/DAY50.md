# day50

## 第五十天| 309 714

### 309 买卖股票的最佳时机含冷冻期 middle
```
给定一个整数数组prices，其中第  prices[i] 表示第 i 天的股票价格 。​
设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:
卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

输入: prices = [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```
思路：与122问题类似，只是多了卖出后第二天不能买入的限制。而122问题的第i天的状态0持股、1不持股，不持股包含当天卖出、之前卖出，当天卖出会导致冷冻期，因此要拆分，状态是0持股、1保持卖股（不持股不卖股）、2卖股、3冷冻期
- dp数组定义：dp[i][j] j的范围[0,3]
- 递推公式：
    - dp[i][0] = max(dp[i-1][0], dp[i-1][3] - prices[i], dp[i-1][1] - prices[i])
        - 保持 dp[i-1][0]
        - 买股，前一天是冷冻期 dp[i-1][3] - prices[i]
        - 买股，前一天是不持股不卖股 dp[i-1][1] - prices[i]
    - dp[i][1] = max(dp[i-1][1], dp[i-1][3])
    - dp[i][2] = dp[i-1][0] + prices[i]
    - dp[i][3] = dp[i-1][2]
- dp数组初始化：dp[0][0] = -prices[i], dp[0][1] = 0, dp[0][2] = 0, dp[i][3] = 0
- 遍历顺序：从左到右
```java
class Solution {
    public int maxProfit(int[] prices) {
        int[][] dp = new int[prices.length][4];
        dp[0][0] = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            dp[i][0] = Math.max(Math.max(dp[i-1][0], dp[i-1][3] - prices[i]), dp[i-1][1] - prices[i]);
            dp[i][1] = Math.max(dp[i-1][1], dp[i-1][3]);
            dp[i][2] = dp[i-1][0] + prices[i];
            dp[i][3] = dp[i - 1][2];
        }
        return Math.max(Math.max(dp[prices.length - 1][1], dp[prices.length - 1][2]), dp[prices.length - 1][3]);
    }
}
```
### 714 买卖股票的最佳时机含手续费 middle
```
给定一个整数数组 prices，其中 prices[i]表示第 i 天的股票价格 ；整数 fee 代表了交易股票的手续费用。
你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。
返回获得利润的最大值。
注意：这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

输入：prices = [1, 3, 2, 8, 4, 9], fee = 2
输出：8
解释：能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8
```
思路：与122问题类似，只是多了卖出时收手续费的限制。
- dp数组定义：dp[i][0] dp[i][1]分别表示第i天持股、不持股的最大金额
- 递推公式：
    - 第i天持有：dp[i][0] = max(dp[i-1][0], dp[i-1][1]-prices[i])
    - 第i天不持有：dp[i][1] = max(dp[i-1][1], dp[i-1][0]+prices[i]-fee)
- dp数组初始化：dp[0][0]=-prices[0]
- 遍历顺序：从左到右
```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int[][] dp = new int[prices.length][2];
        dp[0][0] = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee);
        }
        return dp[prices.length - 1][1];
    }
}
```