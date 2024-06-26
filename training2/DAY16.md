# day16

## 513 找树左下角的值 middle
```
给定一个二叉树的 根节点 root，请找出该二叉树的 最底层 最左边 节点的值。

假设二叉树中至少有一个节点。
```
https://programmercarl.com/0513.%E6%89%BE%E6%A0%91%E5%B7%A6%E4%B8%8B%E8%A7%92%E7%9A%84%E5%80%BC.html

思路：
- 递归前序遍历，当截止条件是到叶节点且深度大于此前的最大深度时，说明已经到了树的某个左下角但不一定是最底层，更新此时的深度和结果，继续递归最终取得结果
- 迭代
```java
//递归
class Solution {
    private int maxDepth = Integer.MIN_VALUE;
    private int result = 0;
    public int findBottomLeftValue(TreeNode root) {
        recurse(root, 1);
        return result;
    }
    private void recurse(TreeNode node, int depth) {
        if (node.left == null && node.right == null && depth > maxDepth) {
            maxDepth = depth;
            result = node.val;
        }
        if (node.left != null) recurse(node.left, depth + 1);
        if (node.right != null) recurse(node.right, depth + 1);
    }
}
//迭代
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int res = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode poll = queue.poll();
                if (i == 0) res = poll.val;
                if (poll.left != null) queue.offer(poll.left);
                if (poll.right != null) queue.offer(poll.right);
            }
        }
        return res;
    }
}
```

## 112 路径总和 easy
```
给你二叉树的根节点 root 和一个表示目标和的整数 targetSum 。判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 targetSum 。如果存在，返回 true ；否则，返回 false 。
```
https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html

思路：递归前序遍历，终止条件是叶子节点时的sum等于目标值返回true，注意回溯。原理和257找出所有路径类似
```java
class Solution {
    private int sum = 0;
    private boolean flag = false;
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;
        return recurse(root, targetSum);
    }
    private boolean recurse(TreeNode node, int targetSum) {
        sum += node.val;
        if (node.left == null && node.right == null && sum == targetSum) flag = true;
        if (node.left != null) {
            hasPathSum(node.left, targetSum);
            sum -= node.left.val;
        }
        if (node.right != null) {
            hasPathSum(node.right, targetSum);
            sum -= node.right.val;
        }
        return flag;
    }
}
```

## 106 从中序与后序遍历序列构造二叉树 middle
```
给定两个整数数组 inorder 和 postorder ，其中 inorder 是二叉树的中序遍历， postorder 是同一棵树的后序遍历，请你构造并返回这颗 二叉树 。
```

思路：

先从后序最后一个节点确定根节点，再从中序切割出左右，再从后序切割出中，注意区间统一是左闭右开，迭代确定二叉树

第一步：如果数组大小为零的话，说明是空节点了。

第二步：如果不为空，那么取后序数组最后一个元素作为节点元素。

第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点

第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）

第五步：切割后序数组，切成后序左数组和后序右数组

第六步：递归处理左区间和右区间
```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();  // 方便根据数值查找位置
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for (int i = 0; i < inorder.length; i++) { // 用map保存中序序列的数值对应位置
            map.put(inorder[i], i);
        }
        return findNode(inorder, 0, inorder.length, postorder, 0, postorder.length);  // 前闭后开
    }

    public TreeNode findNode(int[] inorder, int inBegin, int inEnd, int[] postorder, int postBegin, int postEnd) {
        // 参数里的范围都是前闭后开
        if (inBegin >= inEnd || postBegin >= postEnd) {  // 不满足左闭右开，说明没有元素，返回空树
            return null;
        }
        int rootIndex = map.get(postorder[postEnd - 1]);  // 找到后序遍历的最后一个元素在中序遍历中的位置
        TreeNode root = new TreeNode(inorder[rootIndex]);  // 构造结点
        int lenOfLeft = rootIndex - inBegin;  // 保存中序左子树个数，用来确定后序数列的个数
        root.left = findNode(inorder, inBegin, rootIndex,
                            postorder, postBegin, postBegin + lenOfLeft); //用左中序、左后序构造左子树
        root.right = findNode(inorder, rootIndex + 1, inEnd,
                            postorder, postBegin + lenOfLeft, postEnd - 1); //用右中序、右后序构造右子树
        return root;
    }
}
```