## [Intersection of Two ArraysII](https://leetcode.com/problems/intersection-of-two-arrays-ii/)
1. Use a count for one array, iterate through another array to determine if push to ans or not 

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> ans;
        unordered_map<int,int> nums1_map;
        for(int n1: nums1) nums1_map[n1]++;
        for(int n2: nums2){
            if (nums1_map[n2] >0){
                ans.push_back(n2);
                nums1_map[n2]--;
            }   
        }   
        return ans;
    }   

```


## [692.Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)
1. use set to store and sort the word
2. define cmp function as the sorting criteria

```c++
unordered_map<string,int> cont;
bool cmp(string a, string b){
        return cont[a]==cont[b]?a<b:cont[a]>cont[b]; 
    }

class Solution {
public:       
    vector<string> topKFrequent(vector<string>& words, int k) {  
        cont.clear();
        vector<string> ans;
        for(string word: words) cont[word]+=1;
        set<string,function<bool(string, string)>> s(words.begin(),words.end(),cmp);
        int count=0;
        for(string word: s){
            if(count>=k) break;
            ans.push_back(word);
            cout<<cont[word]<<"\n";
            count++;
        }
        return ans;
    }
};
```
