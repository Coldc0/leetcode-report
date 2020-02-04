# LeetCode 213.打家劫舍II
    执行用时 :0 ms, 在所有 Java 提交中击败了100.00%的用户

**序列型动态规划**
本题和**198.打家劫舍**对比，房子由一序列升级到圈，那么需要将圈拆开，还可利用前题的题解。圈与序列不同的情况在于**第一个房子**，可分为两种情况：
**偷第一个房子，那么不能偷第n-1个房子：** 那么只需要考虑**第1到n-2个房子**。
**不偷第一个房子** 那么只需要考虑**第2到n-1个房子**。
**所以只需要用198题对两种情况分别求解即可。**

用`public int calc(int[] nums,int l,int r)`函数包装下198题的题解。
````java
class Solution {
    public int rob(int[] nums) {
        int m=nums.length;
        if(m==0)
            return 0;
        if(m==1)
            return nums[0];
        int res=Integer.MIN_VALUE;
        res=Math.max(res,calc(nums,0,m-2));
        res=Math.max(res,calc(nums,1,m-1));
        return res;
    }
    public int calc(int[] nums,int l,int r){
        int m=r-l+1;
        if(m==0)
            return 0;
        int []f=new int[m+1];
        f[0]=0;
        f[1]=nums[l];
        for(int i=2;i<=m;i++)
        {
            f[i]=Math.max(f[i-1],f[i-2]+nums[i+l-1]);
        }
        return f[m];
    }
}
````