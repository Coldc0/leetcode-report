# 118.杨辉三角

模拟就好了。

注意一下集合的声明：
` List<List<Integer>> ans = new ArrayList<List<Integer>>();`

```` java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ans = new ArrayList<List<Integer>>();
        for (int i = 0; i < numRows; i++) {

            List<Integer> t = new ArrayList<Integer>();
            t.add(1);
            if(i>1) {
                List<Integer> p = ans.get(i - 1);
                for (int j = 1; j < i; j++) {
                    t.add(p.get(j - 1) + p.get(j));
                }
            }
            if (i > 0) t.add(1);
            ans.add(t);
        }
        return ans;
    }
}
````
