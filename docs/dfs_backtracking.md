## [341. Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/)
1. Similar to pre-order traversal in binary tree, for each node push all subtree in the stack first 
2. For each next() operation, search through next integer node.
3. use reverse_iterator instead of iterator

```c++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class NestedIterator {
public:
    vector<NestedInteger> nums;
    stack<vector<NestedInteger>::reverse_iterator> s;
    NestedIterator(vector<NestedInteger> &nestedList) {
        nums=nestedList;
        vector<NestedInteger>::reverse_iterator p=nums.rbegin();
        while(p!=nums.rend()){
            s.push(p);
            p++;
        }
    }

    int next() {
       if(!hasNext()) return -1;
       vector<NestedInteger>::reverse_iterator p=s.top();
       s.pop();
       return p->getInteger();
        
    }

    bool hasNext() {
       vector<NestedInteger>::reverse_iterator p;
       while(!s.empty()){
           p=s.top();
           if(p->isInteger())  return true;
           s.pop();
           for(vector<NestedInteger>::reverse_iterator i=p->getList().rbegin();i!=p->getList().rend();i++)
               s.push(i);
       }
        return false;
    }
};
```


## [489. Robot Room Cleaner](https://leetcode.com/problems/robot-room-cleaner/)
1. recursive DFS to traverse each pos
2. Use a map to store all visited pos.
3. x,y and dir must sync with the actual states 

```c++
/**
 * // This is the robot's control interface.
 * // You should not implement it, or speculate about its implementation
 * class Robot {
 *   public:
 *     // Returns true if the cell in front is open and robot moves into the cell.
 *     // Returns false if the cell in front is blocked and robot stays in the current cell.
 *     bool move();
 *
 *     // Robot will stay in the same cell after calling turnLeft/turnRight.
 *     // Each turn will be 90 degrees.
 *     void turnLeft();
 *     void turnRight();
 *
 *     // Clean the current cell.
 *     void clean();
 * };
 */

class Solution {
public:
    unordered_map<int, unordered_map<int, int>> data;
    int x=0;
    int y=0;
    int dir=0;
    int dx[4]={1, 0, -1, 0};
    int dy[4]={0, 1, 0, -1};
    void cleanRoom(Robot& robot) {
       int old_x=x;
       int old_y=y;
       robot.clean();
       for(int i=0;i<4;i++){
           int new_x=x+dx[dir];
           int new_y=y+dy[dir];
             if(data[new_x][new_y]==0 && robot.move()){//enter a new position
                 x=new_x;
                 y=new_y;
                 data[new_x][new_y]++;
                 cleanRoom(robot);
                 //robot back up
                 robot.turnRight();
                 robot.turnRight();
                 robot.move();
                 robot.turnRight();
                 robot.turnRight();
                 x=old_x;
                 y=old_y;
             } 
           robot.turnRight();
           dir=(dir+1)%4;
       }
       
    }
};
```

## [46.Permutations](https://leetcode.com/problems/permutations/)
1. only collect leaf nodes.
2. swap in place


```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<vector<int>> permute(vector<int>& nums) {   
       pt(nums,0);
       return ans;
        
    }   
    void pt(vector<int>& nums,int first){     
        if(first==nums.size())
            ans.push_back(nums);
        for(int i=first;i<nums.size();i++){
          swap(nums[i],nums[first]);
          pt(nums,first+1);
          swap(nums[i],nums[first]);       
       }
    }
};
```

## [79. Word Search](https://leetcode.com/problems/word-search/)
1. current node is represented by i and j
2. recursive solution


```c++
class Solution {
public:
   bool exist(vector<vector<char>>& board, string word) {
    for (unsigned int i = 0; i < board.size(); i++) 
        for (unsigned int j = 0; j < board[0].size(); j++) 
            if (dfs(board, i, j, word))
                return true;
    return false;
}

bool dfs(vector<vector<char>>& board, int i, int j, string& word) {
    if (!word.size())
        return true;
    if (i<0 || i>=board.size() || j<0 || j>=board[0].size() || board[i][j] != word[0])  
        return false;
    char c = board[i][j];
    board[i][j] = '*';
    string s = word.substr(1);
    bool ret = dfs(board, i-1, j, s) || dfs(board, i+1, j, s) || dfs(board, i, j-1, s) || dfs(board, i, j+1, s);
    board[i][j] = c;
    return ret;
  }       
};
```

## [753. Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)
1. dfs traversal on a graph, for each node, append the corresponding char.
2. append the char after traverse a circle back (post order)
3. record visited edge, not node.

```c++
class Solution {
public:
    unordered_set<string> seen;
    string ans;
    string crackSafe(int n, int k) {
        if (n == 1 && k == 1) return "0";

        string start ;
        for (int i = 0; i < n-1; ++i)
            start+='0';

        dfs(start, k);
        ans+=start;
        return ans;
    }
     
     void dfs(string node, int k) {
        for (int x = 0; x < k; ++x) {
            string nei = node + to_string(x);
            if (!seen.count(nei)) {
                seen.insert(nei);
                dfs(nei.substr(1), k);
                ans+=to_string(x);
            }
        }
    }    
};
```


## [679. 24 Game](https://leetcode.com/problems/24-game/)
1. each node is a vector of numbers, and we recursively solve these numbers.
2. backtrack when finish one branch

```c++
class Solution {
public:
    bool judgePoint24(vector<int>& nums) {
        vector<double> A;
        for(int v: nums) A.push_back((double)v);
        return solve(A);
    }
    
    bool solve(vector<double>& nums){
        if(nums.size()==0) return false;
        if(nums.size()==1) return abs(nums[0]-24)<1e-6;
        for(int i=0; i<nums.size();i++){
            for(int j=0; j<nums.size();j++){
                if(i!=j){
                    vector<double> nums2;
                    for(int k=0; k<nums.size();k++)
                        if(k!=i && k!=j)
                            nums2.push_back(nums[k]);
                    for(int k=0; k<4;k++){
                        //try different child
                        if(k<2 && j>i) continue;
                        if(k==0) nums2.push_back(nums[i]+nums[j]);
                        if(k==1) nums2.push_back(nums[i]*nums[j]);
                        if(k==2) nums2.push_back(nums[i]-nums[j]);
                        if(k==3){
                            if(nums[j]!=0)
                                nums2.push_back(nums[i]/nums[j]);
                            else
                                continue;
                        }
                        if(solve(nums2)) return true;
                        nums2.pop_back(); //backtracking
                    }
                }        
            }
        }
        return false;
    }
};
```


## [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)
1. use a cache to memorize the longest path of each cell

```c++
class Solution {
//DFS+ Memoization solution
private:
    int dirs[5]={0,1,0,-1,0};
    int m,n;
    vector<vector<int>> cache;
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if(matrix.size()==0) return 0;
        m = matrix.size();
        n = matrix[0].size();
        for(int i=0;i<m;i++){
            cache.push_back(vector<int>(n));
            fill(cache[i].begin(),cache[i].end(),0);
        }
        int ans=0;
        for(int i=0; i<m; i++)
            for(int j=0; j<n; j++)
                ans= max(ans,dfs(matrix,i,j));
        return ans;
    }
    
    int dfs(vector<vector<int>>& matrix, int i, int j){
        if(cache[i][j]!=0) return cache[i][j];
        for(int k=0;k<4; k++){
            int x=i+dirs[k], y=j+dirs[k+1];
            if(0<=x && x<m && 0<=y && y<n && matrix[x][y] >matrix[i][j])
                cache[i][j]=max(cache[i][j],dfs(matrix,x,y));
        }
        return ++cache[i][j];
        
    } 
};
```

## [465. Optimal Account Balancing](https://leetcode.com/problems/optimal-account-balancing/)
1. dfs on the debt state vector.
2. for each node, clean the debt of cur position, and traverse all the choices of children. Accumulate the min transfer

```c++
class Solution {
public:
    int minTransfers(vector<vector<int>>& transactions) {
        vector<int> debt;
        unordered_map<int,int> debt_map;
        for(auto trans : transactions){
            debt_map[trans[0]]+=trans[2];
            debt_map[trans[1]]-=trans[2];
        }
        for(auto p : debt_map){
            debt.push_back(p.second);
        }
        
        return get_min_trans(0,debt);
    }
    int get_min_trans(int cur, vector<int> debt){
        while(cur < debt.size() && debt[cur]==0)
            cur++;
        if(cur==debt.size()) return 0;
        int min_trans=INT_MAX;
        for(int i=cur+1;i<debt.size();i++){
            if(debt[i]*debt[cur]<0){
                debt[i]+=debt[cur];
                min_trans=min(min_trans,get_min_trans(cur+1,debt)+1);
                debt[i]-=debt[cur];
            }  
        }
        return min_trans;
    }    
};
```

## [386. Lexicographical Numbers](https://leetcode.com/problems/lexicographical-numbers/)
1. dfs on a tree, where each node represents the starting digits


```c++
class Solution {
public:
    vector<int> ans;
    int limit;
    vector<int> lexicalOrder(int n) {
        limit=n;
        for(int i=1;i<10;i++)
            lexical_start_with(i);
        return ans;
    }
    
    void lexical_start_with(int m){
        if(m>limit) return;
        ans.push_back(m);
        for(int i=0;i<10;i++)
           lexical_start_with(m*10+i);
    }
};
```
## [351. Android Unlock Patterns](https://leetcode.com/problems/android-unlock-patterns/)
1. dfs all possible patterns, backtrack when come back


```c++
class Solution {
public:
    bool used[9];
    int numberOfPatterns(int m, int n) {
        int res = 0;
        for (int len = m; len <= n; len++) {	            
            res += calcPatterns(-1, len);
            for (int i = 0; i < 9; i++) {	                
                used[i] = false;
            }            
        }
        return res;
    }
    
    bool isValid(int index, int last) {
        if (used[index])
            return false;
        // first digit of the pattern    
        if (last == -1)
            return true;
        // knight moves or adjacent cells (in a row or in a column)	       
        if ((index + last) % 2 == 1)
            return true;
        // indexes are at both end of the diagonals for example 0,0, and 8,8          
        int mid = (index + last)/2;
        if (mid == 4)
            return used[mid];
        // adjacent cells on diagonal  - for example 0,0 and 1,0 or 2,0 and //1,1
        if ((index%3 != last%3) && (index/3 != last/3)) {
            return true;
        }
        // all other cells which are not adjacent
        return used[mid];
    }
    
     int calcPatterns(int last, int len) {
        if (len == 0)
            return 1;    
        int sum = 0;
        for (int i = 0; i < 9; i++) {
            if (isValid(i, last)) {
                used[i] = true;
                sum += calcPatterns(i, len - 1);
                used[i] = false;                    
            }
        }
        return sum;
    }
    
};
```

## [332. Reconstruct Itinerary]()
1. the tickets can appear multiple times, so we use multiset and also keeps the lexicographical order as well.
2. 

```c++


class Solution {
public:
	vector<string> findItinerary(vector<pair<string, string>> tickets) {
		// Each node (airport) contains a set of outgoing edges (destination).
		unordered_map<string, multiset<string>> graph;
		// We are always appending the deepest node to the itinerary, 
		// so will need to reverse the itinerary in the end.
		vector<string> itinerary;
		if (tickets.size() == 0){
			return itinerary;
		}
		// Construct the node and assign outgoing edges
		for (pair<string, string> eachTicket : tickets){
			graph[eachTicket.first].insert(eachTicket.second);
		}
		stack<string> dfs;
		dfs.push("JFK");
		while (!dfs.empty()){
			string topAirport = dfs.top();
			if (graph[topAirport].empty()){
				// If there is no more outgoing edges, append to itinerary
				// Two cases: 
				// 1. If it searchs the terminal end first, it will simply get
				//    added to the itinerary first as it should, and the proper route
				//    will still be traversed since its entry is still on the stack.
				// 2. If it search the proper route first, the dead end route will also
				//    get added to the itinerary first.
				itinerary.push_back(topAirport);
				dfs.pop();
			}
			else {
				// Otherwise push the outgoing edge to the dfs stack and 
				// remove it from the node.
				dfs.push(*(graph[topAirport].begin()));
				graph[topAirport].erase(graph[topAirport].begin());
			}
		}
		// Reverse the itinerary.
		reverse(itinerary.begin(), itinerary.end());
		return itinerary;
	}
};
```
## [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)
1. dfs to find the number of connected components
2. another method is union find


```c++
class Solution {
public: 
    vector<vector<char>> grid;
    int height=0;
    int width=0;
    void dfs( int i, int j){
         grid[i][j]='0';
        if(i>=1 && grid[i-1][j]=='1') dfs(i-1,j);
        if(i<=height-2 && grid[i+1][j]=='1') dfs(i+1,j);
        if(j>=1 && grid[i][j-1]=='1') dfs(i,j-1);
        if(j<=width-2 && grid[i][j+1]=='1') dfs(i,j+1);
             
    }    
    int numIslands(vector<vector<char>>& grid) {
         int ans=0;
         this->grid=grid;
         height=grid.size();
         if(height==0) return ans;
         width=grid[0].size();
         for(int i=0;i<height;i++){
             for(int j=0;j<width;j++){
                 if(this->grid[i][j]=='1'){
                     dfs(i,j);
                     ans++;
                 }
                 
             }
         }
        return ans;
        
    }
};


```
