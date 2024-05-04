# day16

## 第十六天| 二叉树 104 111 222

### 104 二叉树的最大深度 easy
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

### 111 二叉树的最小深度
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
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        if (root.left == null) return right + 1;
        if (root.right == null) return left + 1;
        // if (root.left == null || root.right == null) return Math.max(left, right) + 1;
        return Math.min(left, right) + 1;
    }
}
```

### 222 完全二叉树的节点个数 easy
```
给你一棵 完全二叉树 的根节点 root ，求出该树的节点个数。
完全二叉树 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。
```
https://programmercarl.com/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.html

思路：
- 第一种：递归左右中，这也可以用于普通的二叉树节点个数计算，普适的
  - 终止条件：当前节点为空直接返回0
  - 单层递归：获取左右子树的节点个数，加起来后再加1
- 第二种：使用完全二叉树的性质进行计算，左右子树的深度相同时说明左子树是满二叉树，此时节点数是右子树节点数+2^left，深度不同时时说明最后一层不是满的，倒数第二层满，即右子树是满二叉树，此时节点数是左子数节点数+2^right
```java
//递归左右中
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) return 0;
        return countNodes(root.left) + countNodes(root.right) + 1;
    }
}
//使用完全二叉树的性质
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) return 0;
        int leftLevel = countLevels(root.left);
        int rightLevel = countLevels(root.right);
        if (leftLevel == rightLevel) return countNodes(root.right) + (1 << leftLevel);
        else return countNodes(root.left) + (1 << rightLevel);
    }
    private int countLevels(TreeNode node) {
        if (node == null) return 0;
        return Math.max(countLevels(node.left), countLevels(node.right)) + 1;
    }
}
```