# 动态规划

## 固定根的树形 DP 题目

### [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

简单

给你一棵二叉树的根节点，返回该树的 **直径** 。

二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。这条路径可能经过也可能不经过根节点 `root` 。

两节点之间路径的 **长度** 由它们之间边数表示。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
输入：root = [1,2,3,4,5]
输出：3
解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。
```

C++版本

```c++
// 方法一：深度优先搜索
class Solution {
    int ans;
    int depth(TreeNode* rt){
        if (rt == NULL) {
            return 0; // 访问到空节点了，返回0
        }
        int L = depth(rt->left); // 左儿子为根的子树的深度
        int R = depth(rt->right); // 右儿子为根的子树的深度
        ans = max(ans, L + R + 1); // 计算d_node即L+R+1 并更新ans
        return max(L, R) + 1; // 返回该节点为根的子树的深度
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        ans = 1;
        depth(root);
        return ans - 1;
    }
};
```

Java版本

```java
// 方法一：深度优先搜索
class Solution {
    int ans;
    public int diameterOfBinaryTree(TreeNode root) {
        ans = 1;
        depth(root);
        return ans - 1;
    }
    public int depth(TreeNode node) {
        if (node == null) {
            return 0; // 访问到空节点了，返回0
        }
        int L = depth(node.left); // 左儿子为根的子树的深度
        int R = depth(node.right); // 右儿子为根的子树的深度
        ans = Math.max(ans, L+R+1); // 计算d_node即L+R+1 并更新ans
        return Math.max(L, R) + 1; // 返回该节点为根的子树的深度
    }
}
```



### [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

困难

二叉树中的 **路径** 被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中 **至多出现一次** 。该路径 **至少包含一个** 节点，且不一定经过根节点。

**路径和** 是路径中各节点值的总和。

给你一个二叉树的根节点 `root` ，返回其 **最大路径和** 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

C++版本

```c++
// 方法一：递归
class Solution {
private:
    int maxSum = INT_MIN;

public:
    int maxGain(TreeNode* node) {
        if (node == nullptr) {
            return 0;
        }
        
        // 递归计算左右子节点的最大贡献值
        // 只有在最大贡献值大于 0 时，才会选取对应子节点
        int leftGain = max(maxGain(node->left), 0);
        int rightGain = max(maxGain(node->right), 0);

        // 节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值
        int priceNewpath = node->val + leftGain + rightGain;

        // 更新答案
        maxSum = max(maxSum, priceNewpath);

        // 返回节点的最大贡献值
        return node->val + max(leftGain, rightGain);
    }

    int maxPathSum(TreeNode* root) {
        maxGain(root);
        return maxSum;
    }
};
```

Java版本

```java
// 方法一：递归
class Solution {
    int maxSum = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        maxGain(root);
        return maxSum;
    }

    public int maxGain(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        // 递归计算左右子节点的最大贡献值
        // 只有在最大贡献值大于 0 时，才会选取对应子节点
        int leftGain = Math.max(maxGain(node.left), 0);
        int rightGain = Math.max(maxGain(node.right), 0);

        // 节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值
        int priceNewpath = node.val + leftGain + rightGain;

        // 更新答案
        maxSum = Math.max(maxSum, priceNewpath);

        // 返回节点的最大贡献值
        return node.val + Math.max(leftGain, rightGain);
    }
}
```



### [2246. 相邻字符不同的最长路径](https://leetcode.cn/problems/longest-path-with-different-adjacent-characters/)

困难

给你一棵 **树**（即一个连通、无向、无环图），根节点是节点 `0` ，这棵树由编号从 `0` 到 `n - 1` 的 `n` 个节点组成。用下标从 **0** 开始、长度为 `n` 的数组 `parent` 来表示这棵树，其中 `parent[i]` 是节点 `i` 的父节点，由于节点 `0` 是根节点，所以 `parent[0] == -1` 。

另给你一个字符串 `s` ，长度也是 `n` ，其中 `s[i]` 表示分配给节点 `i` 的字符。

请你找出路径上任意一对相邻节点都没有分配到相同字符的 **最长路径** ，并返回该路径的长度。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2022/03/25/testingdrawio.png)

```
输入：parent = [-1,0,0,1,1,2], s = "abacbe"
输出：3
解释：任意一对相邻节点字符都不同的最长路径是：0 -> 1 -> 3 。该路径的长度是 3 ，所以返回 3 。
可以证明不存在满足上述条件且比 3 更长的路径。 
```

C++版本

```c++
// 树形Dp
class Solution {
public:
    int longestPath(vector<int> &parent, string &s) {
        int n = parent.size();
        vector<vector<int>> g(n);
        for (int i = 1; i < n; ++i)
            g[parent[i]].push_back(i);

        int ans = 0;
        function<int(int)> dfs = [&](int x) -> int {
            int maxLen = 0;
            for (int y : g[x]) {
                int len = dfs(y) + 1;
                if (s[y] != s[x]) {
                    ans = max(ans, maxLen + len);
                    maxLen = max(maxLen, len);
                }
            }
            return maxLen;
        };
        dfs(0);
        return ans + 1;
    }
};
```

Java版本

```java
// 树形Dp
class Solution {
    List<Integer>[] g;
    String s;
    int ans;

    public int longestPath(int[] parent, String s) {
        this.s = s;
        var n = parent.length;
        g = new ArrayList[n];
        Arrays.setAll(g, e -> new ArrayList<>());
        for (var i = 1; i < n; i++) g[parent[i]].add(i);

        dfs(0);
        return ans + 1;
    }

    int dfs(int x) {
        var maxLen = 0;
        for (var y : g[x]) {
            var len = dfs(y) + 1;
            if (s.charAt(y) != s.charAt(x)) {
                ans = Math.max(ans, maxLen + len);
                maxLen = Math.max(maxLen, len);
            }
        }
        return maxLen;
    }
}

// 
class Solution {
    public int longestPath(int[] parents, String s) {
        char[] cs = s.toCharArray();
        int n = cs.length, result = 1; 
        
        // topo sort, count in-degree first (in=degree here means child -> parent)
        int[] degree = new int[n];
        for (int i = 1; i < n; i++)
            degree[parents[i]]++; // skip root
        
        // stack: for topo sort, start with leaf nodes(in-degree = 0)
        int[] stack = new int[n];
        int top = -1; 
        for (int i = 1; i < n; i++) // skip root
            if (degree[i] == 0)  
                 stack[++top] = i;
        
        int[] path = new int[n]; // max length of path for each node
        Arrays.fill(path, 1); // every node is a path of length 1
        
        while (top >= 0) { // topo sort till the end
            int v = stack[top--]; // c: child index
            int u = parents[v]; // p: parent index
            if (--degree[u] == 0 && u != 0)
                 stack[++top] = u; // do not process root
            
            if (cs[u] == cs[v]) continue; // nothing to update
            
            // we must update res first, otherwise we may double count the same path
            result = Math.max(result, path[u] + path[v]);
            path[u] = Math.max(path[u], path[v] + 1);
        }

        return result;
    }
}
```



### [687. 最长同值路径](https://leetcode.cn/problems/longest-univalue-path/)

中等

给定一个二叉树的 `root` ，返回 *最长的路径的长度* ，这个路径中的 *每个节点具有相同值* 。 这条路径可以经过也可以不经过根节点。

**两个节点之间的路径长度** 由它们之间的边数表示。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

```
输入：root = [5,4,5,1,1,5]
输出：2
```

C++版本

```c++
// 方法一：深度优先搜索
class Solution {
private:
    int res;

public:
    int longestUnivaluePath(TreeNode* root) {
        res = 0;
        dfs(root);
        return res;
    }

    int dfs(TreeNode *root) {
        if (root == nullptr) {
            return 0;
        }
        int left = dfs(root->left), right = dfs(root->right);
        int left1 = 0, right1 = 0;
        if (root->left && root->left->val == root->val) {
            left1 = left + 1;
        }
        if (root->right && root->right->val == root->val) {
            right1 = right + 1;
        }
        res = max(res, left1 + right1);
        return max(left1, right1);
    }
};
```

Java版本

```java
// 方法一：深度优先搜索
class Solution {
    int res;

    public int longestUnivaluePath(TreeNode root) {
        res = 0;
        dfs(root);
        return res;
    }

    public int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = dfs(root.left), right = dfs(root.right);
        int left1 = 0, right1 = 0;
        if (root.left != null && root.left.val == root.val) {
            left1 = left + 1;
        }
        if (root.right != null && root.right.val == root.val) {
            right1 = right + 1;
        }
        res = Math.max(res, left1 + right1);
        return Math.max(left1, right1);
    }
}
```



### [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)

中等

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。

除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。

给定二叉树的 `root` 。返回 ***在不触动警报的情况下** ，小偷能够盗取的最高金额* 。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

```
输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    unordered_map <TreeNode*, int> f, g;

    void dfs(TreeNode* node) {
        if (!node) {
            return;
        }
        dfs(node->left);
        dfs(node->right);
        f[node] = node->val + g[node->left] + g[node->right];
        g[node] = max(f[node->left], g[node->left]) + max(f[node->right], g[node->right]);
    }

    int rob(TreeNode* root) {
        dfs(root);
        return max(f[root], g[root]);
    }
};

// 优化
struct SubtreeStatus {
    int selected;
    int notSelected;
};

class Solution {
public:
    SubtreeStatus dfs(TreeNode* node) {
        if (!node) {
            return {0, 0};
        }
        auto l = dfs(node->left);
        auto r = dfs(node->right);
        int selected = node->val + l.notSelected + r.notSelected;
        int notSelected = max(l.selected, l.notSelected) + max(r.selected, r.notSelected);
        return {selected, notSelected};
    }

    int rob(TreeNode* root) {
        auto rootStatus = dfs(root);
        return max(rootStatus.selected, rootStatus.notSelected);
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    Map<TreeNode, Integer> f = new HashMap<TreeNode, Integer>();
    Map<TreeNode, Integer> g = new HashMap<TreeNode, Integer>();

    public int rob(TreeNode root) {
        dfs(root);
        return Math.max(f.getOrDefault(root, 0), g.getOrDefault(root, 0));
    }

    public void dfs(TreeNode node) {
        if (node == null) {
            return;
        }
        dfs(node.left);
        dfs(node.right);
        f.put(node, node.val + g.getOrDefault(node.left, 0) + g.getOrDefault(node.right, 0));
        g.put(node, Math.max(f.getOrDefault(node.left, 0), g.getOrDefault(node.left, 0)) + Math.max(f.getOrDefault(node.right, 0), g.getOrDefault(node.right, 0)));
    }
}

// 优化
class Solution {
    public int rob(TreeNode root) {
        int[] rootStatus = dfs(root);
        return Math.max(rootStatus[0], rootStatus[1]);
    }

    public int[] dfs(TreeNode node) {
        if (node == null) {
            return new int[]{0, 0};
        }
        int[] l = dfs(node.left);
        int[] r = dfs(node.right);
        int selected = node.val + l[1] + r[1];
        int notSelected = Math.max(l[0], l[1]) + Math.max(r[0], r[1]);
        return new int[]{selected, notSelected};
    }
}
```



### [1617. 统计子树中城市之间最大距离](https://leetcode.cn/problems/count-subtrees-with-max-distance-between-cities/)

困难

给你 `n` 个城市，编号为从 `1` 到 `n` 。同时给你一个大小为 `n-1` 的数组 `edges` ，其中 `edges[i] = [ui, vi]` 表示城市 `ui` 和 `vi` 之间有一条双向边。题目保证任意城市之间只有唯一的一条路径。换句话说，所有城市形成了一棵 **树** 。

一棵 **子树** 是城市的一个子集，且子集中任意城市之间可以通过子集中的其他城市和边到达。两个子树被认为不一样的条件是至少有一个城市在其中一棵子树中存在，但在另一棵子树中不存在。

对于 `d` 从 `1` 到 `n-1` ，请你找到城市间 **最大距离** 恰好为 `d` 的所有子树数目。

请你返回一个大小为 `n-1` 的数组，其中第 `d` 个元素（**下标从 1 开始**）是城市间 **最大距离** 恰好等于 `d` 的子树数目。

**请注意**，两个城市间距离定义为它们之间需要经过的边的数目。

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/11/p1.png)**

```
输入：n = 4, edges = [[1,2],[2,3],[2,4]]
输出：[3,4,0]
解释：
子树 {1,2}, {2,3} 和 {2,4} 最大距离都是 1 。
子树 {1,2,3}, {1,2,4}, {2,3,4} 和 {1,2,3,4} 最大距离都为 2 。
不存在城市间最大距离为 3 的子树。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:      
    vector<int> countSubgraphsForEachDiameter(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adj(n);        
        for (auto &edge : edges) {
            int x = edge[0] - 1;
            int y = edge[1] - 1;
            adj[x].emplace_back(y);
            adj[y].emplace_back(x);
        }
        function<int(int, int&, int&)> dfs = [&](int root, int& mask, int& diameter)->int {
            int first = 0, second = 0;
            mask &= ~(1 << root);
            for (int vertex : adj[root]) {
                if (mask & (1 << vertex)) {
                    mask &= ~(1 << vertex);
                    int distance = 1 + dfs(vertex, mask, diameter);
                    if (distance > first) {
                        second = first;
                        first = distance;
                    } else if (distance > second) {
                        second = distance;
                    }
                }
            }
            diameter = max(diameter, first + second);
            return first;
        };

        vector<int> ans(n - 1);
        for (int i = 1; i < (1 << n); i++) {
            int root = 32 - __builtin_clz(i) - 1;
            int mask = i;
            int diameter = 0;
            dfs(root, mask, diameter);
            if (mask == 0 && diameter > 0) {
                ans[diameter - 1]++;
            }
        }
        return ans;
    }
};

// 方法二：深度优先搜索
class Solution {
public:      
    vector<int> countSubgraphsForEachDiameter(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adj(n);        
        for (auto &edge : edges) {
            int x = edge[0] - 1;
            int y = edge[1] - 1;
            adj[x].emplace_back(y);
            adj[y].emplace_back(x);
        }

        function<int(int, int, int)> dfs = [&](int parent, int u, int mask)->int {
            int depth = 0;
            for (int v : adj[u]) {
                if (v != parent && mask & (1 << v)) {
                    depth = max(depth, 1 + dfs(u, v, mask));
                }
            }
            return depth;
        };

        vector<int> ans(n - 1);
        for (int i = 1; i < (1 << n); i++) {
            int x = 32 - __builtin_clz(i) - 1;
            int mask = i;
            int y = -1;
            queue<int> qu;
            qu.emplace(x);
            mask &= ~(1 << x);
            while (!qu.empty()) {
                y = qu.front();
                qu.pop();
                for (int vertex : adj[y]) {
                    if (mask & (1 << vertex)) {
                        mask &= ~(1 << vertex);
                        qu.emplace(vertex);
                    }
                }
            }
            if (mask == 0) {
                int diameter = dfs(-1, y, i);
                if (diameter > 0) {
                    ans[diameter - 1]++;
                }
            }
        }
        return ans;
    }
};

// 方法三：枚举任意两点直径
class Solution {
public:      
    vector<int> countSubgraphsForEachDiameter(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adj(n);        
        vector<vector<int>> dist(n, vector<int>(n, INT_MAX));
        for (auto &edge : edges) {
            int x = edge[0] - 1;
            int y = edge[1] - 1;
            adj[x].emplace_back(y);
            adj[y].emplace_back(x);
            dist[x][y] = dist[y][x] = 1;
        }
        for (int i = 0; i < n; i++) {
            dist[i][i] = 0;
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    if (dist[j][i] != INT_MAX && dist[i][k] != INT_MAX) {
                        dist[j][k] = min(dist[j][k], dist[j][i] + dist[i][k]);
                    }
                }
            }
        }
        function<int(int, int, int, int)> dfs = [&](int u, int parent, int x, int y) -> int {
            if (dist[u][x] > dist[x][y] || dist[u][y] > dist[x][y]) {
                return 1;
            }
            if ((dist[u][y] == dist[x][y] && u < x) || (dist[u][x] == dist[x][y] && u < y)) {
                return 1;
            }
            int ret = 1;
            for (int v : adj[u]) {
                if (v != parent) {
                    ret *= dfs(v, u, x, y);
                }
            }
            if (dist[u][x] + dist[u][y] > dist[x][y]) {
                ret++;
            }
            return ret;
        };
        vector<int> ans(n - 1);
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                ans[dist[i][j] - 1] += dfs(i, -1, i, j);
            }
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    int mask;
    int diameter;

    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {
        List<Integer>[] adj = new List[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<Integer>();
        }
        for (int[] edge : edges) {
            int x = edge[0] - 1;
            int y = edge[1] - 1;
            adj[x].add(y);
            adj[y].add(x);
        }

        int[] ans = new int[n - 1];
        for (int i = 1; i < (1 << n); i++) {
            int root = 32 - Integer.numberOfLeadingZeros(i) - 1;
            mask = i;
            diameter = 0;
            dfs(root, adj);
            if (mask == 0 && diameter > 0) {
                ans[diameter - 1]++;
            }
        }
        return ans;
    }

    public int dfs(int root, List<Integer>[] adj) {
        int first = 0, second = 0;
        mask &= ~(1 << root);
        for (int vertex : adj[root]) {
            if ((mask & (1 << vertex)) != 0) {
                mask &= ~(1 << vertex);
                int distance = 1 + dfs(vertex, adj);
                if (distance > first) {
                    second = first;
                    first = distance;
                } else if (distance > second) {
                    second = distance;
                }
            }
        }
        diameter = Math.max(diameter, first + second);
        return first;
    }
}

// 方法二：深度优先搜索
class Solution {
    int mask;
    int diameter;

    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {
        List<Integer>[] adj = new List[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<Integer>();
        }
        for (int[] edge : edges) {
            int x = edge[0] - 1;
            int y = edge[1] - 1;
            adj[x].add(y);
            adj[y].add(x);
        }

        int[] ans = new int[n - 1];
        for (int i = 1; i < (1 << n); i++) {
            int x = 32 - Integer.numberOfLeadingZeros(i) - 1;
            int mask = i;
            int y = -1;
            Queue<Integer> queue = new ArrayDeque<Integer>();
            queue.offer(x);
            mask &= ~(1 << x);
            while (!queue.isEmpty()) {
                y = queue.poll();
                for (int vertex : adj[y]) {
                    if ((mask & (1 << vertex)) != 0) {
                        mask &= ~(1 << vertex);
                        queue.offer(vertex);
                    }
                }
            }
            if (mask == 0) {
                int diameter = dfs(adj, -1, y, i);
                if (diameter > 0) {
                    ans[diameter - 1]++;
                }
            }
        }
        return ans;
    }

    public int dfs(List<Integer>[] adj, int parent, int u, int mask) {
        int depth = 0;
        for (int v : adj[u]) {
            if (v != parent && (mask & (1 << v)) != 0) {
                depth = Math.max(depth, 1 + dfs(adj, u, v, mask));
            }
        }
        return depth;
    }
}

// 方法三：枚举任意两点直径
class Solution {
    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {
        List<Integer>[] adj = new List[n];
        int[][] dist = new int[n][n];
        for (int i = 0; i < n; i++) {
            Arrays.fill(dist[i], Integer.MAX_VALUE);
            dist[i][i] = 0;
        }
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<Integer>();
        }
        for (int[] edge : edges) {
            int x = edge[0] - 1;
            int y = edge[1] - 1;
            adj[x].add(y);
            adj[y].add(x);
            dist[x][y] = dist[y][x] = 1;
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    if (dist[j][i] != Integer.MAX_VALUE && dist[i][k] != Integer.MAX_VALUE) {
                        dist[j][k] = Math.min(dist[j][k], dist[j][i] + dist[i][k]);
                    }
                }
            }
        }
        int[] ans = new int[n - 1];
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                ans[dist[i][j] - 1] += dfs(adj, dist, i, -1, i, j);
            }
        }
        return ans;
    }

    public int dfs(List<Integer>[] adj, int[][] dist, int u, int parent, int x, int y) {
        if (dist[u][x] > dist[x][y] || dist[u][y] > dist[x][y]) {
            return 1;
        }
        if ((dist[u][y] == dist[x][y] && u < x) || (dist[u][x] == dist[x][y] && u < y)) {
            return 1;
        }
        int ret = 1;
        for (int v : adj[u]) {
            if (v != parent) {
                ret *= dfs(adj, dist, v, u, x, y);
            }
        }
        if (dist[u][x] + dist[u][y] > dist[x][y]) {
            ret++;
        }
        return ret;
    }
}
```



### [2538. 最大价值和与最小价值和的差值](https://leetcode.cn/problems/difference-between-maximum-and-minimum-price-sum/)

困难

给你一个 `n` 个节点的无向无根图，节点编号为 `0` 到 `n - 1` 。给你一个整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间有一条边。

每个节点都有一个价值。给你一个整数数组 `price` ，其中 `price[i]` 是第 `i` 个节点的价值。

一条路径的 **价值和** 是这条路径上所有节点的价值之和。

你可以选择树中任意一个节点作为根节点 `root` 。选择 `root` 为根的 **开销** 是以 `root` 为起点的所有路径中，**价值和** 最大的一条路径与最小的一条路径的差值。

请你返回所有节点作为根节点的选择中，**最大** 的 **开销** 为多少。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2022/12/01/example14.png)

```
输入：n = 6, edges = [[0,1],[1,2],[1,3],[3,4],[3,5]], price = [9,8,7,6,10,5]
输出：24
解释：上图展示了以节点 2 为根的树。左图（红色的节点）是最大价值和路径，右图（蓝色的节点）是最小价值和路径。
- 第一条路径节点为 [2,1,3,4]：价值为 [7,8,6,10] ，价值和为 31 。
- 第二条路径节点为 [2] ，价值为 [7] 。
最大路径和与最小路径和的差值为 24 。24 是所有方案中的最大开销。
```

C++版本

```c++
class Solution {
public:
    long long maxOutput(int n, vector<vector<int>> &edges, vector<int> &price) {
        vector<vector<int>> g(n);
        for (auto &e : edges) {
            int x = e[0], y = e[1];
            g[x].push_back(y);
            g[y].push_back(x); // 建树
        }

        long ans = 0;
        function<pair<long, long>(int, int)> dfs = [&](int x, int fa) -> pair<long, long> {
            long p = price[x], max_s1 = p, max_s2 = 0;
            for (int y : g[x])
                if (y != fa) {
                    auto[s1, s2] = dfs(y, x);
                    // 前面最大带叶子的路径和 + 当前不带叶子的路径和
                    // 前面最大不带叶子的路径和 + 当前带叶子的路径和
                    ans = max(ans, max(max_s1 + s2, max_s2 + s1));
                    max_s1 = max(max_s1, s1 + p);
                    max_s2 = max(max_s2, s2 + p); // 这里加上 p 是因为 x 必然不是叶子
                }
            return {max_s1, max_s2};
        };
        dfs(0, -1);
        return ans;
    }
};
```

Java版本

```java
class Solution {
    private List<Integer>[] g;
    private int[] price;
    private long ans;

    public long maxOutput(int n, int[][] edges, int[] price) {
        this.price = price;
        g = new ArrayList[n];
        Arrays.setAll(g, e -> new ArrayList<>());
        for (var e : edges) {
            int x = e[0], y = e[1];
            g[x].add(y);
            g[y].add(x); // 建树
        }
        dfs(0, -1);
        return ans;
    }

    private long[] dfs(int x, int fa) {
        long p = price[x], maxS1 = p, maxS2 = 0;
        for (var y : g[x])
            if (y != fa) {
                var res = dfs(y, x);
                long s1 = res[0], s2 = res[1];
                // 前面最大带叶子的路径和 + 当前不带叶子的路径和
                // 前面最大不带叶子的路径和 + 当前带叶子的路径和
                ans = Math.max(ans, Math.max(maxS1 + s2, maxS2 + s1));
                maxS1 = Math.max(maxS1, s1 + p);
                maxS2 = Math.max(maxS2, s2 + p); // 这里加上 p 是因为 x 必然不是叶子
            }
        return new long[]{maxS1, maxS2};
    }
}
```



### [1569. 将子数组重新排序得到同一个二叉搜索树的方案数](https://leetcode.cn/problems/number-of-ways-to-reorder-array-to-get-same-bst/)

困难

给你一个数组 `nums` 表示 `1` 到 `n` 的一个排列。我们按照元素在 `nums` 中的顺序依次插入一个初始为空的二叉搜索树（BST）。请你统计将 `nums` 重新排序后，统计满足如下条件的方案数：重排后得到的二叉搜索树与 `nums` 原本数字顺序得到的二叉搜索树相同。

比方说，给你 `nums = [2,1,3]`，我们得到一棵 2 为根，1 为左孩子，3 为右孩子的树。数组 `[2,3,1]` 也能得到相同的 BST，但 `[3,2,1]` 会得到一棵不同的 BST 。

请你返回重排 `nums` 后，与原数组 `nums` 得到相同二叉搜索树的方案数。

由于答案可能会很大，请将结果对 `10^9 + 7` 取余数。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/30/bb.png)

```
输入：nums = [2,1,3]
输出：1
解释：我们将 nums 重排， [2,3,1] 能得到相同的 BST 。没有其他得到相同 BST 的方案了。
```

C++版本

```c++
// 方法一：动态规划 + 组合计数
struct TNode {
    TNode* left;
    TNode* right;
    int value;
    int size;
    int ans;
    
    TNode(int val): left(nullptr), right(nullptr), value(val), size(1), ans(0) {}
};

class Solution {
private:
    static constexpr int mod = 1000000007;
    vector<vector<int>> c;

public:
    void insert(TNode* root, int val) {
        TNode* cur = root;
        while (true) {
            ++cur->size;
            if (val < cur->value) {
                if (!cur->left) {
                    cur->left = new TNode(val);
                    return;
                }
                cur = cur->left;
            }
            else {
                if (!cur->right) {
                    cur->right = new TNode(val);
                    return;
                }
                cur = cur->right;
            }
        }
    }

    void dfs(TNode* node) {
        if (!node) {
            return;
        }
        dfs(node->left);
        dfs(node->right);
        int lsize = node->left ? node->left->size : 0;
        int rsize = node->right ? node->right->size : 0;
        int lans = node->left ? node->left->ans : 1;
        int rans = node->right ? node->right->ans : 1;
        node->ans = (long long)c[lsize + rsize][lsize] % mod * lans % mod * rans % mod;
    }

    int numOfWays(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return 0;
        }

        c.assign(n, vector<int>(n));
        c[0][0] = 1;
        for (int i = 1; i < n; ++i) {
            c[i][0] = 1;
            for (int j = 1; j < n; ++j) {
                c[i][j] = (c[i - 1][j - 1] + c[i - 1][j]) % mod;
            }
        }

        TNode* root = new TNode(nums[0]);
        for (int i = 1; i < n; ++i) {
            int val = nums[i];
            insert(root, val);
        }

        dfs(root);
        return (root->ans - 1 + mod) % mod;
    }
};

// 方法二：并查集 + 乘法逆元优化
struct TNode {
    TNode* left;
    TNode* right;
    int size;
    int ans;
    
    TNode(): left(nullptr), right(nullptr), size(1), ans(0) {}
};

class UnionFind {
public:
    vector<int> parent, size, root;
    int n;
    
public:
    UnionFind(int _n): n(_n), parent(_n), size(_n, 1), root(_n) {
        iota(parent.begin(), parent.end(), 0);
        iota(root.begin(), root.end(), 0);
    }
    
    int findset(int x) {
        return parent[x] == x ? x : parent[x] = findset(parent[x]);
    }

    int getroot(int x) {
        return root[findset(x)];
    }
    
    void unite(int x, int y) {
        root[y] = root[x];
        if (size[x] < size[y]) {
            swap(x, y);
        }
        parent[y] = x;
        size[x] += size[y];
    }
    
    bool findAndUnite(int x, int y) {
        int x0 = findset(x);
        int y0 = findset(y);
        if (x0 != y0) {
            unite(x0, y0);
            return true;
        }
        return false;
    }
};

class Solution {
private:
    static constexpr int mod = 1000000007;
    vector<int> fac, inv, facInv;

public:
    int numOfWays(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return 0;
        }

        fac.resize(n);
        inv.resize(n);
        facInv.resize(n);
        fac[0] = inv[0] = facInv[0] = 1;
        fac[1] = inv[1] = facInv[1] = 1;
        for (int i = 2; i < n; ++i) {
            fac[i] = (long long)fac[i - 1] * i % mod;
            inv[i] = (long long)(mod - mod / i) * inv[mod % i] % mod;
            facInv[i] = (long long)facInv[i - 1] * inv[i] % mod;
        }

        unordered_map<int, TNode*> found;
        UnionFind uf(n);
        for (int i = n - 1; i >= 0; --i) {
            int val = nums[i] - 1;
            TNode* node = new TNode();
            if (val > 0 && found.count(val - 1)) {
                int lchild = uf.getroot(val - 1);
                node->left = found[lchild];
                node->size += node->left->size;
                uf.findAndUnite(val, lchild);
            }
            if (val < n - 1 && found.count(val + 1)) {
                int rchild = uf.getroot(val + 1);
                node->right = found[rchild];
                node->size += node->right->size;
                uf.findAndUnite(val, rchild);
            }
            
            int lsize = node->left ? node->left->size : 0;
            int rsize = node->right ? node->right->size : 0;
            int lans = node->left ? node->left->ans : 1;
            int rans = node->right ? node->right->ans : 1;
            node->ans = (long long)fac[lsize + rsize] * facInv[lsize] % mod * facInv[rsize] % mod * lans % mod * rans % mod;
            found[val] = node;
        }

        return (found[nums[0] - 1]->ans - 1 + mod) % mod;
    }
};
```

Java版本

```java
// 方法一：动态规划 + 组合计数
class Solution {
    static final int MOD = 1000000007;
    long[][] c;

    public int numOfWays(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return 0;
        }

        c = new long[n][n];
        c[0][0] = 1;
        for (int i = 1; i < n; ++i) {
            c[i][0] = 1;
            for (int j = 1; j < n; ++j) {
                c[i][j] = (c[i - 1][j - 1] + c[i - 1][j]) % MOD;
            }
        }

        TreeNode root = new TreeNode(nums[0]);
        for (int i = 1; i < n; ++i) {
            int val = nums[i];
            insert(root, val);
        }

        dfs(root);
        return (root.ans - 1 + MOD) % MOD;
    }

    public void insert(TreeNode root, int value) {
        TreeNode cur = root;
        while (true) {
            ++cur.size;
            if (value < cur.value) {
                if (cur.left == null) {
                    cur.left = new TreeNode(value);
                    return;
                }
                cur = cur.left;
            } else {
                if (cur.right == null) {
                    cur.right = new TreeNode(value);
                    return;
                }
                cur = cur.right;
            }
        }
    }

    public void dfs(TreeNode node) {
        if (node == null) {
            return;
        }
        dfs(node.left);
        dfs(node.right);
        int lsize = node.left != null ? node.left.size : 0;
        int rsize = node.right != null ? node.right.size : 0;
        int lans = node.left != null ? node.left.ans : 1;
        int rans = node.right != null ? node.right.ans : 1;
        node.ans = (int) (c[lsize + rsize][lsize] % MOD * lans % MOD * rans % MOD);
    }
}

class TreeNode {
    TreeNode left;
    TreeNode right;
    int value;
    int size;
    int ans;

    TreeNode(int value) {
        this.value = value;
        this.size = 1;
        this.ans = 0;
    }
}

// 方法二：并查集 + 乘法逆元优化
class Solution {
    static final int MOD = 1000000007;
    long[] fac;
    long[] inv;
    long[] facInv;

    public int numOfWays(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return 0;
        }

        fac = new long[n];
        inv = new long[n];
        facInv = new long[n];
        fac[0] = inv[0] = facInv[0] = 1;
        fac[1] = inv[1] = facInv[1] = 1;
        for (int i = 2; i < n; ++i) {
            fac[i] = fac[i - 1] * i % MOD;
            inv[i] = (MOD - MOD / i) * inv[MOD % i] % MOD;
            facInv[i] = facInv[i - 1] * inv[i] % MOD;
        }

        Map<Integer, TreeNode> found = new HashMap<Integer, TreeNode>();
        UnionFind uf = new UnionFind(n);
        for (int i = n - 1; i >= 0; --i) {
            int val = nums[i] - 1;
            TreeNode node = new TreeNode();
            if (val > 0 && found.containsKey(val - 1)) {
                int lchild = uf.getroot(val - 1);
                node.left = found.get(lchild);
                node.size += node.left.size;
                uf.findAndUnite(val, lchild);
            }
            if (val < n - 1 && found.containsKey(val + 1)) {
                int rchild = uf.getroot(val + 1);
                node.right = found.get(rchild);
                node.size += node.right.size;
                uf.findAndUnite(val, rchild);
            }
            
            int lsize = node.left != null ? node.left.size : 0;
            int rsize = node.right != null ? node.right.size : 0;
            int lans = node.left != null ? node.left.ans : 1;
            int rans = node.right != null ? node.right.ans : 1;
            node.ans = (int) (fac[lsize + rsize] * facInv[lsize] % MOD * facInv[rsize] % MOD * lans % MOD * rans % MOD);
            found.put(val, node);
        }

        return (found.get(nums[0] - 1).ans - 1 + MOD) % MOD;
    }
}

class TreeNode {
    TreeNode left;
    TreeNode right;
    int size;
    int ans;

    TreeNode() {
        size = 1;
        ans = 0;
    }
}

class UnionFind {
    public int[] parent;
    public int[] size;
    public int[] root;
    public int n;

    public UnionFind(int n) {
        this.n = n;
        parent = new int[n];
        size = new int[n];
        root = new int[n];
        Arrays.fill(size, 1);
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            root[i] = i;
        }
    }

    public int findset(int x) {
        return parent[x] == x ? x : (parent[x] = findset(parent[x]));
    }

    public int getroot(int x) {
        return root[findset(x)];
    }
    
    public void unite(int x, int y) {
        root[y] = root[x];
        if (size[x] < size[y]) {
            int temp = x;
            x = y;
            y = temp;
        }
        parent[y] = x;
        size[x] += size[y];
    }
    
    public boolean findAndUnite(int x, int y) {
        int x0 = findset(x);
        int y0 = findset(y);
        if (x0 != y0) {
            unite(x0, y0);
            return true;
        }
        return false;
    }
}

// 方法三：
class Solution {
    private final static int MOD = 1000000007;

    public int numOfWays(int[] nums) {
        List<Integer> numList = new ArrayList<>();
        for (int num : nums) {
            numList.add(num);
        }

        return (int) getCombs(numList, getTriangle(numList.size() + 1)) - 1;
    }

    private long getCombs(List<Integer> nums, long[][] combs) {
        if (nums.size() <= 2)
            return 1;
        int root = nums.get(0);
        List<Integer> left = new ArrayList<>();
        List<Integer> right = new ArrayList<>();
        for (int i = 1; i < nums.size(); i++) {
            int num = nums.get(i);
            if (num < root) {
                left.add(num);
            } else if (num > root) {
                right.add(num);
            }
        }
        return (combs[left.size() + right.size()][left.size()] * (getCombs(left, combs) % MOD) % MOD)
                * getCombs(right, combs) % MOD;
    }

    private long[][] getTriangle(int len) {
        long[][] combs = new long[len][len];
        for (int i = 0; i < len; i++) {
            combs[i][i] = combs[i][0] = 1;
        }

        for (int i = 2; i < len; i++) {
            for (int j = 1; j < i; j++) {
                combs[i][j] = (combs[i - 1][j] + combs[i - 1][j - 1]) % MOD;
            }
        }
        return combs;
    }
}
```



### [1372. 二叉树中的最长交错路径](https://leetcode.cn/problems/longest-zigzag-path-in-a-binary-tree/)

中等

给你一棵以 `root` 为根的二叉树，二叉树中的交错路径定义如下：

- 选择二叉树中 **任意** 节点和一个方向（左或者右）。
- 如果前进方向为右，那么移动到当前节点的的右子节点，否则移动到它的左子节点。
- 改变前进方向：左变右或者右变左。
- 重复第二步和第三步，直到你在树中无法继续移动。

交错路径的长度定义为：**访问过的节点数目 - 1**（单个节点的路径长度为 0 ）。

请你返回给定树中最长 **交错路径** 的长度。

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/07/sample_1_1702.png)**

```
输入：root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1,null,1]
输出：3
解释：蓝色节点为树中最长交错路径（右 -> 左 -> 右）。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    unordered_map <TreeNode*, int> f, g;
    
    queue <pair <TreeNode *, TreeNode *>> q;
    
    void dp(TreeNode *o) {
        f[o] = g[o] = 0;
        q.push({o, nullptr});
        while (!q.empty()) {
            auto y = q.front(); q.pop();
            auto x = y.second, u = y.first;
            f[u] = g[u] = 0;
            if (x) {
                if (x->left == u) f[u] = g[x] + 1;
                if (x->right == u) g[u] = f[x] + 1;
            }
            if (u->left) q.push({u->left, u});
            if (u->right) q.push({u->right, u});
        }
    }
    
    int longestZigZag(TreeNode* root) {
        dp(root);
        int maxAns = 0;
        for (const auto &u: f) maxAns = max(maxAns, max(u.second, g[u.first]));
        return maxAns;
    }
};

// 方法二：深度优先搜索
class Solution {
public:
    int maxAns;

    /* 0 => left, 1 => right */
    void dfs(TreeNode* o, bool dir, int len) {
        maxAns = max(maxAns, len);
        if (!dir) {
            if (o->left) dfs(o->left, 1, len + 1);
            if (o->right) dfs(o->right, 0, 1);
        } else {
            if (o->right) dfs(o->right, 0, len + 1);
            if (o->left) dfs(o->left, 1, 1);
        }
    } 

    int longestZigZag(TreeNode* root) {
        if (!root) return 0;
        maxAns = 0;
        dfs(root, 0, 0);
        dfs(root, 1, 0);
        return maxAns;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    Map<TreeNode, Integer> f = new HashMap<TreeNode, Integer>();
    Map<TreeNode, Integer> g = new HashMap<TreeNode, Integer>();
    Queue<TreeNode[]> q = new LinkedList<TreeNode[]>();

    public int longestZigZag(TreeNode root) {
        dp(root);
        int maxAns = 0;
        for (TreeNode u : f.keySet()) {
            maxAns = Math.max(maxAns, Math.max(f.get(u), g.get(u)));
        }
        return maxAns;
    }
    
    public void dp(TreeNode o) {
        f.put(o, 0);
        g.put(o, 0);
        q.offer(new TreeNode[]{o, null});
        while (!q.isEmpty()) {
            TreeNode[] y = q.poll();
            TreeNode u = y[0], x = y[1];
            f.put(u, 0);
            g.put(u, 0);
            if (x != null) {
                if (x.left == u) {
                    f.put(u, g.get(x) + 1);
                }
                if (x.right == u) {
                    g.put(u, f.get(x) + 1);
                }
            }
            if (u.left != null) {
                q.offer(new TreeNode[]{u.left, u});
            }
            if (u.right != null) {
                q.offer(new TreeNode[]{u.right, u});
            }
        }
    }
}

// 方法二：深度优先搜索
class Solution {
    int maxAns;

    public int longestZigZag(TreeNode root) {
        if (root == null) {
            return 0;
        }
        maxAns = 0;
        dfs(root, false, 0);
        dfs(root, true, 0);
        return maxAns;
    }

    public void dfs(TreeNode o, boolean dir, int len) {
        maxAns = Math.max(maxAns, len);
        if (!dir) {
            if (o.left != null) {
                dfs(o.left, true, len + 1);
            }
            if (o.right != null) {
                dfs(o.right, false, 1);
            }
        } else {
            if (o.right != null) {
                dfs(o.right, false, len + 1);
            }
            if (o.left != null) {
                dfs(o.left, true, 1);
            }
        }
    }
}

// 方法三：
class Solution {
    int maxPath = 0;

    public int longestZigZag(TreeNode root) {
        findPath(root);
        return maxPath;
    }

    public int[] findPath(TreeNode root) {
        if (root == null) {
            return new int[] { -1, -1 };
        }
        int[] leftChildPath = findPath(root.left);
        int[] rightChildPath = findPath(root.right);
        int leftSum = leftChildPath[1] + 1;
        int rightSum = rightChildPath[0] + 1;
        maxPath = Math.max(Math.max(maxPath, leftSum), rightSum);
        return new int[] { leftSum, rightSum };
    }
}
```



### [1373. 二叉搜索子树的最大键值和](https://leetcode.cn/problems/maximum-sum-bst-in-binary-tree/)

困难

给你一棵以 `root` 为根的 **二叉树** ，请你返回 **任意** 二叉搜索子树的最大键值和。

二叉搜索树的定义如下：

- 任意节点的左子树中的键值都 **小于** 此节点的键值。
- 任意节点的右子树中的键值都 **大于** 此节点的键值。
- 任意节点的左子树和右子树都是二叉搜索树。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/07/sample_1_1709.png)

```
输入：root = [1,4,3,2,4,2,5,null,null,null,null,null,null,4,6]
输出：20
解释：键值为 3 的子树是和最大的二叉搜索树。
```

C++版本

```c++
// 方法一：递归
class Solution {
public:
    static constexpr int inf = 0x3f3f3f3f;
    int res;
    struct SubTree {
        bool isBST;
        int minValue;
        int maxValue;
        int sumValue;
        SubTree(bool isBST, int minValue, int maxValue, int sumValue) : isBST(isBST), minValue(minValue), maxValue(maxValue), sumValue(sumValue) {}
    };

    SubTree dfs(TreeNode* root) {
        if (root == nullptr) {
            return SubTree(true, inf, -inf, 0);
        }
        auto left = dfs(root->left);
        auto right = dfs(root->right);

        if (left.isBST && right.isBST &&
                root->val > left.maxValue && 
                root->val < right.minValue) {
            int sum = root->val + left.sumValue + right.sumValue;
            res = max(res, sum);
            return SubTree(true, min(left.minValue, root->val), 
                           max(root->val, right.maxValue), sum);
        } else {
            return SubTree(false, 0, 0, 0);
        }
    }

    int maxSumBST(TreeNode* root) {
        res = 0;
        dfs(root);
        return res;
    }
};
```

Java版本

```java
// 方法一：递归
class Solution {
    static final int INF = 0x3f3f3f3f;
    int res;

    class SubTree {
        boolean isBST;
        int minValue;
        int maxValue;
        int sumValue;

        SubTree(boolean isBST, int minValue, int maxValue, int sumValue) {
            this.isBST = isBST;
            this.minValue = minValue;
            this.maxValue = maxValue;
            this.sumValue = sumValue;
        }
    }

    public int maxSumBST(TreeNode root) {
        res = 0;
        dfs(root);
        return res;
    }

    public SubTree dfs(TreeNode root) {
        if (root == null) {
            return new SubTree(true, INF, -INF, 0);
        }
        SubTree left = dfs(root.left);
        SubTree right = dfs(root.right);

        if (left.isBST && right.isBST && root.val > left.maxValue && root.val < right.minValue) {
            int sum = root.val + left.sumValue + right.sumValue;
            res = Math.max(res, sum);
            return new SubTree(true, Math.min(left.minValue, root.val), Math.max(root.val, right.maxValue), sum);
        } else {
            return new SubTree(false, 0, 0, 0);
        }
    }
}
```



### [968. 监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/)

困难

给定一个二叉树，我们在树的节点上安装摄像头。

节点上的每个摄影头都可以监视**其父对象、自身及其直接子对象。**

计算监控树的所有节点所需的最小摄像头数量。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/bst_cameras_01.png)

```
输入：[0,0,null,0,0]
输出：1
解释：如图所示，一台摄像头足以监控所有节点。
```

C++版本

```c++
// 方法一：动态规划
struct Status {
    int a, b, c;
};

class Solution {
public:
    Status dfs(TreeNode* root) {
        if (!root) {
            return {INT_MAX / 2, 0, 0};
        }
        auto [la, lb, lc] = dfs(root->left);
        auto [ra, rb, rc] = dfs(root->right);
        int a = lc + rc + 1;
        int b = min(a, min(la + rb, ra + lb));
        int c = min(a, lb + rb);
        return {a, b, c};
    }

    int minCameraCover(TreeNode* root) {
        auto [a, b, c] = dfs(root);
        return b;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int minCameraCover(TreeNode root) {
        int[] array = dfs(root);
        return array[1];
    }

    public int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[]{Integer.MAX_VALUE / 2, 0, 0};
        }
        int[] leftArray = dfs(root.left);
        int[] rightArray = dfs(root.right);
        int[] array = new int[3];
        array[0] = leftArray[2] + rightArray[2] + 1;
        array[1] = Math.min(array[0], Math.min(leftArray[0] + rightArray[1], rightArray[0] + leftArray[1]));
        array[2] = Math.min(array[0], leftArray[1] + rightArray[1]);
        return array;
    }
}
```



### [1519. 子树中标签相同的节点数](https://leetcode.cn/problems/number-of-nodes-in-the-sub-tree-with-the-same-label/)

中等

给你一棵树（即，一个连通的无环无向图），这棵树由编号从 `0` 到 `n - 1` 的 n 个节点组成，且恰好有 `n - 1` 条 `edges` 。树的根节点为节点 `0` ，树上的每一个节点都有一个标签，也就是字符串 `labels` 中的一个小写字符（编号为 `i` 的 节点的标签就是 `labels[i]` ）

边数组 `edges` 以 `edges[i] = [ai, bi]` 的形式给出，该格式表示节点 `ai` 和 `bi` 之间存在一条边。

返回一个大小为 *`n`* 的数组，其中 `ans[i]` 表示第 `i` 个节点的子树中与节点 `i` 标签相同的节点数。

树 `T` 中的子树是由 `T` 中的某个节点及其所有后代节点组成的树。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/q3e1.jpg)

```
输入：n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], labels = "abaedcd"
输出：[2,1,1,1,1,1,1]
解释：节点 0 的标签为 'a' ，以 'a' 为根节点的子树中，节点 2 的标签也是 'a' ，因此答案为 2 。注意树中的每个节点都是这棵子树的一部分。
节点 1 的标签为 'b' ，节点 1 的子树包含节点 1、4 和 5，但是节点 4、5 的标签与节点 1 不同，故而答案为 1（即，该节点本身）。
```

C++版本

```c++
// 方法一：深度优先搜索
class Solution {
public:
    vector<vector<int>> g;
    vector<vector<int>> f; 
    
    void dfs(int o, int pre, const string &labels) {
        f[o][labels[o] - 'a'] = 1;
        for (const auto &nex: g[o]) {
            if (nex == pre) {
                continue;
            } 
            dfs(nex, o, labels);
            for (int i = 0; i < 26; ++i) {
                f[o][i] += f[nex][i];
            }
        }
    }
    
    vector<int> countSubTrees(int n, vector<vector<int>>& edges, string labels) {
        g.resize(n);
        for (const auto &edge: edges) {
            g[edge[0]].push_back(edge[1]);
            g[edge[1]].push_back(edge[0]);
        }
        f.assign(n, vector<int>(26));
        dfs(0, -1, labels);
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            ans.push_back(f[i][labels[i] - 'a']);
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：深度优先搜索
class Solution {
    public int[] countSubTrees(int n, int[][] edges, String labels) {
        Map<Integer, List<Integer>> edgesMap = new HashMap<>();
        for (int[] edge : edges) {
            int node0 = edge[0], node1 = edge[1];
            edgesMap.computeIfAbsent(node0, k -> new ArrayList<>()).add(node1);
            edgesMap.computeIfAbsent(node1, k -> new ArrayList<>()).add(node0);
            /*
            int node0 = edge[0], node1 = edge[1];
            List<Integer> list0 = edgesMap.getOrDefault(node0, new ArrayList<Integer>());
            List<Integer> list1 = edgesMap.getOrDefault(node1, new ArrayList<Integer>());
            list0.add(node1);
            list1.add(node0);
            edgesMap.put(node0, list0);
            edgesMap.put(node1, list1);
            */
        }
        
        int[] counts = new int[n];
        boolean[] visited = new boolean[n];
        depthFirstSearch(0, counts, visited, edgesMap, labels);
        return counts;
    }

    public int[] depthFirstSearch(int node, int[] counts, boolean[] visited, Map<Integer, List<Integer>> edgesMap, String labels) {
        visited[node] = true;
        int[] curCounts = new int[26];
        curCounts[labels.charAt(node) - 'a']++;
        
        List<Integer> nodesList = edgesMap.get(node);
        if (nodesList != null) {
            for (int nextNode : nodesList) {
                if (!visited[nextNode]) {
                    int[] childCounts = depthFirstSearch(nextNode, counts, visited, edgesMap, labels);
                    for (int i = 0; i < 26; i++) {
                        curCounts[i] += childCounts[i];
                    }
                }
            }
        }
        
        counts[node] = curCounts[labels.charAt(node) - 'a'];
        return curCounts;
    }
}
```



## 不定根的树形 DP 题目

### [310. 最小高度树](https://leetcode.cn/problems/minimum-height-trees/)

中等

树是一个无向图，其中任何两个顶点只通过一条路径连接。 换句话说，任何一个没有简单环路的连通图都是一棵树。

给你一棵包含 `n` 个节点的树，标记为 `0` 到 `n - 1` 。给定数字 `n` 和一个有 `n - 1` 条无向边的 `edges` 列表（每一个边都是一对标签），其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间存在一条无向边。

可选择树中任何一个节点作为根。当选择节点 `x` 作为根节点时，设结果树的高度为 `h` 。在所有可能的树中，具有最小高度的树（即，`min(h)`）被称为 **最小高度树** 。

请你找到所有的 **最小高度树** 并按 **任意顺序** 返回它们的根节点标签列表。

树的 **高度** 是指根节点和叶子节点之间最长向下路径上边的数量。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/01/e1.jpg)

```
输入：n = 4, edges = [[1,0],[1,2],[1,3]]
输出：[1]
解释：如图所示，当根是标签为 1 的节点时，树的高度是 1 ，这是唯一的最小高度树。
```

C++版本

```c++
// 方法一：广度优先搜索
class Solution {
public:
    int findLongestNode(int u, vector<int> & parent, vector<vector<int>>& adj) {
        int n = adj.size();
        queue<int> qu;
        vector<bool> visit(n);
        qu.emplace(u);
        visit[u] = true;
        int node = -1;
  
        while (!qu.empty()) {
            int curr = qu.front();
            qu.pop();
            node = curr;
            for (auto & v : adj[curr]) {
                if (!visit[v]) {
                    visit[v] = true;
                    parent[v] = curr;
                    qu.emplace(v);
                }
            }
        }
        return node;
    }

    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if (n == 1) {
            return {0};
        }
        vector<vector<int>> adj(n);
        for (auto & edge : edges) {
            adj[edge[0]].emplace_back(edge[1]);
            adj[edge[1]].emplace_back(edge[0]);
        }
        
        vector<int> parent(n, -1);
        /* 找到与节点 0 最远的节点 x */
        int x = findLongestNode(0, parent, adj);
        /* 找到与节点 x 最远的节点 y */
        int y = findLongestNode(x, parent, adj);
        /* 求出节点 x 到节点 y 的路径 */
        vector<int> path;
        parent[x] = -1;
        while (y != -1) {
            path.emplace_back(y);
            y = parent[y];
        }
        int m = path.size();
        if (m % 2 == 0) {
            return {path[m / 2 - 1], path[m / 2]};
        } else {
            return {path[m / 2]};
        }
    }
};

// 方法二：深度优先搜索
class Solution {
public:
    void dfs(int u, vector<int> & dist, vector<int> & parent, const vector<vector<int>> & adj) {
        for (auto & v : adj[u]) {
            if (dist[v] < 0) {
                dist[v] = dist[u] + 1;
                parent[v] = u;
                dfs(v, dist, parent, adj); 
            }
        }
    }

    int findLongestNode(int u, vector<int> & parent, const vector<vector<int>> & adj) {
        int n = adj.size();
        vector<int> dist(n, -1);
        dist[u] = 0;
        dfs(u, dist, parent, adj);
        int maxdist = 0;
        int node = -1;
        for (int i = 0; i < n; i++) {
            if (dist[i] > maxdist) {
                maxdist = dist[i];
                node = i;
            }
        }
        return node;
    }

    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if (n == 1) {
            return {0};
        }
        vector<vector<int>> adj(n);
        for (auto & edge : edges) {
            adj[edge[0]].emplace_back(edge[1]);
            adj[edge[1]].emplace_back(edge[0]);
        }
        vector<int> parent(n, -1);
        /* 找到距离节点 0 最远的节点  x */
        int x = findLongestNode(0, parent, adj);
        /* 找到距离节点 x 最远的节点  y */
        int y = findLongestNode(x, parent, adj);
        /* 找到节点 x 到节点 y 的路径 */
        vector<int> path;
        parent[x] = -1;
        while (y != -1) {
            path.emplace_back(y);
            y = parent[y];
        }
        int m = path.size();
        if (m % 2 == 0) {
            return {path[m / 2 - 1], path[m / 2]};
        } else {
            return {path[m / 2]};
        }
    }
};

// 方法三：拓扑排序
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if (n == 1) {
            return {0};
        }
        vector<int> degree(n);
        vector<vector<int>> adj(n);
        for (auto & edge : edges){
            adj[edge[0]].emplace_back(edge[1]);
            adj[edge[1]].emplace_back(edge[0]);
            degree[edge[0]]++;
            degree[edge[1]]++;
        }
        queue<int> qu;
        vector<int> ans;
        for (int i = 0; i < n; i++) {
            if (degree[i] == 1) {
                qu.emplace(i);
            }
        }
        int remainNodes = n;
        while (remainNodes > 2) {
            int sz = qu.size();
            remainNodes -= sz;
            for (int i = 0; i < sz; i++) {
                int curr = qu.front();
                qu.pop();
                for (auto & v : adj[curr]) {
                    if (--degree[v] == 1) {
                        qu.emplace(v);
                    }
                }
            }
        }
        while (!qu.empty()) {
            ans.emplace_back(qu.front());
            qu.pop();
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：广度优先搜索
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer> ans = new ArrayList<Integer>();
        if (n == 1) {
            ans.add(0);
            return ans;
        }
        List<Integer>[] adj = new List[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<Integer>();
        }
        for (int[] edge : edges) {
            adj[edge[0]].add(edge[1]);
            adj[edge[1]].add(edge[0]);
        }

        int[] parent = new int[n];
        Arrays.fill(parent, -1);
        /* 找到与节点 0 最远的节点 x */
        int x = findLongestNode(0, parent, adj);
        /* 找到与节点 x 最远的节点 y */
        int y = findLongestNode(x, parent, adj);
        /* 求出节点 x 到节点 y 的路径 */
        List<Integer> path = new ArrayList<Integer>();
        parent[x] = -1;
        while (y != -1) {
            path.add(y);
            y = parent[y];
        }
        int m = path.size();
        if (m % 2 == 0) {
            ans.add(path.get(m / 2 - 1));
        }
        ans.add(path.get(m / 2));
        return ans;
    }

    public int findLongestNode(int u, int[] parent, List<Integer>[] adj) {
        int n = adj.length;
        Queue<Integer> queue = new ArrayDeque<Integer>();
        boolean[] visit = new boolean[n];
        queue.offer(u);
        visit[u] = true;
        int node = -1;
  
        while (!queue.isEmpty()) {
            int curr = queue.poll();
            node = curr;
            for (int v : adj[curr]) {
                if (!visit[v]) {
                    visit[v] = true;
                    parent[v] = curr;
                    queue.offer(v);
                }
            }
        }
        return node;
    }
}

// 方法二：深度优先搜索
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer> ans = new ArrayList<Integer>();
        if (n == 1) {
            ans.add(0);
            return ans;
        }
        List<Integer>[] adj = new List[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<Integer>();
        }
        for (int[] edge : edges) {
            adj[edge[0]].add(edge[1]);
            adj[edge[1]].add(edge[0]);
        }

        int[] parent = new int[n];
        Arrays.fill(parent, -1);
        /* 找到与节点 0 最远的节点 x */
        int x = findLongestNode(0, parent, adj);
        /* 找到与节点 x 最远的节点 y */
        int y = findLongestNode(x, parent, adj);
        /* 求出节点 x 到节点 y 的路径 */
        List<Integer> path = new ArrayList<Integer>();
        parent[x] = -1;
        while (y != -1) {
            path.add(y);
            y = parent[y];
        }
        int m = path.size();
        if (m % 2 == 0) {
            ans.add(path.get(m / 2 - 1));
        }
        ans.add(path.get(m / 2));
        return ans;
    }

    public int findLongestNode(int u, int[] parent, List<Integer>[] adj) {
        int n = adj.length;
        int[] dist = new int[n];
        Arrays.fill(dist, -1);
        dist[u] = 0;
        dfs(u, dist, parent, adj);
        int maxdist = 0;
        int node = -1;
        for (int i = 0; i < n; i++) {
            if (dist[i] > maxdist) {
                maxdist = dist[i];
                node = i;
            }
        }
        return node;
    }

    public void dfs(int u, int[] dist, int[] parent, List<Integer>[] adj) {
        for (int v : adj[u]) {
            if (dist[v] < 0) {
                dist[v] = dist[u] + 1;
                parent[v] = u;
                dfs(v, dist, parent, adj); 
            }
        }
    }
}

// 方法三：拓扑排序
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer> ans = new ArrayList<Integer>();
        if (n == 1) {
            ans.add(0);
            return ans;
        }
        int[] degree = new int[n];
        List<Integer>[] adj = new List[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<Integer>();
        }
        for (int[] edge : edges) {
            adj[edge[0]].add(edge[1]);
            adj[edge[1]].add(edge[0]);
            degree[edge[0]]++;
            degree[edge[1]]++;
        }
        Queue<Integer> queue = new ArrayDeque<Integer>();
        for (int i = 0; i < n; i++) {
            if (degree[i] == 1) {
                queue.offer(i);
            }
        }
        int remainNodes = n;
        while (remainNodes > 2) {
            int sz = queue.size();
            remainNodes -= sz;
            for (int i = 0; i < sz; i++) {
                int curr = queue.poll();
                for (int v : adj[curr]) {
                    degree[v]--;
                    if (degree[v] == 1) {
                        queue.offer(v);
                    }
                }
            }
        }
        while (!queue.isEmpty()) {
            ans.add(queue.poll());
        }
        return ans;
    }
}
```



### [834. 树中距离之和](https://leetcode.cn/problems/sum-of-distances-in-tree/)

困难

给定一个无向、连通的树。树中有 `n` 个标记为 `0...n-1` 的节点以及 `n-1` 条边 。

给定整数 `n` 和数组 `edges` ， `edges[i] = [ai, bi]`表示树中的节点 `ai` 和 `bi` 之间有一条边。

返回长度为 `n` 的数组 `answer` ，其中 `answer[i]` 是树中第 `i` 个节点与所有其他节点之间的距离之和。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist1.jpg)

```
输入: n = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
输出: [8,12,6,10,10,10]
解释: 树如图所示。
我们可以计算出 dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5) 
也就是 1 + 1 + 2 + 2 + 2 = 8。 因此，answer[0] = 8，以此类推。
```

C++版本

```c++

```

Java版本

```java

```



### [2581. 统计可能的树根数目](https://leetcode.cn/problems/count-number-of-possible-root-nodes/)

困难

Alice 有一棵 `n` 个节点的树，节点编号为 `0` 到 `n - 1` 。树用一个长度为 `n - 1` 的二维整数数组 `edges` 表示，其中 `edges[i] = [ai, bi]` ，表示树中节点 `ai` 和 `bi` 之间有一条边。

Alice 想要 Bob 找到这棵树的根。她允许 Bob 对这棵树进行若干次 **猜测** 。每一次猜测，Bob 做如下事情：

- 选择两个 **不相等** 的整数 `u` 和 `v` ，且树中必须存在边 `[u, v]` 。
- Bob 猜测树中 `u` 是 `v` 的 **父节点** 。

Bob 的猜测用二维整数数组 `guesses` 表示，其中 `guesses[j] = [uj, vj]` 表示 Bob 猜 `uj` 是 `vj` 的父节点。

Alice 非常懒，她不想逐个回答 Bob 的猜测，只告诉 Bob 这些猜测里面 **至少** 有 `k` 个猜测的结果为 `true` 。

给你二维整数数组 `edges` ，Bob 的所有猜测和整数 `k` ，请你返回可能成为树根的 **节点数目** 。如果没有这样的树，则返回 `0`。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2022/12/19/ex-1.png)

```
输入：edges = [[0,1],[1,2],[1,3],[4,2]], guesses = [[1,3],[0,1],[1,0],[2,4]], k = 3
输出：3
解释：
根为节点 0 ，正确的猜测为 [1,3], [0,1], [2,4]
根为节点 1 ，正确的猜测为 [1,3], [1,0], [2,4]
根为节点 2 ，正确的猜测为 [1,3], [1,0], [2,4]
根为节点 3 ，正确的猜测为 [1,0], [2,4]
根为节点 4 ，正确的猜测为 [1,3], [1,0]
节点 0 ，1 或 2 为根时，可以得到 3 个正确的猜测。
```

C++版本

```c++

```

Java版本

```java

```

