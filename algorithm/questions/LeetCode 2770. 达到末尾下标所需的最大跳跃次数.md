### LeetCode 2770. 达到末尾下标所需的最大跳跃次数

![image-20230726113411486](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230726113411486.png)

![image-20230726113425520](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230726113425520.png)

![image-20230726113432803](C:\Users\23694\AppData\Roaming\Typora\typora-user-images\image-20230726113432803.png)

```java
class Solution {
    public int maximumJumps(int[] nums, int target) {
        int n = nums.length;
        // 跳到第i个点所需要的跳跃次数
        int[] dp = new int[n];
        // 初始化
        dp[0] = 1;
        for (int i = 1 ; i < n ; i ++){
            for (int j = 0 ; j < i ; j ++){
                // 如果符合要求差值小于等于target并且j点是已经跳过的
                if (Math.abs(nums[i] - nums[j]) <= target && dp[j] != 0) dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        //      到达n-1坐标 返回值 否则返回-1
        return dp[n-1] == 0 ? -1 : dp[n-1] - 1;
    }
}
```

