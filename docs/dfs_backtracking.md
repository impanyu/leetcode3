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
