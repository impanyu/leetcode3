## [208. Implement Trie(Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

```c++
class TrieNode {
    // R links to node children
    private:
     TrieNode** links;
     bool End;
    public:
    TrieNode() {
        links = new TrieNode*[26];
        End=false;
        for(int i=0;i<26;i++)
            links[i]=0;
    }
    ~ TrieNode() {
        for(int i=0;i<26;i++){
           if(links[i]) delete links[i];
        }  
    }
    bool containsKey(char ch) {
        return links[ch -'a'] != nullptr;
    }
    TrieNode* get(char ch) {
        return links[ch -'a'];
    }
    void put(char ch, TrieNode * node) {
        links[ch -'a'] = node;
    }
    void setEnd() {
        End = true;
    }
    bool isEnd() {
        return End;
    }
};

class Trie {
    private:
        TrieNode* root;
        // search a prefix or whole key in trie and
        // returns the node where search ends
        TrieNode* searchPrefix(string word) {
        TrieNode* node = root;
        for (int i = 0; i < word.length(); i++) {
           char curLetter = word[i];
           if (node->containsKey(curLetter)) {
               node = node->get(curLetter);
           } else {
               return nullptr;
           }
        }
        return node;
    }
    public:
        Trie() {
            root = new TrieNode();
        }
        ~Trie() {
            delete root;
        }
    
        // Inserts a word into the trie.
       void insert(string word) {
         TrieNode* node = root;
         for (int i = 0; i < word.length(); i++) {
            char currentChar = word[i];
            if (!node->containsKey(currentChar)) {
                node->put(currentChar, new TrieNode());
            }
            node = node->get(currentChar);
        }
        node->setEnd();
      }
        
      // Returns if the word is in the trie.
       bool search(string key) {
             TrieNode* node = searchPrefix(key);
             return node != nullptr && node->isEnd();
        }
    
        // Returns if there is any word in the trie
        // that starts with the given prefix.
        bool startsWith(string prefix) {
            TrieNode* node = searchPrefix(prefix);
            return node != nullptr;
        }
};
```


## [212. Word Search II](https://leetcode.com/problems/word-search-ii/)
1. use a trie to store all the words in input words
2. essentially a problem to traverse on two structures simultaneously (one is the grid, the other is the trie)
3. backtrack both on the grid and the trie.


```c++
class TrieNode {
    // R links to node children
    private:
     TrieNode** links;
     bool End;
    public:
    TrieNode() {
        links = new TrieNode*[26];
        End=false;
        for(int i=0;i<26;i++)
            links[i]=0;
    }
    ~ TrieNode() {
        for(int i=0;i<26;i++){
           if(links[i]) delete links[i];
        }  
    }
    bool containsKey(char ch) {
        return links[ch -'a'] != nullptr;
    }
    TrieNode* get(char ch) {
        return links[ch -'a'];
    }
    void put(char ch, TrieNode * node) {
        links[ch -'a'] = node;
    }
    void setEnd() {
        End = true;
    }
    bool isEnd() {
        return End;
    }
};


class Trie {
    private:    
        // search a prefix or whole key in trie and
       // returns the node where search ends
        TrieNode* searchPrefix(string word) {
            TrieNode* node = root;
            for (int i = 0; i < word.length(); i++) {
               char curLetter = word[i];
               if (node->containsKey(curLetter)) {
                   node = node->get(curLetter);
               } else {
                   return nullptr;
               }
            }
            return node;
    }
    public:
        TrieNode* root;
        Trie() {
            root = new TrieNode();
        }
        ~Trie() {
            delete root;
        } 
        // Inserts a word into the trie.
       void insert(string word) {
         TrieNode* node = root;
         for (int i = 0; i < word.length(); i++) {
            char currentChar = word[i];
            if (!node->containsKey(currentChar)) {
                node->put(currentChar, new TrieNode());
            }
            node = node->get(currentChar);
        }
        node->setEnd();
      }     
        // Returns if the word is in the trie.
      bool search(string key) {
             TrieNode* node = searchPrefix(key);
             return node != nullptr && node->isEnd();
        }   
        // Returns if there is any word in the trie
        // that starts with the given prefix.
        bool startsWith(string prefix) {
            TrieNode* node = searchPrefix(prefix);
            return node != nullptr;
        }
};



class Solution {
public:
    Trie trie;
    TrieNode* root = trie.root;
    TrieNode* cur = root;
    
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        vector<string> ans;
        for(string word: words){
            trie.insert(word);
        }
        for(string word: words){
            if(exist(board,word)) ans.push_back(word); 
        }
       return ans;  
    }
       
    bool exist(vector<vector<char>>& board, string word) {
         for (unsigned int i = 0; i < board.size(); i++)
            for (unsigned int j = 0; j < board[0].size(); j++) {
                //cur=root;
                if (dfs(board, i, j, word))
                    return true;    
            }         
         return false;
    }
    
    bool dfs(vector<vector<char>>& board, int i, int j, string& word) {
        if(!word.size()){
            return true;
        }
        if(i<0 || i>=board.size() || j<0 || j>=board[0].size() || board[i][j] != word[0]){
            return false;
        }        
        char c = board[i][j];
        TrieNode* pre=cur;
        if(!cur->containsKey(c)) return false;
        cur=cur->get(c);
        
        board[i][j] = '*';
        string s = word.substr(1);
        bool ret = dfs(board, i-1, j, s) || dfs(board, i+1, j, s) || dfs(board, i, j-1, s) || dfs(board, i, j+1, s);
        
        board[i][j] = c; 
        cur=pre;
        
        return ret;
  }    
};
```
