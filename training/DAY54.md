# day54

## 第五十四天| 动态规划 392 115

### 392 判断子序列 easy
```
给定字符串 s 和 t ，判断 s 是否为 t 的子序列。
字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。
进阶：
如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

输入：s = "abc", t = "ahbgdc"
输出：true

输入：s = "axc", t = "ahbgdc"
输出：false
```
思路：等价于求最长公共子序列的长度，若等于s的长度，说明是子序列
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int[][] dp = new int[s.length() + 1][t.length() + 1];
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 1; j <= t.length(); j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[s.length()][t.length()] == s.length();
    }
}
```
### 115 不同的子序列 hard
```
给你两个字符串 s 和 t ，统计并返回在 s 的 子序列 中 t 出现的个数，结果需要对 109 + 7 取模。

输入：s = "babgbag", t = "bag"
输出：5
解释：
如下所示, 有 5 种可以从 s 中得到 "bag" 的方案。 
babgbag
babgbag
babgbag
babgbag
babgbag
```
思路：
- dp数组定义：dp[i][j] 以i-1下标结尾的s出现以j-1下标结尾的t的子序列个数
- 递推公式
    - 若s[i-1]==t[i-1]，说明i-1下标、j-1下标都不影响子序列个数，dp[i][j] = dp[i-1][j-1] + dp[i-1][j] _//TODO：还要多想想_
        - 考虑i-1元素来匹配，dp[i-1][j-1]
        - 不考虑i-1元素来匹配，此时s比t多退一格用来模拟删除i-1元素，为了防止负索引，只能是j加上1，dp[i-1][j]
    - 若s[i-1]!=t[i-1]，此时只能不考虑i-1元素来匹配，因此dp[i][j] = dp[i-1][j]
- dp数组初始化：由递推公式可知由左上方、正上方推出，因此第一行、第一列要初始化。dp[i][0]=1
- 遍历顺序：先遍历s再遍历t
```java
class Solution {
    public int numDistinct(String s, String t) {
        int[][] dp = new int[s.length() + 1][t.length() + 1];
        for (int i = 0; i < s.length() + 1; i++) {
            dp[i][0] = 1;
        }
        
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 1; j <= t.length(); j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                else dp[i][j] = dp[i - 1][j];
            }
        }
        
        return dp[s.length()][t.length()];
    }
}
```