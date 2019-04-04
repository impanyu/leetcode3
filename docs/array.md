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

## [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
1. The observation is the maximum product will always start from one end except the existence of 0
2. When there are 0 inside, we just restart the array.

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        const int n = nums.size();
        int ans = INT_MIN;
        int front = 0;
        int back = 0;
        for (int i=0; i<n; i++) {
            front = front ? front*nums[i] : nums[i];
            back = back ? back*nums[n-1-i] : nums[n-1-i];
            ans = max(ans, max(front, back));
        }
        return ans;
    }
};
```
