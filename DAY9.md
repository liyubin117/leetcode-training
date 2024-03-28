# day8

## 第八天| 字符串 28 49

### 28 找出字符串中第一个匹配项的下标 easy
```
给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串的第一个匹配项的下标（下标从 0 开始）。如果 needle 不是 haystack 的一部分，则返回  -1 。
```
https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html

https://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924907&idx=1&sn=6f6fc1475be2d7d2ca5ab6e0ec755bca&chksm=ff22fa26c8557330a906f6ed9f444d71064a590109b093d8e97f0ab1cd82e5106a5138e8aecd&scene=21#wechat_redirect

https://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475925022&idx=1&sn=c115447f8ac5ef02793ec28918f7e032&chksm=ff22f993c85570856c237165a93340c76f7322f7538d16a7c15782f7cf8cd7375887e8db329f&token=290396534&lang=zh_CN#rd

思路：内置函数indexOf()、暴力法或KMP

复杂度：时间O(M+N) 空间O(M)
```java
//内置函数indexOf()
class Solution {
    public int strStr(String haystack, String needle) {
        return haystack.indexOf(needle);
    }
}
//暴力法+内置函数startsWith()
class Solution {
    public int strStr(String haystack, String needle) {
        int result = 0;
        while (result < haystack.length()) {
            if (haystack.substring(result).startsWith(needle)) return result;
            result++;
        }
        return -1;
    }
}
//KMP
class Solution {
    public void getNext(int[] next, String s){
        int j = -1;
        next[0] = j;
        for (int i = 1; i < s.length(); i++){
            while(j >= 0 && s.charAt(i) != s.charAt(j+1)){
                j=next[j];
            }

            if(s.charAt(i) == s.charAt(j+1)){
                j++;
            }
            next[i] = j;
        }
    }
    public int strStr(String haystack, String needle) {
        if(needle.length()==0){
            return 0;
        }

        int[] next = new int[needle.length()];
        getNext(next, needle);
        int j = -1;
        for(int i = 0; i < haystack.length(); i++){
            while(j>=0 && haystack.charAt(i) != needle.charAt(j+1)){
                j = next[j];
            }
            if(haystack.charAt(i) == needle.charAt(j+1)){
                j++;
            }
            if(j == needle.length()-1){
                return (i-needle.length()+1);
            }
        }

        return -1;
    }
}
```

### 459 重复的子字符串 easy
```
给定一个非空的字符串 s ，检查是否可以通过由它的一个子串重复多次构成。
```
https://programmercarl.com/%E5%8F%8C%E6%8C%87%E9%92%88%E6%80%BB%E7%BB%93.html

思路：两个字符串连在一起掐头去尾若能找到此字符串则符合要求。也可以用KMP做

```java
//推导出特性进行验证
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return (s + s).indexOf(s, 1) != s.length();
    }
}
//KMP，len(s) % (len(s) -  maxLen) = 0，len(s) 为字符串 s 的长度，maxLen 为最长公共前后缀的长度，含义是如果 s 是周期串，那【s 的长度】是【s 的长度减去最长公共前后缀的长度】的倍数，那字符串 s 就是周期串。
class Solution {
    public int[] getNext(String s){
        int i = 0, j = -1, n = s.length();
        int[] next = new int[n];
        next[0] = -1;

        while(i < n - 1){
            if(j == -1 || s.charAt(i) == s.charAt(j)){
                i++;
                j++;
                next[i] = j;
            } else j = next[j];
        }
        return next;
    }

    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();
        if (n == 0) return false;
        int[] next = getNext(s);
        int maxLen = next[n - 1] + 1;
        if(maxLen == 0 || s.charAt(n - 1) != s.charAt(n - 1 - maxLen)) return false;
        return n % (n - maxLen) == 0;
    }
}
```