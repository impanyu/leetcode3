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
