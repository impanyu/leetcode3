## [347.Top K Frequent Element](https://leetcode.com/problems/top-k-frequent-elements/)
1. use a heap to store the index
2. use counts to act as the comparing criteria


```
unordered_map<int,int> counts;
bool compare(int a, int b){
        return counts[a]<counts[b];
    };

class Solution {
public: 
    vector<int> topKFrequent(vector<int>& nums, int k) {
        counts.clear();
        priority_queue<int,vector<int>,function<bool(int,int)>> pq(compare);
        vector<int> ans;
        for(int n :nums) counts[n]++;
        for(auto p : counts)
            pq.push(p.first);
        while(k>0){
            ans.push_back(pq.top());
            pq.pop();
            k--;
        }
        return ans;
    }
};


// an slightly differnt method defining a new class

class count_node{
public:
    int count;
    int index;
    count_node(int index,int count){
        this->count=count;
        this->index=index;
    }
    bool operator < (count_node other) const{
        return count<other.count;
    }   
};

unordered_map<int,int> counts;
bool compare(int a, int b){
        return counts[a]<counts[b];
    }
class Solution {
public:
    
    
    vector<int> topKFrequent(vector<int>& nums, int k) {
        counts.clear();
        priority_queue<int,vector<int>,function<bool(int,int)>> pq(compare);
        vector<int> ans;
        for(int n :nums) counts[n]++;
        for(auto p : counts)
            pq.push(p.first);
        while(k>0){
            ans.push_back(pq.top());
            pq.pop();
            k--;
        }
        return ans;
    }
};
```


## [253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)
1. create two compare class, one to sort end, one to sort intervals, the pq is used to register conference rooms
2. for each iterval, if there's no conflict, we pop the old meeting, otherwise we allocate the new meeting directly.
3. we can also think of a method using starting and closing events from left to right, and accumulate the maximum concurrent events, which should be the minimal conference rooms required

```c++
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class Compare1{
        public:
        bool operator()(int a,int b) {
             return a>b;
        }
    };
class Compare2{
        public:
        bool operator()(Interval a,Interval b) {
             return a.start<b.start;
        }
    };

class Solution {
public:
    int minMeetingRooms(vector<Interval>& intervals) {
        priority_queue<int, vector<int>, Compare1> pq;
        //unordered_map<int,int> active_events;
        sort(intervals.begin(),intervals.end(),Compare2());
        for(auto interval: intervals){
            if(!pq.empty() && pq.top()<=interval.start)
                pq.pop();
            pq.push(interval.end);   
        }
        return pq.size();
    }
};
```
