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
