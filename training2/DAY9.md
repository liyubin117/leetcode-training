# day9

## 151 翻转字符串里的单词 middle
```java
class Solution {
    public String reverseWords(String s) {
        String[] arr = s.trim().split("( )+");
        for (int l = 0, r = arr.length - 1; l < r; l++, r--) {
            String tmp = arr[l];
            arr[l] = arr[r];
            arr[r] = tmp;
        }
        return String.join(" ", arr);
    }
}
```

## 28 找出字符串中第一个匹配项的下标 easy
KMP算法写起来没那么简单
```java
class Solution {
    //前缀表（不减一）
    public int strStr(String haystack, String needle) {
        if (needle.length() == 0) return 0;
        int[] next = new int[needle.length()];
        getNext(next, needle);

        int j = 0;
        for (int i = 0; i < haystack.length(); i++) {
            while (j > 0 && needle.charAt(j) != haystack.charAt(i)) 
                j = next[j - 1];
            if (needle.charAt(j) == haystack.charAt(i)) 
                j++;
            if (j == needle.length()) 
                return i - needle.length() + 1;
        }
        return -1;

    }
    
    private void getNext(int[] next, String s) {
        int j = 0;
        next[0] = 0;
        for (int i = 1; i < s.length(); i++) {
            while (j > 0 && s.charAt(i) != s.charAt(j)) j = next[j - 1];
            if (s.charAt(i) == s.charAt(j)) j++;
            next[i] = j;
        }
    }
}
```

