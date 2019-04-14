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


## [947. Most Stones Removed with Same Row or Column]()
1. Two methods, a. union find b. dfs

```c++
class DSU{
public:
   vector<int> par;
   DSU(int N){
       par=vector<int>(N);
       for(int i=0; i<N; i++) par[i]=i;
   }
   int find(int x){
       while(par[x]!=x) x=par[x];
       return x;
   }
   void unify(int x, int y){
       par[find(x)]=find(y);
   }   
};

class Solution {
//1. count the number of connected components, and the answer will be stones.size()-number of connected components
//2. can be completed by dfs or union find
public:
    int removeStones(vector<vector<int>>& stones) {
        int N = stones.size();
        DSU dsu(20000);      
        for(auto stone: stones){
            dsu.unify(stone[0],stone[1]+10000);
        }
        int ans=0;
        unordered_set<int> st;
        for(auto stone : stones){
            int s=stone[0];
            //ans+=(dsu.find(s)==s);
            st.insert(dsu.find(s));
        }
        return N-st.size(); 
    }
};


//method 2:dfs
#define FILL(x,v) memset(x,v,sizeof(x))
class Solution {
//1. count the number of connected components, and the answer will be stones.size()-number of connected components
//2. pay attention to the representation of graph, which is a pseudo list
public:
    int removeStones(vector<vector<int>>& stones) {
         int N = stones.size();
       // graph[i][0] = the length of the 'list' graph[i][1:]
        int graph[N][N];
        for (int i = 0; i < N; ++i) graph[i][0]=0;
        for (int i = 0; i < N; ++i){
            for (int j = i+1; j < N; ++j){
                if (stones[i][0] == stones[j][0] || stones[i][1] == stones[j][1]) {
                    graph[i][++graph[i][0]] = j;
                    graph[j][++graph[j][0]] = i;
                }
            }
        }

        int ans = 0;
        bool seen[N];
        FILL(seen,0);
        for (int i = 0; i < N; ++i) if (!seen[i]) {
            stack<int> stack;
            stack.push(i);
            seen[i] = true;
            while (!stack.empty()) {
                int node = stack.top();
                stack.pop();
                
                for (int k = 1; k <= graph[node][0]; ++k) {
                    int nei = graph[node][k];
                    if (!seen[nei]) {
                        ans++;
                        stack.push(nei);
                        seen[nei] = true;
                    }
                }
            }
        }

        return ans;
    }
};
```
