# LeetCode 97.交错字符串
双序列型动态规划
**确定状态：**
判断`s1`(长度为`m`)，与`s2`(长度为`n`)是否交错生成`s3`(长度为`p`)
- 若`m+n!=p`，直接返回`false`
- `s3[k]`不是等于`s1[i-1]`就是等于`s2[j-1]`
- `k=i+j-1`
**状态：**
设`f[k][i][j]`为`s3`中前`k`个字符是否由`s1`中前`i`个字符或`s2`中前`j`个字符组成。
**其中，`k=i+j-1`，优化为`f[i][j]`**

**转移方程：**
`f[i][j]=((f[i-1][j] and s1[i]==s3[k] ) or f[i][j-1] and s2[i]==s3[k])`
**初始条件和边界:**
空串由`s1`的空串和`s2`的空串交错形成`f[0][0] = True`
`i==0`或者`j==0`只考虑一种情况即可。

````java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        char []str1=s1.toCharArray();
        char []str2=s2.toCharArray();
        char []str3=s3.toCharArray();
        int m=str1.length;
        int n=str2.length;
        int p=str3.length;
        if((m+n)!=p)
            return false;
        boolean [][]f=new boolean[m+1][n+1];
        int i,j;
        for(i=0;i<=m;i++)
        {
            for(j=0;j<=n;j++)
            {
                if(i==0&&j==0)
                {
                    f[i][j]=true;
                    continue;
                }
                if(j>0)
                    f[i][j]|=(str2[j-1]==str3[i+j-1])&&f[i][j-1];
                if(i>0)
                    f[i][j]|=(str1[i-1]==str3[i+j-1])&&f[i-1][j];
            }
        }
        return f[m][n];
    }
}
````


