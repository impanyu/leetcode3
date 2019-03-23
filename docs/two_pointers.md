### [360. Sort Transformed Array](https://leetcode.com/problems/sort-transformed-array/)
```c++
class Solution {
public:
    int a,b,c;
    vector<int> sortTransformedArray(vector<int>& nums, int a, int b, int c) {
         this->a=a;
         this->b=b;
         this->c=c;
         vector<int> ans(nums.size());
         int i=0, j=nums.size()-1;
         int inx=0;
         if(a<0){
             while(i<=j){
                 if(f(nums[i])<=f(nums[j])){
                      ans[inx]=f(nums[i]);
                       i++;
                     }
                 else{
                     ans[inx]=f(nums[j]);
                       j--;
                 }
                 inx++;
             }
         }
        else{
            inx=nums.size()-1;
            while(i<=j){
                 if(f(nums[i])>=f(nums[j])){
                      ans[inx] = f(nums[i]);
                       i++;
                     }
                 else{
                     ans[inx] = f(nums[j]);
                      j--;
                 }
                inx--;
            }
        }
        return ans;    
    }
    int f(int x){
        return a*x*x+b*x+c;
    }
};
```
