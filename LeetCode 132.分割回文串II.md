# LeetCode 132.分割回文串II
    执行用时 :13 ms, 在所有 Java 提交中击败了71.11%的用户
    
   ### 判断回文串：
   **回文串分两种：长度为奇数、长度为偶数**
   **考虑生成回文串**，**长度为奇数的回文串会有一个数作为中心，沿两侧扩展。，长度为偶数的回文串会有一条线作为对称轴，向两侧扩展。**
   那么找到所有的回文串，将**每个字符按奇数长度和偶数长度各生成一轮回文串**，新建一个二维数组 `sign[i][j]`**，表示从`i`到`j`是否是回文串
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200207171836370.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE4Njc2,size_16,color_FFFFFF,t_70)
   ## 确定状态：
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200207164053504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE4Njc2,size_16,color_FFFFFF,t_70)
   关注最优策略中最后一段回文串，设为`S[j..N-1]`，设`f[j]`为在`j`之前最少可以划分多少个回文串
   ### 子问题：
   由求`S[0..N-1]`的最优策略分解为求`S[0..j-1]`的最优策略。
   ### 转移方程：
  $f[i] = min_{j=0,…,i-1}\{f[j] + 1(S[j..i-1]是回文串)\}$ 
  ---->$f[i] = min_{j=0,…,i-1}\{f[j] + 1(sign[j][i-1] = True)\}$
  ### 初始条件和边界：
`f[0]=0`，最后答案为`f[n]-1`，因为原题求的是分割次数，`f[]`代表的是回文串个数。
````java
class Solution {
    private boolean[][] judge(char []ch)
    {
        int m=ch.length;
        boolean [][]f=new boolean[m][m];
        for(int i=0;i<m;i++)
            Arrays.fill(f[i],false);
        for(int c=0;c<m;c++)\\奇数长度回文串
        {
            for(int j=c,i=c;i>=0&&j<m&&ch[i]==ch[j];)
            {
                f[i][j]=true;
                i--;j++;
            }
        }
         for(int c=0;c<m;c++)\\偶数长度回文串
        {
            for(int j=c+1,i=c;i>=0&&j<m&&ch[i]==ch[j];)
            {
                f[i][j]=true;
                i--;j++;
            }
        }
        return f;
    }
    public int minCut(String s) {
        char []ch=s.toCharArray();
        int m=ch.length;
        if(m==0)
            return 0;
        boolean [][]sign=judge(ch);
        int []f=new int[m+1];
        f[0]=0;
        for(int i=1;i<=m;i++)\\dp
        {
            f[i]=Integer.MAX_VALUE;
            for(int j=0;j<i;j++)
            {
                if(sign[j][i-1])
                    f[i]=Math.min(f[i],f[j]+1);
            }
        }
        return f[m]-1;
    }
}
````
  
   
