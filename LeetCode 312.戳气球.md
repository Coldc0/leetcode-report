# LeetCode 312.戳气球
	说明:
	你可以假设 nums[-1] = nums[n] = 1，但注意它们不是真实存在的所以并不能被戳破。
    0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100

**动态规划**

**确定状态：**
**分析：** 所有N个气球被扎破
- **最后一步：** 一定有一个最后一个被扎破的气球编号为`i`。扎破`i`时，左边是气球`0(nums[-1])`，右边是气球`n+1(nums[n])`，获得金币`1*a[i]*1`
- 此时气球`1~i-1`以及`i+1~n`都已经被扎破，并且已经获得对应的金币。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200220172904106.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE4Njc2,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200220172928896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE4Njc2,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020022017293947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE4Njc2,size_16,color_FFFFFF,t_70)
- 子问题：要求`1~N`号气球最多获得的金币数：**需要知道`1~i-1`号最多获得的金币数**和 **`i+1~N`号气球最多获得的金币数** 
- **状态：设`f[i][j]`为扎破`i+1~j-1`号气球，最多获得的金币数。(`i`和`j`不能扎破)**

**转移方程：** 
$f[i][j]=max_{i<k<j}\{f[i][k]+f[k][j]+a[i]*a[k]*a[j]\}$
`f[i][j]`：扎破`i+1~j-1`号气球最多获得的金币数。
`f[i][k]`：扎破`i+1~k-1`号气球最多获得的金币数。
`f[k][j]`：扎破`k+1~j-1`号气球最多获得的金币数。
`a[i]*a[k]*a[j]`：最后扎破`k`号气球获得的金币数。

**初始条件：**
`f[0][1]=f[1][2]=...=f[N][N+1]=0`
**按照区间从小到大的顺序计算**
````java
class Solution {
    public int maxCoins(int[] nums) {
        int n=nums.length;
        if(n==0)
            return 0;
          //原数组中nums[-1]和nums[n]不合法，拷贝到新数组A中。
        int []A=new int[n+2];
        int i,j,k,len;
        A[0]=A[n+1]=1;
        for(i=0;i<n;i++)
        {
            A[i+1]=nums[i];
        }
        n+=2;
        int [][]f=new int[n][n];
        //区间长度为2时
        for(i=0;i<n-1;i++)
        {
            f[i][i+1]=0;
        }
        //从区间长度为3开始遍历
        for(len=3;len<=n;len++)
        {
            for(i=0;i<=n-len;i++)
            {
                j=i+len-1;
                f[i][j]=Integer.MIN_VALUE;
                for(k=i+1;k<j;k++)
                {
                    f[i][j]=Math.max(f[i][j],f[i][k]+f[k][j]+A[i]*A[k]*A[j]);
                }
            }
        }
        return f[0][n-1];
    }
}
````