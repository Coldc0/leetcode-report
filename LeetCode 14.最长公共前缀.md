# 14.最长公共前缀

    执行用时 :0 ms, 在所有 Java 提交中击败了100.00%的用户

LCS为这些字符串的最长公共前缀，那么

$LCS(...LCS(LCS(LCS(S0,S1),S2)S3)...Sn)$

随意取`strs[0]`为初始p，那么strs[]的最长前缀必是`strs[0]`或及其子串，从`strs[1]`开始遍历，找出`p`与`strs[i]`的最长公共前缀前缀并存到p中。
````java
class Solution {
     public String longestCommonPrefix(String[] strs) {
        if(strs.length==0)
            return "";
        String p=strs[0];
        for (int i = 1; i <strs.length ; i++) {
            while(strs[i].indexOf(p)!=0)
            {
                p=p.substring(0,p.length()-1);
            }
        }
        return p;
        
    }
}
````