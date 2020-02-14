# LintCode 92.背包问题
**背包问题**
对于每个总重量，我们能知道有没有方案能做到，就可以解决。
**背包问题中，数组大小和总承重有关**

**确定状态：**
需要知道N个物品是否能拼出重量$W (W =0, 1, …, M)$。
**最后一步：最后一个物品（重量$A_{N-1}$）是否进入背包**，**情况一：**如果前$N-1$个物品能拼出$W$，当然前N个物品也能拼出$W$。**情况二：**如果前N-1个物品能拼出$W- A_{N-1}$ ，再加上最后的物品$A_{N-1}$ ，拼出$W$。
**状态：**设`f[i][w]=能否用前i个物品拼出重量w`
**转移方程：** `f[i][w]=f[i-1][w] || f[i-1][w-A[i-1]]`
**初始条件和边界情况：**
i个物品可以拼出重量0：`f[i][0]=true`
0个物品不能拼出大于0的重量：`f[0][1...M]=false`
**边界**：`w>A[i-1]`时才能使用`f[i-1][w-A[i-1]`
````java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        int n=A.length;
        if(m==0||n==0)
            return 0;
        boolean [][]f=new boolean[n+1][m+1];
        Arrays.fill(f[0],false);
        f[0][0]=true;
        for(int i=1;i<=n;i++)
        {
            f[i][0]=true;
            for(int j=1;j<=m;j++)
            {
                if(j>=A[i-1])
                f[i][j]=f[i-1][j]||f[i-1][j-A[i-1]];
                else
                f[i][j]=f[i-1][j];
            }
        }
        int res=0;
        for(int i=m;i>=0;i--)
        {
            if(f[n][i])
            {
                res=i;
                break;
            }
        }
        return res;
    }
}
````
