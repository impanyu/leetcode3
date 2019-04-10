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

## [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
1. The observation is the maximum product will always start from one end except the existence of 0
2. When there are 0 inside, we just restart the array.

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        const int n = nums.size();
        int ans = INT_MIN;
        int front = 0;
        int back = 0;
        for (int i=0; i<n; i++) {
            front = front ? front*nums[i] : nums[i];
            back = back ? back*nums[n-1-i] : nums[n-1-i];
            ans = max(ans, max(front, back));
        }
        return ans;
    }
};
```

## [729.My Calendar I](https://leetcode.com/problems/my-calendar-i/solution/)
1.store the calendar in a map
2.use map iterator and reverse_iterator to get the pos before and after the insertion position


```c++
class MyCalendar {
public:
    map<int,int> calendar;
    MyCalendar() {
        
    }
    
    bool book(int start, int end) {
        map<int,int>::iterator i=calendar.begin();
        map<int,int>::reverse_iterator j=calendar.rbegin();
        for(;i!=calendar.end();i++){
            if(start<=i->first) break;//next pointer
        }
        for(;j!=calendar.rend();j++){
            if(start>=j->first) break;//prev pointer
        }
        if((j==calendar.rend() || j->second<=start) && (i==calendar.end() || end<=i->first)){
            calendar[start]=end;
            return true;
        }
        return false;
    }
};
```

## [731. My Calendar II](https://leetcode.com/problems/my-calendar-ii/)
1.use two vector of pairs to store calendar itself and overlaps respectively
2.when book, check the overlap with any element in the overlaps, if exist, return false.

```c++
class MyCalendarTwo {
public:
    vector<pair<int,int>> calendar;
    vector<pair<int,int>> overlaps;
    MyCalendarTwo() {
        
    }
    
    bool book(int start, int end) {
       for(pair ov: overlaps)
           if(ov.first<end && ov.second>start) return false;
       for(pair iv: calendar)
           if(iv.first<end && iv.second>start) 
               overlaps.push_back(make_pair(max(start,iv.first),min(end,iv.second)));
       
       calendar.push_back(make_pair(start,end));
       return true;
        
    }
};
```
