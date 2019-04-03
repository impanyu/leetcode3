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
