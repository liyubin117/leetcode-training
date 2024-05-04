# day21

## 第二十一天| 二叉树 530 501 236

### 530 二叉搜索树的最小绝对差 easy
```
给你一个二叉搜索树的根节点 root ，返回 树中任意两不同节点值之间的最小差值 。

差值是一个正数，其数值等于两值之差的绝对值。
```
思路：中序遍历，用pre记录前一个节点与当前节点进行比较
```java
class Solution {
    TreeNode pre = null;
    int result = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        if (root == null) return Integer.MAX_VALUE;
        getMinimumDifference(root.left);
        if (pre != null) result = Math.min(Math.abs(root.val - pre.val), result);
        pre = root;
        getMinimumDifference(root.right);
        return result;
    }
}
```

### 501 二叉搜索树中的众数 easy
```
给你一个含重复值的二叉搜索树（BST）的根节点 root ，找出并返回 BST 中的所有 众数（即，出现频率最高的元素）。

如果树中有不止一个众数，可以按 任意顺序 返回。

假定 BST 满足如下定义：
结点左子树中所含节点的值 小于等于 当前节点的值
结点右子树中所含节点的值 大于等于 当前节点的值
左子树和右子树都是二叉搜索树
```
思路：中序遍历，全局变量：上一个节点pre、结果集list、当前节点的频率count、当前最大的频率maxCount。关键在于count和maxCount的比较
```java
class Solution {
    TreeNode pre = null;
    List<Integer> list = new ArrayList<>();
    int count = 0, maxCount = Integer.MIN_VALUE;
    public int[] findMode(TreeNode root) {
        recurse(root);
        int[] ints = new int[list.size()];
        for (int i = 0; i < list.size();i++) {
            ints[i] = list.get(i);
        }
        return ints;
    }
    private void recurse(TreeNode cur) {
        if (cur == null) return;
        recurse(cur.left); //左
        //中
        if (pre == null) count = 1;
        else if (cur.val == pre.val) count++;
        else count = 1;
        if (count == maxCount) list.add(cur.val); //这样处理就只需要遍历一次二叉树
        if (count > maxCount) { //这样处理就只需要遍历一次二叉树，当发现更多频率的元素时直接清空原有结果集
            list.clear();
            list.add(cur.val);
            maxCount = count;
        }
        pre = cur;
        recurse(cur.right); //右
    }
}
```

### 236 二叉树的最近公共祖先 middle
```
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
```
思路：若 root 是 p,q 的 最近公共祖先 ，则只可能为以下情况之一：
- p 和 q 在 root 的子树中，且分列 root 的 异侧（即分别在左、右子树中）；
- p=root，且 q 在 root 的左或右子树中；
- q=root ，且 p 在 root 的左或右子树中；
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left == null) return right;
        if(right == null) return left;
        return root;
    }
}
```