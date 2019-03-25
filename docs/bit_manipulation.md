## [78.Subsets](https://leetcode.com/problems/subsets/)
1. Each subset is represented by a int number under nums.size(), in which each bit represent the existence of the element
2. Can also be accomplished by backtracking, where in each node we choose if the element is chosen or not.
3. Pay attention the backtracking in 2 is not the same as the backtraking if the permutation of the set is required. If that is the case, for each node we need to use a array to store the state of a node, in which each element has three states, 1,0,-1, and -1 means the element is prohibited.
```c++
#define FILL(a,v) memset(a,v,sizeof(a));
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        int number_of_subsets=pow(2,nums.size());
        for(int i=0;i<number_of_subsets;i++){
            vector<int> subset;
            for(int j=0;j<nums.size();j++){
                if((i & (1<< j))!=0) subset.push_back(nums[j]);
            }
            ans.push_back(subset);
        }
        return ans;       
    }
     
};
```
