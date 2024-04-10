# day22

## 第二十二天| 二叉树 235 701 450

### 235 二叉搜索树的最近公共祖先 middle
```
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
```
思路：若左右节点都小于当前节点，则去左子树查，若一大一小则是当前节点
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return null;
        if (p.val < root.val && q.val < root.val) return lowestCommonAncestor(root.left, p, q);
        if (p.val > root.val && q.val > root.val) return lowestCommonAncestor(root.right, p, q);
        return root;
    }
}
```

### 701 二叉搜索树中的插入操作 middle
```
给定二叉搜索树（BST）的根节点 root 和要插入树中的值 value ，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 保证 ，新值和原始二叉搜索树中的任意节点值都不同。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 任意有效的结果 。
```
思路：
- 递归，根据目标值与当前元素值的大小比较，决定递归方向。更好理解
- 迭代
```java
//递归
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) // 如果当前节点为空，也就意味着val找到了合适的位置，此时创建节点直接返回。
            return new TreeNode(val);

        if (root.val < val){
            root.right = insertIntoBST(root.right, val); // 递归创建右子树
        }else if (root.val > val){
            root.left = insertIntoBST(root.left, val); // 递归创建左子树
        }
        return root;
    }
}
//迭代
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) return new TreeNode(val);
        TreeNode newRoot = root;
        TreeNode pre = null;
        while (root != null) {
            pre = root; //记录当前节点为上一个节点，用于下次比较
            if (root.val > val) {
                root = root.left;
            } else if (root.val < val) {
                root = root.right;
            }
        }
        if (pre.val > val) {
            pre.left = new TreeNode(val);
        } else {
            pre.right = new TreeNode(val);
        }

        return newRoot;
    }
}
```

### 450 删除二叉搜索树中的节点 middle
```
给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：
首先找到需要删除的节点；
如果找到了，删除它。
```
思路：递归
- 终止条件：遇到空返回
- 单层递归：共五种情况
  - 第一种情况：没找到删除的节点，遍历到空节点直接返回了 
  - 找到删除的节点 
    - 第二种情况：左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点
    - 第三种情况：删除节点的左孩子为空，右孩子不为空，删除节点，右孩子补位，返回右孩子为根节点
    - 第四种情况：删除节点的右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
    - 第五种情况：左右孩子节点都不为空，则将删除节点的左子树头结点（左孩子）放到删除节点的右子树的最左面节点的左孩子上，返回删除节点右孩子为新的根节点。
```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        root = delete(root,key);
        return root;
    }

    private TreeNode delete(TreeNode root, int key) {
        if (root == null) return null; //第一种情况

        if (root.val > key) {
            root.left = delete(root.left,key);
        } else if (root.val < key) {
            root.right = delete(root.right,key);
        } else {
            if (root.left == null) return root.right; //第二、三种情况
            if (root.right == null) return root.left; //第四种情况
            TreeNode tmp = root.right;
            while (tmp.left != null) { //第五种情况，找到左叶节点
                tmp = tmp.left;
            }
            root.val = tmp.val;
            root.right = delete(root.right,tmp.val);
        }
        return root;
    }
}
```