#LintCode 515. 房屋染色

**序列型动态规划**
**最后一步：** 想要知道第 n 个房子的最小花费，那么就需要知道第 n-1 个房子的颜色和最小花费，若第 n-1 个房子颜色为红，那么第n个房子就是蓝或绿，选择两者最小的。
**子问题：** 求第n-1个房子的颜色和最小花费，就要知道第n-2个房子的最小花费。
**转移方程：**
$f[i][j]=min\{f[i][j],f[i][k]+cost[i][j]\}$
求f[i][j]，只需要遍历前面房子所有颜色的同时，加上另外两种颜色在该房子的花费，取最小值。
**注意不要遗漏cost[i][j]，即为第i个房子颜色j的花费。**
**初始条件：** $f[0][0]=0,f[0][1]=0,f[0][2]=0$
````java
public class Solution {
    /**
     * @param costs: n x 3 cost matrix
     * @return: An integer, the minimum cost to paint all houses
     */
    public int minCost(int[][] costs) {
        if(costs.length==0)
            return 0;
        int m=costs.length+1;
        int n=costs[0].length;
        int [][]f=new int[m][n];
        f[0][0]=0;f[0][1]=0;f[0][2]=0;
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
        return Math.min(Math.min(f[m-1][0],f[m-1][1]),f[m-1][2]);
}
}
````

