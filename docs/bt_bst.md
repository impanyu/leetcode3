## [100.Same Tree](https://leetcode.com/problems/same-tree/)
``` c++
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
          if(p==nullptr && q==nullptr) return true;
          else if(p==nullptr || q==nullptr) return false;
          else if(p->val != q->val) return false;
          else return isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
        
    }
};
```

## [98.Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
1. iterative in order traverse is shortest method
2. each node only pop once.
3. for each node, push all the left children of its right child.

```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {  
      stack<TreeNode*> s;
      long prev=(long)INT_MIN-1;
      while(root!=nullptr){
          s.push(root);
          root=root->left;
      }
      while(!s.empty()){
          TreeNode* current=s.top();
          s.pop();
          if(prev>= current->val) return false;
          prev=current->val;
          TreeNode* right=current->right;
          while(right!=nullptr){
              s.push(right);
              right=right->left;
          }
      }
        return true;
    }
    
};
```

## [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
1. Both recursive and iterative are ok
2. The following method can also be modified to only use one shared stack
```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr) return true;
        stack<TreeNode*> l;
        stack<TreeNode*> r;
        l.push(root->left);
        r.push(root->right);
        while(!l.empty() && !r.empty()){
            TreeNode* cl=l.top();
            TreeNode* cr=r.top();
            l.pop();
            r.pop();
            if(cl==nullptr && cr==nullptr) continue;
            else if(cl==nullptr || cr==nullptr) return false;
            else if(cl->val!=cr->val) return false;
            else {
                l.push(cl->left);
                l.push(cl->right);
                r.push(cr->right);
                r.push(cr->left);
            }
        }
        return true;    
    }
};
```
