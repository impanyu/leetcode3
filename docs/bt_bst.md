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

## [173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)
1. Essentially a inorder iterative traversal of BST.
2. For each call of next(), only proceed for one step.
  
```c++
class BSTIterator {
public:
    stack<TreeNode*> nodes;
    BSTIterator(TreeNode* root) {
        while(root!=nullptr){
            nodes.push(root);
            root=root->left;
        }
    }
    
    /** @return the next smallest number */
    int next() {
        TreeNode* current;
        if(hasNext()) {
            current=nodes.top();
            nodes.pop();
            TreeNode* right_child=current->right;
            while(right_child!=nullptr){
                        nodes.push(right_child);
                        right_child=right_child->left;
                    }    
            
            }
        else return -1;
        return current->val;   
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !nodes.empty();
    }
};
```

## [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)
1. Recursive solution. Each ft flatten the subtree and also return the last node in the pre-order.
2. Then for each node, we insert the left flattened subtree between current node and the right subtree. 
3. At last we return the last node of current subtree.  

```c++
class Solution {
public:
    void flatten(TreeNode* root) {
        ft(root);
    }
    TreeNode* ft(TreeNode* root){
         if(root==nullptr) return nullptr;
         TreeNode* left_last=ft(root->left);
         TreeNode* right_last=ft(root->right);
         if(left_last!=nullptr) {
              left_last->right=root->right;
              root->right=root->left;
         }
         root->left=nullptr;
         TreeNode* last_node=right_last==nullptr?left_last:right_last;
         last_node=last_node==nullptr?root:last_node;
         return last_node;
    }
};

```
