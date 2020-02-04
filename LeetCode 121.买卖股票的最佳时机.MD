# LeetCode 121.买卖股票的最佳时机
    执行用时 :2 ms, 在所有 Java 提交中击败了60.88%的用户

 很简单的**序列型动态规划**
 **前i天的最大收益 = max{前i-1天的最大收益，第i天的价格-前i-1天中的最小价格}**
 ````java
 class Solution {
    public int maxProfit(int[] prices) {
        int min=Integer.MAX_VALUE;
        int res=0;
        for(int i=0;i<prices.length;i++)
        {
            int t=prices[i]-min;
            if(t<=0)
            {
                min=prices[i];
                continue;
            }
            if(t>res)
                res=t;
        }
        return res;
    }
}
 ````
