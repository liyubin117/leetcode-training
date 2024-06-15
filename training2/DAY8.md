# day8

## 344 反转字符串 easy
```java
class Solution {
    public void reverseString(char[] s) {
        for (int l = 0, r = s.length - 1; l < r; l++, r--) {
            char tmp = s[l];
            s[l] = s[r];
            s[r] = tmp;
        }
    }
}
```

## 541 反转字符串2 easy
```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] arr = s.toCharArray();
        for (int l = 0; l < s.length(); l += 2 * k) {
            int begin = l, end = Math.min(begin + k - 1, s.length() - 1);
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

## 卡码网54 替换数字
```java
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
```