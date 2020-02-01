#338.比特位计数
    执行用时 :2 ms, 在所有 Java 提交中击败了78.26%的用户

**位操作型动态规划**
**最后一步：** 对于一个int n,**以第n位为结尾的01串1的个数=以n-1位为结尾的01串的个数+第n位(0或1)。**
**子问题：** 原问题转为求以第n-1位为结尾的01串1的个数。
**转移方程：** $f[n]=(n\&0x00000001)+f[n>>1] $
````java
class Solution {
    public int[] countBits(int num) {
        int[] f=new int[num+1];
        for(int i=0;i<=num;i++)
        {
            int t=i;
            f[i]=(t&0x00000001)+f[t>>1];
        }
        return f;
    }
}
````