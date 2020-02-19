#LeetCode 139.单词拆分

**确定状态：**
设`f[i]`前i个是否能被完全拆分。
**转移方程：**
设最后匹配的字典长度为`len`；
`f[i]=f[i-len]||true(len==i)`
**初始条件和边界：**
前0个可被完全拆分：`f[0]=true`
**当与字典匹配时退出循环。**
````java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        char []ch=s.toCharArray();
        int n=ch.length;
        boolean []f=new boolean[n+1];
        f[0]=true;
        for(int i=1;i<=n;i++)
        {
            for(int j=i;j>=1;j--)
            {
                if(wordDict.indexOf(s.substring(j-1,i))!=-1)
                {
                    if(j==1)
                        f[i]=true;
                    else
                    {
                        f[i]=f[j-1];
                        if(f[i])
                            break;
                    }
                }
            }
        }
        return f[n];
    }
}
````
