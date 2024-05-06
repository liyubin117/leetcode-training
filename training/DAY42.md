# day42

## 第四十二天| 动态规划 背包01 416

### 卡码网46 携带研究材料
```
题目描述
小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。他需要带一些研究材料，但是他的行李箱空间有限。这些研究材料包括实验设备、文献资料和实验样本等等，它们各自占据不同的空间，并且具有不同的价值。 
小明的行李空间为 N，问小明应该如何抉择，才能携带最大价值的研究材料，每种研究材料只能选择一次，并且只有选与不选两种选择，不能进行切割。

输入描述
第一行包含两个正整数，第一个整数 M 代表研究材料的种类，第二个正整数 N，代表小明的行李空间。
第二行包含 M 个正整数，代表每种研究材料的所占空间。 
第三行包含 M 个正整数，代表每种研究材料的价值。

输出描述
输出一个整数，代表小明能够携带的研究材料的最大价值。
```
思路：
- 二维dp数组
```java
import java.util.*;

public class Main {
    
    public static void main(String[] args) {
        // 背包容量 N
        // 物品种类 M
        Scanner sc = new Scanner(System.in);
        int M = sc.nextInt();
        int N = sc.nextInt();
        int[] values = new int[M];
        int[] weights = new int[M];
        for(int i = 0; i < M;i++) {
            weights[i] = sc.nextInt();
        }
        for(int i = 0; i < M;i++) {
            values[i] = sc.nextInt();
        }
        
        int[][] dp = new int[M][N+1];
        // 初始化
        for(int j = weights[0]; j <= N; j++) {
            dp[0][j] = values[0];
        }
        
        // 填充dp数组
        for (int i = 1; i < M; i++) { // 遍历物品
            for (int j = 1; j <= N; j++) { // 遍历容量
                if (j < weights[i]) {
                    /**
                     * 当前背包的容量都没有当前物品i大的时候，是不放物品i的
                     * 那么前i-1个物品能放下的最大价值就是当前情况的最大价值
                     */
                    dp[i][j] = dp[i-1][j];
                } else {
                    /**
                     * 当前背包的容量可以放下物品i
                     * 那么此时分两种情况：
                     *    1、不放物品i
                     *    2、放物品i
                     * 比较这两种情况下，哪种背包中物品的最大价值最大
                     */
                    dp[i][j] = Math.max(dp[i-1][j] , dp[i-1][j-weights[i]] + values[i]);
                }
            }
        }
        System.out.println(dp[M-1][N]);
    }
}
```
- 一维dp数组
```java
import java.util.*;
public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        int M=0;
        int N=0;
        M=scanner.nextInt();
        N=scanner.nextInt();
        int[] weight=new int[M+1];
        int[] value=new int[M+1];
        int tmp=0;
        for(int i=1;i<=M;i++){
            tmp=scanner.nextInt();
            weight[i]=tmp;
        }
        for(int i=1;i<=M;i++){
            tmp=scanner.nextInt();
            value[i]=tmp;
        }
        
        int[] bag=new int[N+1];
        for(int i=0;i<=M;i++){
            for(int j=N;j>=weight[i];j--){
                bag[j]=Math.max(bag[j-weight[i]]+value[i],bag[j]);
            }
        }

        System.out.print(bag[N]);
        
    }
}
```
### 416 分割等和子集 middle
```
给你一个 只包含正整数 的 非空 数组 nums 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```
思路：求和后除2，找出哪些元素相加等于这个值，自然剩下的也是这个值。回溯会超时。用动态规划，转化成01背包问题，每个元素都是一种物品，数值既是重量也是价值，dp[j]表示容量j的背包的最大价值，若价值等于重量等于目标则说明已装满这个背包：target=sum/2，dp[target]==target即装满
```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num: nums) {
            sum += num;
        }
        if(sum % 2 != 0) return false;
        int target = sum / 2;
        int[] dp = new int[target + 1];
        for(int i = 0; i < nums.length; i++) {
            for (int j = target; j >= nums[i]; j--) {
                dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
            }
        }
        return dp[target] == target;
    }
}
```