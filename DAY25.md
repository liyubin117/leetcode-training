# day25

## 第二十五天| 回溯法 216 17

### 216 组合总和3 middle
```
找出所有相加之和为 n 的 k 个数的组合，且满足下列条件：
只使用数字1到9
每个数字 最多使用一次 

返回 所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。
```
思路：回溯
- 终止条件：path元素个数满足k，且此时的sum满足targetSum，则加到result返回
- 单层递归：从startIndex开始递增到9，循环添加到path，增加sum，进行递归后回滚。可剪枝优化，startIndex递增到9-(k-path.size())+1即可，不然会递归无法满足k个元素的情况
```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(k, n, 0, 1);
        return result;
    }
    private void backtracking(int k, int targetSum, int sum, int startIndex) {
        if (path.size() == k) {
            if (targetSum == sum) result.add(new ArrayList<>(path));
            return;
        }
        for (int i = startIndex; i <= 10 - (k - path.size()); i++) {
            path.add(i);
            backtracking(k, targetSum, sum + i, i + 1);
            path.removeLast();
        }
    }
}
```

### 17 电话号码的字母组合 middle
```
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。
```
思路：回溯
- 入参出参：出参都是void，入参是digits和index，每次递归是从digits字符串的index索引处开始计算
- 终止条件：index达到digits长度的索引位置，说明此时已经完成传入的digits的遍历，达到叶节点，可收集结果
- 单层递归：digits在index索引位置的数字代表的如abc、def等，这个决定了本轮递归的多叉树的宽度，深度由digits剩下的数字决定，所以在进行下一轮递归时index+1
```java
class Solution {
    List<String> result = new ArrayList<String>();
    StringBuffer path = new StringBuffer();
    Map<Character, String> phoneMap = new HashMap<Character, String>() {{
        put('2', "abc");
        put('3', "def");
        put('4', "ghi");
        put('5', "jkl");
        put('6', "mno");
        put('7', "pqrs");
        put('8', "tuv");
        put('9', "wxyz");
    }};
    public List<String> letterCombinations(String digits) {
        if (digits.length() == 0) return result;
        backtrack(digits, 0);
        return result;
    }

    public void backtrack(String digits, int index) {
        if (index == digits.length()) {
            result.add(path.toString());
            return;
        }
        for (char ch: phoneMap.get(digits.charAt(index)).toCharArray()) {
            path.append(ch);
            backtrack(digits, index + 1);
            path.deleteCharAt(index);
        }
    }
}
```