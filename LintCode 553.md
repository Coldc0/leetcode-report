#LintCode 553. Bomb Enemy
    Total runtime 560 ms. Your submission beats 41.20% Submissions!

**坐标型动态规划**
注意题目要求不允许将炸弹放在敌人和墙上。
**最后一步：** 首先规定一个方向：向上，显然若 $grid[i][j]$ 没有敌人，等于 $grid[i-1][j]$ ，若有敌人则再加1，其他方向同理。
**子问题** 转为求 $grid[i-1][j]$向上的敌人数。
**转移方程:** 
$$\begin{cases}
    up[i][j]=up[i-1][j](grid[i][j]是空地)\\
    up[i][j]=up[i-1][j]+1(grid[i][j]是敌人)\\
    up[i][j]=0(grid[i][j]是墙)\\
\end{cases}$$
**其他方向以此类推** 
````java
public class Solution {
    /**
     * @param grid: Given a 2D grid, each cell is either 'W', 'E' or '0'
     * @return: an integer, the maximum enemies you can kill using one bomb
     */
    public int maxKilledEnemies(char[][] grid) {
        int m=grid.length;
        if(m==0)
            return 0;
        int n=grid[0].length;
        int[][] up=new int[m][n];
        int[][] down=new int[m][n];
        int[][] left=new int[m][n];
        int[][] right=new int[m][n];
        //up
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                up[i][j]=0;
                if(grid[i][j]=='W')
                    continue;
                if(grid[i][j]=='E')
                    up[i][j]++;
                if(i>0)
                    up[i][j]+=up[i-1][j];
            }
        }
        //left
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                left[i][j]=0;
                if(grid[i][j]=='W')
                    continue;
                if(grid[i][j]=='E')
                    left[i][j]++;
                if(j>0)
                    left[i][j]+=left[i][j-1];
            }
        }
         //down
        for(int i=m-1;i>=0;i--)
        {
            for(int j=0;j<n;j++)
            {
                down[i][j]=0;
                if(grid[i][j]=='W')
                    continue;
                if(grid[i][j]=='E')
                    down[i][j]++;
                if(i<m-1)
                    down[i][j]+=down[i+1][j];
            }
        }
        //right
        for(int i=0;i<m;i++)
        {
            for(int j=n-1;j>=0;j--)
            {
                right[i][j]=0;
                if(grid[i][j]=='W')
                    continue;
                if(grid[i][j]=='E')
                    right[i][j]++;
                if(j<n-1)
                    right[i][j]+=right[i][j+1];
            }
        }
        //result
        int res=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]=='0')
                    res=Math.max(res,up[i][j]+down[i][j]+left[i][j]+right[i][j]);
            }
        }
        return res;
    }
}
````