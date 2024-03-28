# day8

## 第八天| 字符串 344 541 54 151 55

### 344 反转字符串 easy
```
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 s 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。
```
https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html

思路：双指针

复杂度：时间O(N) 空间O(1)
```java
class Solution {
    public void reverseString(char[] s) {
        int l = 0, r = s.length - 1;
        while (l < r) {
            char tmp = s[l];
            s[l] = s[r];
            s[r] = tmp;
            l++;
            r--;
        }
    }
}
```

### 541 反转字符串II easy
```
给定一个字符串 s 和一个整数 k，从字符串开头算起，每计数至 2k 个字符，就反转这 2k 字符中的前 k 个字符。

如果剩余字符少于 k 个，则将剩余字符全部反转。
如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。
```
https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html

思路：双指针，注意右指针的初始值和每次迭代后的边界要与字符串长度做比较

复杂度：时间O(N) 空间O(1)
```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] arr = s.toCharArray();
        for (int l = 0, r = Math.min(k, arr.length) - 1; l < r && r < arr.length;  l += 2 * k, r = Math.min(r + 2 * k, arr.length - 1)) {
            int begin = l, end = r;
            while (begin < end) {
                char tmp = arr[begin];
                arr[begin] = arr[end];
                arr[end] = tmp;
                begin++;
                end--;
            }
            
        }
        return new String(arr);
    }
}
```

### 卡码网54 替换数字
```
给定一个字符串 s，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为number。 例如，对于输入字符串 "a1b2c3"，函数应该将其转换为 "anumberbnumbercnumber"。
```
https://programmercarl.com/kama54.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html

思路：直接用String replaceAll正则匹配后替换，或者也能匹配出字符串后替换生成StringBuffer

复杂度：时间O(N) 空间O(1)
```java
//第一种
import java.util.Scanner;
public class Main {
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String str = scanner.next();
        System.out.println(convert(str));
        scanner.close();
    }
    public static String convert(String str) {
        return str.replaceAll("[0-9]", "number");
    }
}
//第二种
import java.util.Scanner;
public class Main {
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String str = scanner.next();
        System.out.println(convert(str));
        scanner.close();
    }
    public static String convert(String s) {
        char[] arr = s.toCharArray();
        StringBuffer res = new StringBuffer();
        for(int i = 0; i < s.length();i++){
            if (arr[i] - 'a' >= 0 && arr[i] - 'z' <= 0){
                res.append(arr[i]);
            }else
                res.append("number");
        }

        return res.toString();
    }
}
```

### 151 翻转字符串里的单词 middle
```
给你一个字符串 s ，请你反转字符串中 单词 的顺序。

单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的 单词 分隔开。

返回 单词 顺序颠倒且 单词 之间用单个空格连接的结果字符串。

注意：输入字符串 s中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。
```
https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html

思路：直接用trim加split处理，容易理解但效率比较低，若不用内置方法，可使用1.去除首尾以及中间多余空格 2.反转整个字符串 3.反转各个单词

```java
//用trim加split处理
class Solution {
    public String reverseWords(String s) {
        List<String> list = Arrays.asList(s.trim().split("( )+"));
        String result = "";
        for (int i = list.size() - 1; i > 0; i--) {
            result += list.get(i) + " ";
        }
        result += list.get(0);
        return result;
    }
}
//1.去除首尾以及中间多余空格 2.反转整个字符串 3.反转各个单词
class Solution {
    public String reverseWords(String s) {
        // 1.去除首尾以及中间多余空格
        StringBuffer sb = removeSpace(s);
        // 2.反转整个字符串
        reverseString(sb, 0, sb.length() - 1);
        // 3.反转各个单词
        reverseEachWord(sb);
        return sb.toString();
    }

    private StringBuffer removeSpace(String s) {
        int start = 0;
        int end = s.length() - 1;
        while (s.charAt(start) == ' ') start++;
        while (s.charAt(end) == ' ') end--;
        StringBuffer sb = new StringBuffer();
        while (start <= end) {
            char c = s.charAt(start);
            if (c != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }
            start++;
        }
        return sb;
    }

    public void reverseString(StringBuffer sb, int start, int end) {
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start++, sb.charAt(end));
            sb.setCharAt(end--, temp);
        }
    }

    private void reverseEachWord(StringBuffer sb) {
        int start = 0;
        for (int i = 0; i <= sb.length(); i++) {
            if (i == sb.length() || sb.charAt(i) == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
                reverseString(sb, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
                start = i + 1; //更新下一个单词的开始下标start
            }
        }
    }
}
```

### 55 右旋字符串 
```
字符串的右旋转操作是把字符串尾部的若干个字符转移到字符串的前面。给定一个字符串 s 和一个正整数 k，请编写一个函数，将字符串中的后面 k 个字符移到字符串的前面，实现字符串的右旋转操作。 

例如，对于输入字符串 "abcdefg" 和整数 2，函数应该将其转换为 "fgabcde"。
```
https://programmercarl.com/kama55.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html

思路：用substring

```java
import java.util.Scanner;
public class Main {
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        int k = scanner.nextInt();
        String s = scanner.next();
        System.out.println(convert(k, s));
        scanner.close();
    }
    public static String convert(int k, String s) {
        return s.substring(s.length() - k, s.length()) + s.substring(0, s.length() - k);
    }
}
```
