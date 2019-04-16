## [681. Next Closet Time](https://leetcode.com/problems/next-closest-time/)
1. generate all statisfied HH:MM string and accumulate the smallest elapse
2 .string to int: stoi
3. int to string: oss


```c++
class Solution {
public:
    string nextClosestTime(string time) {
        
        unordered_set<int> allowed;
        int min_elapse=24*60;
        int current=stoi(time.substr(0,2))*60+stoi(time.substr(3));
        int ans=current;
        for(char c : time){
            if(c!=':') allowed.insert(c-'0');
        }
        
        for(int a: allowed){
            for(int b: allowed){
                for(int c: allowed){
                    for(int d: allowed){
                        int hour=(a*10+b);
                        int minute=c*10+d;
                        if(hour>=24 || minute>=60) continue;
                        int t=hour*60+minute;
                        int elapse=(t-current+60*24)%(60*24);
                        if(elapse>0 && elapse<min_elapse){
                            min_elapse=elapse;
                            ans=t;
                        }      
                    }
                }
            }
        }
        
      ostringstream oss;
      oss<<setfill('0')<<setw(2)<<ans/60<<":"<<setw(2)<<ans%60;
        return oss.str();             
    }
};
```


## [524. Longest Word in Dictionary through Deleting](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/)

```c++
class compare{
public:
    bool operator()(string a, string b){
        return a.size()==b.size()?a<b:a.size()>b.size();
    }
};
class Solution {
//0. find max length subsequence under ordering
//1. The use of Compare for sorting
//2. For each word in the dictionary, if it is a subsequence of s, then return it
//3. Can be accomplished without sorting
//4. The use of two pointers for subseq determination. This is the two pointers running on two different array.
public:
    bool subseq(string sub, string s){
        int i=0;
        int j=0;
        while(i<sub.size() && j<s.size()){
            if(sub[i]==s[j]) i++;
            j++;
        }
        return i==sub.size();
    }
    string findLongestWord(string s, vector<string>& d) {
        sort(begin(d),end(d),compare());
        for(string word : d){
            if(subseq(word,s))
                return word;
        }
        return "";
    }
};
```
