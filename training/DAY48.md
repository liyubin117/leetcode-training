# day48

## 第四十八天| 动态规划 121 122

### 121 买卖股票的最佳时机 easy
```
给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。
你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。
返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。
```
思路：
- 贪心法：只需要遍历价格数组一遍，记录历史最低点，然后在每一天考虑这么一个问题：如果我是在历史最低点买进的，那么我今天卖出能赚多少钱？当考虑完所有天数之时，我们就得到了最好的答案
```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = prices[0];
        int maxProfit = 0;
        for (int price: prices) {
            minPrice = Math.min(minPrice, price);
            maxProfit = Math.max(maxProfit, price - minPrice);
        }
        return maxProfit;
    }
}
```
- 动态规划：更通用
    - dp数组定义：dp[i][0] dp[i][1]分别代表第i天持有、不持有该股票的最大金额，定初始是0，此时最大金额相当于是最大利润
    - 递推公式：
        - 在第i天持有 dp[i][0] = max(dp[i-1][0], -prices[i])
            - 当天未买，之前买的 dp[i-1][0]
            - 当天买 -prices[i] 由于只能买卖一次，因此买之前手上的现金必是0
        - 第i天不持有 dp[i][1] = max(dp[i-1][1], dp[i-1][0] + prices[i])
            - 当天未卖，之前卖的 dp[i-1][1]
            - 当天卖 dp[i-1][0] + prices[i]
    - dp数组初始化：dp[0][0]=-prices[0], dp[0][1]=0
    - 遍历顺序：从左往右
```java
class Solution {
    public int maxProfit(int[] prices) {
        int[][] dp = new int[prices.length][2];
        dp[0][0] = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], -prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
        }
        return dp[prices.length - 1][1];
    }
}
```
### 122 买卖股票的最佳时机2 middle
```
给你一个整数数组 prices ，其中 prices[i] 表示某支股票第 i 天的价格。
在每一天，你可以决定是否购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。你也可以先购买，然后在 同一天 出售。
返回 你能获得的 最大 利润 。

输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3 。
     总利润为 4 + 3 = 7 。
```
思路：相比于121问题，可以买卖多次。买卖多次和买卖一次，不持有股票的递推公式是一样的，而持有股票的不同，第i天买了股票造成了持有，因为可以买卖多次，买股票之前手上的钱不一定是0，而是dp[i-1][1]，因此这种情况下dp[i-1][1] - prices[i]
```java
class Solution {
    public int maxProfit(int[] prices) {
        int[][] dp = new int[prices.length][2];
        dp[0][0] = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
        }
        return dp[prices.length - 1][1];
    }
}
```