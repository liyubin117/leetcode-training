# day19

## 第十九天| 654 617 700 98

### 654 最大二叉树 middle
```
给定一个不重复的整数数组 nums 。 最大二叉树 可以用下面的算法从 nums 递归地构建:

创建一个根节点，其值为 nums 中的最大值。
递归地在最大值 左边 的 子数组前缀上 构建左子树。
递归地在最大值 右边 的 子数组后缀上 构建右子树。
返回 nums 构建的 最大二叉树 。
```
https://programmercarl.com/0654.%E6%9C%80%E5%A4%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html

思路：前序遍历
- 确定入参和返回值：参数传入的是存放元素的数组、左闭索引、右开索引，返回该数组构造的二叉树的头结点
- 终止条件：如果传入的数组大小为1，说明遍历到了叶子节点则返回，如果右标小于左标说明没有元素直接返回null
- 单层递归的逻辑：先找到最大值作为根节点，并记录其下标用来分割数组maxIndex，[leftIndex, maxIndex)作为左子树，[maxIndex+1, rightIndex)作为右子树
```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return recurse(nums, 0, nums.length);
    }
    private TreeNode recurse(int[] nums, int l, int r) {
        if (r == l) return null;
        if (r - l == 1) return new TreeNode(nums[l]);
        int maxValue = nums[l];
        int maxIndex = l;
        for (int i = l + 1; i < r; i++) {
            if (nums[i] > maxValue) {
                maxValue = nums[i];
                maxIndex = i;
            }
        }
        TreeNode root = new TreeNode(maxValue);
        //根据最大值划分左右子树
        root.left = recurse(nums, l, maxIndex);
        root.right = recurse(nums, maxIndex + 1, r);
        return root;
    }
}
```

### 617 合并二叉树 easy
```
给你两棵二叉树： root1 和 root2 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，不为 null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

注意: 合并过程必须从两个树的根节点开始。
```
https://programmercarl.com/0617.%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.html

思路：前序递归
- 入参出参：入参是root1、root2，出参是合并后的树
- 终止条件：root1为空则返回roo2，root2为空则返回root1
- 单层递归：确定根节点是值为root1 root2合并后的新节点，然后对左右子树递归
```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) return root2;
        if (root2 == null) return root1;
        TreeNode root = new TreeNode(root1.val + root2.val);
        root.left = mergeTrees(root1.left, root2.left);
        root.right = mergeTrees(root1.right, root2.right);
        return root;
    }
}
```

### 700 二叉搜索树中的搜索 easy
```
给定二叉搜索树（BST）的根节点 root 和一个整数值 val。

你需要在 BST 中找到节点值等于 val 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 null 。
```
https://programmercarl.com/0700.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%90%9C%E7%B4%A2.html

思路：前序遍历
```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) return root;
        if (root.val < val) return searchBST(root.right, val);
        else return searchBST(root.left, val);
    }
}
```

### 98 验证二叉搜索树 middle
```
给你一个二叉树的根节点 root ，判断其是否是一个有效的二叉搜索树。

有效 二叉搜索树定义如下：

节点的左子树只包含 小于 当前节点的数。
节点的右子树只包含 大于 当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。
```
https://programmercarl.com/0700.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%90%9C%E7%B4%A2.html

思路：递归中序遍历。遍历中节点时设置当前节点为前一个节点，后续进行比较，更好些，不用担心最大值的类型问题
```java
class Solution {
    TreeNode pre = null;
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        boolean left = isValidBST(root.left);
        if (pre != null && pre.val >= root.val) return false;
        pre = root;
        boolean right = isValidBST(root.right);
        return left && right;
    }
}
```