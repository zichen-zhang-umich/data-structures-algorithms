### Alien Dictioanry

*核心知识点：拓扑排序，深度优先搜索*

Given a sorted dictionary of an alien language having N words and k starting alphabets of standard dictionary. Find the order of characters in the alien language.

**Note**: Many orders may be possible for a particular test case, thus you may return any valid order and output will be 1 if the order of string returned by the function is correct else 0 denoting incorrect string returned.

**Example 1**
```
Input: 
N = 5, K = 4
dict = {"baa","abcd","abca","cab","cad"}

Output:
1

Explanation:
Here order of characters is 
'b', 'd', 'a', 'c' Note that words are sorted 
and in the given language "baa" comes before 
"abcd", therefore 'b' is before 'a' in output.
Similarly we can find other orders.
```
**Example 2**
```
Input: 
N = 3, K = 3
dict = {"caa","aaa","aab"}

Output:
1

Explanation:
Here order of characters is
'c', 'a', 'b' Note that words are sorted
and in the given language "caa" comes before
"aaa", therefore 'c' is before 'a' in output.
Similarly we can find other orders.
```

**Your Task:**
You don't need to read or print anything. Your task is to complete the function `findOrder()` which takes  the string array `dict[]`, its size `N` and the integer `K` as input parameter and returns a string denoting the order of characters in the alien language.


**Expected Time Complexity:** $O({N}\times{|S|} + K)$ , where $|S|$ denotes maximum length.

**Expected Space Complexity:** $O(K)$

**My Solution:**
```C++
class Solution {
public:
    string findOrder(string dict[], int N, int K) {
        //create a graph
        vector<vector<char>> adj_list(K);
        for (int i = 0; i < N-1; i++) {
            int range = min(dict[i].size(), dict[i+1].size());
            for (int j = 0; j < range; j++) {
                if (dict[i][j] != dict[i+1][j]) {
                    adj_list[dict[i][j]-'a'].push_back(dict[i+1][j]);
                    break;
                }
            }
        }
        string order = "";
        vector<bool> visited(K, false);
        for (int vertex = 0; vertex < K; vertex++) {
            if (!visited[vertex]) {
                dfs_helper(adj_list, visited, order, vertex);
            }
        }
        reverse(order.begin(), order.end());
        return order;
    }
    
    void dfs_helper(const vector<vector<char>> &adj_list, vector<bool> &visited, string &order, int vertex) {
        visited[vertex] = true;
        for (auto neighbor : adj_list[vertex]) {
            if (!visited[neighbor-'a']) {
                dfs_helper(adj_list, visited, order, neighbor-'a');
            }
        }
        order.push_back(vertex+'a');
    }
};
```