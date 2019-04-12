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
