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



## [803. Bricks Falling When Hit](https://leetcode.com/problems/bricks-falling-when-hit/)
1. starting from when all the hits are completed
2. join bricks from end to start, and count the number of bricks attached to the roof before and after each join, then the faling bricks will be the discrepancy of the two minus 1.  
3. there's still bugs


```c++
class DSU{
public:
    vector<int> parent;
    vector<int> rank;
    vector<int> sz;//the size of each set;
    
    DSU(int N){
        parent=vector<int>(N);
        rank=vector<int>(N);
        sz=vector<int>(N);
        for(int i=0; i<N;i++) parent[i]=i;
        fill(begin(sz),begin(sz),1);
    }
    
    int find(int x){
        if(parent[x]!=x) return find(parent[x]);
        return parent[x];
    }
    
    void uni(int x, int y){
        int xp=find(x);
        int yp=find(y);
        if(xp==yp) return;
        if(rank[xp]<rank[yp]) swap(xp,yp);
        if(rank[xp]==rank[yp]) rank[xp]++;
        parent[yp]=xp;
        sz[xp]+=sz[yp];     
    }  
    
    int size(int x){
        return sz[find(x)];
    }  
    int top(){
        return size(sz.size()-1);
    }  
};


class Solution {
public:
    vector<int> hitBricks(vector<vector<int>>& grid, vector<vector<int>>& hits){  
        int R =grid.size(), C=grid[0].size();
        int dr[]={1,0,-1,0};
        int dc[]={0,1,0,-1};
        
        vector<vector<int>> A(grid);
        for(auto hit : hits)
            A[hit[0]][hit[1]]=0;//starting from all hits completed
        
        DSU dsu(R*C+1);
        for(int r=0;r<R;r++){
            for(int c=0;c<C;c++){
                if(A[r][c]==1){
                    int id=r*C+c;
                    if(r==0) dsu.uni(id,R*C);
                    if(r>0 && A[r-1][c]==1) dsu.uni(id,(r-1)*C+c);
                    if(c>0 && A[r][c-1]==1) dsu.uni(id,r*C+(c-1));
                }      
            }
        }
        
        int h=hits.size();
        vector<int> ans(h--);
        
        
        for(auto hit : hits){
            int pre_roof=dsu.top();
            int r=hit[0];
            int c=hit[1];
            if(grid[r][c]==0) {
                h--;
                continue;
            }
            int id=r*C+c;
            for(int k=0;k<4;k++){
                int nr=r+dr[k];
                int nc=c+dc[k];
                if(nr>=0 && nr<R && nc>=0 && nc<C && A[nr][nc]==1)
                    dsu.uni(id,nr*C+nc);
            }
            if(r==0) dsu.uni(id,R*C);
            A[r][c]=1;
            ans[h--]=max(0,dsu.top()-pre_roof-1);
            
        }
        return ans;
    }
};
```
