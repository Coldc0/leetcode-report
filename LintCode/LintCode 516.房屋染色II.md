# 房屋染色II

**序列型动态规划**
与LintCode 515类似，由于颜色数从3变为k，源代码经过简单修改也可AC。

    time:3142ms

````java
public class Solution {
    /**
     * @param costs: n x k cost matrix
     * @return: an integer, the minimum cost to paint all houses
     */
    public int minCostII(int[][] costs) {
        if(costs.length==0)
            return 0;
        int m=costs.length+1;
        int n=costs[0].length;
        int [][]f=new int[m][n];
        Arrays.fill(f[0],0);
        for (int i=1;i<m;i++ )
        {
            for (int j=0;j<n ;j++ )
            {
                f[i][j]=Integer.MAX_VALUE;
                for (int k=0;k<n ;k++ )
                {
                    if(j!=k)
                        f[i][j]=Math.min(f[i][j],f[i-1][k]+costs[i-1][j]);
                }
            }
        }
        int res=Integer.MAX_VALUE;
        for(int i=0;i<n;i++)
        {
            res=Math.min(res,f[m-1][i]);
        }
        return res;
    }
}
````
但可在**寻找最小值时进行优化**，将原算法复杂度$O(nk^2)$降为$O(nk)$。
`min1`为在`f[i-1]`行中的最小值，`min2`为在`f[i-1]`行中次小值，`f[i][j]`如果与`f[i-1]`行得最小值颜色相同，则加上`min2`，如果不同，直接加`min1`。
**注意一下维护最小值和次小值的方法.**

    time:1626ms

````java
public class Solution {
    /**
     * @param costs: n x k cost matrix
     * @return: an integer, the minimum cost to paint all houses
     */
    public int minCostII(int[][] costs) {
        if(costs.length==0)
            return 0;
        int m=costs.length+1;
        int n=costs[0].length;
        int [][]f=new int[m][n];
        Arrays.fill(f[0],0);
        int min1,min2,p1=0,p2=0;
        for (int i=1;i<m;i++ )
        {
            // calculate min1 min2
            min1=min2=Integer.MAX_VALUE;
            for(int j=0;j<n;j++)
            {
                if(f[i-1][j]<min1)
                {
                    min2=min1;
                    p2=p1;
                    min1=f[i-1][j];
                    p1=j;
                }
                else if(f[i-1][j]<min2)
                {
                    min2=f[i-1][j];
                    p2=j;
                }
            }
            for (int j=0;j<n ;j++ )
            {
               if(p1!=j)
                f[i][j]=min1+costs[i-1][j];
               else
                f[i][j]=min2+costs[i-1][j];
            }
        }
        int res=Integer.MAX_VALUE;
        for(int i=0;i<n;i++)
        {
            res=Math.min(res,f[m-1][i]);
        }
        return res;
    }
}
````
