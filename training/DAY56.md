# day56

## 第五十六天| 动态规划 647 516

### 647 回文子串 middle
```
给你一个字符串 s ，请你统计并返回这个字符串中 回文子串 的数目。
回文字符串 是正着读和倒过来读一样的字符串。
子字符串 是字符串中的由连续字符组成的一个序列。

输入：s = "abc"
输出：3
解释：三个回文子串: "a", "b", "c"

输入：s = "aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```
思路：
- 动态规划
    - dp数组定义：布尔数组dp[i][j] s[i,j]子串是否回文
    - 递推公式：
        - 若s[i]==s[j]
            - 若j-i<=1 说明[i,j]是一个1元素或2元素的子串，且两端元素一样，必然是回文，dp[i][j]=true, count++
            - 若j-i>1 说明[i,j]是一个2以上元素的子串，若dp[i+1][j-1]==true则dp[i][j]=true, count++
    - dp数组初始化：全初始化成false
    - 遍历顺序：根据状态图可知由左下角推得，从下往上，从左往右
```java
class Solution {
    public int countSubstrings(String s) {
        int len = s.length();
        boolean[][] dp = new boolean[len][len];
        
        int res = 0;
        for (int i = len - 1; i >= 0; i--) {
            for (int j = i; j < len; j++) {
                if (s.charAt(i) == s.charAt(j) && (j - i <= 1 || dp[i + 1][j - 1])) {
                    res++;
                    dp[i][j] = true;
                }
            }
        }
        return res;
    }
}
```
- 双指针法，具体是中心扩散法，找中心然后向两边扩散看是不是对称的就可以了，一个元素可以作为中心点，两个元素也可以作为中心点。`TODO:待理解` 时间复杂度也是O(N^2)，空间复杂度低O(1)
```java
class Solution {
    public int countSubstrings(String s) {
        int len, ans = 0;
        if (s == null || (len = s.length()) == 0) return 0;
        //总共有2 * len - 1个中心点
        for (int i = 0; i < 2 * len - 1; i++) {
            //通过遍历每个回文中心，向两边扩散，并判断是否回文字串
            //有两种情况，left == right，right = left + 1，这两种回文中心是不一样的
            int left = i / 2, right = left + i % 2;
            while (left >= 0 && right < len && s.charAt(left) == s.charAt(right)) {
                //如果当前是一个回文串，则记录数量
                ans++;
                left--;
                right++;
            }
        }
        return ans;
    }
}
```
### 516 最长回文子序列 middle
```
给你一个字符串 s ，找出其中最长的回文子序列，并返回该序列的长度。
子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。

输入：s = "bbbab"
输出：4
解释：一个可能的最长回文子序列为 "bbbb" 。
```
思路：
- dp数组定义：dp[i][j] `s[i,j]范围内，以i开头j结尾的最长回文子序列长度`
- 递推公式：
    - 若s[i]==s[j]，则dp[i][j]=dp[i+1][j-1]+2
    - 若s[i]!=s[j]，考虑i则是dp[i][j-1]，考虑j则是dp[i+1][j]，取较大值即dp[i][j]=max(dp[i][j-1], dp[i+1][j])
- dp数组初始化：当i和j相同时dp[i][j]只有一个字符因此应该是1，其他情况是0
- 遍历顺序：由状态图知从左、左下、下推出，因此从下往上、从左到右遍历
```java
public class Solution {
    public int longestPalindromeSubseq(String s) {
        int len = s.length();
        int[][] dp = new int[len + 1][len + 1];
        for (int i = len - 1; i >= 0; i--) { // 从后往前遍历 保证情况不漏
            dp[i][i] = 1; // 初始化
            for (int j = i + 1; j < len; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    dp[i][j] = Math.max(dp[i + 1][j], Math.max(dp[i][j], dp[i][j - 1]));
                }
            }
        }
        return dp[0][len - 1];
    }
}
```