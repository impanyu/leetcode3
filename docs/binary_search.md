## [74.Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
## [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
1. These two problems can be solved in one general method, which treat the matrix as a binary search tree rooted at the upper-left corner
2. Problem 74 can be transformed to a binary search on 1D array, where we flatten the 2D matrix row by row.


```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0) return false;
        int i=0;
        int j=matrix[0].size()-1;
        while(i<matrix.size() && j>=0){
            if(matrix[i][j]==target) return true;
            else if(matrix[i][j]>target) j--;
            else i++;
        }
        return false;
    }
};
```

[69. Sqrt(x)](https://leetcode.com/problems/sqrtx/)
1. a variation of binary search for upperbound case
2. here we return the index left to upperbound instead of upperbound of itself

```c++
class Solution {
public:
    int mySqrt(int x) {
        long a=0;
        long b=(long)x+1;//here must be x+1 instead of x
        long mid=0;
        while(a<b){
            mid=(a+b)/2;
            if(mid*mid>x) b=mid;
            else a=mid+1;       
        }
        return a-1;
    }
};

```



## [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
1. consider the number of smaller elements as a function of the value of matrix


```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int l=matrix[0][0];
        int r=matrix[matrix.size()-1][matrix[0].size()-1];
        while(l<r){
            int mid=(l+r)/2;
            int total=0;
            for(auto& row: matrix) total+=find_upperbound(row,mid);
            if(total>=k) r=mid;
            else l=mid+1;
        }
        return l;
    }
    
    int find_upperbound(vector<int>& row, int m){
        int i=0,j=row.size();
        while(i<j){
            int mid=(i+j)/2;
            if(row[mid]>m) j=mid;
            else i=mid+1;
        }
        return i;
    }
};
```


## [911. Online Election]()
1. iterate through all the time steps and update the leader and time stamp accordingly
2. do a binary search on the result vector from 1.



```c++
class Vote {
public:
    int person, time;
    Vote(int p, int t) {
        person = p;
        time = t;
    }
};
class TopVotedCandidate {
public:
    vector<Vote> A;//leader/time for all time steps when there's a leader change
    TopVotedCandidate(vector<int> persons, vector<int> times) {
        unordered_map<int,int> vote_count;
        int current_leader=-1;
        int current_votes=0;
        for(int i=0;i<persons.size();i++){
            vote_count[persons[i]]++;
            if(vote_count[persons[i]]>=current_votes ){
                if(persons[i]!=current_leader){
                    current_leader=persons[i];
                    A.push_back(Vote(current_leader,times[i]));  
                }
                current_votes=vote_count[persons[i]];
            }
            
        }
        
        
    }
    
     int q(int t) {// exactly the same as binary search, except return lo-1 once outside the loop
        int lo = 0, hi = A.size();
        while (lo < hi) {
            int mi = (hi + lo) / 2;
            //if(A[mi].time == t) return A[mi].person;
            if (A[mi].time <= t)
                lo = mi + 1;
            else
                hi = mi;
        }

        return A[lo-1].person;
    }
};

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * TopVotedCandidate obj = new TopVotedCandidate(persons, times);
 * int param_1 = obj.q(t);
 */
```



## [774. Minimize Max Distance to Gas Station](https://leetcode.com/problems/minimize-max-distance-to-gas-station/)
1. k is the decreasing function of d, use binary search


```c++
class Solution {
public:
    vector<int> stations;
    double minmaxGasDist(vector<int>& stations, int K) {
        this->stations=stations;
        double l=0;
        double r=1e8; 
        while(l+1e-6<r){
            double m =(l+r)/2.0;
            if(min_k_for_d(m)<=K) r=m;//take lower bound, since we need to get smallest possible value. 
            else l=m;
        }
        return l;
    }
    
    int min_k_for_d(double D){
        int ans=0;
        for(int i=1;i<stations.size();i++)
            ans+=(stations[i]-stations[i-1])/D;
        return ans;
        
    }
      
};
```
