#LintCode 397.最长上升连续子序列
求最长上升连续子序列的长度，但该题与一般情况不同，**该题认为逆序也成立**如 [5,4,3,2,1,6] , [5,4,3,2,1]从右向左看也为最长上升连续子序列，故对于这种情况直接将数组反转处理，再次求解。
**坐标型动态规划**
**最后一步：** 如果a[j]是该序列中的话，那么a[j-1]一定是该序列中的，且a[j]>a[j-1]。
**子问题：** 由求以a[j]为结尾的LCIS转为求以a[j-1]为结尾的LCIS。
**转移方程:** $f[i]=max\{1,f[i-1]+1(i>1)\&\&f[i-1]<f[i]\}$
````java
public class Solution {
    /**
     * @param A: An array of Integer
     * @return: an integer
     */
    public int LCS(int A[])
    {
        int m=A.length;
        int []f=new int[m];
        f[0]=1;
        for(int i=1;i<m;i++)
        {
            f[i]=1;
            if(A[i]>A[i-1])
                f[i]=f[i-1]+1;
        }
        int res=Integer.MIN_VALUE;
        for(int i=0;i<m;i++)
            res=Math.max(res,f[i]);
        return res;
    }
    public int longestIncreasingContinuousSubsequence(int[] A) {
        if(A.length==0)
            return 0;
        int l=LCS(A);
        for(int i=0,j=A.length-1;i<j;i++,j--)
        {
            int t=A[i];
            A[i]=A[j];
            A[j]=t;
        }
        int r=LCS(A);
        return Math.max(l,r);
    }
}
````