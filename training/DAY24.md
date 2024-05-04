# day24

## 第二十四天| 回溯法 77

### 77 组合 middle
```
给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。

你可以按 任何顺序 返回答案。
```
思路：回溯。每次从集合中选取元素，可选择的范围随着选择的进行而收缩，调整可选择的范围，就是要靠startIndex用来记录本层递归的中，集合从哪里开始遍历（集合就是[1,...,n]）。还可以剪枝优化，可以剪枝的地方就在递归中每一层的for循环所选择的起始位置，如果for循环选择的起始位置之后的元素个数 已经不足 我们需要的元素个数了，那么就没有必要搜索了。
```java
//未剪枝
class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
    private void backtracking(int n, int k, int startIndex) {
        if (path.size() == k) { //终止条件：已经拿到规定长度的路径
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = startIndex; i <= n; i++) {
            path.add(i); //做出选择
            backtracking(n, k, i + 1);
            path.removeLast(); //撤销选择
        }
    }
}
//剪枝优化
class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
    private void backtracking(int n, int k, int startIndex) {
        if (path.size() == k) { //终止条件：已经拿到规定长度的路径
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = startIndex; i <= n - (k - path.size()) + 1; i++) {
            path.add(i); //做出选择
            backtracking(n, k, i + 1);
            path.removeLast(); //撤销选择
        }
    }
}
```