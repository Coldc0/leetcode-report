#63.不同路径 II
    执行用时 :1 ms, 在所有 Java 提交中击败了89.95%的用户
与62题类似，只是存在障碍，在障碍上 `f[i]=0`,**但在转移方程上对于`i==0`和`j==0`的处理与之前不同**，不能直接全设为 1，因为若`f[0][1]`存在障碍，那么它的右侧如`f[0][2]`等也将为0，同理在`j==0`时情况类似。
解决方法是将**所有点都初始化为0**，脱离上题全设为1的总结性的结论，第一行上的点它的上一步只能右移，故只需`f[i][j]+=f[i][j-1]`，同理第一列`f[i][j]+=f[i-1][j]`。

**转移方程：** `if (obstacle[i][j]==false)` $\begin{cases}
    f[i][j]=f[i-1][j]+f[i][j-1](i>0)\\
    f[i][j]=0(i==0\&\&j==0)\\
    f[i][j]+=f[i][j-1](i==0)\\
    f[i][j]+=f[i-1][j](j==0)\\
\end{cases}$
`else `$f[i][j]=0$

````java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m=obstacleGrid.length;
        int n=obstacleGrid[0].length;
        int f[][]=new int[m][n];
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                f[i][j]=0;
                if(obstacleGrid[i][j]!=1)
                {
                    if(i==0&&j==0)
                    {
                        f[i][j]=1;
                        continue;
                    }
                    if(i==0)
                    {
                        f[i][j]+=f[i][j-1];
                        continue;
                    }
                    if(j==0)
                    {
                        f[i][j]+=f[i-1][j];
                        continue;
                    }
                    f[i][j]+=f[i-1][j]+f[i][j-1];
                }
            }
        }
        return f[m-1][n-1];
    }
}
````