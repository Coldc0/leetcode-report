# LeetCode 123.买卖股票的最佳时机 III
    执行用时 :4 ms, 在所有 Java 提交中击败了70.42%的用户
**序列型动态规划**
**首先对于股票交易要有几个认识：**

![图片引用LeetCode](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMubGVldGNvZGUtY24uY29tL2Q0NDdmOTZkMjBkMWNmZGVkMjBhNWQwODk5M2IzNjU4ZWQwOGUyOTVlY2M5YWVhMzAwYWQ1ZTNmNDQ2NmUwZmUtZmlsZV8xNTU1Njk5NTE1MTc0?x-oss-process=image/format,png)

对于上图，若此图为股票的价格走势，那么要求最大利润，直观来说就是图中的$peek_j-valley_i$,但是从另一种角度，**第2天和第5天之间是涨是跌，都与最后最大利润的值无关**，另外，最大利润的求解不仅可以直观地用最大值减去最小值，**若从第2天之后，每天的利润（每天的价格减去前一天的价格）相加，到第五天也会得到最大值。**

### 进入正题：

**确定状态：**

设**截止**到第`i`天(不含第`i`天)，在阶段`j`的最大获利为`f[i][j]`。
**将整个过程分为五个阶段：**
    **1. 第一次未持有股票(第一次买之前)：** 
    要么是初始状态，要么一直没买，没有利润。
    **2. 第一次持有股票：** 
    **若昨天有股票，那么今天保持状态，** 截止到今天的利润等于将截止到昨天这个阶段的利润加上 ==今天这个阶段的利润==，即`f[i][j]=f[i-1][j]+prices[i]-prices[i-1]`。**若昨天没有股票，那么就是今天买入的，** ==截止到今天的利润等于昨天前一阶段的利润==,即`f[i][j]=f[i-1][j-1]`。
   **3. 第二次未持有股票（第一次卖之后，第二次买之前）：** **若昨天就没有股票**，那么今天保持状态也没有，**利润等于截止到昨天这个阶段的**，即`f[i][j]=f[i-1][j]`.**若是今天才卖的**，截止到今天的利润等于**截止到昨天前一阶段的利润**加上**今天所得的利润**，即`f[i][j]=f[i-1][j-1]+prices[i]-prices[i-1]`。
    **4. 第二次持有股票：与第一次持有股票同理**
    **5. 第三次未持有股票（第二次卖之后）：与第二次未持有股票同理** 

**转移方程：** 

**1. 阶段1、3、5：未持有股票**
   $f[i][j]=max\{f[i-1][j](昨天未持有股票),f[i-1][j-1]+prices[i]-prices[i-1](今天卖出)\}$
**2. 阶段2、4：持有股票**
  $f[i][j]=max\{f[i-1][j]+prices[i]-prices[i-1](昨天持有股票),f[i-1][j-1](今天买入)\}$

**初始条件和边界情况：**
**注意实现时prices数组索引从0开始；f数组索引从1开始，0作为初始条件。所以以上所列方程在实现时prices数组中的索引全部-1** 刚开始在阶段1：$f[0][1]=0，f[0][2]=f[0][3]=f[0][4]=f[0][5]=-\infty$。**数组索引别越界，若j-1<1或i-2<0，对应项不参与max运算。**
**最后答案就是$max\{f[n][1],f[n][3],f[n][5]\}$**

````java
class Solution {
    public int maxProfit(int[] prices) {
        int n=prices.length;
        if(n==0)
            return 0;
        int [][]f=new int[n+1][5+1];
        f[0][1]=0;
        f[0][2]=f[0][3]=f[0][4]=f[0][5]=Integer.MIN_VALUE;
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=5;j++)
            {
                if(j==1||j==3||j==5)//未持有股票
                {
                    f[i][j]=f[i-1][j];
                    if(i>1&&j>1&&f[i-1][j-1]!=Integer.MIN_VALUE)
                     f[i][j]=Math.max(f[i-1][j],//因无活动，保持前一天的阶段
                                      prices[i-1]-prices[i-2]+f[i-1][j-1]);//因卖出股票后而未持有，计算利润
                }
                if(j==2||j==4)//持有股票
                {
                    f[i][j]=f[i-1][j-1];
                    if(i>1&&f[i-1][j]!=Integer.MIN_VALUE)
                    f[i][j]=Math.max(prices[i-1]-prices[i-2]+f[i-1][j],//因一直持有股票，计算当天利润
                                     f[i-1][j-1]);//因未持有股票，买入
                }
            }
        }
        return Math.max(f[n][1],Math.max(f[n][3],f[n][5])); 
    }
}
````