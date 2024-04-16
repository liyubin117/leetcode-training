# day28

## 第二十八天| 回溯法 93 78 90

### 93 复原IP地址 middle
```
有效 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 有效 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效 IP 地址。
给定一个只包含数字的字符串 s ，用以表示一个 IP 地址，返回所有可能的有效 IP 地址，这些地址可以通过在 s 中插入 '.' 来形成。你 不能 重新排序或删除 s 中的任何数字。你可以按 任何 顺序返回答案。
```
思路：number表示切割的段数，也是回溯递归的深度。startIndex是每一轮递归开始时的切割起始点，i向右切割，切割出某一段的区间是[startIndex, i]，ip分段的长度是i - startIndex + 1。判断有序ip的条件是：ip分段等于4且此时切割起始点已经指向字串长度，结合剪枝(ip段的长度最大是3，并且ip段处于[0,255])
```
class Solution {
    List<String> result = new ArrayList<String>();

    public List<String> restoreIpAddresses(String s) {
        backtracking(s, 0, 0, "");
        return result;
    }

    // number表示ip段的数量
    public void backtracking(String s, int startIndex, int number, String str) {
        // 如果startIndex等于s的长度并且ip段的数量是4，则加入结果集，并返回
        if (startIndex == s.length() && number == 4) {
            result.add(str.toString());
            return;
        }
        // 如果startIndex等于s的长度但是ip段的数量不为4，或者ip段的数量为4但是startIndex小于s的长度，则直接返回
        if (startIndex == s.length() || number == 4) {
            return;
        }
        // 剪枝：ip段的长度最大是3，并且ip段处于[0,255]
        for (int i = startIndex; 
                i < s.length() && i - startIndex + 1 <= 3
                && Integer.parseInt(s.substring(startIndex, i + 1)) >= 0
                && Integer.parseInt(s.substring(startIndex, i + 1)) <= 255; 
                i++) {
            // 如果ip段的长度大于1，并且第一位为0的话，continue
            if (i - startIndex + 1 > 1 && s.charAt(startIndex) - '0' == 0) {
                continue;
            }
            // 当此时网段数量小于3时，才会加点；如果等于3，说明已经有3段了，最后一段不需要再加点
            String ipPart = number < 3 ? s.substring(startIndex, i + 1) + "." : s.substring(startIndex, i + 1);
            backtracking(s, i + 1, number + 1, str + ipPart);
        }
    }
}
```

### 78 子集 middle
```
给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。
```
思路：
- 基于77组合的解法，调整终止条件即可，每次递归都是一个合理的子集。由于递归指定了深度最多是nums.length+1，所以不需要指定终止条件。此法效率最高
```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> subsets(int[] nums) {
        backtracking(nums, 0);
        return result;
    }
    private void backtracking(int[] nums, int startIndex) {
        result.add(new ArrayList<>(path));
        for (int i = startIndex; i < nums.length; i++) {
            path.add(nums[i]);
            backtracking(nums, i + 1);
            path.removeLast();
        }

    }
}
```
- 基于77组合的解法，子集相当于从数组中找出0、1、2...length个组合，时间复杂度高
```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> subsets(int[] nums) {
        for (int i = 0; i <= nums.length; i++) {
            backtracking(nums, i, 0);
        }
        return result;
    }
    private void backtracking(int[] nums, int k, int startIndex) {
        if (path.size() == k) {
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = startIndex; i < nums.length - (k - path.size()) + 1; i++) {
            path.add(nums[i]);
            backtracking(nums, k, i + 1);
            path.removeLast();
        }
    }
}
```

### 90 子集2 middle
```
给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。

输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```
思路：在78的基础上做树层去重，有两种方法
- 不使用used数组，递归的时候下一个startIndex是i+1而不是0。但如果要是全排列的话，每次要从0开始遍历，为了跳过已入栈的元素，此时必须使用used
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        subsetsWithDupHelper(nums, 0);
        return res;
    }

    private void subsetsWithDupHelper(int[] nums, int start) {
        res.add(new ArrayList<>(path));
        for (int i = start; i < nums.length; i++) {
            // 跳过当前树层使用过的、相同的元素
            if (i > start && nums[i - 1] == nums[i]) {
                continue;
            }
            path.add(nums[i]);
            subsetsWithDupHelper(nums, i + 1);
            path.removeLast();
        }
    }

}
```
- 使用used数组
```java
class Solution {
   List<List<Integer>> result = new ArrayList<>();// 存放符合条件结果的集合
   LinkedList<Integer> path = new LinkedList<>();// 用来存放符合条件结果
   boolean[] used;
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        if (nums.length == 0){
            result.add(path);
            return result;
        }
        Arrays.sort(nums);
        used = new boolean[nums.length];
        subsetsWithDupHelper(nums, 0);
        return result;
    }
    
    private void subsetsWithDupHelper(int[] nums, int startIndex){
        result.add(new ArrayList<>(path));
        if (startIndex >= nums.length){
            return;
        }
        for (int i = startIndex; i < nums.length; i++){
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]){
                continue;
            }
            path.add(nums[i]);
            used[i] = true;
            subsetsWithDupHelper(nums, i + 1);
            path.removeLast();
            used[i] = false;
        }
    }
}
```
