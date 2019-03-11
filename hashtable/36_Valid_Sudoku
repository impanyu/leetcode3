class Solution {
//1.use three hashset to store the encountered value
//2.return false if the rule if violated
//3. the set can also be implemented by a bitmap
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        unordered_set<char> rowset[9];
        unordered_set<char> columnset[9];
        unordered_set<char> boxset[9];
        for(int i=0;i<9;i++){
            for(int j=0;j<9;j++){
                char c=board[i][j];
                if(c=='.') continue;
                if(rowset[i].count(c)>0 || columnset[j].count(c)>0 || boxset[i/3*3+j/3].count(c)>0)
                 return false;
                else{
                    rowset[i].insert(c);
                    columnset[j].insert(c);
                    boxset[i/3*3+j/3].insert(c);      
                }     
            }
        }
        return true;
    }
};
