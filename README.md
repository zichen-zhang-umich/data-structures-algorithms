<div align="center">
<h1>Common DSA Implementation Cheatsheet</h1>
</div>

This repository serves as a collection of my personal notes on the topics of **data structures & algroithms, LeetCode problems and behavioral questions**. Some of the codes are written by me and some of them are from other users with direct links/references to their original works. The following is the ***cheatsheet*** that records some of the most common operations on important data structures and algorithms. The majority of the codes were written in C++.

# Sorting Algorithms
## Quicksort
This divide-and-conquer algorithm is **not stable** and **not adaptive**. The overall efficiency depends on which `pivot` is selected.

| Best Time | Average Time | Worst Time | Space |
| ----------- | ----------- |----------- | ----------- |
| $\Theta(nlog(n))$ | $\Theta(nlog(n))$ | $\Theta(n^2)$ | $\Theta(log(n))$ |

```C++
class Solution {
public:
    void quicksort(vector<int>& vec, int left, int right) {
        // can also be written as if (right - left <= 1) or if (right - left < 2)
        // if there's only one element or no element, then that means we don't need to sort them
        if (left + 1 >= right) return;
        // call partition() to find a pivot idx
        int pivot = partition(vec, left, right - 1);
        // try to use tail recursion on the side with more elements to reduce space complexity
        if (pivot - left < right - pivot) { // right side is longer, so use tail recursion
            quicksort(vec, left, pivot);
            quicksort(vec, pivot + 1, right);
        }
        else { // left side is longer, so use tail recursion
            quicksort(vec, pivot + 1, right);
            quicksort(vec, left, pivot);
        }
    }

    int partition(vector<int>& vec, int left, int right) {
        // increment left pointer until it points to a number greater than the pivot
        // increment right pointer until it points to a number less than the pivot, then swap the two numbers
        // continue this process until left and right 
        int pivot = right--;
        while (true) {
            while (vec[left] < vec[pivot]) {
                ++left;
            }
            while (left < right && vec[right] >= vec[pivot]) {
                --right;
            }
            if (left >= right) break;
            swap(vec[left], vec[right]);
        }
        swap(vec[left], vec[pivot]);
        return left;
    }
};
```

# Tree

## Depth-First Search (DFS)
### Inorder Traversal
This topic relates to *LC 94. Binary Tree Inorder Traversal.*
#### Recursive Approach
```C++
class Solution {
public:
    vector<int> res;

    vector<int> inorderTraversal(TreeNode* root) {
        inorder_helper(root);
        return res;
    }

    void inorder_helper(TreeNode* root) {
        if (!root) return;
        inorder(root->left);
        res.push_back(root->val); // process the curr node
        inorder(root->left);
    }
}
```
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
            res.push_back(root->val); // process the curr node
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

## Level-Order Traversal (Breath-First Search, BFS)
```C++
class Solution {
public:
    vector<int> res;

    void level_order(Node* root) {
        if (!root) return;
        deque<Node*> bfs; // or we can use std::queue
        bfs.push_back(root);
        while (!bfs.empty()) {
            Node* curr = bfs.front();
            bfs.pop();
            res.push_back(curr->val); // process the curr node
            if (curr->left) {
                bfs.push_back(curr->left);
            }
            if (curr->right) {
                bfs.push_back(curr->right);
            }
        }
        return res;
    }
}
```

# Graph

## Depth-First Search (DFS)

## Breath-First Search (BFS)

