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


