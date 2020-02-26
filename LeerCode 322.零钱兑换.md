# LeetCode 322.零钱兑换

**确定状态：**
设`f[n]`为当金额为`n`时的硬币数。
最后一步：遍历所有硬币面值，存在一枚面值`i`的硬币使得`f[n-i]`最小
**转移方程：**
$f[i]=min\{f[i-coins]+1\}$
````java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int n=coins.length;
        int []f=new int[amount+1];
        f[0]=0;
        for(int i=1;i<=amount;i++)
        {
            f[i]=Integer.MAX_VALUE;
            for(int j=0;j<n;j++)
            {
                if(i>=coins[j])
                {
                    if(f[i-coins[j]]==-1)
                        continue;
                    f[i]=Math.min(f[i],f[i-coins[j]]+1);
                }
            }
            if(f[i]==Integer.MAX_VALUE)
            f[i]=-1;
        }
        return f[amount];
    }
}
````