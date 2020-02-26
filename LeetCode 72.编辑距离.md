# LeetCode 72.编辑距离
双序列型
**确定状态：**
设`f[i][j]`为A前i个字符`A[0..i-1]`和B前j个字符`B[0..j-1]`的最小编辑距离。
有三种情况：
- A在最后插入`B[j-1]`
- A的最后一个被替换为`B[j-1]`
- A删掉最后一个字符
- A和B最后一个字符相等

**转移方程**
` f[i][j] = min{f[i][j-1]+1, f[i-1][j-1]+1, f[i-1][j]+1, f[i-1][j-1]|A[i1]=B[j-1]}`
**初始条件：**
一个空串和一个长度为L的串的最小编辑距离是L
- `f[0][j] = j (j = 0, 1, 2, …, n)`
- `f[i][0] = i (i = 0, 1, 2, …, m)`

````java
class Solution {
    public int minDistance(String word1, String word2) {
        char []s1=word1.toCharArray();
        char []s2=word2.toCharArray();
        int m=s1.length;
        int n=s2.length;
        int [][]f=new int[m+1][n+1];
        int i,j;
        for(i=0;i<=m;i++)
        {
            f[i][0]=i;
        }
        for(i=0;i<=n;i++)
        {
            f[0][i]=i;
        }
        for(i=1;i<=m;i++)
        {
            for(j=1;j<=n;j++)
            {
                f[i][j]=Math.min(f[i-1][j],Math.min(f[i][j-1],f[i-1][j-1]))+1;
                if(s1[i-1]==s2[j-1])
                    f[i][j]=Math.min(f[i][j],f[i-1][j-1]);
            }
        }
        return f[m][n]; 
    }
}
````