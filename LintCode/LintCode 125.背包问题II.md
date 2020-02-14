# LintCode 125.背包问题II

**背包问题**

**确定状态：**
- 对于每个总重量，我们知道对应的最大价值是多少，就能知道答案。
- **最后一步：** 最后一个物品：($重量：A_{N-1}$，价值：$V_{N-1}$)是否进入背包。
- **情况一：** 如果前$N-1$个物品能拼出$W$，最大总价值是$V$，前$N$个物品也能拼出$W$并且总价值是$V$
- **情况二：** 如果前N-1个物品能拼出$W- A_{N-1}$，最大总价值是V，则再加上最后一个物品(重量$A_{N-1}$, 价值$V_{N-1)}$，能拼出$W$，总价值是$V+V_{N-1}$
- **状态：** `f[i][w]` **用前`i`个物品**拼出重量`w`时最大总价值(`-1`表示不能拼出`w`)
  
**转移方程：**`f[i][w]=max(f[i-1][w],f[i-1][w-A[i-1]]+V[i-1])`

**初始条件和边界：**
在转移方程中：`w>A[i-1]&&f[i-1][w-A[i-1]]!=-1` 
0个物品可以拼出重量0，最大总价值为0：`f[0][0]=0`
0个物品不能拼出大于0的重量：`f[0][1..M]=-1`

````java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @param V: Given n items with value V[i]
     * @return: The maximum value
     */
    public int backPackII(int m, int[] A, int[] V) {
        // write your code here
        int n=A.length;
        if(n==0)
            return 0;
        int [][]f=new int[n+1][m+1];
        Arrays.fill(f[0],-1);
        f[0][0]=0;
        for(int i=1;i<=n;i++)
        {
            for(int j=0;j<=m;j++)
            {
                f[i][j]=f[i-1][j];
                if(j>=A[i-1])
                    if(f[i-1][j-A[i-1]]!=-1)
                        f[i][j]=Math.max(f[i][j],f[i-1][j-A[i-1]]+V[i-1]);
            }
        }
        int res=0;
        for(int i=0;i<=m;i++)
        {
            res=Math.max(res,f[n][i]);
        }
        return res;
    }
}
````