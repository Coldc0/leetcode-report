# LintCode 563.背包问题V

**确定状态：**
**与LintCode 92题一样，有两种情况：**
**(最后一个物品重量为`nums[N-1]`)**
**情况一：** 如果前$N-1$个物品能拼出$W$，当然前N个物品也能拼出$W$。
**情况二：** 如果前N-1个物品能拼出$W- nums_{N-1}$ ，再加上最后的物品$nums_{N-1}$ ，拼出$W$。
**状态与92题有所不同：** 设`f[i][j]`为前`i`个物品拼出重量`j`的方案数。
**转移方程：** `f[i][j]=f[i-1][j]+f[i-1][j-nums[n-1]]`
**初始条件：**
`f[0][0]=1`；`j>A[n-1]`
````java
public class Solution {
    /**
     * @param nums: an integer array and all positive numbers
     * @param target: An integer
     * @return: An integer
     */
    public int backPackV(int[] nums, int target) {
        int n=nums.length;
        int [][]f=new int [2][target+1];
        int old=1,now=0;
        f[0][0]=1;
        for(int i=1;i<=n;i++)
        {
            old=now;
            now=1-old;
            for(int j=0;j<=target;j++)
            {
                f[now][j]=f[old][j];
                if(j>=nums[i-1])
                    f[now][j]+=f[old][j-nums[i-1]];
            }
        }
        return f[now][target];
    }
}
````

**空间优化：**
由图可以看出，对于每个`f[i][j]`,都用`f[i-1][j]`或`f[i-1][j-nums[i-1]]`。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020021215045653.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE4Njc2,size_16,color_FFFFFF,t_70)
可以将每个结果存至`f[i-1][j]`中，最终可优化为一维数组，**从后往前更新**。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212151249426.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE4Njc2,size_16,color_FFFFFF,t_70)
````java
public class Solution {
    /**
     * @param nums: an integer array and all positive numbers
     * @param target: An integer
     * @return: An integer
     */
    public int backPackV(int[] nums, int target) {
        int n=nums.length;
        int []f=new int [target+1];
        f[0]=1;
        for(int i=1;i<=n;i++)
        {
            for(int j=target;j>=0;j--)
            {
                if(j>=nums[i-1])
                   f[j]+=f[j-nums[i-1]];
            }
        }
        return f[target];
    }
}
````
