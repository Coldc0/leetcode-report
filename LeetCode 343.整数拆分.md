# LeetCode 343.整数拆分

**确定状态：**
**设`f[n]`为整数之间和为`n`的最大乘积。**
要求`f[n]`，一定存在一个整数`i`使得`f[n-i]*i`最大
但是并不只是一种情况，**还有一种可能，整数其本身比拆分后的乘积大：** 即取`(n-i)*i`。
**转移方程：**
$f[i]=max_{1<j<i}\{f[n-i]*i,(n-i)*i\}$
**初始条件：**
`f[0]=0,f[1]=1`
````java
class Solution {
    public int integerBreak(int n) {
        int []f=new int[n+1];
        f[0]=0;
        f[1]=1;
        for(int i=2;i<=n;i++)
        {
            for(int j=1;j<i;j++)
            {
                f[i]=Math.max(f[i],Math.max(f[i-j]*j,(i-j)*j));
            }
        }
        return f[n];
    }
}
````