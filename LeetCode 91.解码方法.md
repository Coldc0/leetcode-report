#91.解码方法
    执行用时 :1 ms, 在所有 Java 提交中击败了100.00%的用户

**划分型动态规划**
**最后一步：** 设字符串为 s [0...n-1]，最后一个字母要么用到 s [n-1]，要么用到 s [n-2] 和 s [n-1]，假设到s [n-3] 有6种方式，s[n-2] 有3种方式，**那么到s[n-1]就有 6+3=9种方式。**
**子问题：** **自然转化成求s[n-3]和s[n-2]有多少种方式。**
**转移方程：** 创建一个大小为 **s.length()+1** 的数组，f[i]代表第s[i-1]的方式数。
**$f[i]=f[i-1]$  （对应一个字母）
$f[i]=f[i-1]+f[i+2]$  （对应两个字母）**

````java
class Solution {
      public int numDecodings(String s) {
        if(s.length()==0)
            return 1;
        int f[]=new int [s.length()+1];
        f[0]=1;
        for(int i=1;i<s.length()+1;i++)
        {
            f[i]=0;
            if(s.charAt(i-1)>'0'&&s.charAt(i-1)<='9')
                f[i]=f[i-1];
            if(i>1)
            {
                int t=(s.charAt(i-2)-'0')*10+(s.charAt(i-1)-'0');
                if(t>=10&&t<=26)
                f[i]+=f[i-2];
            }
        }
        return f[s.length()];
    }
}
````