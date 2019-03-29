## [695. Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)

```c++
class Solution {
public:
    bool isPossible(vector<int>& nums) {
        unordered_map<int,int> count;
        unordered_map<int,int> tails;
        for (int x: nums) count[x]++;

        for (int x: nums) {//iterate through nums, not count
            if (count[x] == 0) {
                continue;
            } else if (tails[x] > 0) {//x is the tail of a seq, push the tail one position to the right
                tails[x]--;
                tails[x+1]++;
            } else if (count[x+1] > 0 && count[x+2] > 0) {//not a tail, start a new seq
                count[x+1]--;
                count[x+2]--;
                tails[x+3]++;
            } else {
                return false;
            }
            count[x]--;
        }
        return true;
       
    }
};
```
