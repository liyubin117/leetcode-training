# day29

## 第二十九天| 回溯 491 46 47

### 491 非递减子序列 middle
```
给你一个整数数组 nums ，找出并返回所有该数组中不同的递增子序列，递增子序列中 至少有两个元素 。你可以按 任意顺序 返回答案。
数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。
```
思路：回溯。进行树层去重，由于是子序列，不能排序后通过i > startIndex来去重，因此不能用used数组，而是通过HashSet记录树层遍历的内容。树枝可以重复，树枝递归时要剪枝比前一个元素小的
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    Deque<Integer> path = new LinkedList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        backTracking(nums, 0);
        return result;
    }
    private void backTracking(int[] nums, int startIndex){
        if(path.size() >= 2) result.add(new ArrayList<>(path));
        HashSet<Integer> hs = new HashSet<>();
        for(int i = startIndex; i < nums.length; i++){
            if(!path.isEmpty() && path.getLast() > nums[i] || hs.contains(nums[i])) //前两个判断是树枝剪枝，后一个判断是树层去重
                continue;
            hs.add(nums[i]);
            path.add(nums[i]);
            backTracking(nums, i + 1);
            path.removeLast();
        }
    }
}
```
### 46 全排列 middle
```
给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。

输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```
思路：回溯
- 终止条件：path元素数量满足要求即收集结果
- 单层递归：遍历数组，若path已添加该元素则跳过，未添加则加入后进行递归再回溯
```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    Deque<Integer> path = new LinkedList<>();
    public List<List<Integer>> permute(int[] nums) {
        backtracking(nums);
        return result;
    }
    private void backtracking(int[] nums) {
        if (path.size() == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }
        for (int num: nums) {
            if (path.contains(num)) continue;
            path.add(num);
            backtracking(nums);
            path.removeLast();
        }
    }
}
```
### 47 全排列2 middle
```
给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```
思路：回溯。排序后使用used数组区分某节点在同一树层或树支是否使用过

解法是树层去重且树枝不去重，这样效率更高。而关键点在于同样的元素在树层中要去重，树枝中不去重，而一个元素要么在树层要么在树枝用过。因此使用used布尔数组区分，false说明此时的元素在树层用过，true代表在树枝用过

去重的逻辑是i > 0 && nums[i] == nums[i - 1]，而used布尔数组用于区分某元素在树层还是在树枝用过
```
if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
    continue; //说明该元素在树层用过，直接去重，注意此时比下面的判断还多了前两个，这说明树层是要去重的
}
if (used[i] == false) //说明该元素只在树层用过，且并不需要判断树枝是否已出现该元素，可以进行树枝递归
```
```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    boolean[] used;
    public List<List<Integer>> permuteUnique(int[] nums) {
        used = new boolean[nums.length];
        Arrays.sort(nums);
        backtracking(nums);
        return result;
    }

    private void backtracking(int[] nums) {
        if (path.size() == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            // used[i - 1] == false，说明nums[i - 1]在树层上，要做去重
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            //如果同⼀树⽀nums[i]还未在树枝上，则进行树枝的递归
            if (used[i] == false) {
                used[i] = true;//标记nums[i]此时在树枝上，防止同一树枝重复使用
                path.add(nums[i]);
                backtracking(nums);
                path.removeLast();//回溯，说明同⼀树层nums[i]使⽤过，防止下一树层重复  
                used[i] = false;//回溯
            }
        }
    }
}
```