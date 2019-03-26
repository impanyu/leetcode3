## [1021.Best Sightseeing Pair](https://leetcode.com/contest/weekly-contest-129/problems/best-sightseeing-pair/)
1. update max of A[i]+i and max of ans each iteration
```c++
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& A) {
        int ans=A[0]+A[1]-1;
        int max_ai=max(A[0],A[1]+1);
        for(int i=2;i<A.size();i++){
            int aj=A[i]-i;
            int ai=A[i]+i;
            ans=max(ans,max_ai+aj);
            max_ai=max(ai,max_ai);
            
        }
        return ans;
        
    }
};
```

## [410.Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)
1. 2D dp, first dimension is the ending position in nums and the second position is the number of splitted parts 
2. Another way to view this problem: splitting feasibility is a monotonous function of w.r.t the minimum largest sum.
Thus we can solve this problem using binary search.
```c++
#define FILL(a,v) memset(a,v,sizeof(a))
class Solution {
public:
    int splitArray(vector<int>& nums, int m) {
        long dp[nums.size()+1][m+1];// 1-indexed
        long sub[nums.size()+1];//1-indexed
        sub[0]=0;
        for(int i=0;i<=nums.size();i++)
          for(int j=0;j<=m;j++)
              dp[i][j]=INT_MAX;
        for(int i=0; i<nums.size();i++)
             sub[i+1]=sub[i]+nums[i];
        
        dp[0][0]=0;
        for(int i=1; i<=nums.size();i++)
            for(int j=1; j<=m;j++)
                for(int k=0;k<i;k++)
                    dp[i][j] = min(dp[i][j],max(dp[k][j-1],sub[i]-sub[k]));
         
        return dp[nums.size()][m];
         
    }
};
```
