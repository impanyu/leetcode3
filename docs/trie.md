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
