# LeetCode 279.完全平方数

**划分型动态规划**
**确定状态：** 设`f[i]`为在`i`时的最优方案，若存在一个完全平方数j，那么
`f[i+j]=f[i]+1`
**转移方程：** $f[i]=min_{1<=j^2<=i}\{f[i+j^2]+1\}$
**初始条件：** `f[0]=0`，最后答案为`f[n]`
````java
class Solution {
    public int numSquares(int n) {
        int []f=new int[n+1];
        f[0]=0;
        for(int i=1;i<=n;i++)
        {
            f[i]=Integer.MAX_VALUE;
            for(int j=1;j*j<=i;j++)
            {
                f[i]=Math.min(f[i],f[i-(j*j)]+1);
            }
        }
        return f[n];
    }
}
````