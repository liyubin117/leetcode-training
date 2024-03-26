# day7

## 第七天| 哈希表 454 383 15 18

### 454 四数相加II middle
```
给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：
0 <= i, j, k, l < n
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
```

思路：由于哈希值不可控，使用map结构。两个数组遍历后写入hash，另外两个数组遍历后找-key并将count加上所得的map值

复杂度：时间O(N*N) 空间O(N)

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> hash = new HashMap<>();
        int count = 0;
        for (int i : nums1) {
            for (int j : nums2) {
                hash.compute(i + j, (key, value) -> value == null ? 1 : value + 1);
            }
        }
        for (int k : nums3) {
            for (int l : nums4) {
                count = count + hash.getOrDefault(-(k + l), 0);
            }
        }
        return count;
    }
}
```

### 383 赎金信 easy
```
给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。

如果可以，返回 true ；否则返回 false 。

magazine 中的每个字符只能在 ransomNote 中使用一次。
```

思路：用哈希表，但一开始想的是HashMap，其实因为题目中的字符串都是小写字母，因此哈希空间可控，可以直接用数组作为哈希表

复杂度：时间O(N) 空间O(1)

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if (ransomNote.length() > magazine.length()) return false;
        int[] nums = new int[26];
        for (Character c: ransomNote.toCharArray()) {
            nums[c - 'a']++;
        }
        for (Character c: magazine.toCharArray()) {
            nums[c - 'a']--;
        }
        for (int num: nums) {
            if (num > 0) return false;
        }
        return true;
    }
}
```

### 15 三数之和 middle
```
给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请

你返回所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。
```

思路：此题用哈希法比较复杂，还是用双指针吧。先排序，固定一个点i后双指针left/right，再移动这个点，要注意剪枝和去重，剪枝后break即可

复杂度：时间O(N^2) 空间O(1)

```java
class Solution {
        public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) break; // 排序之后如果第一个元素已经大于零，那么无论如何组合都不可能凑成三元组，直接返回结果就可以了
            if (i > 0 && nums[i] == nums[i - 1]) continue; //去重a
            int left = i + 1, right = nums.length - 1;
            while (left < right) {
                int tmp = nums[i] + nums[left] + nums[right];
                if (tmp < 0) left++;
                else if (tmp > 0) right--;
                else {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while (left < right && nums[left] == nums[left + 1]) left++; //去重b
                    while (left < right && nums[right] == nums[right - 1]) right--; //去重c
                    left++;
                    right--;
                }
            }
        }
        return result;
    }
}
```

### 18 四数之和 middle
```
给你一个由 n 个整数组成的数组 nums ，和一个目标值 target 。请你找出并返回满足下述全部条件且不重复的四元组 [nums[a], nums[b], nums[c], nums[d]] （若两个四元组元素一一对应，则认为两个四元组重复）：

0 <= a, b, c, d < n
a、b、c 和 d 互不相同
nums[a] + nums[b] + nums[c] + nums[d] == target
你可以按 任意顺序 返回答案 。
```

思路：此题用哈希法比较复杂，还是用双指针吧。在三数之和基础上多了一层for循环，注意有一级剪枝/去重，二级剪枝/去重，且不能过早return结果，剪枝后break即可

复杂度：时间O(N^3) 空间O(1)

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int k = 0; k < nums.length; k++) {
            if (nums[k] >= 0 && nums[k] > target) break; //一级剪枝：之所以还加上非负判断，是因为防止-1 -1 0 target为-2的情况而过早剪枝丢失部分结果
            if (k > 0 && nums[k] == nums[k - 1]) continue;
            for (int i = k + 1; i < nums.length; i++) {
                if (nums[k] + nums[i] >= 0 && nums[k] + nums[i] > target) break; //二级剪枝
                if (i > k + 1 && nums[i] == nums[i - 1]) continue;
                int left = i + 1, right = nums.length - 1;
                while (left < right) {
                    long sum = (long) nums[k] + nums[i] + nums[left] + nums[right];
                    if (sum > target) right--;
                    else if (sum < target) left++;
                    else {
                        result.add(Arrays.asList(nums[k], nums[i], nums[left], nums[right]));
                        while (left < right && nums[left] == nums[left + 1]) left++;
                        while (left < right && nums[right] == nums[right - 1]) right--;
                        left++;
                        right--;
                    } 
                }
            }
        }
        return result;
    }
}
```