# day14

## 226 翻转二叉树 easy
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

## 101 对称二叉树 easy
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

## 104 二叉树的最大深度 easy
```
给定一个二叉树 root ，返回其最大深度。
二叉树的 最大深度 是指从根节点到最远叶子节点的最长路径上的节点数。
```
https://programmercarl.com/0104.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%A4%A7%E6%B7%B1%E5%BA%A6.html

思路：递归左右中
- 终止条件：当前节点为空则返回当前的深度是0
- 单层递归：获取左右子树深度，取更大的深度再加1
```java
class Solution {
    int result = 0;
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

## 111 二叉树的最小深度
```
给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
```
https://programmercarl.com/0111.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E6%B7%B1%E5%BA%A6.html

思路：递归左右中
- 终止条件：当前节点为空则直接返回0
- 单层递归：获取左右子树的最小深度，若左子树为空直接返回右子树深度+1，若右子树为空直接返回左子树深度+1，若都不为空返回更大的深度+1
```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        if (root.left == null) return minDepth(root.right) + 1;
        if (root.right == null) return minDepth(root.left) + 1;
        return Math.min(minDepth(root.right), minDepth(root.left)) + 1;
    }
}
```