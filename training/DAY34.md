# day34

## 第三十四天| 贪心法 1005 134 135

### 1005 K次取反后最大化的数组和 easy
```
给你一个整数数组 nums 和一个整数 k ，按以下方法修改该数组：
选择某个下标 i 并将 nums[i] 替换为 -nums[i] 。重复这个过程恰好 k 次。可以多次选择同一个下标 i 。
以这种方式修改数组后，返回数组 可能的最大和 。

输入：nums = [4,2,3], k = 1
输出：5
解释：选择下标 1 ，nums 变为 [4,-2,3] 。
```
思路：局部最优即取反绝对值最大的负数，若无负数则取反绝对值最小的，效果最好。推广到全局最优，优先取反绝对值更大的负数，若K未用完而负数已用完则反复取反绝对值最小的正数，由于此时都为非负数，所以只要反复取反最小的值即可
```
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        // 对数组进行升序排序。
        Arrays.sort(nums);
        int sum = 0;
        // 从数组头尽可能多地取反负数。
        for (int i = 0; i < nums.length && k > 0; i++) {
            if (nums[i] < 0){
                k = k - 1;
                nums[i] = -nums[i];
            }
            else
                break;
        }
        // 再次对数组进行升序排序。
        Arrays.sort(nums);
        // 如果剩余的 k 是奇数，则将数组中绝对值最小的元素取反。
        nums[0] = k % 2 == 1 ? -nums[0] : nums[0];
        // 计算整个数组的元素之和并返回。
        for (int i = 0; i < nums.length; i++) {
            sum = sum + nums[i];
        }
        return sum;
    }
}
```

### 134 加油站 middle
```
在一条环路上有 n 个加油站，其中第 i 个加油站有汽油 gas[i] 升。
你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。
给定两个整数数组 gas 和 cost ，如果你可以按顺序绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1 。如果存在解，则 保证 它是 唯一 的。

输入: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
输出: 3
解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
```
思路：若在某个站点消耗比加的多，那么此站点及之前的站点都不可作为起始点，因此要跑完圈必须从这个站点的下一个点开始跑。
```java
class Solution {
    int canCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.length;
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += gas[i] - cost[i];
        }
        if (sum < 0) {
            // 总油量小于总的消耗，无解
            return -1;
        }
        // 记录油箱中的油量
        int tank = 0;
        // 记录起点
        int start = 0;
        for (int i = 0; i < n; i++) {
            tank += gas[i] - cost[i];
            if (tank < 0) {
                // 无法从 start 到达 i + 1，所以站点 i + 1 应该是起点
                tank = 0;
                start = i + 1;
            }
        }
        return start;
    }
}
```

### 135 分发糖果 hard
```
n 个孩子站成一排。给你一个整数数组 ratings 表示每个孩子的评分。

你需要按照以下要求，给这些孩子分发糖果：
每个孩子至少分配到 1 个糖果。
相邻两个孩子评分更高的孩子会获得更多的糖果。
请你给每个孩子分发糖果，计算并返回需要准备的 最少糖果数目 。

输入：ratings = [1,0,2]
输出：5
解释：你可以分别给第一个、第二个、第三个孩子分发 2、1、2 颗糖果。

输入：ratings = [1,2,2]
输出：4
解释：你可以分别给第一个、第二个、第三个孩子分发 1、2、1 颗糖果。
     第三个孩子只得到 1 颗糖果，这满足题面中的两个条件。
```
思路：先确定一边从左到右，即右小孩比左的得分高的情况，再确定另一边从右到左，即左小孩比右的得分高的情况。取以上 2 轮遍历 left 和 right 对应学生糖果数的最大值 ，这样则同时满足左规则和右规则，即得到每个同学的最少糖果数量
```java
class Solution {
    public int candy(int[] ratings) {
        int[] left = new int[ratings.length];
        int[] right = new int[ratings.length];
        Arrays.fill(left, 1);
        Arrays.fill(right, 1);
        for (int i = 1; i < ratings.length; i++)
            if (ratings[i] > ratings[i - 1])
                left[i] = left[i - 1] + 1;
        int count = left[ratings.length - 1];
        for (int i = ratings.length - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1])
                right[i] = right[i + 1] + 1;
            count += Math.max(left[i], right[i]);
        }
        return count;
    }
}
```