## [941. Valid Mountain Array](https://leetcode.com/problems/valid-mountain-array/)
1. one pass through the array to count the ascending slop and descending slop.
2. return false when reaching peak at both ends or encountering more than one peak. 

```c++
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        int N = A.size();
        int i = 0;
        // walk up
        while (i+1 < N && A[i] < A[i+1])
            i++;

        // peak can't be first or last
        if (i == 0 || i == N-1)
            return false;

        // walk down
        while (i+1 < N && A[i] > A[i+1])
            i++;

        return i == N-1;
    }
};
```


