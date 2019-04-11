## [799. Champagne Towner](https://leetcode.com/problems/champagne-tower/)
1. simulate the whole process, pour down excessive champagne from top to bottom

```c++
class Solution {
public:
    double champagneTower(int poured, int query_row, int query_glass) {
        double A[102][102];
        for(int i=0;i<102;i++)
            for(int j=0;j<102;j++)
                A[i][j]=0;
        A[0][0]=(double) poured;
        for(int r=0; r<=query_row;r++){
            for(int c=0; c<=r; c++){
                double q= (A[r][c]-1.0)/2.0;//the excessive champagne poured to next layer
                if(q>0){
                    A[r+1][c]+=q;
                    A[r+1][c+1]+=q;
                }
            }
        }
        return min(1.0,A[query_row][query_glass]);
    }
};
```
