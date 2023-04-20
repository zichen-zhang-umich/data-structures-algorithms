最后执行的输入：`["z","z"]`

需要考虑的特殊情况：
```
["z","x"]
["aabc"]
["z","x","z"]
["abc","ab"]
["ab","ab"]
["z","ab","ab"]
["z","ab","abc"]
["ab","abc"]
["z","ab","ab","cd","cdef"]
["w","z","x","z"]
["z","abc","ab"]
["zy","zx"]
```

### 我的解法
```C++
class Solution {
public:
    string alienOrder(vector<string>& words) {
        //需要优化
        bool is_valid = true;
        unordered_map<char, vector<char>> adj_list;
        for (int i = 0; i < words.size()-1; i++) {
            int range = min(words[i].size(), words[i+1].size());
            if (words[i] == words[i+1]) continue;
            for (int j = 0; j < range; j++) {
                if (words[i][j] != words[i+1][j]) {
                    adj_list[words[i][j]].push_back(words[i+1][j]);
                    break;
                }
                if (j == range-1 && words[i].size() > words[i+1].size()) {
                    is_valid = false;
                }
            }
        }
        string order = "";
        unordered_map<char, int> visited;
        for (auto it = adj_list.begin(); it != adj_list.end(); ++it) {
            visited[it->first] = 0; //not visited
        }
        for (auto it = adj_list.begin(); it != adj_list.end(); ++it) {
            if (!visited[it->first]) {
                char c = it->first;
                dfs_helper(adj_list, is_valid, visited, order, c);
            }
        }
        if (!is_valid) return "";
        reverse(order.begin(), order.end());
        return order;
    }
    void dfs_helper(unordered_map<char, vector<char>> &adj_list, bool &is_valid,
            unordered_map<char, int> &visited, string &order, char vertex) {
        visited[vertex] = 1; //visiting
        for (auto neighbor : adj_list[vertex]) {
            if (visited[neighbor] == 0) {
                dfs_helper(adj_list, is_valid, visited, order, neighbor);
            }
            else if (visited[neighbor] == 1) {
                //detected cycle
                is_valid = false;
            }
        }
        visited[vertex] = 2; //visited
        order.push_back(vertex);
    }
};

### 参考解法
```C++
class Solution {
public:
    string alienOrder(vector<string>& words) {
        vector<int> in(26, 0); // 每个节点的入度
        vector<vector<int> > g(26); // 建图
        vector<bool> st(26, false); // 字母是否出现
        int cnt = 0; // 字母出现的数量
        for (string& word: words) {
            for (char& ch: word) {
                if (st[ch - 'a'] == false) cnt ++ ;
                st[ch - 'a'] = true;
            }
        }
        int n = words.size();
        for (int i = 0; i < n - 1; i ++ ) {
            string s = words[i], p = words[i + 1];
            int lens = s.size(), lenp = p.size(), minn = min(lens, lenp);
            bool ok = false; // 是否有不同的位置
            for (int j = 0; j < minn; j ++ ) {
                if (s[j] == p[j]) continue;
                ok = true;
                in[p[j] - 'a'] ++ ;
                g[s[j] - 'a'].push_back(p[j] - 'a');
                break;
            }
            if (!ok && lens > lenp) return ""; // 后一个是前一个的前缀
        }

        queue<int> q;
        for (int i = 0; i < 26; i ++ ) {
            if (in[i] == 0 && st[i] == true)
                q.push(i);
        }

        string res = "";
        while (!q.empty()) {
            int p = q.front(); q.pop();
            res.push_back(p + 'a');
            for (int u : g[p]) 
                if ( -- in[u] == 0) 
                    q.push(u);
        }
        return res.size() == cnt ? res : ""; // 出现的单词都要在答案中
    }
};
```
```C++
class Solution {
public:
    string alienOrder(vector<string>& words) {
        unordered_map<char, unordered_set<char>> graph;
        unordered_map<char, int> indegress;

        // 初始化图
        for(const string& word : words) {
            for(const char c : word) {
                graph.insert({c, {}});
                indegress.insert({c, 0});
            }
        }

        // 建图
        for(int i = 1; i < words.size(); ++i) {
            string first = words[i - 1], second = words[i];
            if(first.find(second) == 0 && first.size() > second.size())  return "";
            
            for(int index = 0; index < first.size() && index < second.size(); ++index) {
                char ch1 = first[index], ch2 = second[index];
                if(ch1 != ch2) {
                    if(graph[ch1].count(ch2) == 0) {
                        graph[ch1].insert(ch2);
                        ++indegress[ch2];
                    }
                    break;
                }
            }
        }

        // 找到入度为 0 的节点
        queue<char> que;
        for(auto it = indegress.begin(); it != indegress.end(); ++it) {
            // cout << "indegress : " << it->first << " -> " << it->second << endl;
            if(it->second == 0)
                que.emplace(it->first);
        }

        string res;
        // 拓扑排序
        while(!que.empty()) {
            char cur = que.front();
            que.pop();
            res += cur;

            for(auto neighbor : graph[cur]) {
                if(--indegress[neighbor] == 0)
                    que.emplace(neighbor);
            }
        }

        // cout << res << endl;
        return res.size() == graph.size() ? res : "";
    }
};
```