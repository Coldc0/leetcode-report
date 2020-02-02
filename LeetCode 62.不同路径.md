#62.不同路径
    执行用时 :1 ms, 在所有 Java 提交中击败了63.54%的用户

**坐标型动态规划**
**最后一步：** 当到右下角时，最后一步不是向右就是向下。
设右下角坐标为 (m-1,n-1),那么前一步坐标为(m-2,n-1)或者是(m-1,n-2)，
**子问题：** 求 (m-2,n-1)的路径数 X 和 (m-1,n-2) 的路径数 Y，X+Y 即为 (m-1,n-1) 的路径数。
**转移方程：**$\begin{cases}
    f[i][j]=f[i-1][j]+f[i][j-1](i>0)\\
    f[i][j]=0(i==0\&\&j==0)\\
    f[i][j]=1(i==0||j==1)
\end{cases}$

**初始条件**：$f[0][0]=1$
````java
class Solution {
    public int uniquePaths(int m, int n) {
        int f[][]=new int[m][n];
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(i==0||j==0)
                    f[i][j]=1;
                else 
                    f[i][j]+=f[i-1][j]+f[i][j-1];
            }
        }
        return f[m-1][n-1];
    }
}
````
