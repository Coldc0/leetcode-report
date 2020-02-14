# LeetCode 377.组合总和IV
    执行用时 :2 ms, 在所有 Java 提交中击败了90.17%的用户
**背包问题：**
**注意：我们需要求出有多少个组合的和是Target,组合中的数字可以按不同的顺序：1+1+2与1+2+1算两种组合**

**确定状态：**
如果最后一个物品重量是$A_0$, 则要求有多少种组合能拼成$Target – A_0$
如果最后一个物品重量是$A_1$, 则要求有多少种组合能拼成$Target – A_1$
$.....$
如果最后一个物品重量是$A_{N-1}$, 则要求有多少种组合能拼成$Target – A_{N-1}$
**任何一个正确的组合中，所有物品总重量是Target.**
**如果最后一个物品重量是K，则前面的物品重量是Target-K**
**状态：** 设`f[i]`有多少种组合拼出重量`i`。
**转移方程：** 
`f[i]=f[i-A[0]]+f[i-A[1]]+....+f[i-A[N-1]]`。
**初始条件：**
`f[0]=1`,如果`i<A[j]`则对应的`f[i-A[j]]`不加入`f[i]`。
````java
class Solution {
    public int combinationSum4(int[] nums, int target) {
         int n=nums.length;
        if(n==0)
            return 0;
        int []f=new int[target+1];
        f[0]=1;
        for(int i=1;i<=target;i++)
        {
            f[i]=0;
            for(int j=0;j<n;j++)
            {
                if(i>=nums[j])
                    f[i]+=f[i-nums[j]];
            }
        }
        return f[target];
    }
}
````
