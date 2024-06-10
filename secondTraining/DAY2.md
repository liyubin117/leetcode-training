# day2

## 977 有序数组的平方 easy
双指针平方比较后倒序赋值新数组
```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] result = new int[nums.length];
        int l = 0, r = nums.length - 1, index = r, a, b;
        while (l <= r) {
            a = nums[l] * nums[l];
            b = nums[r] * nums[r];
            if (a > b) {
                result[index--] = a;
                l++;
            } else {
                result[index--] = b;
                r--;
            }
        }
        return result;
    }
}
```

## 209 长度最小的子数组 middle
滑动窗口
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int begin = 0, end = 0, sum = 0, result = Integer.MAX_VALUE;
        while (end < nums.length) {
            sum += nums[end];
            while (sum >= target) {
                result = Math.min(result, end - begin + 1);
                sum -= nums[begin++];
            }
            end++;
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }

}
```

## 59 螺旋数组2 middle
left right top bottom代表四边，从左到右增加，从上到下增加
```java
class Solution {
    public int[][] generateMatrix(int n) {
        int left = 0, right = n - 1, top = 0, bottom = n - 1, count = 1;
        int[][] result = new int[n][n];
        while (true) {
            for (int j = left; j <= right; j++) result[top][j] = count++;
            if(++top > bottom) break;
            for (int i = top; i <= bottom; i++) result[i][right] = count++;
            if(--right < left) break;
            for (int j = right; j >= left; j--) result[bottom][j] = count++;
            if(--bottom < top) break;
            for (int i = bottom; i >= top; i--) result[i][left] = count++;
            if(++left > right) break;
        }
        return result;
    }
}
```