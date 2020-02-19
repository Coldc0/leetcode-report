# LeetCode 87.扰乱字符串
	给定一个字符串 s1，我们可以把它递归地分割成两个非空子字符串，从而将其表示为二叉树。
**注意题目描述： 不一定是从中间等分。**

**动态规划**

**确定状态：** 
设两个字符串分别为`T` 和`S`，*题目求判断一个字符串`T`是否可以由`S`经过题意中的变换而成*。
**因此，首要前提是`T`和`S`的长度相等。**
按照题意，`S`会被分解为`S=S1+S2`，同理，`T=T1+T2`。**若满足`S`可变换为`T`则会有以下两种情况：**
**情况一：** `S1`对应`T1`，`S2`对应`T2`。
**情况二：** `S1`对应`T2`，`S2`对应`T1`。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200219160924462.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE4Njc2,size_16,color_FFFFFF,t_70)
那么只需要判断是否满足二者之一即可，引出了**子问题：**
- **S1是否对应T1，S2是否对应T2**
- **S1是否对应T2，S2是否对应T1**

**状态：**
**`f[i][j][k][h]`表示`S[i...j]`是否对应`T[k...h]`**
由于`S`和`T`的子串长度相同，可以只记录起始位置，**可优化为：`f[i][j][k]`，`k`为子串的长度**。
**例如：
`S,T`存在这样的一个划分，`Si`和`Tj`为`S,T`的子串，`Si`从下标`i=7`开始，`Tj`从下标`j=0`开始，长度为`5`，`f[7][0][5]`可表示`Si(i=7)`对应`Tj(j=0)`。**

**转移方程：**
枚举所有子串情况一与情况二取OR。
**`f[i][j][k]`表示`Si`是否对应`Tj`，枚举`Si`和`Tj`小于k的子串，若他们的子串存在两种情况之一，`Si`和`Ti`即成立。**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200219164344844.png)

**初始条件和边界：**
对于长度为1的子串：若`S[i]=T[j]`，`f[i][j][1]=true`，否则为false
**计算顺序k从小到大计算**
**答案为`f[0][0][N]`**
````java
class Solution {
    public boolean isScramble(String s1, String s2) {
        char []s=s1.toCharArray();
        char []t=s2.toCharArray();
        int m=s.length;
        int n=t.length;
        if(m!=n)
            return false;
        if(m==0)
            return true;
        boolean [][][]f=new boolean[m][m][m+1];
        int i,j,k,w;
        for(i=0;i<m;i++)
        {
            for(j=0;j<m;j++)
            {
                f[i][j][1]=s[i]==t[j];
            }
        }
        for(k=2;k<=m;k++)
        {
            for(i=0;i<=m-k;i++)
            {
                for(j=0;j<=m-k;j++)
                {
                    f[i][j][k]=false;
                    for(w=1;w<=k-1;w++)
                    {
                        if(f[i][j][w]&&f[i+w][j+w][k-w]){
                            f[i][j][k]=true;
                                break;
                        }
                    }
                    for(w=1;w<=k-1;w++)
                    {
                        if(f[i][j+k-w][w]&&f[i+w][j][k-w]){
                            f[i][j][k]=true;
                                break;
                        }
                    }
                }
            }
        }
        return f[0][0][m];
    }
}
````