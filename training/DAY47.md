# day47

## 第四十七天| 动态规划 198 213 337

### 198 打家劫舍 middle
```
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```
思路：不需要复杂的背包思想，基本的dp思路即可
- dp数组定义：dp[j]在下标j能偷到的最大金额
- 递推公式：dp[j] = max(dp[j-2]+nums[j], dp[j-1])
    - 在下标j偷：dp[j-2] + nums[j]
    - 在下标j不偷：dp[j-1]
- dp数组初始化：dp[0]=nums[0]，dp[1]=max(nums[0],nums[1]), 非0和1下标的元素初始化成非负最小值0防止影响max计算
- 遍历顺序：从左到右
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        if (nums.length == 2) return Math.max(nums[0], nums[1]);
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for (int j = 2; j < nums.length; j++) {
            dp[j] = Math.max(dp[j - 2] + nums[j], dp[j - 1]);
        }
        return dp[nums.length - 1];
    }
}
```
### 213 打家劫舍2 middle
```
你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。
给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。

输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```
思路：相对于198问题，房间连成了环，因此第一个和最后一个房间不能同时偷。分三种情况转化成198的非环形的线性数组情况：不偷第1、末1；不偷第1；不偷末1，而后两种情况的遍历范围已经包含了第一种情况，因此只算后两种情况即可
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        if (nums.length == 2) return Math.max(nums[0], nums[1]);
        return Math.max(linearFunc(Arrays.copyOfRange(nums, 0, nums.length - 1)), linearFunc(Arrays.copyOfRange(nums, 1, nums.length)));
    }

    private int linearFunc(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for (int j = 2; j < nums.length; j++) {
            dp[j] = Math.max(dp[j - 2] + nums[j], dp[j - 1]);
        }
        return dp[nums.length - 1];
    }
}
```
### 337 打家劫舍3 middle
```
小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 root 。
除了 root 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 两个直接相连的房子在同一天晚上被打劫 ，房屋将自动报警。
给定二叉树的 root 。返回 在不触动警报的情况下 ，小偷能够盗取的最高金额 。

输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```
思路：**树形dp**。相对于198问题，是二叉树结构，若偷了某节点，其父节点、左右子节点都不能偷。
- dp数组定义：dp[0]表示不偷当前节点时的最大金额，dp[1]表示偷当前节点时的最大金额。递归时会带dp[]参数表示当前节点的dp数组，不需要定义很多个
- 递归：
    - 函数定义：int[] robTree(TreeNode node)
    - 终止条件：若当前节点为空则返回初始化为0的dp数组
    - 单层递归（递推公式）：**先得到左右孩子偷、不偷的最大金额leftDp rightDp，然后分别得到当前节点的dp[0] dp[1]，因此是后序遍历**
        - 偷当前节点：node.val + leftDp[0] + rightDp[0]
        - 不偷当前节点：左右孩子都是可偷可不偷，都选最大的然后相加即可，max(leftDp[0], leftDp[1]) + max(rightDp[0], rightDp[1]))
- 遍历顺序：后序遍历
```java
class Solution {
    public int rob(TreeNode root) {
        int[] dp = robTree(root);
        return Math.max(dp[0], dp[1]);
    }

    private int[] robTree(TreeNode node) {
        if (node == null) return new int[2];
        int[] leftDp = robTree(node.left);
        int[] rightDp = robTree(node.right);
        int curRob = node.val + leftDp[0] + rightDp[0];
        int curNotRob = Math.max(leftDp[0], leftDp[1]) + Math.max(rightDp[0], rightDp[1]);
        return new int[]{curNotRob, curRob};
    }
}
```