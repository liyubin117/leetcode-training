# day17

## 第十七天| 二叉树 110 257 404

### 110 平衡二叉树 easy
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

### 257 二叉树的所有路径 easy
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

### 404 左子叶之和 easy
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