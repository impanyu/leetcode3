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

## [406.Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/)
1. add larger height first, then the number of people in front will reflect current order
2. define comp as the sort function
```c++
class Solution {
public:
    vector<pair<int, int>> reconstructQueue(vector<pair<int, int>>& people) {
          auto comp = [](const pair<int, int>& p1, const pair<int, int>& p2){ 
              return p1.first > p2.first || (p1.first == p2.first && p1.second < p2.second); 
          };
    sort(people.begin(), people.end(), comp);
    vector<pair<int, int>> res;
    for (auto& p : people) 
        res.insert(res.begin() + p.second, p);
    return res;
    }
};
```


## [134. Gas Station](https://leetcode.com/problems/gas-station/)
1. find the last point after which the accumulative sum(represented by remaining) is positive
2. check the total accumulative sum, if it is positive, then we find a starting point

```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost)    {
       int total=0, remaining=0;
       int start=0;
       for(int i=0;i<gas.size();i++){
           total+=gas[i]-cost[i];
           remaining+=gas[i]-cost[i];
           if(remaining<0){
               remaining=0;
               start=i+1;
           }
       }
      return total<0?-1:start%gas.size();

    }
};
```

## [135. Candy](https://leetcode.com/problems/candy/)
1. two pass from both directions, only increase the candy when necessary


```c++
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candies(ratings.size());
        fill(candies.begin(),candies.end(),1);
        for(int i=1; i<ratings.size();i++){
            if(ratings[i]>ratings[i-1])
                candies[i]=candies[i-1] + 1;
        }
        int sum= candies[ratings.size()-1];
        for(int i = ratings.size()-2;i>=0;i--){
            if(ratings[i]> ratings[i+1])
                candies[i]=max(candies[i],candies[i+1]+1);
            sum+=candies[i];
        }
        return sum;
        
    }
};
```

