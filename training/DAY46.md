# day46

## 第四十六天| 动态规划 139

### 139 单词拆分 middle
```
给你一个字符串 s 和一个字符串列表 wordDict 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 s 则返回 true。
注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```
思路：转化成完全背包问题，排列。
- dp数组定义：dp[j] 字符串s的j长度的子串是否能由wordDict组成
- 递推公式：若dp[j] && wordDict.contains(s.substring(i, j))为true，则dp[j] = true
- dp数组初始化：dp[0] = true 不然后面的所有结果都是false，非零下标的元素初始化为true
- 遍历顺序：背包正序，先遍历背包
```
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int j = 1; j <= s.length(); j++) {
            for (int i = 0; i < j; i++) {
                if (dp[i] && wordDict.contains(s.substring(i, j))) {
                    dp[j] = true;
                }
            }
        }
        return dp[s.length()];
    }
}
```