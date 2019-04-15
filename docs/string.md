## [1023. Binary String With Substrings Representint 1 to N](https://leetcode.com/contest/weekly-contest-129/problems/binary-string-with-substrings-representing-1-to-n/)
1. Key observation: to contain all the 1 to N binary representations, the length of string S.length()>logN/2*N/2 => N<4*S.length();
2. Then we can check all the numbers under N.

```c++
class Solution {
public:
    bool queryString(string S, int N) {
        if (N > 1000)
            return false;

        for (int i = 1; i <= N; i++) {
            string str = "";
            int x = i;

            while (x != 0) {
                str += x % 2 + '0';
                x /= 2;
            }

            reverse(str.begin(), str.end());

            if (S.find(str) == string::npos)
                return false;
        }

        return true;
    }
};
```


## [929.Unique Email Addresses](https://leetcode.com/problems/unique-email-addresses)

```
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> s;
        for(string email : emails){
            string ename;
            bool inlocal=true;
            bool ignore=false;
            for(int i=0;i<email.size();i++){
                if(email[i]=='@') inlocal=false;
                if(email[i]=='+' && inlocal) ignore=true;
                if((email[i] !='.' && inlocal && (!ignore)) || (!inlocal)) 
                    ename+=email[i];    
            }
            s.insert(ename);
        }
        return s.size();
    }
};

//better solution using string.find() and string.replace()
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> email_set;
        for(string email : emails){
            int i=email.find('@');
            string local = email.substr(0,i);
            string domain=email.substr(i);
            local= local.substr(0,local.find('+'));
            while((i=local.find('.'))!=string::npos)
                local.replace(i,1,"");
             email_set.insert(local+domain);
        }
        return email_set.size();
    }
};
```

## [158. Read N Characters Given Read4 II- Call multiple times](https://leetcode.com/problems/read-n-characters-given-read4-ii-call-multiple-times/)

1. use buf4 as the intermediate buffer between buf and file
2. keep buf4 across sessions
3. reminscent of two pointers on two arrays


```c++
// Forward declaration of the read4 API.
int read4(char *buf);

class Solution {
public:
    char buf4[4];
    int i4 = 0, n4 = 0;
    int read(char *buf, int n) {
       int i=0;
       while(i<n && (i4<n4 || (i4=0) < (n4=read4(buf4)))) {
            buf[i++]=buf4[i4++];
       }
        return i;
    }
};
```

## [157. Read N Characters Given Read4](https://leetcode.com/problems/read-n-characters-given-read4/)
1. break the loop when either n end or file end
2. can also use the solution of 158

```c++
// Forward declaration of the read4 API.
int read4(char *buf);

class Solution {
public:
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    int read(char *buf, int n) {
        int count=0;
        int r=0;
        while(count<=n/4){
           r=read4(buf+count*4);
           if(r<4 || n-count*4<4) break;
           count++;
        }
        return count*4+min(r,n-count*4);
    }
};
```


## [482. License Key Formatting](https://leetcode.com/problems/license-key-formatting/)
1. first count the number of valid char
2. output char by char, only output '-' when count%K==0 and count>0
3. the use of toupper()

```c++
class Solution {
public:
    string licenseKeyFormatting(string S, int K) {
        string ans;
        int count=0;
        for(char c: S)
            if(c!='-')
                count++;
        
        for(char c: S){
            if(c=='-') continue;
            char C=toupper(c);
            count--;
            ans+=C;
            if(count>0 && count%K==0)
                ans+='-';
        }
        return ans;
    }
};
```

## [777. Swap Adjacent in LR String](https://leetcode.com/problems/swap-adjacent-in-lr-string/)
1. double propelled two pointers
2. for each loop:
    a. skip all X
    b. if start[i]!= start[j] or the L of start is to the left of the corresponding L in end, or the R of start is to the right of its counterpart in end. return false
3. return true when pass all the checks

```c++
class Solution {
public:
    bool canTransform(string start, string end) {
        int N = start.size();
        int i = 0, j = 0;
        while (i < N && j < N) {
             while(i<N && start[i]=='X') i++;
             while(j<N && end[j]=='X') j++; 
             if((i<N) ^ (j<N) ) return false; 
             if(i<N && j<N){
                 if(start[i]!=end[j]) return false;
                 if(start[i]=='L' && i<j) return false;
                 if(start[i]=='R' && i>j) return false;           
             }
            i++;
            j++;
        }
        return true;
    }
};
```

## [640. Solve the Equation](https://leetcode.com/problems/solve-the-equation/)
1. parse char by char
2. differentiate between 0 coefficient and no coefficient

```c++
class Solution {
public:
    string solveEquation(string equation) {
        int coef_sum=0, con_sum=0;
        int num=-1;
        int side=1;
        int sign=1;
        for(char c : equation){        
            if(isdigit(c))
                num=(num==-1?c-'0':num*10+c-'0');
            else if(c=='x'){
                coef_sum+=side*sign*(num==-1?1:num);
                num=-1;
            }
            else {
                if(num>=0){
                  con_sum+=side*sign*num;
                  num=-1;
                }
                if(c=='=') {
                    sign=1;
                    side=-1;
                }
                else if(c=='+' ) sign=1;
                else if(c=='-') sign=-1;      
            }
            
        }
       if(num>=0) con_sum+=side*sign*num;
       if(coef_sum==0 &&  con_sum!=0) return "No solution";
       if(coef_sum==0) return "Infinite solutions";
       return "x="+to_string(-con_sum/coef_sum);
       
    }
};
```

## [833. Find and Replace in String](https://leetcode.com/problems/find-and-replace-in-string/)
1. use an array match to store the matching point
2. time complexity o(N*indexes.size())
3. copy from one index system to another, also there are a third index system, which is that of sources and targes.


```c++
class Solution {
public:
    string findReplaceString(string S, vector<int>& indexes, vector<string>& sources, vector<string>& targets) {
        int N=S.size();
        int match[N];
        fill(match,match+N,-1);
        int i=0;
        for(;i<indexes.size();i++){
            int k=indexes[i];
            if(S.substr(k,sources[i].size())==sources[i])
                match[k]=i;
        }
      
        string ans;
        i=0;
        while(i<N){
            if(match[i]>=0){
                ans+=targets[match[i]];
                i+=sources[match[i]].size();
            }
            else{
                ans+=S[i];
                i++;
            }
        }
        return ans;
        
    }
};

```

