# day23

## 第二十三天| 二叉树 669 108 538

### 669 修剪二叉搜索树 middle
```
给你二叉搜索树的根节点 root ，同时给定最小边界low 和最大边界 high。通过修剪二叉搜索树，使得所有节点的值在[low, high]中。修剪树 不应该 改变保留在树中的元素的相对结构 (即，如果没有被移除，原有的父代子代关系都应当保留)。 可以证明，存在 唯一的答案 。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。
```
思路：递归前序遍历。对于当前根节点，若为空则返回空，若小于左界则返回修剪右子树后的根节点，若大于右界则返回修剪左子树后的根节点，再以此方式遍历左、右子树赋值当前根节点
```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) return null;
        if (root.val < low) return trimBST(root.right, low, high);
        if (root.val > high) return trimBST(root.left, low, high);
        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        return root;
    }
}
```

### 108 将有序数组转换为二叉搜索树 easy
```
给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵平衡二叉搜索树。
```
思路：选择中间位置左边的数字作为根节点，确保中间节点的大小大于左、小于右
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return recurse(nums, 0, nums.length - 1);
    }
    private TreeNode recurse(int[] nums, int l, int r) {
        if (l > r) return null;
        int mid = l + (r - l) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = recurse(nums, l, mid - 1);
        root.right = recurse(nums, mid + 1, r);
        return root;
    }
}
```

### 538 把二叉搜索树转换为累加树 middle
```
给出二叉 搜索 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 node 的新值等于原树中大于或等于 node.val 的值之和。

提醒一下，二叉搜索树满足下列约束条件：
节点的左子树仅包含键 小于 节点键的节点。
节点的右子树仅包含键 大于 节点键的节点。
左右子树也必须是二叉搜索树。
注意：本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同
```
思路：递归右中左+pre，使用pre记录上一个累加值，然后与当前节点的值相加再赋值pre用于下一次累加
```java
class Solution {
    private int pre = 0;
    public TreeNode convertBST(TreeNode root) {
        recurse(root);
        return root;
    }
    private void recurse(TreeNode node) {
        if (node == null) return;
        recurse(node.right);
        node.val += pre;
        pre = node.val;
        recurse(node.left);
    }
}
```