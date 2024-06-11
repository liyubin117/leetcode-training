# day7

## 454 四数相加2 middle
两数相加后构建哈希表，值是次数，然后另外两个相加后，map取负值累加次数
```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int result = 0;
        Map<Integer, Integer> hash = new HashMap<>();
        for (int i: nums1) {
            for (int j: nums2) {
                hash.compute(i + j, (k, v) -> v == null ? 1 : v + 1);
            }
        }
        for (int i: nums3) {
            for (int j: nums4) {
                result += hash.getOrDefault(- (i + j), 0);
            }
        }
        return result;
    }
}
```

## 383 赎罪信 easy
```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] hash = new int[26];
        for (char c: magazine.toCharArray()) {
            hash[c - 'a']++;
        }
        for (char c: ransomNote.toCharArray()) {
            if (hash[c - 'a']-- <= 0) return false;
        }
        return true;
    }
}
```

## 15 三数之和 middle
先排序，然后循环固定一个数，双指针判断，还要去重和剪枝
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 2; i++) {
            if (nums[i] > 0) break;
            if (i > 0 && nums[i - 1] == nums[i]) continue;
            int j = i + 1, k = nums.length - 1;
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == 0) {
                    result.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    while (j < k && nums[k - 1] == nums[k]) k--;
                    while (j < k && nums[j + 1] == nums[j]) j++;
                    k--;
                    j++;
                }
                else if (sum > 0) k--;
                else j++;
            }
        }
        return result;
    }
}
```

## 18 四数之和 middle
与三数之和类似，多一层循环
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 3; i++) {
            if (nums[i] > 0 && nums[i] > target) break;
            if (i > 0 && nums[i - 1] == nums[i]) continue;
            for (int j = i + 1; j < nums.length - 2; j++) {
                if (nums[i] + nums[j] > 0 && nums[i] + nums[j] > target) break;
                if (j > i + 1 && nums[j - 1] == nums[j]) continue;
                int k = j + 1, l = nums.length - 1;
                while (k < l) {
                    int sum = nums[i] + nums[j] + nums[k] + nums[l];
                    if (sum == target) {
                        result.add(Arrays.asList(nums[i], nums[j], nums[k], nums[l]));
                        while (k < l && nums[l - 1] == nums[l]) l--;
                        while (k < l && nums[k + 1] == nums[k]) k++;
                        l--;
                        k++;
                    }
                    else if (sum > target) l--;
                    else k++;
                }
            }
        }
        return result;
    }
}
```