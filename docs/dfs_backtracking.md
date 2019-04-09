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


