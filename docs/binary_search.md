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

[69. Sqrt(x)](https://leetcode.com/problems/sqrtx/)
1. a variation of binary search for upperbound case
2. here we return the index left to upperbound instead of upperbound of itself

```c++
class Solution {
public:
    int mySqrt(int x) {
        long a=0;
        long b=(long)x+1;
        long mid=0;
        while(a<b){
            mid=(a+b)/2;
            if(mid*mid>x) b=mid;
            else a=mid+1;       
        }
        return a-1;
    }
};

```



## [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
1. consider the number of smaller elements as a function of the value of matrix


```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int l=matrix[0][0];
        int r=matrix[matrix.size()-1][matrix[0].size()-1];
        while(l<r){
            int mid=(l+r)/2;
            int total=0;
            for(auto& row: matrix) total+=find_upperbound(row,mid);
            if(total>=k) r=mid;
            else l=mid+1;
        }
        return l;
    }
    
    int find_upperbound(vector<int>& row, int m){
        int i=0,j=row.size();
        while(i<j){
            int mid=(i+j)/2;
            if(row[mid]>m) j=mid;
            else i=mid+1;
        }
        return i;
    }
};
```
