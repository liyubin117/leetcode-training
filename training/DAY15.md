# day15

## 第十天| 二叉树 226 101

### 226 翻转二叉树 easy
```
给你一棵二叉树的根节点 root ，翻转这棵二叉树，并返回其根节点。
```
https://programmercarl.com/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.html

思路：递归中左右
- 参数：入参是节点，无出参
- 终止条件：若当前节点为空则直接返回上一次递归
- 单层递归逻辑：中节点左右节点交换，需要用到临时变量。由于此时只是把左右节点反转，还需要将整个左右子树反转，因此进行下一次递归
```java
//递归，简洁的写法
class Solution {
    public TreeNode invertTree(TreeNode root) {
        recurse(root);
        return root;
    }
    private void recurse(TreeNode node) {
        if (node == null) return;
        TreeNode temp = node.left;
        node.left = node.right;
        node.right = temp;
        recurse(node.left);
        recurse(node.right);
    }
}
//递归，自己想的，写的比较复杂，但时间效率仍优于100%。由于左右子树在栈参数里，没有用到临时变量，空间效率更优
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        recurse(root, root.left, root.right);
        return root;
    }
    private TreeNode recurse(TreeNode node, TreeNode left, TreeNode right) {
        if (left == null && right == null) return node;
        if (right != null) node.left = recurse(right, right.left, right.right);
        else node.left = null;
        if (left != null) node.right = recurse(left, left.left, left.right);
        else node.right = null;
        return node;
    }
}
```

### 101 对称二叉树 easy
```
给你一个二叉树的根节点 root ， 检查它是否轴对称。
```
https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html

思路：递归中左右
- 参数：入参左右节点，出参单次递归的比较结果
- 终止条件：左右空的对比，左右不一致直接false
- 单层递归逻辑：中节点本身不需要对比，需要对比外层（左左、右右）和内层（左右、右左），若同时满足则true，否则false
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return recurse(root.left, root.right);
    }
    private boolean recurse(TreeNode left, TreeNode right) {
        if (left == null && right != null) return false;
        if (left != null && right == null) return false;
        if (left == null && right == null) return true;
        if (left.val != right.val) return false;
        boolean outer = recurse(left.left, right.right);
        boolean inner = recurse(left.right, right.left);
        return outer && inner;
    }
}
```