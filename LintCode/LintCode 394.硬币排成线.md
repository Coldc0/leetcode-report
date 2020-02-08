# LintCode 394.Coins in a Line
**博弈型动态规划**

博弈型动态规划通常**从第一步分析，而不是最后一步。** 因为同一般的动态规划不同，越往后越简单。

**确定状态：** 面对N个石子，先手可以第一步拿一个或两个，后手面对N-1个石子或N-2个石子。
假设后手面对N-1个石子，**这和一开始面对N-1个石子的问题是一样的。因为后手就是下一轮的先手。**
**必胜和必败：**
如果取1个石子或2个石子后，让剩下的局面先手必败，则当前先手必胜。
如果不管怎么选择，剩下的局面都是先手必胜，则当前先手必败。
**子问题：
需要知道N-1个石子和N-2个石子是否先手必胜。**
**状态：**
**设 `f[i]`当此时为先手时，面对`i`个石子是否必胜。**
**转移方程：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209002306913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE4Njc2,size_16,color_FFFFFF,t_70)
简化后为：$f[i]=(f[i-1]==false)||(f[i-2]==false)$
**初始状态和边界：**
面对0个石子，先手必败：`f[0]=false`
面对1个或2个石子，先手必胜：`f[1]=f[2]=true`
````java
public class Solution {
    /**
     * @param n: An integer
     * @return: A boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int n) {
        if(n==0)
            return false;
        if(n==1)
            return true;
        boolean []f=new boolean[n+1];
        f[0]=false;
        f[1]=true;
        for(int i=2;i<=n;i++)
        {
            f[i]=(f[i-1]==false)||(f[i-2]==false);
        }
        return f[n];
    }
}
````
