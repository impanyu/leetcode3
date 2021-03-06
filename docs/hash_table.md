## [Intersection of Two ArraysII](https://leetcode.com/problems/intersection-of-two-arrays-ii/)
1. Use a count for one array, iterate through another array to determine if push to ans or not 

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> ans;
        unordered_map<int,int> nums1_map;
        for(int n1: nums1) nums1_map[n1]++;
        for(int n2: nums2){
            if (nums1_map[n2] >0){
                ans.push_back(n2);
                nums1_map[n2]--;
            }   
        }   
        return ans;
    }   

```


## [692.Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)
1. use set to store and sort the word
2. define cmp function as the sorting criteria

```c++
unordered_map<string,int> cont;
bool cmp(string a, string b){
        return cont[a]==cont[b]?a<b:cont[a]>cont[b]; 
    }

class Solution {
public:       
    vector<string> topKFrequent(vector<string>& words, int k) {  
        cont.clear();
        vector<string> ans;
        for(string word: words) cont[word]+=1;
        set<string,function<bool(string, string)>> s(words.begin(),words.end(),cmp);
        int count=0;
        for(string word: s){
            if(count>=k) break;
            ans.push_back(word);
            cout<<cont[word]<<"\n";
            count++;
        }
        return ans;
    }
};
```

## [288. Unique Word Abbreviation](https://leetcode.com/problems/unique-word-abbreviation/)
1. use abbrs to store abbreviation count, use dict to store word set
2. return isunique if no abbreviation in the abbrs or there's only one abbrs which is generated by word itself.
3. two domain:word domain and abbr domain.

```c++
class ValidWordAbbr {
public:
    
    ValidWordAbbr(vector<string>& dictionary) {
        dict=unordered_set<string>(dictionary.begin(),dictionary.end());
        for(string word : dict){
            abbrs[to_abbr(word)]++;
        }
    }
    
    bool isUnique(string word) {
        string abbr = to_abbr(word);
        return abbrs[abbr]==0 || abbrs[abbr]==1 && dict.count(word)!=0;
    }
private: 
     unordered_map<string,int> abbrs;
     unordered_set<string> dict;
     
     string to_abbr(string s) {
        int n = s.length();
        if (n <= 2) {
            return s;
        }
        return string(1,s[0]) + to_string(n - 2) + s[n - 1];
    }
};
```

## [939. Minimum Area Rectangle]()
1. count by diagonal, search through all possible diagnals, determine if it can form a rectangle. Accumulate min are on the way.
2. the encoding of a pair of number is neat
3. another similar method is to search through all possible vertical edge.

```c++
class Solution {
public:
    int minAreaRect(vector<vector<int>>& points) {
        unordered_set<int> ps; 
        int ans=INT_MAX;
        for(auto point: points)
            ps.insert(point[0]*40001+point[1]);
        
        for(int i=0; i<points.size();i++){
            for(int j=i+1;j<points.size();j++){
                if(points[i][0]!=points[j][0] && points[i][1]!=points[j][1]){
                     int point3=40001*points[i][0]+points[j][1];
                     int point4=40001*points[j][0]+points[i][1];
                     if(ps.count(point3)>0 && ps.count(point4)>0){
                         int area=abs(points[i][0]-points[j][0])*abs(points[i][1]-points[j][1]);
                         ans=min(ans,area);
                     }
                }
            }
        }
       return ans<INT_MAX?ans:0;
        
    }
};
```


## [299. Bulls and Cows](https://leetcode.com/problems/bulls-and-cows/)

```c++
class Solution {
//have to be two pass
public:
    string getHint(string secret, string guess) {
        unordered_map<int,int> char_count;
        int cows=0,bulls=0;
        for(int i=0;i < secret.size();i++){
            if(secret[i]==guess[i]) cows++;
            else char_count[secret[i]]++;
        }
        for(int i=0;i < secret.size();i++){
            if(secret[i]!=guess[i] && char_count[guess[i]]){
                   char_count[guess[i]]--;
                    bulls++;
                }
        }
        return to_string(cows)+"A"+to_string(bulls)+"B";
        
    }
};
```

## [734. Sentence Similarity](https://leetcode.com/problems/sentence-similarity/)
1. do not use a map! Because we need to represent a relation here rather than a mapping!!
2. do not forget check words1[i]!=words2[i]



```c++
class Solution {
public:
    bool areSentencesSimilar(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
        if(words1.size()!=words2.size()) return false;
        unordered_set<string> pair_set;
        for(auto p: pairs){
            string pair1=p.first+"#"+p.second;
            string pair2=p.second+"#"+p.first;
            pair_set.insert(pair1);
            pair_set.insert(pair2);
        }
        for(int i=0;i<words1.size();i++){
            if(words1[i]!=words2[i] && pair_set.count(words1[i]+"#"+words2[i])==0 )
                return false;     
        }
        return true;
    }
};
```

## [205. Isomorphic Strings]()

```c++
//method 1:
class Solution {
//1.refuse is easier to consider than accept
//2. consider both directions.
public:
  bool isIsomorphic(string s, string t) {
    unordered_map<char,char> m1;
    unordered_map<char,char> m2;
    if(s.size()!=t.size()) return false;
    for(int i=0; i<s.size();i++){
         if(!m1.count(s[i])) m1[s[i]]=t[i];
         if(!m2.count(t[i])) m2[t[i]]=s[i];
         if(m1[s[i]]!=t[i] || m2[t[i]]!=s[i])
             return false;
     }
     return true;   
}
};

//method 2:
//use map from char to pos, instead of directly mapping from char to char.
class Solution {
public:
  bool isIsomorphic(string s, string t) {
  int ms[128] = {}, mt[128] = {}, n = s.size(), i = 0;
  for (; i < n && ms[s[i]] == mt[t[i]]; ++i) ms[s[i]] = mt[t[i]] = i + 1;
  return i == n;
}
};
```
