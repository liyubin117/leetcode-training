# day6

## 第六天| 哈希表 242 349 202 1

### 242 有效的字母异位词 easy
```
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。
注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。
```
https://programmercarl.com/%E5%93%88%E5%B8%8C%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html

思路：只需要一个数组作为哈希表记录第一个字符串的字符次数，然后遍历第二个字符串进行相减，若最终数组全为0则说明有效

复杂度：时间O(N) 空间O(1)

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] arr = new int[26];
        for (Character c: s.toCharArray()) {
            arr[c - 'a']++;
        }
        for (Character c: t.toCharArray()) {
            arr[c - 'a']--;
        }
        for (int i: arr) {
            if (i != 0) return false;
        }
        return true;
    }
}
```

### 349 两个数组的交集 easy
```
给定两个数组 nums1 和 nums2 ，返回它们的交集。输出结果中的每个元素一定是 唯一 的。我们可以 不考虑输出结果的顺序 。
```
https://programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html

思路：用HashSet作为哈希表

复杂度：时间O(N) 空间O(N)

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> set2 = new HashSet<>();
        for (int i: nums1) {
            set1.add(i);
        }
        for (int j: nums2) {
            if (set1.contains(j)) set2.add(j);
        }
        return set2.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

### 202 快乐数 easy
```
编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」 定义为：
对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
- 然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
- 如果这个过程 结果为 1，那么这个数就是快乐数。
- 如果 n 是 快乐数 就返回 true ；不是，则返回 false 。
```
https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html

思路：递归算出和，循环判断是否为1，若不为1先用哈希表看之前有无算出当前结果，若有说明会死循环直接跳出，若无则继续循环递归计算

复杂度：时间O(logN) 空间O(logN) 这个还得再看看
```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();
        int sum = calc(n);
        while (sum != 1) {
            if (!set.add(sum)) return false;
            sum = calc(sum);
        }
        return true;
    }
    private int calc(int n) {
        if (n < 10) return n * n;
        return (n % 10) * (n % 10) + calc(n / 10);
    }
}
```

### 1 两数之和 easy
```
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。
```
https://programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html

思路：用一个map作为哈希表，键是数组值，值是数组索引，查找时用差值

复杂度：时间O(N) 空间O(N)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int temp = target - nums[i];
            if (map.containsKey(temp)) return new int[]{map.get(temp), i};
            map.put(nums[i], i);
        }
        return null;
    }
}
```