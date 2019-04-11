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
