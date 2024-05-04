# day37

## 第三十七天| 贪心法 738 968

### 738 单调递增的数字 middle
```
当且仅当每个相邻位数上的数字 x 和 y 满足 x <= y 时，我们称这个整数是单调递增的。
给定一个整数 n ，返回 小于或等于 n 的最大数字，且数字呈 单调递增 。

提示：0 <= n <= 109

输入: n = 10
输出: 9

输入: n = 332
输出: 299
```
思路：
- 贪心法：从后往前遍历，只要遇到x>y的立马x-1，y变成9。可以通过将数字转换成字符数组，很容易进行处理。使用replace记录该从哪替换9，初始化为数组长度，便于后续在无需替换时直接跳过循环，遍历字符数组时，遇到x>y的则直接x-1，然后记录此时索引replace为i+1
```java
class Solution {
    public int monotoneIncreasingDigits(int n) {
        String s = String.valueOf(n);
        char[] chars = s.toCharArray();
        int replace = s.length();
        for (int i = s.length() - 2; i >= 0; i--) {
            if (chars[i] > chars[i + 1]) {
                chars[i]--;
                replace = i + 1;
            }
        }
        for (int i = replace; i < s.length(); i++) {
            chars[i] = '9';
        }
        return Integer.parseInt(String.valueOf(chars));
    }
}
```
- 暴力法，也是从后往前遍历，但时间复杂度高，会超出时间限制
```java
class Solution {
    public int monotoneIncreasingDigits(int n) {
        int tmp = 0;
        while (n >= 0) {
            tmp = n;
            while ((tmp / 10) % 10 >= 0) {
                int y = tmp % 10, x = (tmp / 10) % 10;
                if (x > y) {
                    break;
                } else {
                    tmp = tmp / 10;
                    if (tmp == 0) return n;
                }
            }
            n--;
        }
        return n;
    }
}
```

### 968 监控二叉树 hard
```
给定一个二叉树，我们在树的节点上安装摄像头。
节点上的每个摄影头都可以监视其父对象、自身及其直接子对象。
计算监控树的所有节点所需的最小摄像头数量。

输入：[0,0,null,0,0]
输出：1
解释：一台摄像头足以监控所有节点。
```
思路：尽量优先放在叶子节点父节点，自底向上每隔两个节点放摄像头，直到根节点，可以最大限度扩大摄像头覆盖范围。使用后序遍历，父节点根据左右孩子的状态判断是否要放摄像头，有三种状态：
* 0 无覆盖
* 1 有覆盖
* 2 有摄像头

- 空节点要置成1有覆盖状态，因为如果是0则叶节点必放摄像头，是2则叶节点父节点不能放摄像头，都与设计不符，只能是1。
- 若左右节点都有覆盖，则父节点必置无覆盖
- 若左右节点至少有一个无覆盖，则父节点必置摄像头
- 若左右节点至少有一个摄像头，则父节点必置有覆盖
- 若根节点无覆盖，则置有摄像头
```java
class Solution {
    static int ans;
    public int minCameraCover(TreeNode root) {
        ans = 0; // 初始化
        if(f(root) == 0) ans ++; // 若根节点无覆盖则置摄像头
        return ans;
    }
    private int f(TreeNode x) {
        if(x == null) return 1; // 空树认为被监控，但没有相机
        // 左右递归到最深处
        int l = f(x.left);
        int r = f(x.right);
        // 有任意一个子节点为空，就需要当前节点放相机，不然以后没机会
        if(l == 0 || r == 0) {
            ans ++; // 放相机
            return 2;
        }
        // 贪心策略，左右子树都被监控，且没有监控到当前节点，那么最有利的情况就是将相机放置在当前节点父节点上，因为这样能多监控可能的子树节点和父父节点
        if(l == 1 && r == 1) return 0;
        // 剩下情况就是左右子树有可能为 2，即当前节点被监控
        return 1; 
    }
}
```