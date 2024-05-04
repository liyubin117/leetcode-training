# day27

## 第二十七天| 回溯法 39 40 131

### 39 组合总和 middle
```
给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 candidates 中可以使数字和为目标数 target 的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。

candidates 中的 同一个 数字可以 无限制重复被选取 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 target 的不同组合数少于 150 个。
```
思路：先对candidates排序，这样可以提前剪枝防止不必要的回溯法的宽度增加。可以用target-candidates[i]作为新的target传给下一轮递归，这样可以避免sum变量的定义
- 终止条件：target == 0说明此时的和已经满足要求，收集结果
- 单层递归：对candidates从startIndex索引处开始遍历，先剪枝：若此时target-candidates[i]已小于0直接return，然后更新path，进行回溯递归，撤销更新path
```java
class Solution {
    List<List<Integer>> result = new ArrayList<>(); // 结果列表（子集列表）
    List<Integer> path = new ArrayList<>(); // 状态（子集）
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates); // 对 candidates 进行排序
        backtrack(target, candidates, 0);
        return result;
    }
    void backtrack(int target, int[] candidates, int startIndex) {
        // 子集和等于 target 时，记录解
        if (target == 0) {
            result.add(new ArrayList<>(path));
            return;
        }
        // 剪枝二：从 startIndex 开始遍历，避免生成重复子集
        for (int i = startIndex; i < candidates.length; i++) {
            // 剪枝一：若子集和超过 target ，则直接结束循环。这是因为数组已排序，后边元素更大，子集和一定超过 target
            if (target - candidates[i] < 0) return;
            path.add(candidates[i]);
            backtrack(target - candidates[i], candidates, i);
            path.remove(path.size() - 1);
        }
    }
}
```

### 40 组合总和2 middle
```
给定一个候选人编号的集合 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用 一次 。

注意：解集不能包含重复的组合。 
```
思路：与39的区别是candidates中存在重复元素！解决方法是排序后将相邻的元素放在一块，然后对树层去重。如[1,1,2],target=3.回溯往更深层递归时，重复的元素是可以的，而增加宽度时，由于之前的递归已经产生了1,2，此时若再选择第二个1，必然会产生与之前重复的结果，所以在一开始就树层剪枝去重
```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates); // 对 candidates 进行排序
        backtracking(candidates, target, 0);
        return result;
    }
    void backtracking(int[] candidates, int target, int startIndex) {
        // 子集和等于 target 时，记录解
        if (target == 0) {
            result.add(new ArrayList<>(path));
            return;
        }
        // 剪枝一：从 startIndex 开始遍历，避免生成重复子集。从 startIndex 开始遍历，避免重复选择同一元素
        for (int i = startIndex; i < candidates.length; i++) {
            // 剪枝二：若子集和超过 target ，则直接结束循环。这是因为数组已排序，后边元素更大，子集和一定超过 target
            if (target - candidates[i] < 0) {
                break;
            }
            // 剪枝三：如果该元素与左边元素相等，说明该搜索分支重复，直接跳过
            if (i > startIndex && candidates[i] == candidates[i - 1]) {
                continue;
            }
            path.add(candidates[i]);
            backtracking(candidates, target - candidates[i], i + 1);
            path.remove(path.size() - 1);
        }
    }
}
```

### 131 分割回文串 middle
```
给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是回文串。返回 s 所有可能的分割方案。
```
思路：startIndex即本次递归的切割线，决定宽度。0代表第0个元素后切割，由于i代表着切割线不断后移，因此此时切割的子串是s.substring(startIndex, i + 1)，即[startIndex, i]。使用双指针判断回文
```java
class Solution {
    List<List<String>> result = new ArrayList<>();
    Deque<String> path = new LinkedList<>();
    public List<List<String>> partition(String s) {
        backtracking(s, 0);
        return result;
    }
    private void backtracking(String s, int startIndex) { //startIndex即切割线，0代表第0个元素后切割
        if (startIndex >= s.length()) {
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = startIndex; i < s.length(); i++) {
            //startIndex代表着本次递归的初始切割线，由于i代表着切割线不断后移，因此此时切割的子串是s.substring(startIndex, i + 1)
            if (isPalindrome(s, startIndex, i)) {
                path.add(s.substring(startIndex, i + 1));
            } else {
                continue;
            }
            backtracking(s, i + 1);
            path.removeLast();
        }
    }
    private boolean isPalindrome(String s, int startIndex, int end) { //双指针判断回文
        for (int i = startIndex, j = end; i < j; i++, j--) {
            if (s.charAt(i) != s.charAt(j)) return false;
        }
        return true;
    }
}
```