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


## [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
1. traverse through all the positions, and only check back when encountering a descending bar, which is a potential bottle-neck
2. use a stack to store previous positions need to check.


```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack <int> stk;
        stk.push(-1);
        int maxarea = 0;
        
        for (int i = 0; i < heights.size(); ++i) {
            while (stk.top() != -1 && heights[stk.top()] >= heights[i]){
                int height=heights[stk.top()];
                stk.pop();
                maxarea = max(maxarea, height * (i - stk.top() - 1));            
         }
            stk.push(i);
        }
        while (stk.top() != -1){
            int height=heights[stk.top()];
            stk.pop();
            maxarea = max(maxarea, height* ((int)heights.size() - stk.top() -1));

        }
        return maxarea;
    }
};
```

## [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)
1. reduced to problem 84, and for each row, we pass the histogram to 84, accumulate the maximal area 


```c++
class Solution {
public:
    int leetcode84(vector<int>& heights) {
        stack <int> stk;
        stk.push(-1);
        int maxarea = 0;
    
        for (int i = 0; i < heights.size(); ++i) {
            while (stk.top() != -1 && heights[stk.top()] >= heights[i]){
                int height=heights[stk.top()];
                stk.pop();
                maxarea = max(maxarea, height * (i - stk.top() - 1));    
         }   
            stk.push(i);
        }   
        while (stk.top() != -1){
            int height=heights[stk.top()];
            stk.pop();
            maxarea = max(maxarea, height* ((int)heights.size() - stk.top() -1));

        }   
        return maxarea;
    }   

    
    int maximalRectangle(vector<vector<char>>& matrix) {
        if(matrix.size()==0) return 0;
        int ans=0;
        vector<int> dp(matrix[0].size(),0);
        for(int i=0;i<matrix.size();i++){
            for(int j=0;j<matrix[0].size();j++){
                dp[j]=matrix[i][j]=='1'?dp[j]+1:0;
            }
            ans=max(ans,leetcode84(dp));
        }
        return ans;
        
    }
};
```

## [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)
1. define a Compare functor for Interval
2. iterate through the intervals and update the ans accordingly
3. reference to 253. meeting rooms II, which also traverse through all sorted intervals and accumulate maximal rooms required.
4. 56 pay attention to last end, 253 pay attention to first end. 

```c++
class compare
{
     public:
       bool operator() (Interval a, Interval b)
       {
           return a.start<b.start;
        }
};

/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class Solution {
public:
    vector<Interval> merge(vector<Interval>& intervals) {       
        sort(intervals.begin(),intervals.end(),compare());
        vector<Interval> ans;
        for(auto interval: intervals){
            if(ans.empty() || ans.back().end<interval.start)
                ans.push_back(interval);
            else
                ans.back().end=max(interval.end,ans.back().end);
        }
        return ans;
        
    }
};
```

## [849. Maximize Distance to Closest Person](https://leetcode.com/problems/maximize-distance-to-closest-person/)
1. define left and right as the distance from the closet person sitting to the left and right
2. three pass of the input array
3. similar to trapping rain water



```c++
class Solution {
public:
    int maxDistToClosest(vector<int>& seats) {
        int left[seats.size()];
        int right[seats.size()];
        fill(left,left+seats.size(),seats.size());
        fill(right,right+seats.size(),seats.size());
        int ans=0;
        cout<<seats.size()<<"\n";
        for(int i=0;i<seats.size();i++){
            if(seats[i]==1) left[i]=0;
            else if(i>0) left[i]=left[i-1]+1;
        }
        for(int i=seats.size()-1;i>=0;i--){
            if(seats[i]==1) right[i]=0;
            else if(i<seats.size()-1) right[i]=right[i+1]+1;
        }
        for(int i=0;i<seats.size();i++){
           if(seats[i]==0) ans=max(ans,min(left[i],right[i]));          
        }
        return ans;
    }
};
```


## [900. RLE Iterator](https://leetcode.com/problems/rle-iterator/)
1. there are two states (i,q), which need to keep across session
2. there are one extra session n, which need to keep with one session


```c++
class RLEIterator {
public:
    vector<int> A;
    int i;//A[i+1] is the current value
    int q;//the exhausted count of current value
    RLEIterator(vector<int> A) {
        this->A=A;
        i=q=0;
    }
    
    int next(int n) {
        while(i<A.size()){
            if(n>A[i]-q){ //n exceeds the left count of current value 
               n-=A[i]-q;
               i+=2;
               q=0;
            }
            else{
                q+=n;
                return A[i+1];
            }
        }
        return -1;
    }
};

/**
 * Your RLEIterator object will be instantiated and called as such:
 * RLEIterator obj = new RLEIterator(A);
 * int param_1 = obj.next(n);
 */
```
