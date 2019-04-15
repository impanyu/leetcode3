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
