## [74.Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
## [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
1. These two problems can be solved in one general method, which treat the matrix as a binary search tree rooted at the upper-left corner
2. Problem 74 can be transformed to a binary search on 1D array, where we flatten the 2D matrix row by row.


```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0) return false;
        int i=0;
        int j=matrix[0].size()-1;
        while(i<matrix.size() && j>=0){
            if(matrix[i][j]==target) return true;
            else if(matrix[i][j]>target) j--;
            else i++;
        }
        return false;
    }
};
```
