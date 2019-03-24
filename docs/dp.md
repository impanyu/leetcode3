## [1021.Best Sightseeing Pair](https://leetcode.com/contest/weekly-contest-129/problems/best-sightseeing-pair/)
1. update max of A[i]+i and max of ans each iteration
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& A) {
        int ans=A[0]+A[1]-1;
        int max_ai=max(A[0],A[1]+1);
        for(int i=2;i<A.size();i++){
            int aj=A[i]-i;
            int ai=A[i]+i;
            ans=max(ans,max_ai+aj);
            max_ai=max(ai,max_ai);
            
        }
        return ans;
        
    }
};
