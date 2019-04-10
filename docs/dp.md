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


## [90. SubsetsII](https://leetcode.com/problems/subsets-ii/)
1. No need to do back track, squeeze the tree into one dimension
2. similar to dp

```c++
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> ans;
        ans.push_back(vector<int>());
        unordered_map<int,int> count_map;
        for(int n :nums) count_map[n]++;
        for(auto p: count_map){//for each unique number
            int pre_count=ans.size();       
                for(int i=0;i<pre_count;i++){//for each previous subset
                    vector<int> new_vector(ans[i]);
                    for(int j=p.second;j>0;j--){//push j times
                        new_vector.push_back(p.first);
                        ans.push_back(new_vector);
                    }
             }
       }
      return ans;
    }
};
```

## [90. Number of Music Playlists](https://leetcode.com/problems/number-of-music-playlists/)
1. Use an array to store the number of music playlists for i unique songs with j songs in list.
2. Two previous conditions in transfer function. 
```c++
class Solution {
public:
    int numMusicPlaylists(int N, int L, int K) {
        int mod=1e9+7;
        long dp[N+1][L+1];
        for(int i=0;i<=N;i++)
            for(int j=0; j<=L;j++)
                dp[i][j]=0;
        dp[0][0]=1;
        for(int i=1;i<=N;i++)
            for(int j=1; j<=L;j++){
                dp[i][j]=dp[i-1][j-1]*(N-i+1);
                dp[i][j]+=dp[i][j-1]*max(i-K,0);
                dp[i][j]%=mod;
            }
        return (int)dp[N][L];
    }
};
```

## [837.New 21 Game](https://leetcode.com/problems/new-21-game/)
1. Relaxed 21 game, instead of hit 21, player only need to hit a value in [K,N]
2. dp[k]= 1/W*(dp[k+1]+dp[k+2]+...+dp[k+W])
3. always keep a window size of W, and shift the window from right to left
```c++
public:
    double new21Game(int N, int K, int W) {
        double dp[N + W + 1];
        FILL(dp,0);
        // dp[x] = the answer when Alice has x points
        for (int k = K; k <= N; ++k)
            dp[k] = 1.0;

        double S = min(N - K + 1, W);
        // S = dp[k+1] + dp[k+2] + ... + dp[k+W]
        for (int k = K - 1; k >= 0; --k) {
            dp[k] = S / W;
            S += dp[k] - dp[k + W];
        }
        return dp[0];
    }
};
```

## [198.House Robber](https://leetcode.com/problems/house-robber/)
1. no need to store the whole dp array, only need most recent two max value.

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
       int prev_max = 0;
       int curr_max = 0;
       for(int x : nums) {
          int temp = curr_max;
          curr_max = max(prev_max + x, curr_max);
          prev_max = temp;
       }
       return curr_max;
    }
};
```

## [213. House Robber II](https://leetcode.com/problems/house-robber-ii/)
1. similar to house robber I, except in this case, we need to be careful with the corner cases.

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==0) return 0;
        if(nums.size()==1) return nums[0];
        int pre_max=0, cur_max=0;
        int ans;
        for(int i=0;i<nums.size()-1;i++){
            int tmp=cur_max;
            cur_max=max(cur_max,pre_max+nums[i]);
            pre_max=tmp;
        }
        ans=cur_max;
        pre_max=0;
        cur_max=0;
        for(int i=1;i<nums.size();i++){
            int tmp=cur_max;
            cur_max=max(cur_max,pre_max+nums[i]);
            pre_max=tmp;
        }
        
        return max(ans,cur_max);
    }
};
```

## [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/submissions/)
1. each cell in dp indicates starting point
2. two conditions for transfer function: if '*' exists

```c++
class Solution {
public:
    bool isMatch(string text, string pattern) {
      bool dp[text.length() + 1][pattern.length() + 1];
      for(int i=0;i<=text.length();i++)
          for(int j=0;j<=pattern.length();j++)
              dp[i][j]=false;
      dp[text.length()][pattern.length()] = true;
      for (int i = text.length(); i >= 0; i--){
          for (int j = pattern.length() - 1; j >= 0; j--){
          bool first_match = (i < text.length() && (pattern[j] == text[i] || pattern[j] == '.'));
          if (j + 1 < pattern.length() && pattern[j+1] == '*'){
              dp[i][j] = dp[i][j+2] || first_match && dp[i+1][j];
             } 
          else {
               dp[i][j] = first_match && dp[i+1][j+1];
                }
            }
      }
      return dp[0][0];  
    }
};
```

## [221. Maximal Square](https://leetcode.com/problems/maximal-square/)
1. dp[i][j] represent the side length of the max square right-cornered at i,j


```c++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        if(matrix.size()==0) return 0;
        int rows=matrix.size(), cols=matrix[0].size();
        int dp[rows][cols];
        for(int i=0;i<rows;i++)
            for(int j=0;j<cols;j++)
                dp[i][j]=0;
        
        int max_side=0;
        for(int i=0;i<rows;i++){
            for(int j=0;j<cols;j++){
                if(i==0 || j==0) dp[i][j]=(matrix[i][j]=='1'?1:0);
                else if(matrix[i][j]=='1')
                    dp[i][j]=min(min(dp[i-1][j],dp[i][j-1]),dp[i-1][j-1])+1;
                max_side=max(dp[i][j],max_side);
              }
        }
        return max_side*max_side;
    
    }
};
```
