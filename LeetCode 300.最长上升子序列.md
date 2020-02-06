# LeetCode 300.最长上升子序列
    执行用时 :11 ms, 在所有 Java 提交中击败了67.46%的用户
**坐标型动态规划**
方法一(时间复杂度：$O(n^2)$)
**确定状态：** 以`nums[i]`为结尾的LIS，一定是以`nums[j](j<i)`为结尾的LIS再加上`nums[i]`。
**转移方程：** 设f[i]为以下标为`i`为结尾的LIS的长度
 $f[i]=max\{1,f[j](nums[j]<nums[i])+1\}$

````java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int m=nums.length;
        if(m==0)
            return 0;
        int []f=new int[m];
        for(int i=0;i<m;i++)
        {
            f[i]=1;
            for(int j=0;j<i;j++)
            {
                if(nums[i]>nums[j])
                    f[i]=Math.max(f[j]+1,f[i]);
            }
        }
        int res=1;
        for(int i=0;i<m;i++)
        {
            res=Math.max(res,f[i]);
        }
        return res;
    }
}
````