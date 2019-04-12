## [261. Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)
1. union find can be used to detect circle and number of components

```c++
class union_set{
public:
    vector<int> pars;
    vector<int> ranks;
    union_set(int n){
        pars=vector<int>(n,-1);
        ranks=vector<int>(n,1);
    }
    int find_root(int node){
       while(pars[node]!=-1)
           node=pars[node];
       return node;
    }
    bool merge(int n1, int n2){
        int root_n1=find_root(n1);
        int root_n2=find_root(n2);
        if(root_n1 == root_n2) return false;// merge two nodes which are already in a set, fail
        if(ranks[root_n1]>=ranks[root_n2]) {
            pars[root_n2]=root_n1;
            ranks[root_n1]=max(ranks[root_n1],ranks[root_n2]+1); 
        }
        else pars[root_n1]=root_n2;
        return true;
    } 
    
    int set_num(){
        int ans=0;
        for(int p: pars){
            ans+=(p==-1);
        }
        return ans;
    }
};

class Solution {
public:
    bool validTree(int n, vector<pair<int, int>>& edges) {
        union_set u(n);
        for(auto p: edges){
            bool r=u.merge(p.first,p.second);
            if(!r) return false;
        }
        return u.set_num()==1;     
    }
};
```
