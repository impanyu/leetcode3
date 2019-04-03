## [941. Valid Mountain Array](https://leetcode.com/problems/valid-mountain-array/)
1. one pass through the array to count the ascending slop and descending slop.
2. return false when reaching peak at both ends or encountering more than one peak. 

```c++
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        int N = A.size();
        int i = 0;
        // walk up
        while (i+1 < N && A[i] < A[i+1])
            i++;

        // peak can't be first or last
        if (i == 0 || i == N-1)
            return false;

        // walk down
        while (i+1 < N && A[i] > A[i+1])
            i++;

        return i == N-1;
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
