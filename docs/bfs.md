## [317. Shortest Distance from All Buildings](https://leetcode.com/problems/shortest-distance-from-all-buildings/)
1. use bfs to search from every existing buildings
2. accumulate distance sum for the empty space
3. avoid visit unreachable positions when doing bfs


```c++
class Solution {
public:
   int shortestDistance(vector<vector<int>> grid) {
    int m = grid.size(), n = grid[0].size();
    auto total = grid;
    int walk = 0, mindist, delta[] = {0, 1, 0, -1, 0};
    for (int i=0; i<m; ++i) {
        for (int j=0; j<n; ++j) {
            if (grid[i][j] == 1) {
                mindist = -1;
                auto dist = grid;
                queue<pair<int, int>> q;
                q.emplace(i, j);
                while (q.size()) {
                    auto ij = q.front();
                    q.pop();
                    for (int d=0; d<4; ++d) {
                        int i = ij.first + delta[d];
                        int j = ij.second + delta[d+1];
                        if (i >= 0 && i < m && j >= 0 && j < n && grid[i][j] == walk) {
                            grid[i][j]--;
                            dist[i][j] = dist[ij.first][ij.second] + 1;
                            total[i][j] += dist[i][j]-1;
                            q.emplace(i, j);
                            if (mindist < 0 || mindist > total[i][j])
                                mindist = total[i][j];
                        }
                    }
                }
                walk--;
            }
        }
    }
    return mindist;
  }
};
```


## [743.Network Delay Time]()
1. dijkstra's algorithm
2. accumulate max visiting time

```c++
class compare{
public:
    bool operator()(pair<int,int> a, pair<int,int> b){
       return a.second>b.second;
    }
};

class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        unordered_map<int,unordered_map<int,int>> graph;
        unordered_set<int> visited;
        for(auto edge: times) graph[edge[0]][edge[1]]=edge[2];
        priority_queue<pair<int,int>,vector<pair<int,int>>,compare> pq;
        pq.push(make_pair(K,0));
        int ans=INT_MIN;
        
        while(!pq.empty()){
            pair<int,int> cur=pq.top();
            pq.pop();
            int cur_id=cur.first, cur_time=cur.second;
            if(visited.count(cur_id)) continue;
            ans=max(cur_time,ans);
            visited.insert(cur_id);
            if(graph.count(cur_id))
                for(auto child: graph[cur_id])
                    if(!visited.count(child.first))
                        pq.push(make_pair(child.first,child.second+cur_time));
            
        }
        if(visited.size()!=N) return -1;
        return  ans;
    }
};
```


[913. Cat and Mouse](https://leetcode.com/problems/cat-and-mouse/)
1. the new graph is a direct graph with each node is a triple representing the game state: (pos of mouse, pos of cat, who play first)
2. the original graph keeps the new graph structure
3. color and degree matrix keeps the state for each node
4. traverse from the final states in a bottom up fansion

```c++
class Solution {
public:
    vector<vector<int>> graph;
    const int DRAW=0, MOUSE=1, CAT=2;
    int catMouseGame(vector<vector<int>>& graph) {
        this->graph=graph;
        int N= graph.size();
        //two types of node states
        int color[N][N][3];
        int degree[N][N][3];
        
        for(int m=0;m<N;m++){
            for(int c=0;c<N;c++){
                degree[m][c][1]=graph[m].size();
                degree[m][c][2]=graph[c].size();
                for(int x: graph[c])
                    if(x==0){
                        degree[m][c][2]--;
                        break;
                    }
            }
        }
        
        //initialize the q, pushing all MOUSE and CAT nodes
        deque<vector<int>> q;
        for(int c=0; c<N;c++){
            for(int t=1; t<=2;t++){
                color[0][c][t]=MOUSE;
                q.push_back({0,c,t});
                if(c>0){
                    color[c][c][t]=CAT;
                    q.push_back({c,c,t});
                }
            }
        }
        
        //bfs starts..
        while(!q.empty()){
           vector<int> cur=q.front();
           q.pop_front();
           for (vector<int> parent: parents(cur)){
               if(color[parent[0]][parent[1]][parent[2]]==DRAW){
                   if(parent[2]==color[cur[0]][cur[1]][cur[2]]){//parent win move, immediate coloring
                       color[parent[0]][parent[1]][parent[2]]=color[cur[0]][cur[1]][cur[2]];
                       q.push_back(parent);
                   }
                   else{
                       degree[parent[0]][parent[1]][parent[2]]--;
                       if(degree[parent[0]][parent[1]][parent[2]]==0){
                           color[parent[0]][parent[1]][parent[2]]=3-parent[2];
                           q.push_back(parent);
                       }
                   }
               }
               
           }
            
        }
        return color[1][2][1];      
    }
    
    vector<vector<int>> parents(vector<int> state){
        vector<vector<int>> ans;
        int m=state[0];
        int c=state[1];
        int t=state[2];
        if(t==2)
            for(int m2: graph[m])
                ans.push_back({m2,c,3-t});
        else
            for(int c2:graph[c])
                if(c2>0)
                    ans.push_back({m,c2,3-t});
        return ans;       
        
    }
};
```
