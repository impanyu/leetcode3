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


## [Intersection of Two ArraysII](https://leetcode.com/problems/intersection-of-two-arrays-ii/)
1. two pointers i1, i2 without propeller 

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        int n1 = (int)nums1.size(), n2 = (int)nums2.size();
        int i1 = 0, i2 = 0;
        vector<int> res;
        while(i1 < n1 && i2 < n2){
            if(nums1[i1] == nums2[i2]) {
                res.push_back(nums1[i1]);
                i1++;
                i2++;
            }
            else if(nums1[i1] > nums2[i2]){
                i2++;
            }
            else{
                i1++;
            }
        }
        return res;
    }
};
```

## [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
1. Two pointers first and second.
2. First put first on the n+1 th position and then advance first and second at the same time. When first reach the end, second will be at the position prior to the node need to be deleted

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy=new ListNode(0);
        dummy->next=head;
        ListNode* first=dummy;
        ListNode* second=dummy;
        while(n>=0){
            first=first->next;
            n--;
        }
        while(first!=nullptr){
            first=first->next;
            second=second->next;
        }
        second->next=second->next->next;
        return dummy->next;
    }
};
```

## [941. Valid Mountain Array](https://leetcode.com/problems/valid-mountain-array/)
1.two pointers start from both ends, return true only if two pointers meet at a non-end position

```c++
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        int i=0, j=A.size()-1;
        while(i<j){
           bool progress=false;
           if(A[i]<A[i+1]) {
               i++;
               progress=true;
           }
           if(A[j]<A[j-1]) {
               j--;
               progress=true;
           }
            if(!progress) return false;
        }
        return i==j && (i!=0 && i!=A.size()-1);
    }
};
```

## [904. Fruit into Baskets](https://leetcode.com/problems/fruit-into-baskets/)

1. typical two pointers(i,j)
2. two sub methods:
   a. propelled by i, then for each i, check j(approach 1)
   b. propelled by j, and keeps a starting pointer i.
3. this is method b.

```c++
class Solution {
public:
    int totalFruit(vector<int>& tree) {
        int ans=0, i=0,j=0;
        unordered_map<int,int> fruits;//maps from tree type to the number of fruits of this type

        for(;j<tree.size();j++){
            fruits[tree[j]]++;
            while(fruits.size()>2){//loop until there are no more type fruits[i]
                fruits[tree[i]]--;
                if(fruits[tree[i]]==0)
                    fruits.erase(tree[i]);
                i++;
            }
            ans=max(ans,j-i+1);

        }
        return ans;
    }
};
```
