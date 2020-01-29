#67.二进制求和
    执行用时 :3 ms, 在所有 Java 提交中击败了60.62%的用户
将每一位数对应并相加，再加上进位(cout)，cout只需要将这一位的结果除以2即可 ( 2/2 \== 1 , 1/2\==0 , 0/2 \== 0 ) ,最后将结果mod2加入到StringBuilder并反转输出

````java
class Solution {
   public String addBinary(String a, String b) {
        int al=a.length()-1;
        int bl=b.length()-1;
        StringBuilder stringBuilder=new StringBuilder();
        int cout=0;
        int t=0;
        for (int i = al, j=bl; i>=0||j>=0 ; i--,j--) {
            int l=i>=0?a.charAt(i)-'0':0;
            int r=j>=0?b.charAt(j)-'0':0;
            t=l+r+cout;
            cout=t/2;
            stringBuilder.append(t%2);
        }
        if(cout==1)
        stringBuilder.append(cout);

        return stringBuilder.reverse().toString();
    }
}
````