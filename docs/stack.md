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



## [394. Decode String](https://leetcode.com/problems/decode-string/)
1. use a stack to store current replication and another stack to store previous string of the samee level.
2. once encoutering a close bracket, we pop the previous string and concatenate it with current string(tmp) within the bracket for replications.pop() times



```c++
//2[ab3[bc]cd]ab--->top(2)2[top(1)3[tmp]cd]ab

class Solution {
public:
    string decodeString(string s) {
        if(!s.size()) return "";
        stack<int> replications;
        stack<string> strings;
        int rep=0;
        string tmp;
        for(char c : s){
            if(isdigit(c)) rep=rep*10+c-'0';
            else if(c=='['){
                strings.push(tmp);
                replications.push(rep);
                rep=0;
                tmp="";           
            }
            else if(c==']'){
                int count=replications.top();
                replications.pop();
                string top=strings.top();
                strings.pop();
                while(count--)
                    top+=tmp;
                tmp=top;
            }
            else
                tmp+=c;
        }
          
        return tmp;
    }
};
```
