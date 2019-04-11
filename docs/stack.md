## [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)

```c++
class Solution {
//check empty before pop a stack
//two pointer is another method
public:
    bool backspaceCompare(string S, string T) {
        stack<char> stackS;
        stack<char> stackT;
        for(char c: S){
            if(c!='#') stackS.push(c);
            else if(!stackS.empty())
                stackS.pop();
        }
        for(char c: T){
            if(c!='#') stackT.push(c);
            else if(!stackT.empty())
                stackT.pop();
        }
        string s,t;
        while(!stackS.empty()){
            s+=stackS.top();
            stackS.pop();
        }
        while(!stackT.empty()){
            t+=stackT.top();
            stackT.pop();
        }
        return s==t;
    }
};
```


## [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
1. keep the current unsolved day index, and try our best to solve it when encountering a larger temperature
2. an similar and symmetric solution is to traver from right to left and keep in the stack the possible larger temperatures(see standard solution)

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        stack<int> st;
        vector<int> ans(T.size());
        for(int i=0;i<T.size();i++){ 
            while(!st.empty() && T[st.top()]<T[i]){
                ans[st.top()]=(i-st.top());
                st.pop();
            }
            st.push(i);
        }
        return ans;
    }
};


```
