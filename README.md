<div align="center">
<h1>Common DSA Implementation Cheatsheet</h1>
</div>

This repository serves as a collection of my personal notes on the topics of **data structures & algroithms, LeetCode problems and behavioral questions**. Some of the codes are written by me and some of them are from other users with direct links/references to their original works. The following is the ***cheatsheet*** that records some of the most common operations on important data structures and algorithms. The majority of the codes were written in `C++`.

# Sorting Algorithms

# Tree Algorithms

## Depth-First Search (DFS)
### Inorder Traversal
This topic is tested in *LC 94. Binary Tree Inorder Traversal.*
#### Recursive Approach
#### Iterative Approach
```C++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (!root) return {};
        vector<TreeNode*> stk;
        vector<int> res;
        while (root || !stk.empty()) {
            while (root) {
                stk.push_back(root);
                root = root->left;
            }
            root = stk.back();
            stk.pop_back();
            res.push_back(root->val);
            root = root->right;
        }
        return res;
    }
};
```
#### Morris Traversal Approach
This approach is not very commonly tested in coding interviews.
```C++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (!root) return {};
        vector<int> res;
        TreeNode* x = root;
        while (x) {
            if (!x->left) {
                res.push_back(x->val);
                x = x->right;
            }
            //x->left not nullptr
            else {
                TreeNode* pred = findRightMostNode(x->left, x);
                if (!pred->right) {
                    pred->right = x;
                    x = x->left;
                }
                //pred->right not nullptr
                else {
                    res.push_back(x->val);
                    pred->right = nullptr;
                    x = x->right;
                }
            }
        }
        return res;
    }
    TreeNode* findRightMostNode(TreeNode* root, TreeNode* curNode) {
        while (root->right && root->right != curNode) {
            root = root->right;
        }
        return root;
    }
}
```

## Breath-First Search (BFS)

# Graph Algorithms

## Depth-First Search (DFS)

## Breath-First Search (BFS)

