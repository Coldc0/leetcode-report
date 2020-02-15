# LeetCode 5.最长回文子串
**解法一：**
**直接使用中心扩展判断回文串**，具体方法在：[LeetCode 132.分割回文串II](https://blog.csdn.net/qq_43118676/article/details/104212068)
再从大到小枚举长度，若区间[i,j+1)为回文串，输出。
````java
class Solution {
    private boolean[][] judge(char []ch)
    {
        int m=ch.length;
        boolean [][]f=new boolean[m][m];
        for(int i=0;i<m;i++)
            Arrays.fill(f[i],false);
        for(int c=0;c<m;c++)
        {
            for(int j=c,i=c;i>=0&&j<m&&ch[i]==ch[j];)
            {
                f[i][j]=true;
                i--;j++;
            }
        }
         for(int c=0;c<m;c++)
        {
            for(int j=c+1,i=c;i>=0&&j<m&&ch[i]==ch[j];)
            {
                f[i][j]=true;
                i--;j++;
            }
        }
        return f;
    }
    public String longestPalindrome(String s) {
        int n=s.length();
        char []ch=s.toCharArray();
        if(n==0)
            return "";
        boolean [][]sign=judge(ch);
        for(int len=n;len>0;len--)
        {
            for(int i=0,j;i<=n-len;i++)
            {
                j=i+len-1;
                if(sign[i][j])
                    return s.substring(i,j+1);
            }
        }
        return "";
    }
}
````
**解法二：动态规划**
**确定状态：**
如果子区间`[i+1..j-1]`是回文串，且`s[i]==s[j]`，那么子区间`[i....j]`就是回文串。
找到区间长度最大的回文串即所求。
**状态：设`f[i][j]`为区间`[i.....j]`是否为回文串。**
**转移方程：** ` f[i][j]=f[i+1][j-1]&&ch[i]==ch[j]`
**初始状态：** 
区间长度为1，即一个字母为回文串：`f[i][i]=true`
区间长度为2，两个字母相等的为回文串： `f[i][i+1]=true (ch[i]==ch[i+1])`
**计算顺序：** 按照区间从小到大计算。
````java
class Solution {
    public String longestPalindrome(String s) {
        char []ch=s.toCharArray();
        int n=ch.length;
        if(n==0)
            return "";
        boolean [][]f=new boolean[n][n];
        for(int i=0;i<n;i++)
        {
            f[i][i]=true;
        }
        for(int i=0;i<n-1;i++)
        {
            if(ch[i]==ch[i+1])
                f[i][i+1]=true;
        }
        for(int len=3;len<=n;len++)
        {
            for(int i=0,j;i<=n-len;i++)
            {
                j=i+len-1;
                f[i][j]=f[i+1][j-1]&&ch[i]==ch[j];
            }
        }
        for(int len=n;len>0;len--)
        {
            for(int i=0,j;i<=n-len;i++)
            {
                j=i+len-1;
                if(f[i][j])
                    return s.substring(i,j+1);
            }
        }
        return "";
    }
}
````