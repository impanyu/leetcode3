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

## [527. Word Abbreviation](https://leetcode.com/problems/word-abbreviation/)
1. first abbrev each word
2. find the collision and solve the collision with prefix in a trie
3. each word add and search from index 1, can also start from 0, but pay attention to skip 0, otherwise for group with only 1 word, the output will not include the first letter

```c++
class TrieNode{
 public:
    TrieNode* children[26];
    int count=0;
    TrieNode(){
        for(int i=0;i<26;i++)
            children[i]=0;
    }
};

class indexed_word{
    public:
    string word;
    int index;
    indexed_word(string w, int i){
        word=w;
        index=i;
    }
};

class Solution {
public:
    vector<string> wordsAbbreviation(vector<string>& dict) {
        unordered_map<string,vector<indexed_word>> groups;
        vector<string> ans(dict.size());
        
        int index=0;
        for(string word: dict){
            string ab= abbrev(word,0);
            groups[ab].push_back(indexed_word(word,index));
            index++;
        }
        
        for(auto p: groups){
            TrieNode* trie=new TrieNode();
            vector<indexed_word> group=p.second;
            for(indexed_word iw: group){
                TrieNode* cur=trie;
                for(char letter: iw.word.substr(1)){
                    if(cur->children[letter-'a']==nullptr)
                        cur->children[letter-'a']=new TrieNode();
                    cur->count++;
                    cur=cur->children[letter-'a'];
                }
            }
            
            for(indexed_word iw: group){
                TrieNode* cur=trie;
                int i=1;
                for(char letter: iw.word.substr(1)){
                    if(cur->count==1) break;
                    cur=cur->children[letter-'a'];
                    i++;
                }
                ans[iw.index]=abbrev(iw.word,i-1);
            }
        }
        return ans;
    }
    
    string abbrev(string word, int i){//i is the last letter to be included in the prefix(or to say,the first letter which can differentiate within the group)
        int N=word.size();
        if(N-i<=3) return word;
        return word.substr(0,i+1)+to_string(N-i-2)+string(1,word[N-1]);
    }    
};
```

## [642. Design Search Autocomplete System](https://leetcode.com/problems/design-search-autocomplete-system/)
1. use a trie to store previous sentences
2. lookup sentence by prefix and sort them
3. still have bugs


```c++
class Trie{
public:
    int times=0;
    Trie* branches[27];
    Trie(){
        for(int i=0;i<27;i++) branches[i]=nullptr;
        this->times=times;
    }
};

class sentence{
public:
    string s;
    int times;
    sentence(string s, int t){
        this->s=s;
        this->times=t;
    }
    bool operator < (sentence b){
        return times==b.times?s<b.s:times<b.times;
    }
};


class AutocompleteSystem {
public:
    Trie* root;
    string cur_sent;
    int ctoi(char c){
        return c==' '?26:c-'a';
    }
    char itoc(int i){
        return i<=25?(char)(i+'a'):' ';
    }
    
    AutocompleteSystem(vector<string>& sentences, vector<int>& times) {
        root=new Trie();
        for(int i=0;i<sentences.size();i++)
            insert(root,sentences[i],times[i]);
    }
        
    void insert(Trie* root, string s, int times){
        for(int i=0;i<s.size();i++){
            if(root->branches[ctoi(s[i])]==nullptr) 
                root->branches[ctoi(s[i])]=new Trie();
            root=root->branches[ctoi(s[i])];    
        } 
        root->times+=times;
    }
    
    vector<sentence> lookup(Trie* root, string s){
        vector<sentence> list ;
        
        for(int i=0; i< s.size();i++){
            cout<<ctoi(s[i])<<"\n";
            if(root->branches[ctoi(s[i])]==nullptr)
                return list;
            root=root->branches[ctoi(s[i])];
        }
        traverse(root,s,list);
        return list;
        
    } 
    
    void traverse(Trie* root, string s,vector<sentence> list){
        if(root->times>0) list.push_back(sentence(s,root->times));
        for(int i=0;i<=26;i++)
            if(root->branches[itoc(i)]!=nullptr)
                traverse(root->branches[i],s+itoc(i),list);
    }
    
    
    
    vector<string> input(char c) {
        vector<string> ans;
        if(c=='#'){
            insert(root,cur_sent,1);
            cur_sent="";
        }
        else{
            cur_sent+=c;
            vector<sentence> list=lookup(root,cur_sent);
            sort(begin(list),end(list));
            for(int i=0; i<min(3,(int)list.size());i++)
                ans.push_back(list[i].s);
            
        }
        return ans;
    }
};

/**
 * Your AutocompleteSystem object will be instantiated and called as such:
 * AutocompleteSystem* obj = new AutocompleteSystem(sentences, times);
 * vector<string> param_1 = obj->input(c);
 */
```
