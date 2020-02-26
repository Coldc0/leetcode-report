# LeetCode 1143.最长公共子序列(LCS)
双序列型
**确定状态：**
设字符串为`s1`和`s2`，长度分别为`m`和`n`。
此时LCS长度为`l`
存在三种情况：
- `s1[i]`此时不在LCS中，是多余的
- `s2[j]`此时不在LCS中，是多余的
- `s1[i]==s2[j]`，此时`l=l+1`

**状态：设`f[i][j]`为`s1`前`i`个字符和`s2`前`j`个字符的LCS长度**
**转移方程：**
`f[i][j]=max{ f[i-1][j],f[i][j-1], f[i-1][j-1]+1(s1[i]==s2[j]) }`
**初始条件：**
和一般的序列型相同，前0个都为0，`f[i][0]=0,f[0][j]=0`

````java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        char []s1=text1.toCharArray();
        char []s2=text2.toCharArray();
        int m=s1.length;
        int n=s2.length;
        if(m==0||n==0)
            return 0;
        int [][]f=new int[m+1][n+1];
        int i,j;
        for(i=1;i<=m;i++)
        {
            for(j=1;j<=n;j++)
            {
                f[i][j]=Math.max(f[i-1][j],f[i][j-1]);
                if(s1[i-1]==s2[j-1])
                    f[i][j]=Math.max(f[i][j],f[i-1][j-1]+1);
            }
        }
        return f[m][n];
    }
}
````