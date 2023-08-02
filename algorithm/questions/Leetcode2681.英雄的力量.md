### Leetcode2681.英雄的力量

[2681. 英雄的力量 - 力扣（LeetCode）](https://leetcode.cn/problems/power-of-heroes/)

#### 1.动态规划 + 前缀和

​	考虑如何计算一个nums [i]，0 < i < n，由于数组已经排好序，所以以nums [i] 结尾的子序列的最大值	就是nums [i]，所以可以只考虑所有子序列的最小值之和。设dp [ j ] 表示以 nums [ j ] 结尾的所有子序	列最小值之和，得到以下式子

​                                                     	![image-20230801220232039](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230801220232039.png)

​	那么以nums [i] 结尾的全部子序列的英雄组力量和为 nums [i] * nums [i] * dp [i]

​	计算dp [i] 需要 O (n) 的复杂度，总体复杂度为 n² ，会超时。所以可以使用前缀和数组prefix 进行优化

​																	prefix [i+1] = prefix[i] + dp[i]

​	初始式就可以优化为：

​															 ![image-20230801220713005](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230801220713005.png)	

```java
class Solution {
    public int sumOfPower(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        // 表示以i结尾的所有子序列的最小值之和
        int[] dp = new int[n];
        int[] prefix = new int[n + 1];
        int mod = 1000000007;
        int res = 0;
        for (int i = 0 ; i < n ; i ++) {
            dp[i] = (nums[i] + prefix[i]) % mod;
            prefix[i + 1] = (prefix[i] + dp[i]) % mod;
            res = (int)((res + (long) nums[i] * nums[i] % mod * dp[i]) % mod);
            if (res < 0) res += mod;
        }
        return res;
    }
}
```

#### 2.滚动数组优化版

​	因为dp [i] 和 prefix [i] 的计算只与前一个状态有关，所以可以使用滚动数组来优化

```java
class Solution {
    public int sumOfPower(int[] nums) {
        Arrays.sort(nums);
        int dp = 0, preSum = 0;
        int res = 0, mod = 1000000007;
        for (int i = 0; i < nums.length; i++) {
            dp = (nums[i] + preSum) % mod;
            preSum = (preSum + dp) % mod;
            res = (int) ((res + (long) nums[i] * nums[i] % mod * dp) % mod);
            if (res < 0) {
                res += mod;
            }
        }
        return res;
    }
}
```

