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

## [844.Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)
1. use two pointer i and j to iteratively compare each valid positions of two strings
2. stack is another naive solution

```c++
class Solution {
    public:
    bool backspaceCompare(string S, string T) {
        int i = S.length() - 1, j = T.length() - 1;
        int skipS = 0, skipT = 0;

        while (i >= 0 || j >= 0) { // While there may be chars in build(S) or build (T)
            while (i >= 0) { // Find position of next possible char in build(S)
                if (S[i] == '#') {skipS++; i--;}
                else if (skipS > 0) {skipS--; i--;}
                else break;
            }
            while (j >= 0) { // Find position of next possible char in build(T)
                if (T[j] == '#') {skipT++; j--;}
                else if (skipT > 0) {skipT--; j--;}
                else break;
            }
            // If two actual characters are different
            if (i >= 0 && j >= 0 && S[i] != T[j])
                return false;
            // If expecting to compare char vs nothing
            if ((i >= 0) != (j >= 0))
                return false;
            i--; j--;
        }
        return true;
    }
};
```

## [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
1. Two pointer i, j propelled by j. i point to the current pos written to, j iterate through all the elements
2. Copy only happens when a new element is encountered

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size()<=1) return nums.size();
        int i=1;
        for(int j=1;j<nums.size();j++){
            if(nums[j]!=nums[j-1]){
                nums[i]=nums[j];
                i++;
            }
        }
        return i;
    }
};
```

## [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
1. Two pointers i j meet in the middle.

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        int i=0, j=s.size()-1;
        while(i<j){
            char a=tolower(s[i]);
            char b=tolower(s[j]);
            if(a==b){
                i++;
                j--;
            } 
            else if(!isalpha(a) && !isdigit(a)) i++;
            else if(!isalpha(b) && !isdigit(b)) j--;
            else return false;
            
        }
        return true;
    }
};
```
