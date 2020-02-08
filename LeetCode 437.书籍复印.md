# LintCode 437.书籍复印
**分析：** 如果要知道k个人需要多少时间抄写完所有的书，那么 **根据木桶原理，最后时间取决于最长的抄写员**，每个抄写员写第`i`本到第`j`本书，则需要时间`pages[i] + pages[i+1] + … + pages[j]。

**确定状态：** 第`t`个抄写员若从第`j`本到第`N-1`本书，抄写时间为`pages[j]+....+pages[N-1]`.并和前面`t-1`个人的抄写时间作比较。
**子问题：** 我们需要知道前面`t-1`个人抄写前面`0`到`j-1`本书的时间。
**状态：** 设`f[k][i]`为前k个抄写员最少需要多少时间抄完前`i`本书。
**转移方程：** $$f[k][i]=min_{j=0,...,i}\{max\{f[k-1][j], pages[j]+...+pages[i-1]\}\}$$

**初始条件和边界：**
1. `0`个抄写员抄`0`本书时间为`0`：`f[0][0]=0`。
2. `0`个抄写员抄`1`本及以上书时间为$\infty$：`f[0][1]=f[0][2]=....=f[0][n]=`$\infty$
3.` t`个抄写员`(t>0)`需要`0`时间抄`0`本书：`f[t][0]=0 (t>0)`

**时间复杂度：** $O(n^2k)$
**空间复杂度：** $O(nk)$
````java
public class Solution {
    /**
     * @param pages: an array of integers
     * @param k: An integer
     * @return: an integer
     */
    public int copyBooks(int[] pages, int k) {
        int  n=pages.length;
        if(n==0)
            return 0;
        if(k>n)
            k=n;
        int [][]f=new int[k+1][n+1];
        f[0][0]=0;
        for(int t=1;t<=n;t++)
        {
            f[0][t]=Integer.MAX_VALUE;
        }
        for(int t=1;t<=k;t++)
        {
            f[t][0]=0;
            for(int i=1;i<=n;i++)
            {
                f[t][i]=Integer.MAX_VALUE;
                int sum=0;
                for(int j=i;j>=0;j--)
                {
                    f[t][i]=Math.min(f[t][i],Math.max(f[t-1][j],sum));
                    if(j>0)
                        sum+=pages[j-1];
                }
            }
        }
        return f[k][n];
    }
    
}
````