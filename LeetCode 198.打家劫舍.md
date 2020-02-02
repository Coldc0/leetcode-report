# LeetCode 198.打家劫舍

    执行用时 :0 ms, 在所有 Java 提交中击败了100.00%的用户

**序列型动态规划**
**最后一步：** 首先设`f[i]`为前`i-1`项的最优解(`f[]`数组索引从1开始，0作为初始条件搁置)，要知道`f[i]`，不妨设不偷第`i`个房子，那么最优解就为`f[i]`，若偷，那么最优解为`f[i-1]+nums[i-2]`，两种情况取最大的即可。
**子问题：** 由求`f[i]`简化到求`f[i-1]`。
**转移方程：** $f[i]=max\{f[i],f[i-1]+nums[i-2]\}$
**初始条件：** 作为序列型动态规划，很多情况`f[0]`都作为初始条件：令`f[0]=0`，`f[1]=nums[0]`，由`i-2>0`循环从2开始。
````java
class Solution {
    public int rob(int[] nums) {
        int m=nums.length;
        if(m==0)
            return 0;
        int []f=new int[m+1];
        f[0]=0;
        f[1]=nums[0];
        for(int i=2;i<=m;i++)
        {
            f[i]=Math.max(f[i-1],f[i-2]+nums[i-1]);
        }
        return f[m];
    }
}
````