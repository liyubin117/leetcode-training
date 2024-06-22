# day15

## 110 平衡二叉树 easy
```
给定一个二叉树，判断它是否是平衡二叉树
```
https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html

思路：其实就是在求二叉树深度基础上多了左右子树深度差的判断，后序遍历，在求节点高度的过程中，判断左右节点高度差是否大于1，若是则直接返回-1即不符合平衡二叉树，其判断不符合的依据是高度差大于1、左节点已不符合、右节点已不符合
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getHeight(root) == -1 ? false : true;
    }
    private int getHeight(TreeNode node) {
        if (node == null) return 0;
        int left = getHeight(node.left);
        int right = getHeight(node.right);
        if (left == -1 || right == -1 || Math.abs(right - left) > 1) return -1;
        return Math.max(left, right) + 1;
    }
}
```

## 257 二叉树的所有路径 easy
```
给你一个二叉树的根节点 root ，按 任意顺序 ，返回所有从根节点到叶子节点的路径。
叶子节点 是指没有子节点的节点。
```
https://programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html

思路：前序遍历+回溯
```java
//递归入参包含所有的变量，不需要定义实例变量
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        recurse(root, new ArrayList<String>(), result);
        return result;
    }
    private void recurse(TreeNode node, List<String> path, List<String> result) {
        path.add(String.valueOf(node.val)); //中序遍历
        if (node.left == null && node.right == null) result.add(String.join("->", path)); //遇到叶节点
        if (node.left != null) {
            recurse(node.left, path, result);
            path.remove(path.size() - 1); //回溯
        }
        if (node.right != null) {
            recurse(node.right, path, result);
            path.remove(path.size() - 1); //回溯
        }
    }
}
//简单递归入参，需要额外定义实例变量
class Solution {
    private List<String> result = new ArrayList<>();
    private List<String> path = new ArrayList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        recurse(root);
        return result;
    }
    private void recurse(TreeNode node) {
        path.add(String.valueOf(node.val));
        if (node.left == null && node.right == null) {
            result.add(String.join("->", path));
        }
        if (node.left != null) {
            recurse(node.left);
            path.remove(path.size() - 1);
        }
        if (node.right != null) {
            recurse(node.right);
            path.remove(path.size() - 1);
        }
    }
}
```

## 404 左子叶之和 easy
```
给定二叉树的根节点 root ，返回所有左叶子之和。
```
https://programmercarl.com/0404.%E5%B7%A6%E5%8F%B6%E5%AD%90%E4%B9%8B%E5%92%8C.html

思路：后序遍历，当中间节点满足左孩子是左叶节点时，加上左叶节点的值
```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root==null) return 0;
        return sumOfLeftLeaves(root.left)
                + sumOfLeftLeaves(root.right)
                + (root.left!=null && root.left.left==null && root.left.right==null ? root.left.val : 0);
    }
}
```

## 222 完全二叉树的节点个数 easy
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