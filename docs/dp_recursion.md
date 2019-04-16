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


//0-indexed method
class Solution {
public:
    int splitArray(vector<int>& nums, int m) {
        long dp[nums.size()][m+1];// 1-indexed
        long sub[nums.size()];//1-indexed
        sub[0]=nums[0];
        for(int i=0;i<nums.size();i++)
          for(int j=0;j<=m;j++)
              dp[i][j]=INT_MAX;
        
        for(int i=1; i<nums.size();i++){
             sub[i]=sub[i-1]+nums[i];
        }
        
        for(int i=0; i<nums.size();i++)
            for(int j=1; j<=m;j++){
                if(j==1){
                    dp[i][j]=sub[i];
                    continue;
                }
                for(int k=0;k<i;k++)
                    dp[i][j] = min(dp[i][j],max(dp[k][j-1],sub[i]-sub[k]));
            }       
        return dp[nums.size()-1][m];      
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

## [943. Find the Shortest Superstring]()
1. each cell of dp[i][j] stores the shortest string for a subset of A ended with j 


```c++
class Solution {
public:
     string shortestSuperstring(vector<string>& A) {
        int n = A.size();
        // dp[mask][i] : min superstring made by strings in mask,
        // and the last one is A[i]
        vector<vector<string>> dp( 1<<n, vector<string>(n) );
        vector<vector<int>> overlap(n,vector<int>(n,0));
        
        // 1. calculate overlap for A[i]+A[j].substr(?)
        for(int i=0; i<n; ++i) 
            for(int j=0; j<n; ++j) 
                if(i!=j){
                  for(int k = min(A[i].size(), A[j].size()); k>0; --k)
                    if(A[i].substr(A[i].size()-k)==A[j].substr(0,k)){
                        overlap[i][j] = k; 
                        break;
                    }
          }
        // 2. set inital val for dp
        for(int i=0; i<n; ++i) dp[1<<i][i] += A[i];
        // 3. fill the dp
        for(int mask=1; mask<(1<<n); ++mask){
            for(int j=0; j<n; ++j) 
                if((mask&(1<<j))>0){
                    for(int i=0; i<n; ++i) 
                        if(i!=j && (mask&(1<<i))>0){
                            string tmp = dp[mask^(1<<j)][i]+A[j].substr(overlap[i][j]);
                            if(dp[mask][j].empty() || tmp.size()<dp[mask][j].size())
                                dp[mask][j] = tmp;
                    }
            }
        }
        // 4. get the result
        int last = (1<<n)-1;
        string res = dp[last][0];
        for(int i=1; i<n; ++i) 
            if(dp[last][i].size()<res.size()){
              res = dp[last][i];
        }
        return res;
    }
};
```


## [471. Encode String with Shortest Length](https://leetcode.com/problems/encode-string-with-shortest-length/)
1. accumulate each pair of starting point and length of a substring of s
2. return dp[n][0]
3. pay attention to the technique to find duplicate substring of a string


```c++
class Solution {
public:
    string encode(string s) {
        int n = (int)s.size();  
        if(!n)  return s;
        // dp[N][i] means length is N, start from i
        vector<vector<string>> DP(n+1,vector<string>(n,""));  
        for(int i = 0; i < n; ++i)  DP[1][i] += s[i];
        for(int N = 2; N <= n; ++N){
            for(int i = 0; i+N <= n; ++i){
                DP[N][i] = s.substr(i,N);
                auto k = ( DP[N][i]+ DP[N][i]).find( DP[N][i],1);
                if(k < DP[N][i].size()){
                    string tmp = to_string(N/k)+"["+DP[k][i]+"]";
                    if(tmp.size() < DP[N][i].size())    
                        DP[N][i] = tmp;
                }
                for(int k=1;k<N;k++)  {
                    string tmp = DP[k][i]+DP[N-k][i+k];
                    if(tmp.size() < DP[N][i].size())    
                        DP[N][i] = tmp;
                }
            }
        }
        return DP[n][0];
 }
};
```



## [688.Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)
1. each state is represented by an array
2. state transfer function is calculate per cell in the state array


```c++
class Solution {
public:
    double knightProbability(int N, int K, int r, int c) {
        vector<vector<double>> dp(N,vector<double>(N));

        int dr[]={2,2,1,1,-1,-1,-2,-2};
        int dc[]={1,-1,2,-2,2,-2,1,-1};
        dp[r][c]=1;
        for(;K>0;K--){
            vector<vector<double>> dp2(N,vector<double>(N));
            for(int rr=0; rr<N;rr++){
                for(int cc=0; cc<N;cc++){
                    for(int k=0;k<8;k++){
                        int nr=rr+dr[k];
                        int nc=cc+dc[k];
                        if(0<=nr && nr<N && 0<=nc && nc<N){
                            dp2[nr][nc]+=dp[rr][cc]/8;
                            //cout<<dp2[nr][nc]<<"\n";
                        }
                        
                    }
                }
            }
             for(int rr=0; rr<N;rr++)
                for(int cc=0; cc<N;cc++)
                    dp[rr][cc]=dp2[rr][cc];
            
        }
        double ans=0;
        for(int rr=0; rr<N;rr++)
                for(int cc=0; cc<N;cc++)
                    ans+=dp[rr][cc];
        return ans;
        
    }
};
```


## [518. Coin Change 2](https://leetcode.com/problems/coin-change-2/)
1. pay attention to the loop order, we can not transfer mainly on amount, because we need to keep the order of the coins

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int dp[amount+1];
        fill(dp,dp+amount+1,0);
        dp[0]=1;
        for(int j=0;j<coins.size();j++){
          for(int i=1;i<=amount;i++){           
                if(i>=coins[j]) dp[i]+=dp[i-coins[j]];
                //cout<<dp[i]<<"\n";
            }     
        }          
        return dp[amount];
    }
};
```

## [322. Coin Change](https://leetcode.com/problems/coin-change/)
1. here we only care about the minimal coins we need, so the accumulating order didn't matter.


```c++
class Solution {
//a bottom up dp
public:
    int coinChange(vector<int>& coins, int amount) {
        int dp[amount+1];
        for(auto& d: dp) d=amount+1;
        dp[0]=0;
        for(int i=1;i<=amount;i++){
            for(auto coin :coins){
                if(i-coin>=0)
                    dp[i]=min(dp[i],dp[i-coin]+1);
            }
        }
        return dp[amount]>amount? -1:dp[amount];
    }
};
```

## [361. Bomb Enemy](https://leetcode.com/problems/bomb-enemy/)
1. only use an array colhits to represent column hits
2. use int rowhits to represent current row hits.


```c++
class Solution {
public:
    int maxKilledEnemies(vector<vector<char>>& grid) {
        if(grid.size()==0 || grid[0].size()==0) return 0;
        int m = grid.size(), n = m ? grid[0].size() : 0;
        int result = 0, rowhits, colhits[n];
        for (int i=0; i<m; i++) {
            for (int j=0; j<n; j++) {
                if (!j || grid[i][j-1] == 'W') {
                    rowhits = 0;
                    for (int k=j; k<n && grid[i][k] != 'W'; k++)
                        rowhits += grid[i][k] == 'E';
                }
                if (!i || grid[i-1][j] == 'W') {
                    colhits[j] = 0;
                    for (int k=i; k<m && grid[k][j] != 'W'; k++)
                        colhits[j] += grid[k][j] == 'E';
                }
                if (grid[i][j] == '0')
                    result = max(result, rowhits + colhits[j]);
            }
        }
        return result;
 }
};
```



## [975. Odd Even Jump](https://leetcode.com/problems/odd-even-jump/)
0. a bottom up dp problem
1. use tree map to store the values up until now. we can say map vals is for keeping the transfer structure
2. use two arrays(odd, even) to keep the state for each position



```c++
class Solution {
public:
    int oddEvenJumps(vector<int>& A) {
        int N = A.size();
        if (N <= 1) return N;
        bool odd[N], even[N];
        map<int,int> val_map; // map value to index
        fill(odd,odd+N,false);
        fill(even,even+N,false);
        odd[N-1]=even[N-1]=true;
        val_map[A[N-1]]=N-1;
        
        for(int i=N-2;i>=0;i--){
            if(val_map.count(A[i])){
                odd[i]=even[val_map[A[i]]];
                even[i]=odd[val_map[A[i]]];
            }
            else{
                map<int,int>::iterator higher=val_map.lower_bound(A[i]);
                map<int,int>::iterator lower=higher==val_map.begin()?val_map.end():--val_map.lower_bound(A[i]);
       
                if(higher!=val_map.end())//have larger val
                  odd[i]=even[val_map[higher->first]]; 
                if(lower!=val_map.end())
                  even[i]=odd[val_map[lower->first]];
            }
            val_map[A[i]]=i;//update the value map
        }
        int ans=0;
        for(bool k: odd)
            ans+=k;
        return ans;
        
    }
};
```

## [248.Strobogrammatic Number III](https://leetcode.com/problems/strobogrammatic-number-iii/)
1. build on top of strobogrammatic number II, and for each number length, generate and count the satisfied number

```c++
class Solution {
public:
    int count=0;
    int total;
    long low;
    long high;
    
    int strobogrammaticInRange(string low, string high) {
        this->low= stol(low);
        this->high=stol(high);
        for(int i=low.size();i<=high.size();i++){
             total=i;
             count_strobo(i);       
       }
        return count;
    }
    
    vector<string> count_strobo(int n){
        vector<string> ans;
        if(n==0) ans= {""};
        else if(n==1) ans={"0","1","8"};
        else{
            vector<string> nums=count_strobo(n-2);
            for(string num: nums){
                ans.push_back("1"+num+"1");
                ans.push_back("6"+num+"9");
                ans.push_back("9"+num+"6");
                ans.push_back("8"+num+"8");
                if(n!=total) ans.push_back("0"+num+"0");        
            } 
        }
        if(n==total){// in outer layer, count the satisfying number
            for(string num : ans){
                long current_num=stol(num);
                if(current_num>=low &&  current_num<=high)
                    count++;
            }
        }
       return ans;
    }
};
```


## [920. Number of Music Playlists](https://leetcode.com/problems/number-of-music-playlists/)
1. transfer function and two cases

```c++
class Solution {
public:
    int numMusicPlaylists(int N, int L, int K) {
       int mod=1e9+7;
       long dp[N+1][L+1];//n songs play l times
       for(int i=0;i<=N;i++)
           for(int j=0;j<=N;j++)
               dp[i][j]=0;
       dp[0][0]=1;
       for(int i=1;i<=N;i++){
           for(int j=1;j<=N;j++){
               dp[i][j]=dp[i-1][j-1]*(N-i+1);
               dp[i][j]+=dp[i][j-1]*max(i-K,0); 
               dp[i][j]%=mod;
           }
       }
       return (int)dp[N][L];
      
    }
};
```

