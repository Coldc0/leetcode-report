#498.对角线遍历

    执行用时 :6 ms , 在所有 Java 提交中击败了 20.72% 的用户

错一点debug一年的那种题。
目前是最直接的方法
为了方便写了一个judge方法判断是否越界。  

右上方向越界：
1. 首先保证索引恢复未越界前的状态
2. 尝试索引向右平移若仍越界则尝试向下平移
3. 若仍越界则检查是否遍历完毕，若是则返回，否则退出循环。 

左下方向越界：
1. 首先保证索引恢复未越界前的状态
2. 尝试向下平移若仍越界则尝试向右平移
3. 若仍越界则检查是否遍历完毕，若是则返回，否则退出循环。  

```` java
class Solution {
    public int[] findDiagonalOrder(int[][] matrix) {

              //up:i-1 j+1 switch: i==0 j+1 then i+1
        //down i+1 j-1 switch: j==0 i+1 then j+1
        
        boolean s=true;
        if(matrix.length==0)
            return new int[0];
        int i=0,j=0,m=matrix.length,n=matrix[0].length;
      
        int []ans=new int[m*n];

        int count=0;
        while(true){
            while(s)
            {
                if(!judge(i,j,m,n))
                {
                    i++;
                    j--;
                    if(!judge(i,j+1,m,n))
                    {
                        if(!judge(i+1,j,m,n))
                        {
                            if(count<m*n)
                                break;
                            else
                                return ans; 
                        }
                          
                        else
                            i++;
                    }
                    else
                        j++;
                    s=false;
                    break;
                }
                ans[count++]=matrix[i][j];
                i--;
                j++;

            }
            while(!s)
            {
                if(!judge(i,j,m,n))
                {
                    i--;
                    j++;
                    if(!judge(i+1,j,m,n))
                    {
                        if(!judge(i,j+1,m,n))
                        {
                            if(count<m*n)
                                break;
                            else
                                return ans;
                        }
                          
                        else
                            j++;
                    }
                    else
                        i++;
                    s=true;
                    break;
                }
                ans[count++]=matrix[i][j];
                i++;
                j--;

            }
        }

    }
    public boolean judge(int i,int j,int m,int n)
    {
        return i>=0&&j>=0&&i<m&&j<n;
    }

}
````  
