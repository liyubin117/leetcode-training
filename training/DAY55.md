# day55

## 第五十五天| 动态规划 583 72

### 583 两个字符串的删除操作 middle
```
给定两个单词 word1 和 word2 ，返回使得 word1 和  word2 相同所需的最小步数。
每步 可以删除任意一个字符串中的一个字符。

输入: word1 = "sea", word2 = "eat"
输出: 2
解释: 第一步将 "sea" 变为 "ea" ，第二步将 "eat "变为 "ea"
```
思路：
- 转化成两字符串长度和-2*最长公共子序列长度，调用1143问题的解法
```java
class Solution {
    public int minDistance(String text1, String text2) {
        int[][] dp = new int[text1.length() + 1][text2.length() + 1];
        for (int i = 1; i <= text1.length(); i++) {
            for (int j = 1; j <= text2.length(); j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return text1.length() + text2.length() - 2 * dp[text1.length()][text2.length()];
    }
}
```
- 与115问题相比，两字符串都可删，较复杂，但可作为解72编辑距离问题的基础
    - dp数组定义：dp[i][j] 以i-1下标结尾的word1、以j-1下标结尾的word2各自都可删除后相同所需的最小步数
    - 递推公式：
        - 若word1[i-1]==word2[j-1]，i-1下标、j-1下标都不影响最小步数，不考虑这些下标且不需要加1步数，dp[i][j]=dp[i-1][j-1]
        - 若word1[i-1]!=word2[j-1]，dp[i][j]=min(dp[i-1][j]+1, dp[i][j-1]+1)
            - 模拟删除word1：dp[i-1][j]+1
            - 模拟删除word2：dp[i][j-1]+1
            - 模拟删除word1、word2，与上面两种情况有重叠：dp[i-1][j-1]+2
    - dp数组初始化：第一行是dp[0][j]，此时word1是空串，而此时对应的word2是j-1下标为结尾，因此最小删除步数是j。第一列是dp[i][0]，同理是i
    - 遍历顺序：先遍历word1或word2都行
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int[][] dp = new int[word1.length() + 1][word2.length() + 1];
        for (int i = 0; i < word1.length() + 1; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j < word2.length() + 1; j++) {
            dp[0][j] = j;
        }
        
        for (int i = 1; i <= word1.length(); i++) {
            for (int j = 1; j <= word2.length(); j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1);
            }
        }
        
        return dp[word1.length()][word2.length()];
    }
}
```
### 72 编辑距离 middle
```
给你两个单词 word1 和 word2， 请返回将 word1 转换成 word2 所使用的最少操作数  。
你可以对一个单词进行如下三种操作：
* 插入一个字符
* 删除一个字符
* 替换一个字符

输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (word1 删除 'e') 或者 ros -> rose (word2 增加 'e')
```
思路：对一个单词的操作相当于另一个单词的逆操作，因此可对两个单词同时操作，最终的最少操作数是一样的，与538问题类似，只是多了增、改的操作类型
- dp数组定义：dp[i][j] 以i-1下标结尾的word1、以j-1下标结尾的word2各自都可增删改后相同所需的最小步数
- 递推公式：
    - 若word1[i-1]==word2[j-1]，i-1下标、j-1下标都不影响最小步数，不考虑这些下标且不需要加1步数，dp[i][j]=dp[i-1][j-1]
    - 若word1[i-1]!=word2[j-1]，dp[i][j]=min(dp[i-1][j]+1, dp[i][j-1]+1)
        - 模拟删除word1：dp[i-1][j]+1
        - 模拟删除word2（相当于模拟增加word1）：dp[i][j-1]+1
        - 替换word1或word2：dp[i-1][j-1]+1
- dp数组初始化：第一行是dp[0][j]，此时word1是空串，而此时对应的word2是j-1下标为结尾，因此最小操作也即最小删除步数是j。第一列是dp[i][0]，同理是i
- 遍历顺序：先遍历word1或word2都行
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int[][] dp = new int[word1.length() + 1][word2.length() + 1];
        for (int i = 0; i < word1.length() + 1; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j < word2.length() + 1; j++) {
            dp[0][j] = j;
        }
        
        for (int i = 1; i <= word1.length(); i++) {
            for (int j = 1; j <= word2.length(); j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = Math.min(Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1), dp[i - 1][j - 1] + 1);
            }
        }
        
        return dp[word1.length()][word2.length()];
    }
}
```