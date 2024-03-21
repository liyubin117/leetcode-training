# day2

## 第二天|数组 977 209 59

### 977 有序数组的平方
题目链接：https://leetcode.cn/problems/squares-of-a-sorted-array/

文章讲解：https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html

视频讲解： https://www.bilibili.com/video/BV1QB4y1D7ep 

思路：双指针

复杂度：时间O(N) 空间O(N)

```java
    public int[] sortedSquares(int[] nums) {
        int[] result = new int[nums.length];
        int k = nums.length - 1;
        for (int l = 0, r = nums.length - 1; l <= r;) {
            int a = nums[l] * nums[l], b = nums[r] * nums[r];
            if (a > b) {
                result[k] = a;
                l++;
            }
            else {
                result[k] = b;
                r--;
            }
            k--;
        }
        return result;
    }
```

### 209 长度最小的子数组
题目链接：https://leetcode.cn/problems/minimum-size-subarray-sum/

文章讲解：https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html

视频讲解：https://www.bilibili.com/video/BV1tZ4y1q7XE

思路：滑动窗口。根据当前子序列和大小的情况，不断调节子序列的起始位置

复杂度：时间O(N) 空间O(1) 
```java
    public int minSubArrayLen(int target, int[] nums) {
        int begin = 0, end = 0, sum = 0, result = Integer.MAX_VALUE;
        for(; end < nums.length; end++) {
            sum += nums[end];
            while (sum >= target) {
                result = Math.min(result, end - begin + 1);
                sum -= nums[begin++];
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
```

## 59 螺旋矩阵II
题目链接：https://leetcode.cn/problems/spiral-matrix-ii/

文章讲解：https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html

视频讲解：https://www.bilibili.com/video/BV1SL4y1N7mV/

思路：边界是left right top bottom，i代表横边，j代表纵边，循环到n*n次赋值结束

复杂度：时间O(N*N) 空间O(N*N)
```java
    public int[][] generateMatrix(int n) {
        int left = 0, right = n - 1, top = 0, bottom = n - 1, count = 1;
        int[][] result = new int[n][n];
        while (count <= n * n) {
            for (int j = left; j <= right; j++) result[top][j] = count++;
            top++;
            for (int i = top; i <= bottom; i++) result[i][right] = count++;
            right--;
            for (int j = right; j >= left; j--) result[bottom][j] = count++;
            bottom--;
            for (int i = bottom; i >= top; i--) result[i][left] = count++;
            left++;
        }
        return result;
    }
```