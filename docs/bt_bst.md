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

## [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
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


## [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
1. Use ans to store the max value of the length of path through each node
2. No need to return both depth and ans in the same function
3. Count nodes, not edges 

```c++
class Solution {
public:
    int ans=1;
    int diameterOfBinaryTree(TreeNode* root) {
        depth(root);
        return ans-1;
    }
    int depth(TreeNode* root){
        if(root==nullptr) return 0;
        int left=depth(root->left);
        int right=depth(root->right);
        ans=max(ans,left+right+1);
        return max(left,right)+1;     
    }
};
```

## [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)
1. Recursive solution.
2. If find the key, return the root of right children(or left in case no right child), and append left subtree to the left-most leave of right subtree.

```c++
ass Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(root==nullptr) return root;
        if(key==root->val){
            if(root->right==nullptr) return root->left;
            TreeNode* p=root->right;
            while(p->left!=nullptr) p=p->left;
            p->left=root->left;
            return root->right;
        }      
        else if(key<root->val) root->left=deleteNode(root->left,key);
        else root->right=deleteNode(root->right,key);
        return root;
    }
};
```


## [337. House Robber III](https://leetcode.com/problems/house-robber-iii/)
1. Different from I and II, the topology is a binary tree, so we use recursive post order to solve it.
2. Each time the helper function return two values, one for cur_max and one for the sum of sub_layers. 

```c++
class Solution {
public:
    int rob(TreeNode* root) {
        return rb(root).first;
    }
    
    pair<int,int> rb(TreeNode* root){
        int cur_max=0;
        if(root==nullptr) return make_pair(0,0);
        pair<int,int> left=rb(root->left);
        pair<int,int> right=rb(root->right);
        int sub_layer=left.first+right.first;
        cur_max=max(sub_layer,root->val+left.second+right.second);
        return make_pair(cur_max,sub_layer);
    }
};
```

## [774. Minimize Max Distance to Gas Station]()
1. consider the boolean answer as a function of minimal distance
2. doing a binary search on the function to find the approximated min dist.


```c++
class Solution {
public:
    double minmaxGasDist(vector<int>& stations, int K) {
        double lo=0, hi= 1e8;
        while(hi-lo > 1e-6){
            double mi=(lo+hi)/2.0;
            if(possible(mi,stations,K))
                hi=mi;
            else
                lo=mi;
        }
        return lo;
    }
    
    bool possible(double D, vector<int> stations, int K){
        int used=0;
        for(int i=0; i< stations.size()-1;i++)
            used+= (int)((stations[i+1]-stations[i])/D);
        return used<=K;
    }
    
};
```

## [497. Random Point in Non-overlapping Rectangles](https://leetcode.com/problems/random-point-in-non-overlapping-rectangles/)
1. randomly sample a point between 0 to tot
2. find the rect through binary search
3. pick the point within the rect
4. pay attention to the format of binary search, which find the upper_bound of a value



```c++
class Solution {
public:
    vector<vector<int>> rects;
    vector<int> psum;
    int tot=0;
    Solution(vector<vector<int>>& rects) {
        this->rects=rects;
        for(auto x : rects){
            tot+=((x[2]-x[0])+1)*(x[3]-x[1]+1);
            psum.push_back(tot);
        }
    }
    
    vector<int> pick() {
        int targ = rand()%tot;
        int lo=0;
        int hi=rects.size()-1;
        while(lo<hi){
            int mid=(lo+hi)/2;
            if(targ>=psum[mid]) lo=mid+1;
            else hi=mid;
        }
        
        vector<int> x = rects[lo];
        int width= x[2]-x[0]+1;
        int height = x[3]-x[1]+1;
        int base= psum[lo]-width*height;
        vector<int> ans;
        ans.push_back(x[0]+(targ-base)%width);
        ans.push_back(x[1]+(targ-base)/width);
        return ans;     
    }
};
```

[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
1. mimic quick sort, and only deal with one side, rather than both sides.
2. time complexity O(n)
3. another method is to use a heap.

```c++
class Solution {
public:
    vector<int> nums;
    int findKthLargest(vector<int>& nums, int k) {
        this->nums = nums;
       int size = nums.size();
       return quickselect(0, size - 1, size - k);
    }
    
    
    int partition(int left, int right, int pivot_index) {
        int pivot = nums[pivot_index];
        // 1. move pivot to end
        swap(nums[pivot_index], nums[right]);
        int store_index = left;

        // 2. move all smaller elements to the left
        for (int i = left; i <= right; i++) {
          if (this->nums[i] < pivot) {
            swap(nums[store_index], nums[i]);
            store_index++;
          }
    }

        // 3. move pivot to its final place
        swap(nums[store_index], nums[right]);

        return store_index;
  }
    
    int quickselect(int left, int right, int k_smallest) {
    /*
    Returns the k-th smallest element of list within left..right.
    */

    if (left == right) // If the list contains only one element,
      return this->nums[left];  // return that element

    // select a random pivot_index
 
    int pivot_index = left + rand()%(right - left); 
    
    pivot_index = partition(left, right, pivot_index);

    // the pivot is on (N - k)th smallest position
    if (k_smallest == pivot_index)
      return this->nums[k_smallest];
    // go left side
    else if (k_smallest < pivot_index)
      return quickselect(left, pivot_index - 1, k_smallest);
    // go right side
    return quickselect(pivot_index + 1, right, k_smallest);
  }
    
};
```

## [855.Exam Room](https://leetcode.com/problems/exam-room/)
1. for each seat(), traverset all the seated students, find the largest intervals.
2. deal with two ends with care.
3. use a tree map to store the sorted index of seated students.

```c++
class ExamRoom {
public:
    set<int> seated;
    int N;
    ExamRoom(int N) {
       this->N=N; 
    }
    
    int seat() {
       int ans=0;
       int max_dist=0;
       int prev=0;
       if(seated.size()>0){
           for(set<int>::iterator itr=seated.begin();itr!=seated.end();itr++){
               if(itr==seated.begin())
                   max_dist= *itr;
               else{
                   if(max_dist<(*itr-prev)/2){
                       max_dist=(*itr-prev)/2;
                       ans=prev+(*itr-prev)/2;
                   }
               }
               prev=*itr;
           }
          if(N-1-(*seated.rbegin())>max_dist)
              ans=N-1;
           
       }
       seated.insert(ans);
       return ans;
    }
    
    void leave(int p) {
        seated.erase(p);
    }
};
```
