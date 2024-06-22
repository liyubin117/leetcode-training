# day13

## 144 二叉树的前序遍历 easy
```
给你二叉树的根节点 root ，返回它节点值的 前序 遍历。
```
思路：
- 递归中左右
- 栈迭代中右左
```java
//递归
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        recurse(root);
        return result;
    }
    private void recurse(TreeNode node) {
        if (node == null) return;
        result.add(node.val);
        recurse(node.left);
        recurse(node.right);
    }
}
//迭代
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        List<Integer> result = new ArrayList<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            if (node != null) result.add(node.val);
            else continue;
            stack.push(node.right);
            stack.push(node.left);
        }
        return result;
    }
}
```

## 145 二叉树的后序遍历 easy
```
给你一棵二叉树的根节点 root ，返回其节点值的 后序遍历 。
```
思路：
- 递归左右中
- 迭代，用栈先中左右再双指针进行翻转
```java
//递归
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> postorderTraversal(TreeNode root) {
        recurse(root);
        return result;
    }
    private void recurse(TreeNode node) {
        if (node == null) return;
        recurse(node.left);
        recurse(node.right);
        result.add(node.val);
    }
}
//迭代
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        List<Integer> result = new ArrayList<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            if (node != null) result.add(node.val);
            else continue;
            stack.push(node.left);
            stack.push(node.right);
        }
        for (int l = 0, r = result.size() - 1; l < r; ) {
            int temp = result.get(r);
            result.set(r, result.get(l));
            result.set(l, temp);
            l++;
            r--;
        }
        return result;
    }
}
```

## 94 二叉树的中序遍历 easy
```
给定一个二叉树的根节点 root ，返回 它的 中序 遍历 。
```
思路：
- 递归左中右
- 迭代，循环迭代左侧节点入栈，直到无左侧节点时将当前节点定位到栈的顶端元素即父节点，将其出栈，再将当前节点定位到右孩子节点。直到栈无元素且当前节点为空
```java
//递归
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        recurse(root);
        return result;
    }
    private void recurse(TreeNode node) {
        if (node == null) return;
        recurse(node.left);
        result.add(node.val);
        recurse(node.right);
    }
}
//迭代
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()){
            if (cur != null){
                stack.push(cur);
                cur = cur.left;
            }else{
                cur = stack.pop();
                result.add(cur.val);
                cur = cur.right;
            }
        }
        return result;
    }
}
```