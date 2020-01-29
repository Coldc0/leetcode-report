#152.乘积最大子序列
    执行用时 :3 ms, 在所有 Java 提交中击败了41.29%的用户
考虑最后一步：  
对于最优的策略，一定有最后一个元素a[j];
第一种情况：最优的策略就是{a[j]}，答案就是a[j]
第二种情况：连续子序列长度大于1，那么最优策略中a[j]前一个元素肯定是a[j-1].
**注意**:如果a[j]是正数，需要a[j-1]结尾的连续子序列**最大**；如果a[j]是负数，需要a[j-1]结尾的连续子序列**最小**。
因此，我们需要知道a[j-1]结尾的**最小乘积和最大乘积**。

**状态**：设 f[j]= 以nums[j]结尾的连续子序列最大乘积；设 g[j]= 以nums[j]结尾的连续子序列最小乘积。

**转移方程**：$$f[j] =max\{nums[j],max\{nums[j]*f[j-1],nums[j]*g[j-1]\}\}$$

````java
public class Solution {
   
    public int maxProduct(int[] nums) {
        int f[]=new int[nums.length];
        int g[]=new int[nums.length];
        int res=Integer.MIN_VALUE;
        for(int i=0;i<nums.length;i++)
        {
            f[i]=nums[i];
            g[i]=nums[i];
            if(i>0){
                f[i]=Math.max(f[i],Math.max(f[i]*f[i-1],f[i]*g[i-1]));
                g[i]=Math.min(g[i],Math.min(g[i]*f[i-1],g[i]*g[i-1]));
            }
            res= Math.max(res,f[i]);
        }
        return res;

    }
}
````
