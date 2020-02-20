# LeetCode 516.最长回文子序列

**区间型动态规划**
**确定状态：**
最优策略产生的最长回文子串是`T`，长度`M`。
情况一：回文串长度为1，即一个字母。
情况二：回文串长度大于1，那么必定有`T[0]=T[M-1]`
**设`T[0]`是`S[i]`,`T[M-1]`是`S[j]`**，剩下的`T[1...M-2]`仍然是回文串，而且是`S[i+1...j-1]`的最长回文子序列。
**状态：** 设`f[i][j]`为`S[i..j]`的最长回文子序列的长度。

**转移方程：**
`f[i][j]=max{f[i+1][j],f[i][j-1],f[i+1][j-1]+2(S[i]==S[j])}`
**初始条件：**
一个字母也是一个长度为1的回文串：`f[0][0]=f[1][1]=...=f[N-1][N-1]=1`
如果`S[i]==S[i+1]`,`f[i][i+1]=2`否则`f[i][i+1]=1`
**计算顺序：**
==区间型动态规划：按照j-i从小到大的顺序计算==
长度1:`f[0][0],f[1][1]....f[n-1][n-1]`
长度2:`f[0][1],f[1][2]....f[n-2][n-1]`
....
长度N:`f[0][N-1]`
````java
class Solution {
    public int longestPalindromeSubseq(String s) {
        char []ch=s.toCharArray();
        int n=ch.length;
        int [][]f=new int [n][n];
        // length:1
        for(int i=0;i<n;i++)
        {
            f[i][i]=1;
        }
        //length:2
        for(int i=0;i<n-1;i++)
        {
            f[i][i+1]= (ch[i]==ch[i+1])?2:1;
        }
        //length >2  
        //Enumeration length
        for(int len=3;len<=n;len++)
        {
            //IMPORTANT: Give attention to the border is equal to n-length
            //Example: len=3
            //0,1,2,3....n-3,n-2,n-1
            //            |
            //          Finish
            for(int i=0,j;i<=n-len;i++)
            {
                j=i+len-1;
                //f[i][j]=MAX{f[i+1][j],f[i][j-1],f[i+1][j-1]+2}
                f[i][j]=Math.max(f[i+1][j],f[i][j-1]);
                if(ch[i]==ch[j])
                    f[i][j]=Math.max(f[i][j],f[i+1][j-1]+2);
            }
        }
        return f[0][n-1];
    }
}
````
**拓展：输出最长回文子串**
用`pi[][]`用来记录每一步的操作，从两侧向内扫描。
````java
    public int longestPalindromeSubseq(String s) {
        char []ch=s.toCharArray();
        int n=ch.length;
        int [][]f=new int [n][n];
        int [][]pi=new int[n][n];
        // length:1
        for(int i=0;i<n;i++)
        {
            f[i][i]=1;
        }
        //length:2
        for(int i=0;i<n-1;i++)
        {
            f[i][i+1]= (ch[i]==ch[i+1])?2:1;
        }
        //length >2
        //Enumeration length
        for(int len=3;len<=n;len++)
        {
            for(int i=0,j;i<=n-len;i++)
            {
                j=i+len-1;
                f[i][j]=Math.max(f[i+1][j],f[i][j-1]);
                if(f[i][j]==f[i+1][j])
                    pi[i][j]=0;
                else if(f[i][j]==f[i][j-1])
                    pi[i][j]=1;
                if(ch[i]==ch[j])
                {
                    f[i][j]=Math.max(f[i][j],f[i+1][j-1]+2);
                    pi[i][j]=2;
                }

            }
        }
        char []res=new char[f[0][n-1]];
        int p=0,q=f[0][n-1]-1;
        int i=0,j=n-1;
        while(i<=j)
        {
            if(i==j)
            {
                res[p]=ch[i];
                break;
            }
            if(i+1==j)
            {
                res[p]=ch[i];
                res[q]=ch[j];
                break;
            }
            if(pi[i][j]==0) i++;
            if(pi[i][j]==1) j--;
            if(pi[i][j]==2)
            {
                res[p++]=ch[i++];
                res[q--]=ch[j--];
            }
        }
        System.out.println(String.valueOf(res));
        return f[0][n-1];
    }
````
**记忆化搜索：**
将递归每个计算过的结果保存。
````java
class Solution {
    int [][]f=null;
    char []ch=null;
    int n=0;
    private void compute(int i,int j)
    {
        if(f[i][j]!=-1)
            return;
        if(i==j)
        {
            f[i][j]=1;
            return;
        }
        if(i+1==j)
        {
            f[i][j]=(ch[i]==ch[j])?2:1;
            return;
        }
        compute(i+1,j);
        compute(i,j-1);
        compute(i+1,j-1);
        f[i][j]=Math.max(f[i+1][j],f[i][j-1]);
        if(ch[i]==ch[j])
        {
            f[i][j]=Math.max(f[i][j],f[i+1][j-1]+2);
        }
    }
    public int longestPalindromeSubseq(String s) {
        ch=s.toCharArray();
        n=ch.length;
        if(n==0)
            return 0;
        if(n==1)
            return 1;
        f=new int [n][n];
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                f[i][j]=-1;
            }
        }
        compute(0,n-1);
        return f[0][n-1];
    }
}
````