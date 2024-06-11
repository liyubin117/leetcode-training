# day6

## 242 有效的字母异位词 easy
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] hash = new int[26];
        for (char c: s.toCharArray()) {
            hash[c - 'a']++;
        }
        for (char c: t.toCharArray()) {
            hash[c - 'a']--;
        }
        for (int i: hash) {
            if (i != 0) return false;
        }
        return true;
    }
}
```

## 349 两个数组的交集 easy
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> hash = new HashSet<>();
        Set<Integer> resultSet = new HashSet<>();
        for (int i: nums1) {
            hash.add(i);
        }
        for (int i: nums2) {
            if (hash.contains(i)) resultSet.add(i);
        }
        return resultSet.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

## 202 快乐数 easy
递归算出和，循环判断是否为1，若不为1先用哈希表看之前有无算出当前结果，若有说明会死循环直接跳出，若无则继续循环递归计算
```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> hash = new HashSet<>();
        int sum = calc(n);
        while (sum != 1) {
            if (!hash.add(sum)) return false;
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

## 1 两数之和 easy
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])) return new int[]{map.get(nums[i]), i};
            map.put(target - nums[i], i);
        }
        return null;
    }
}
```