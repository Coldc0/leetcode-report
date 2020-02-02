#64.最小路径和
    执行用时 :3 ms, 在所有 Java 提交中击败了83.83%的用户
**坐标型动态规划**
**最后一步：** 设右下角为 $grid[i][j]$，最小和储存在$f[i][j]$中，右下角的最小和，一定是$Min\{$$f[i][j-1]$，$f[i-1][j]\}$再加上$grid[i][j]。

**子问题：** 原问题从求$f[i][j]$转化为求$f[i][j-1]$和$f[i-1][j]$。

**转移方程：** $f[i][j]=Min\{f[i][j-1]+f[i-1][j]\}+grid[i][j](i>0)(j>0)$

**初始条件：** f[0][0]=grid[0][0]
````java
class Solution {
    public int minPathSum(int[][] grid) {
        int m=grid.length;
        int n=grid[0].length;
        if(m==0)
            return 0;
        int f[][]=new int[m][n];
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                f[i][j]=grid[i][j];
                if(i==0&&j==0)
                    continue;
                else if(i==0)
                    f[i][j]+=f[i][j-1];
                else if (j==0)
                    f[i][j]+=f[i-1][j];
                else
                    f[i][j]+=Math.min(f[i-1][j],f[i][j-1]);
            }
        }
        return f[m-1][n-1];
    }
}
````
**空间优化：滚动数组**
由转移方程可知，只会用到$f[i]$和$f[i-1]$行，因此数组大小可设为$f[2][n]$
，old 和 now 变量用于实现滚动。
````java
class Solution {
    public int minPathSum(int[][] grid) {
        int m=grid.length;
        int n=grid[0].length;
        if(m==0)
            return 0;
        int f[][]=new int[2][n];
        int old=0,now=1;
        for(int i=0;i<m;i++)
        {
            old=now;
            now=1-now;
            for(int j=0;j<n;j++)
            {
                f[now][j]=grid[i][j];
                if(i==0&&j==0)
                    continue;
                else if(i==0)
                    f[now][j]+=f[now][j-1];
                else if (j==0)
                    f[now][j]+=f[old][j];
                else
                    f[now][j]+=Math.min(f[old][j],f[now][j-1]);
            }
        }
        return f[now][n-1];
    }
}
````
**题目拓展：输出最小和路径**
建立一个数组 pi，pi记录每个点上一步是往下走(pi[ i ][ j ]=0)还是往右走(pi[ i ][ j ]=1)，最后从pi数组遍历得到整条路径。

````java
class Solution {
    public int minPathSum(int[][] grid) {
        int m=grid.length;
        int n=grid[0].length;
        if(m==0)
            return 0;
        int f[][]=new int[m][n];
        int pi[][]=new int [m][n];  
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                f[i][j]=grid[i][j];
                if(i==0&&j==0)
                    continue;
                else if(i==0)
                {
                    f[i][j]+=f[i][j-1];
                    pi[i][j]=1;
                }
                else if (j==0)
                {
                    f[i][j]+=f[i-1][j];
                    pi[i][j]=0;
                }
                else
                {
                     int t=Math.min(f[i-1][j],f[i][j-1]);
                     if(t==f[i-1][j])
                        pi[i][j]=0;
                     else
                        pi[i][j]=1;
                    f[i][j]+=t;
                }
            }
        }
        int []path=new int[m+n-1];
        int x=m-1;
        int y=n-1;
        for(int i=0;i<m+n-1;i++)
        {
            path[i]=grid[x][y];
            if(pi[x][y]==0)
            {
                --x;
            }
            else
            {
                --y;
            }
        }
        for(int i=m+n-2;i>=0;i--)
        {
            System.out.print(path[i]+" ");
        }
        return f[m-1][n-1];
    }
}
````