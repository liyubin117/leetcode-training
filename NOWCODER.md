牛客

# 华为机试
## HJ17 坐标移动 middle
```
开发一个坐标计算工具， A表示向左移动，D表示向右移动，W表示向上移动，S表示向下移动。从（0,0）点开始移动，从输入字符串里面读取一些坐标，并将最终输入结果输出到输出文件里面。

输入：

合法坐标为A(或者D或者W或者S) + 数字（两位以内）

坐标之间以;分隔。

非法坐标点需要进行丢弃。如AA10;  A1A;  $%$;  YAD; 等。
```
思路：用正则判断是否合法坐标，然后split("[ASWD]")获得一个数组，首元素是空串，然后遍历某坐标，判断动作若是ASWD合法范围内则迭代索引获取对应数组的动作数进行操作
```java
import java.util.Scanner;
import java.util.regex.Pattern;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String[] arr = in.nextLine().split(";");
        int x = 0, y = 0;
        Pattern pattern = Pattern.compile("([AWSD][0-9]{1,2})+");
        for (String str: arr) {
            int index = 0;
            if (pattern.matcher(str).matches()) {
                String[] nums = str.split("[ASWD]");
                for (char c: str.toCharArray()) {
                    if (c == 'A' || c == 'S' || c == 'W' || c == 'D') {
                        int move = Integer.valueOf(nums[++index]);
                        if (c == 'A') x -= move;
                        if (c == 'S') y -= move;
                        if (c == 'W') y += move;
                        if (c == 'D') x += move;
                    }
                }
            }
        }
        System.out.println(x + "," + y);
    }
}
```
## HJ20 密码验证合格程序 middle
```
密码要求:
1.长度超过8位
2.包括大小写字母.数字.其它符号,以上四种至少三种
3.不能有长度大于2的包含公共元素的子串重复 （注：其他符号不含空格或换行）

输入描述：
一组字符串。

输出描述：
如果符合要求输出：OK，否则输出NG
```
思路：用正则判断1、2规则，用哈希表或递归判断3规则
```java
import java.util.*;
import java.util.regex.*;
public class Main {
    public static void main(String[] arg) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String str = sc.next();
            if (str.length() <= 8) {
                System.out.println("NG");
                continue;
            }
            if (getMatch(str)) {
                System.out.println("NG");
                continue;
            }
            if (getString2(str)) {
                System.out.println("NG");
                continue;
            }
            System.out.println("OK");
        }
    }
    // 校验是否有重复子串
    //第一种：递归
    private static boolean getString(String str, int l, int r) {
        if (r >= str.length()) {
            return false;
        }
        if (str.substring(r).contains(str.substring(l, r))) {
            return true;
        } else {
            return getString(str, l + 1, r + 1);
        }
    }
    //第二种：哈希表
    private static boolean getString2(String str) {
        Set<String> set = new HashSet<>();
        for (int i = 0; i <= str.length() - 3; i++) {
            String findStr = str.substring(i, i + 3);
            boolean flag = set.add(findStr);
            if (!flag) return true;
        }
        return false;
    }
    // 检查是否满足正则
    private static boolean getMatch(String str) {
        int count = 0;
        Pattern p1 = Pattern.compile("[A-Z]");
        if (p1.matcher(str).find()) {
            count++;
        }
        Pattern p2 = Pattern.compile("[a-z]");
        if (p2.matcher(str).find()) {
            count++;
        }
        Pattern p3 = Pattern.compile("[0-9]");
        if (p3.matcher(str).find()) {
            count++;
        }
        Pattern p4 = Pattern.compile("[^a-zA-Z0-9 \n]");
        if (p4.matcher(str).find()) {
            count++;
        }
        if (count >= 3) {
            return false;
        } else {
            return true;
        }
    }
}
```