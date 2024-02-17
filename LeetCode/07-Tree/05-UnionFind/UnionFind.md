## 并查集

### [990. 等式方程的可满足性](https://leetcode.cn/problems/satisfiability-of-equality-equations/)

中等

给定一个由表示变量之间关系的字符串方程组成的数组，每个字符串方程 `equations[i]` 的长度为 `4`，并采用两种不同的形式之一：`"a==b"` 或 `"a!=b"`。在这里，a 和 b 是小写字母（不一定不同），表示单字母变量名。

只有当可以将整数分配给变量名，以便满足所有给定的方程时才返回 `true`，否则返回 `false`。 

**示例 1：**

```
输入：["a==b","b!=a"]
输出：false
解释：如果我们指定，a = 1 且 b = 1，那么可以满足第一个方程，但无法满足第二个方程。没有办法分配变量同时满足这两个方程。
```

C++版本

```c++
// 方法一：并查集
class UnionFind {
private:
    vector<int> parent;

public:
    UnionFind() {
        parent.resize(26);
        iota(parent.begin(), parent.end(), 0);
    }

    int find(int index) {
        if (index == parent[index]) {
            return index;
        }
        parent[index] = find(parent[index]);
        return parent[index];
    }

    void unite(int index1, int index2) {
        parent[find(index1)] = find(index2);
    }
};

class Solution {
public:
    bool equationsPossible(vector<string>& equations) {
        UnionFind uf;
        for (const string& str: equations) {
            if (str[1] == '=') {
                int index1 = str[0] - 'a';
                int index2 = str[3] - 'a';
                uf.unite(index1, index2);
            }
        }
        for (const string& str: equations) {
            if (str[1] == '!') {
                int index1 = str[0] - 'a';
                int index2 = str[3] - 'a';
                if (uf.find(index1) == uf.find(index2)) {
                    return false;
                }
            }
        }
        return true;
    }
};
```

Java版本

```java
// 方法一：并查集
class Solution {
    public boolean equationsPossible(String[] equations) {
        int[] parent = new int[26];
        for (int i = 0; i < 26; i++) {
            parent[i] = i;
        }
        for (String str : equations) {
            if (str.charAt(1) == '=') {
                int index1 = str.charAt(0) - 'a';
                int index2 = str.charAt(3) - 'a';
                union(parent, index1, index2);
            }
        }
        for (String str : equations) {
            if (str.charAt(1) == '!') {
                int index1 = str.charAt(0) - 'a';
                int index2 = str.charAt(3) - 'a';
                if (find(parent, index1) == find(parent, index2)) {
                    return false;
                }
            }
        }
        return true;
    }

    public void union(int[] parent, int index1, int index2) {
        parent[find(parent, index1)] = find(parent, index2);
    }

    public int find(int[] parent, int index) {
        while (parent[index] != index) {
            parent[index] = parent[parent[index]];
            index = parent[index];
        }
        return index;
    }
}
```



### [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/)

中等

有 `n` 个城市，其中一些彼此相连，另一些没有相连。如果城市 `a` 与城市 `b` 直接相连，且城市 `b` 与城市 `c` 直接相连，那么城市 `a` 与城市 `c` 间接相连。

**省份** 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 `n x n` 的矩阵 `isConnected` ，其中 `isConnected[i][j] = 1` 表示第 `i` 个城市和第 `j` 个城市直接相连，而 `isConnected[i][j] = 0` 表示二者不直接相连。

返回矩阵中 **省份** 的数量。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

```
输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出：2
```

C++版本

```c++
// 方法一：并查集
class Solution {
public:
    int Find(vector<int>& parent, int index) {
        if (parent[index] != index) {
            parent[index] = Find(parent, parent[index]);
        }
        return parent[index];
    }

    void Union(vector<int>& parent, int index1, int index2) {
        parent[Find(parent, index1)] = Find(parent, index2);
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        int cities = isConnected.size();
        vector<int> parent(cities);
        for (int i = 0; i < cities; i++) {
            parent[i] = i;
        }
        for (int i = 0; i < cities; i++) {
            for (int j = i + 1; j < cities; j++) {
                if (isConnected[i][j] == 1) {
                    Union(parent, i, j);
                }
            }
        }
        int provinces = 0;
        for (int i = 0; i < cities; i++) {
            if (parent[i] == i) {
                provinces++;
            }
        }
        return provinces;
    }
};
```

Java版本

```java
// 方法一：并查集
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int cities = isConnected.length;
        int[] parent = new int[cities];
        for (int i = 0; i < cities; i++) {
            parent[i] = i;
        }
        for (int i = 0; i < cities; i++) {
            for (int j = i + 1; j < cities; j++) {
                if (isConnected[i][j] == 1) {
                    union(parent, i, j);
                }
            }
        }
        int provinces = 0;
        for (int i = 0; i < cities; i++) {
            if (parent[i] == i) {
                provinces++;
            }
        }
        return provinces;
    }

    public void union(int[] parent, int index1, int index2) {
        parent[find(parent, index1)] = find(parent, index2);
    }

    public int find(int[] parent, int index) {
        if (parent[index] != index) {
            parent[index] = find(parent, parent[index]);
        }
        return parent[index];
    }
}
```



### [684. 冗余连接](https://leetcode.cn/problems/redundant-connection/)

中等

树可以看成是一个连通且 **无环** 的 **无向** 图。

给定往一棵 `n` 个节点 (节点值 `1～n`) 的树中添加一条边后的图。添加的边的两个顶点包含在 `1` 到 `n` 中间，且这条附加的边不属于树中已存在的边。图的信息记录于长度为 `n` 的二维数组 `edges` ，`edges[i] = [ai, bi]` 表示图中在 `ai` 和 `bi` 之间存在一条边。

请找出一条可以删去的边，删除后可使得剩余部分是一个有着 `n` 个节点的树。如果有多个答案，则返回数组 `edges` 中最后出现的那个。

**示例 1：**

![img](https://pic.leetcode-cn.com/1626676174-hOEVUL-image.png)

```
输入: edges = [[1,2], [1,3], [2,3]]
输出: [2,3]
```

C++版本

```c++
// 方法一：并查集
class Solution {
public:
    int Find(vector<int>& parent, int index) {
        if (parent[index] != index) {
            parent[index] = Find(parent, parent[index]);
        }
        return parent[index];
    }

    void Union(vector<int>& parent, int index1, int index2) {
        parent[Find(parent, index1)] = Find(parent, index2);
    }

    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> parent(n + 1);
        for (int i = 1; i <= n; ++i) {
            parent[i] = i;
        }
        for (auto& edge: edges) {
            int node1 = edge[0], node2 = edge[1];
            if (Find(parent, node1) != Find(parent, node2)) {
                Union(parent, node1, node2);
            } else {
                return edge;
            }
        }
        return vector<int>{};
    }
};
```

Java版本

```java
// 方法一：并查集
class Solution {
    public int[] findRedundantConnection(int[][] edges) {
        int n = edges.length;
        int[] parent = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            parent[i] = i;
        }
        for (int i = 0; i < n; i++) {
            int[] edge = edges[i];
            int node1 = edge[0], node2 = edge[1];
            if (find(parent, node1) != find(parent, node2)) {
                union(parent, node1, node2);
            } else {
                return edge;
            }
        }
        return new int[0];
    }

    public void union(int[] parent, int index1, int index2) {
        parent[find(parent, index1)] = find(parent, index2);
    }

    public int find(int[] parent, int index) {
        if (parent[index] != index) {
            parent[index] = find(parent, parent[index]);
        }
        return parent[index];
    }
}
```



### [1319. 连通网络的操作次数](https://leetcode.cn/problems/number-of-operations-to-make-network-connected/)

中等

用以太网线缆将 `n` 台计算机连接成一个网络，计算机的编号从 `0` 到 `n-1`。线缆用 `connections` 表示，其中 `connections[i] = [a, b]` 连接了计算机 `a` 和 `b`。

网络中的任何一台计算机都可以通过网络直接或者间接访问同一个网络中其他任意一台计算机。

给你这个计算机网络的初始布线 `connections`，你可以拔开任意两台直连计算机之间的线缆，并用它连接一对未直连的计算机。请你计算并返回使所有计算机都连通所需的最少操作次数。如果不可能，则返回 -1 。 

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/11/sample_1_1677.png)**

```
输入：n = 4, connections = [[0,1],[0,2],[1,2]]
输出：1
解释：拔下计算机 1 和 2 之间的线缆，并将它插到计算机 1 和 3 上。
```

C++版本

```c++
// 方法一：深度优先搜索
class Solution {
private:
    vector<vector<int>> edges;
    vector<int> used;

public:
    void dfs(int u) {
        used[u] = true;
        for (int v: edges[u]) {
            if (!used[v]) {
                dfs(v);
            }
        }
    }
    
    int makeConnected(int n, vector<vector<int>>& connections) {
        if (connections.size() < n - 1) {
            return -1;
        }

        edges.resize(n);
        for (const auto& conn: connections) {
            edges[conn[0]].push_back(conn[1]);
            edges[conn[1]].push_back(conn[0]);
        }
        
        used.resize(n);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (!used[i]) {
                dfs(i);
                ++ans;
            }
        }
        
        return ans - 1;
    }
};

// 方法二：并查集
// 并查集模板
class UnionFind {
public:
    vector<int> parent;
    vector<int> size;
    int n;
    // 当前连通分量数目
    int setCount;
    
public:
    UnionFind(int _n): n(_n), setCount(_n), parent(_n), size(_n, 1) {
        iota(parent.begin(), parent.end(), 0);
    }
    
    int findset(int x) {
        return parent[x] == x ? x : parent[x] = findset(parent[x]);
    }
    
    bool unite(int x, int y) {
        x = findset(x);
        y = findset(y);
        if (x == y) {
            return false;
        }
        if (size[x] < size[y]) {
            swap(x, y);
        }
        parent[y] = x;
        size[x] += size[y];
        --setCount;
        return true;
    }
    
    bool connected(int x, int y) {
        x = findset(x);
        y = findset(y);
        return x == y;
    }
};

class Solution {
public:
    int makeConnected(int n, vector<vector<int>>& connections) {
        if (connections.size() < n - 1) {
            return -1;
        }

        UnionFind uf(n);
        for (const auto& conn: connections) {
            uf.unite(conn[0], conn[1]);
        }

        return uf.setCount - 1;
    }
};
```

Java版本

```java
// 方法一：深度优先搜索
class Solution {
    List<Integer>[] edges;
    boolean[] used;

    public int makeConnected(int n, int[][] connections) {
        if (connections.length < n - 1) {
            return -1;
        }

        edges = new List[n];
        for (int i = 0; i < n; ++i) {
            edges[i] = new ArrayList<Integer>();
        }
        for (int[] conn : connections) {
            edges[conn[0]].add(conn[1]);
            edges[conn[1]].add(conn[0]);
        }
        
        used = new boolean[n];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (!used[i]) {
                dfs(i);
                ++ans;
            }
        }
        
        return ans - 1;
    }

    public void dfs(int u) {
        used[u] = true;
        for (int v : edges[u]) {
            if (!used[v]) {
                dfs(v);
            }
        }
    }
}

// 方法二：并查集
class Solution {
    public int makeConnected(int n, int[][] connections) {
        if (connections.length < n - 1) {
            return -1;
        }

        UnionFind uf = new UnionFind(n);
        for (int[] conn : connections) {
            uf.unite(conn[0], conn[1]);
        }

        return uf.setCount - 1;
    }
}

// 并查集模板
class UnionFind {
    int[] parent;
    int[] size;
    int n;
    // 当前连通分量数目
    int setCount;

    public UnionFind(int n) {
        this.n = n;
        this.setCount = n;
        this.parent = new int[n];
        this.size = new int[n];
        Arrays.fill(size, 1);
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }
    
    public int findset(int x) {
        return parent[x] == x ? x : (parent[x] = findset(parent[x]));
    }
    
    public boolean unite(int x, int y) {
        x = findset(x);
        y = findset(y);
        if (x == y) {
            return false;
        }
        if (size[x] < size[y]) {
            int temp = x;
            x = y;
            y = temp;
        }
        parent[y] = x;
        size[x] += size[y];
        --setCount;
        return true;
    }
    
    public boolean connected(int x, int y) {
        x = findset(x);
        y = findset(y);
        return x == y;
    }
}
```



### [765. 情侣牵手](https://leetcode.cn/problems/couples-holding-hands/)

困难

`n` 对情侣坐在连续排列的 `2n` 个座位上，想要牵到对方的手。

人和座位由一个整数数组 `row` 表示，其中 `row[i]` 是坐在第 `i `个座位上的人的 **ID**。情侣们按顺序编号，第一对是 `(0, 1)`，第二对是 `(2, 3)`，以此类推，最后一对是 `(2n-2, 2n-1)`。

返回 *最少交换座位的次数，以便每对情侣可以并肩坐在一起*。 *每次*交换可选择任意两人，让他们站起来交换座位。

**示例 1:**

```
输入: row = [0,2,1,3]
输出: 1
解释: 只需要交换row[1]和row[2]的位置即可。
```

C++版本

```c++
// 方法：并查集
class UnionFind {
private:
    vector<int> parent;
    int count;

public:
    UnionFind(int n) : count(n) {
        parent.resize(n);
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }

    int getCount() {
        return count;
    }

    int find(int x) {
        while (x != parent[x]) {
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }

    void unionSets(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX == rootY) {
            return;
        }
        parent[rootX] = rootY;
        count--;
    }
};

class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        int len = row.size();
        int N = len / 2;
        UnionFind unionFind(N);
        for (int i = 0; i < len; i += 2) {
            unionFind.unionSets(row[i] / 2, row[i + 1] / 2);
        }
        return N - unionFind.getCount();
    }
};
```

Java版本

```java
// 方法：并查集
public class Solution {

    public int minSwapsCouples(int[] row) {
        int len = row.length;
        int N = len / 2;
        UnionFind unionFind = new UnionFind(N);
        for (int i = 0; i < len; i += 2) {
            unionFind.union(row[i] / 2, row[i + 1] / 2);
        }
        return N - unionFind.getCount();
    }

    private class UnionFind {

        private int[] parent;

        private int count;

        public int getCount() {
            return count;
        }

        public UnionFind(int n) {
            this.count = n;
            this.parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        public int find(int x) {
            while (x != parent[x]) {
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        }

        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return;
            }

            parent[rootX] = rootY;
            count--;
        }
    }
}
```



### [399. 除法求值](https://leetcode.cn/problems/evaluate-division/)

中等

给你一个变量对数组 `equations` 和一个实数值数组 `values` 作为已知条件，其中 `equations[i] = [Ai, Bi]` 和 `values[i]` 共同表示等式 `Ai / Bi = values[i]` 。每个 `Ai` 或 `Bi` 是一个表示单个变量的字符串。

另有一些以数组 `queries` 表示的问题，其中 `queries[j] = [Cj, Dj]` 表示第 `j` 个问题，请你根据已知条件找出 `Cj / Dj = ?` 的结果作为答案。

返回 **所有问题的答案** 。如果存在某个无法确定的答案，则用 `-1.0` 替代这个答案。如果问题中出现了给定的已知条件中没有出现的字符串，也需要用 `-1.0` 替代这个答案。

**注意：**输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。

**注意：**未在等式列表中出现的变量是未定义的，因此无法确定它们的答案。

**示例 1：**

```
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
条件：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
结果：[6.0, 0.5, -1.0, 1.0, -1.0 ]
注意：x 是未定义的 => -1.0
```

C++版本

```c++
// 方法：并查集
class UnionFind {
private:
    vector<int> parent;
    vector<double> weight;

public:
    UnionFind(int n) {
        parent.resize(n);
        weight.resize(n, 1.0);
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }

    int find(int x) {
        if (x != parent[x]) {
            int origin = parent[x];
            parent[x] = find(parent[x]);
            weight[x] *= weight[origin];
        }
        return parent[x];
    }

    void unionSets(int x, int y, double value) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX == rootY) {
            return;
        }
        parent[rootX] = rootY;
        weight[rootX] = weight[y] * value / weight[x];
    }

    double isConnected(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX == rootY) {
            return weight[x] / weight[y];
        } else {
            return -1.0;
        }
    }
};

class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        int equationsSize = equations.size();

        UnionFind unionFind(2 * equationsSize);
        unordered_map<string, int> hashMap;
        int id = 0;
        for (int i = 0; i < equationsSize; ++i) {
            const string& var1 = equations[i][0];
            const string& var2 = equations[i][1];

            if (hashMap.find(var1) == hashMap.end()) {
                hashMap[var1] = id++;
            }
            if (hashMap.find(var2) == hashMap.end()) {
                hashMap[var2] = id++;
            }
            unionFind.unionSets(hashMap[var1], hashMap[var2], values[i]);
        }

        int queriesSize = queries.size();
        vector<double> res(queriesSize);
        for (int i = 0; i < queriesSize; ++i) {
            const string& var1 = queries[i][0];
            const string& var2 = queries[i][1];

            auto it1 = hashMap.find(var1);
            auto it2 = hashMap.find(var2);
            if (it1 == hashMap.end() || it2 == hashMap.end()) {
                res[i] = -1.0;
            } else {
                res[i] = unionFind.isConnected(it1->second, it2->second);
            }
        }
        return res;
    }
};
```

Java版本

```java
// 方法：并查集
public class Solution {

    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        int equationsSize = equations.size();

        UnionFind unionFind = new UnionFind(2 * equationsSize);
        // 第 1 步：预处理，将变量的值与 id 进行映射，使得并查集的底层使用数组实现，方便编码
        Map<String, Integer> hashMap = new HashMap<>(2 * equationsSize);
        int id = 0;
        for (int i = 0; i < equationsSize; i++) {
            List<String> equation = equations.get(i);
            String var1 = equation.get(0);
            String var2 = equation.get(1);

            if (!hashMap.containsKey(var1)) {
                hashMap.put(var1, id);
                id++;
            }
            if (!hashMap.containsKey(var2)) {
                hashMap.put(var2, id);
                id++;
            }
            unionFind.union(hashMap.get(var1), hashMap.get(var2), values[i]);
        }

        // 第 2 步：做查询
        int queriesSize = queries.size();
        double[] res = new double[queriesSize];
        for (int i = 0; i < queriesSize; i++) {
            String var1 = queries.get(i).get(0);
            String var2 = queries.get(i).get(1);

            Integer id1 = hashMap.get(var1);
            Integer id2 = hashMap.get(var2);

            if (id1 == null || id2 == null) {
                res[i] = -1.0d;
            } else {
                res[i] = unionFind.isConnected(id1, id2);
            }
        }
        return res;
    }

    private class UnionFind {

        private int[] parent;

        /**
         * 指向的父结点的权值
         */
        private double[] weight;


        public UnionFind(int n) {
            this.parent = new int[n];
            this.weight = new double[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                weight[i] = 1.0d;
            }
        }

        public void union(int x, int y, double value) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return;
            }

            parent[rootX] = rootY;
          	// 关系式的推导请见「参考代码」下方的示意图
            weight[rootX] = weight[y] * value / weight[x];
        }

        /**
         * 路径压缩
         *
         * @param x
         * @return 根结点的 id
         */
        public int find(int x) {
            if (x != parent[x]) {
                int origin = parent[x];
                parent[x] = find(parent[x]);
                weight[x] *= weight[origin];
            }
            return parent[x];
        }

        public double isConnected(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return weight[x] / weight[y];
            } else {
                return -1.0d;
            }
        }
    }
}
```



### [959. 由斜杠划分区域](https://leetcode.cn/problems/regions-cut-by-slashes/)

中等

在由 `1 x 1` 方格组成的 `n x n` 网格 `grid` 中，每个 `1 x 1` 方块由 `'/'`、`'\'` 或空格构成。这些字符会将方块划分为一些共边的区域。

给定网格 `grid` 表示为一个字符串数组，返回 *区域的数量* 。

请注意，反斜杠字符是转义的，因此 `'\'` 用 `'\\'` 表示。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/15/1.png)

```
输入：grid = [" /","/ "]
输出：2
```

C++版本

```c++
// 方法：并查集
class Solution {
public:
    int regionsBySlashes(vector<string>& grid) {
        int N = grid.size();
        int size = 4 * N * N;

        UnionFind unionFind(size);
        for (int i = 0; i < N; ++i) {
            string row = grid[i];
            for (int j = 0; j < N; ++j) {
                int index = 4 * (i * N + j);
                char c = row[j];
                if (c == '/') {
                    unionFind.unionSet(index, index + 3);
                    unionFind.unionSet(index + 1, index + 2);
                } else if (c == '\\') {
                    unionFind.unionSet(index, index + 1);
                    unionFind.unionSet(index + 2, index + 3);
                } else {
                    unionFind.unionSet(index, index + 1);
                    unionFind.unionSet(index + 1, index + 2);
                    unionFind.unionSet(index + 2, index + 3);
                }

                if (j + 1 < N) {
                    unionFind.unionSet(index + 1, 4 * (i * N + j + 1) + 3);
                }
                if (i + 1 < N) {
                    unionFind.unionSet(index + 2, 4 * ((i + 1) * N + j));
                }
            }
        }
        return unionFind.getCount();
    }

private:
    class UnionFind {
    private:
        vector<int> parent;
        int count;

    public:
        UnionFind(int n) : count(n) {
            parent.resize(n);
            for (int i = 0; i < n; ++i) {
                parent[i] = i;
            }
        }

        int getCount() {
            return count;
        }

        int find(int x) {
            while (x != parent[x]) {
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        }

        void unionSet(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX != rootY) {
                parent[rootX] = rootY;
                count--;
            }
        }
    };
};
```

Java版本

```java
// 方法：并查集
public class Solution {

    public int regionsBySlashes(String[] grid) {
        int N = grid.length;
        int size = 4 * N * N;

        UnionFind unionFind = new UnionFind(size);
        for (int i = 0; i < N; i++) {
            char[] row = grid[i].toCharArray();
            for (int j = 0; j < N; j++) {
                // 二维网格转换为一维表格，index 表示将单元格拆分成 4 个小三角形以后，编号为 0 的小三角形的在并查集中的下标
                int index = 4 * (i * N + j);
                char c = row[j];
                // 单元格内合并
                if (c == '/') {
                    // 合并 0、3，合并 1、2
                    unionFind.union(index, index + 3);
                    unionFind.union(index + 1, index + 2);
                } else if (c == '\\') {
                    // 合并 0、1，合并 2、3
                    unionFind.union(index, index + 1);
                    unionFind.union(index + 2, index + 3);
                } else {
                    unionFind.union(index, index + 1);
                    unionFind.union(index + 1, index + 2);
                    unionFind.union(index + 2, index + 3);
                }

                // 单元格间合并
                // 向右合并：1（当前）、3（右一列）
                if (j + 1 < N) {
                    unionFind.union(index + 1, 4 * (i * N + j + 1) + 3);
                }
                // 向下合并：2（当前）、0（下一行）
                if (i + 1 < N) {
                    unionFind.union(index + 2, 4 * ((i + 1) * N + j));
                }
            }
        }
        return unionFind.getCount();
    }

    private class UnionFind {

        private int[] parent;

        private int count;

        public int getCount() {
            return count;
        }

        public UnionFind(int n) {
            this.count = n;
            this.parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        public int find(int x) {
            while (x != parent[x]) {
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        }

        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return;
            }

            parent[rootX] = rootY;
            count--;
        }
    }
}
```



### [1631. 最小体力消耗路径](https://leetcode.cn/problems/path-with-minimum-effort/)

中等

你准备参加一场远足活动。给你一个二维 `rows x columns` 的地图 `heights` ，其中 `heights[row][col]` 表示格子 `(row, col)` 的高度。一开始你在最左上角的格子 `(0, 0)` ，且你希望去最右下角的格子 `(rows-1, columns-1)` （注意下标从 **0** 开始编号）。你每次可以往 **上**，**下**，**左**，**右** 四个方向之一移动，你想要找到耗费 **体力** 最小的一条路径。

一条路径耗费的 **体力值** 是路径上相邻格子之间 **高度差绝对值** 的 **最大值** 决定的。

请你返回从左上角走到右下角的最小 **体力消耗值** 。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex1.png)

```
输入：heights = [[1,2,2],[3,8,2],[5,3,5]]
输出：2
解释：路径 [1,3,5,3,5] 连续格子的差值绝对值最大为 2 。
这条路径比路径 [1,2,2,2,5] 更优，因为另一条路径差值最大值为 3 。
```

C++版本

```c++
// 方法一：二分查找
class Solution {
private:
    static constexpr int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    
public:
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        int left = 0, right = 999999, ans = 0;
        while (left <= right) {
            int mid = (left + right) / 2;
            queue<pair<int, int>> q;
            q.emplace(0, 0);
            vector<int> seen(m * n);
            seen[0] = 1;
            while (!q.empty()) {
                auto [x, y] = q.front();
                q.pop();
                for (int i = 0; i < 4; ++i) {
                    int nx = x + dirs[i][0];
                    int ny = y + dirs[i][1];
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n && !seen[nx * n + ny] && abs(heights[x][y] - heights[nx][ny]) <= mid) {
                        q.emplace(nx, ny);
                        seen[nx * n + ny] = 1;
                    }
                }
            }
            if (seen[m * n - 1]) {
                ans = mid;
                right = mid - 1;
            }
            else {
                left = mid + 1;
            }
        }
        return ans;
    }
};

// 方法二：并查集
// 并查集模板
class UnionFind {
public:
    vector<int> parent;
    vector<int> size;
    int n;
    // 当前连通分量数目
    int setCount;
    
public:
    UnionFind(int _n): n(_n), setCount(_n), parent(_n), size(_n, 1) {
        iota(parent.begin(), parent.end(), 0);
    }
    
    int findset(int x) {
        return parent[x] == x ? x : parent[x] = findset(parent[x]);
    }
    
    bool unite(int x, int y) {
        x = findset(x);
        y = findset(y);
        if (x == y) {
            return false;
        }
        if (size[x] < size[y]) {
            swap(x, y);
        }
        parent[y] = x;
        size[x] += size[y];
        --setCount;
        return true;
    }
    
    bool connected(int x, int y) {
        x = findset(x);
        y = findset(y);
        return x == y;
    }
};

class Solution {
public:
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        vector<tuple<int, int, int>> edges;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int id = i * n + j;
                if (i > 0) {
                    edges.emplace_back(id - n, id, abs(heights[i][j] - heights[i - 1][j]));
                }
                if (j > 0) {
                    edges.emplace_back(id - 1, id, abs(heights[i][j] - heights[i][j - 1]));
                }
            }
        }
        sort(edges.begin(), edges.end(), [](const auto& e1, const auto& e2) {
            auto&& [x1, y1, v1] = e1;
            auto&& [x2, y2, v2] = e2;
            return v1 < v2;
        });

        UnionFind uf(m * n);
        int ans = 0;
        for (const auto [x, y, v]: edges) {
            uf.unite(x, y);
            if (uf.connected(0, m * n - 1)) {
                ans = v;
                break;
            }
        }
        return ans;
    }
};

// 方法三：最短路
class Solution {
private:
    static constexpr int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    
public:
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        
        auto tupleCmp = [](const auto& e1, const auto& e2) {
            auto&& [x1, y1, d1] = e1;
            auto&& [x2, y2, d2] = e2;
            return d1 > d2;
        };
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, decltype(tupleCmp)> q(tupleCmp);
        q.emplace(0, 0, 0);

        vector<int> dist(m * n, INT_MAX);
        dist[0] = 0;
        vector<int> seen(m * n);

        while (!q.empty()) {
            auto [x, y, d] = q.top();
            q.pop();
            int id = x * n + y;
            if (seen[id]) {
                continue;
            }
            if (x == m - 1 && y == n - 1) {
                break;
            }
            seen[id] = 1;
            for (int i = 0; i < 4; ++i) {
                int nx = x + dirs[i][0];
                int ny = y + dirs[i][1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && max(d, abs(heights[x][y] - heights[nx][ny])) < dist[nx * n + ny]) {
                    dist[nx * n + ny] = max(d, abs(heights[x][y] - heights[nx][ny]));
                    q.emplace(nx, ny, dist[nx * n + ny]);
                }
            }
        }
        
        return dist[m * n - 1];
    }
};
```

Java版本

```java
// 方法一：二分查找
class Solution {
    int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    public int minimumEffortPath(int[][] heights) {
        int m = heights.length;
        int n = heights[0].length;
        int left = 0, right = 999999, ans = 0;
        while (left <= right) {
            int mid = (left + right) / 2;
            Queue<int[]> queue = new LinkedList<int[]>();
            queue.offer(new int[]{0, 0});
            boolean[] seen = new boolean[m * n];
            seen[0] = true;
            while (!queue.isEmpty()) {
                int[] cell = queue.poll();
                int x = cell[0], y = cell[1];
                for (int i = 0; i < 4; ++i) {
                    int nx = x + dirs[i][0];
                    int ny = y + dirs[i][1];
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n && !seen[nx * n + ny] && Math.abs(heights[x][y] - heights[nx][ny]) <= mid) {
                        queue.offer(new int[]{nx, ny});
                        seen[nx * n + ny] = true;
                    }
                }
            }
            if (seen[m * n - 1]) {
                ans = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }
}

// 方法二：并查集
class Solution {
    public int minimumEffortPath(int[][] heights) {
        int m = heights.length;
        int n = heights[0].length;
        List<int[]> edges = new ArrayList<int[]>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int id = i * n + j;
                if (i > 0) {
                    edges.add(new int[]{id - n, id, Math.abs(heights[i][j] - heights[i - 1][j])});
                }
                if (j > 0) {
                    edges.add(new int[]{id - 1, id, Math.abs(heights[i][j] - heights[i][j - 1])});
                }
            }
        }
        Collections.sort(edges, new Comparator<int[]>() {
            public int compare(int[] edge1, int[] edge2) {
                return edge1[2] - edge2[2];
            }
        });

        UnionFind uf = new UnionFind(m * n);
        int ans = 0;
        for (int[] edge : edges) {
            int x = edge[0], y = edge[1], v = edge[2];
            uf.unite(x, y);
            if (uf.connected(0, m * n - 1)) {
                ans = v;
                break;
            }
        }
        return ans;
    }
}

// 并查集模板
class UnionFind {
    int[] parent;
    int[] size;
    int n;
    // 当前连通分量数目
    int setCount;

    public UnionFind(int n) {
        this.n = n;
        this.setCount = n;
        this.parent = new int[n];
        this.size = new int[n];
        Arrays.fill(size, 1);
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }
    
    public int findset(int x) {
        return parent[x] == x ? x : (parent[x] = findset(parent[x]));
    }
    
    public boolean unite(int x, int y) {
        x = findset(x);
        y = findset(y);
        if (x == y) {
            return false;
        }
        if (size[x] < size[y]) {
            int temp = x;
            x = y;
            y = temp;
        }
        parent[y] = x;
        size[x] += size[y];
        --setCount;
        return true;
    }
    
    public boolean connected(int x, int y) {
        x = findset(x);
        y = findset(y);
        return x == y;
    }
}

// 方法三：最短路
class Solution {
    int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    public int minimumEffortPath(int[][] heights) {
        int m = heights.length;
        int n = heights[0].length;
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] edge1, int[] edge2) {
                return edge1[2] - edge2[2];
            }
        });
        pq.offer(new int[]{0, 0, 0});

        int[] dist = new int[m * n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[0] = 0;
        boolean[] seen = new boolean[m * n];

        while (!pq.isEmpty()) {
            int[] edge = pq.poll();
            int x = edge[0], y = edge[1], d = edge[2];
            int id = x * n + y;
            if (seen[id]) {
                continue;
            }
            if (x == m - 1 && y == n - 1) {
                break;
            }
            seen[id] = true;
            for (int i = 0; i < 4; ++i) {
                int nx = x + dirs[i][0];
                int ny = y + dirs[i][1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && Math.max(d, Math.abs(heights[x][y] - heights[nx][ny])) < dist[nx * n + ny]) {
                    dist[nx * n + ny] = Math.max(d, Math.abs(heights[x][y] - heights[nx][ny]));
                    pq.offer(new int[]{nx, ny, dist[nx * n + ny]});
                }
            }
        }
        
        return dist[m * n - 1];
    }
}
```



### [778. 水位上升的泳池中游泳](https://leetcode.cn/problems/swim-in-rising-water/)

困难

在一个 `n x n` 的整数矩阵 `grid` 中，每一个方格的值 `grid[i][j]` 表示位置 `(i, j)` 的平台高度。

当开始下雨时，在时间为 `t` 时，水池中的水位为 `t` 。你可以从一个平台游向四周相邻的任意一个平台，但是前提是此时水位必须同时淹没这两个平台。假定你可以瞬间移动无限距离，也就是默认在方格内部游动是不耗时的。当然，在你游泳的时候你必须待在坐标方格里面。

你从坐标方格的左上平台 `(0，0)` 出发。返回 *你到达坐标方格的右下平台 `(n-1, n-1)` 所需的最少时间 。*

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/06/29/swim1-grid.jpg)

```
输入: grid = [[0,2],[1,3]]
输出: 3
解释:
时间为0时，你位于坐标方格的位置为 (0, 0)。
此时你不能游向任意方向，因为四个相邻方向平台的高度都大于当前时间为 0 时的水位。
等时间到达 3 时，你才可以游向平台 (1, 1). 因为此时的水位是 3，坐标方格中的平台没有比水位 3 更高的，所以你可以游向坐标方格中的任意位置
```

C++版本

```c++
// 方法一：二分查找 + 遍历
class Solution {
private:
    int N;
    const vector<vector<int>> DIRECTIONS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    bool dfs(vector<vector<int>>& grid, int x, int y, vector<vector<bool>>& visited, int threshold) {
        visited[x][y] = true;
        for (const auto& direction : DIRECTIONS) {
            int newX = x + direction[0];
            int newY = y + direction[1];
            if (inArea(newX, newY) && !visited[newX][newY] && grid[newX][newY] <= threshold) {
                if (newX == N - 1 && newY == N - 1) {
                    return true;
                }
                if (dfs(grid, newX, newY, visited, threshold)) {
                    return true;
                }
            }
        }
        return false;
    }

    bool inArea(int x, int y) {
        return x >= 0 && x < N && y >= 0 && y < N;
    }

public:
    int swimInWater(vector<vector<int>>& grid) {
        N = grid.size();
        int left = 0;
        int right = N * N - 1;
        while (left < right) {
            int mid = (left + right) / 2;
            vector<vector<bool>> visited(N, vector<bool>(N, false));
            if (grid[0][0] <= mid && dfs(grid, 0, 0, visited, mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};

//方法二：并查集
class Solution {
private:
    int N;
    const vector<vector<int>> DIRECTIONS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    class UnionFind {
    private:
        vector<int> parent;

    public:
        UnionFind(int n) {
            parent.resize(n);
            for (int i = 0; i < n; ++i)
                parent[i] = i;
        }

        int root(int x) {
            while (x != parent[x]) {
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        }

        bool isConnected(int x, int y) {
            return root(x) == root(y);
        }

        void unionSet(int x, int y) {
            int rootX = root(x);
            int rootY = root(y);
            if (rootX != rootY)
                parent[rootX] = rootY;
        }
    };

    bool inArea(int x, int y) {
        return x >= 0 && x < N && y >= 0 && y < N;
    }

public:
    int swimInWater(vector<vector<int>>& grid) {
        N = grid.size();
        int len = N * N;
        vector<int> index(len);

        for (int i = 0; i < N; ++i) {
            for (int j = 0; j < N; ++j) {
                index[grid[i][j]] = i * N + j;
            }
        }

        UnionFind uf(len);

        for (int i = 0; i < len; ++i) {
            int x = index[i] / N;
            int y = index[i] % N;

            for (const auto& direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];
                if (inArea(newX, newY) && grid[newX][newY] <= i) {
                    uf.unionSet(index[i], newX * N + newY);
                }

                if (uf.isConnected(0, len - 1)) {
                    return i;
                }
            }
        }
        return -1;
    }
};

// 方法三：Dijkstra 算法
class Solution {
public:
    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();

        auto compare = [&grid](const pair<int, int>& a, const pair<int, int>& b) {
            return grid[a.first][a.second] > grid[b.first][b.second];
        };
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(compare)> minHeap(compare);
        minHeap.push({0, 0});

        vector<vector<bool>> visited(n, vector<bool>(n, false));
        vector<vector<int>> distTo(n, vector<int>(n, n * n));
        distTo[0][0] = grid[0][0];

        vector<vector<int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        while (!minHeap.empty()) {
            pair<int, int> front = minHeap.top();
            minHeap.pop();
            int currentX = front.first;
            int currentY = front.second;
            if (visited[currentX][currentY]) {
                continue;
            }

            visited[currentX][currentY] = true;
            if (currentX == n - 1 && currentY == n - 1) {
                return distTo[n - 1][n - 1];
            }

            for (const auto& direction : directions) {
                int newX = currentX + direction[0];
                int newY = currentY + direction[1];
                if (inArea(newX, newY, n) && !visited[newX][newY] &&
                        max(distTo[currentX][currentY], grid[newX][newY]) < distTo[newX][newY]) {
                    distTo[newX][newY] = max(distTo[currentX][currentY], grid[newX][newY]);
                    minHeap.push({newX, newY});
                }
            }
        }
        return -1;
    }

private:
    bool inArea(int x, int y, int n) {
        return x >= 0 && x < n && y >= 0 && y < n;
    }
};
```

Java版本

```java
// 方法一：二分查找 + 遍历
public class Solution {

    private int N;

    public static final int[][] DIRECTIONS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    public int swimInWater(int[][] grid) {
        this.N = grid.length;

        int left = 0;
        int right = N * N - 1;
        while (left < right) {
            // left + right 不会溢出
            int mid = (left + right) / 2;
            boolean[][] visited = new boolean[N][N];
            if (grid[0][0] <= mid && dfs(grid, 0, 0, visited, mid)) {
                // mid 可以，尝试 mid 小一点是不是也可以呢？下一轮搜索的区间 [left, mid]
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    /**
     * 使用深度优先遍历得到从 (x, y) 开始向四个方向的所有小于等于 threshold 且与 (x, y) 连通的结点
     *
     * @param grid
     * @param x
     * @param y
     * @param visited
     * @param threshold
     * @return
     */
    private boolean dfs(int[][] grid, int x, int y, boolean[][] visited, int threshold) {
        visited[x][y] = true;
        for (int[] direction : DIRECTIONS) {
            int newX = x + direction[0];
            int newY = y + direction[1];
            if (inArea(newX, newY) && !visited[newX][newY] && grid[newX][newY] <= threshold) {
                if (newX == N - 1 && newY == N - 1) {
                    return true;
                }

                if (dfs(grid, newX, newY, visited, threshold)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean inArea(int x, int y) {
        return x >= 0 && x < N && y >= 0 && y < N;
    }
}

// 方法二：并查集
public class Solution {

    private int N;

    public static final int[][] DIRECTIONS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    public int swimInWater(int[][] grid) {
        this.N = grid.length;

        int len = N * N;
        // 下标：方格的高度，值：对应在方格中的坐标
        int[] index = new int[len];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                index[grid[i][j]] = getIndex(i, j);
            }
        }

        UnionFind unionFind = new UnionFind(len);
        for (int i = 0; i < len; i++) {
            int x = index[i] / N;
            int y = index[i] % N;

            for (int[] direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];
                if (inArea(newX, newY) && grid[newX][newY] <= i) {
                    unionFind.union(index[i], getIndex(newX, newY));
                }

                if (unionFind.isConnected(0, len - 1)) {
                    return i;
                }
            }
        }
        return -1;
    }

    private int getIndex(int x, int y) {
        return x * N + y;
    }

    private boolean inArea(int x, int y) {
        return x >= 0 && x < N && y >= 0 && y < N;
    }

    private class UnionFind {

        private int[] parent;

        public UnionFind(int n) {
            this.parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        public int root(int x) {
            while (x != parent[x]) {
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        }

        public boolean isConnected(int x, int y) {
            return root(x) == root(y);
        }

        public void union(int p, int q) {
            if (isConnected(p, q)) {
                return;
            }
            parent[root(p)] = root(q);
        }
    }
}

// 方法三：Dijkstra 算法
public class Solution {

    // Dijkstra 算法（应用前提：没有负权边，找单源最短路径）

    public int swimInWater(int[][] grid) {
        int n = grid.length;

        Queue<int[]> minHeap = new PriorityQueue<>(Comparator.comparingInt(o -> grid[o[0]][o[1]]));
        minHeap.offer(new int[]{0, 0});

        boolean[][] visited = new boolean[n][n];
        // distTo[i][j] 表示：到顶点 [i, j] 须要等待的最少的时间
        int[][] distTo = new int[n][n];
        for (int[] row : distTo) {
            Arrays.fill(row, n * n);
        }
        distTo[0][0] = grid[0][0];

        int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        while (!minHeap.isEmpty()) {
            // 找最短的边
            int[] front = minHeap.poll();
            int currentX = front[0];
            int currentY = front[1];
            if (visited[currentX][currentY]) {
                continue;
            }

            // 确定最短路径顶点
            visited[currentX][currentY] = true;
            if (currentX == n - 1 && currentY == n - 1) {
                return distTo[n - 1][n - 1];
            }

            // 更新
            for (int[] direction : directions) {
                int newX = currentX + direction[0];
                int newY = currentY + direction[1];
                if (inArea(newX, newY, n) && !visited[newX][newY] &&
                        Math.max(distTo[currentX][currentY], grid[newX][newY]) < distTo[newX][newY]) {
                    distTo[newX][newY] = Math.max(distTo[currentX][currentY], grid[newX][newY]);
                    minHeap.offer(new int[]{newX, newY});
                }
            }
        }
        return -1;
    }

    private boolean inArea(int x, int y, int n) {
        return x >= 0 && x < n && y >= 0 && y < n;
    }
}
```



### [1202. 交换字符串中的元素](https://leetcode.cn/problems/smallest-string-with-swaps/)

中等

给你一个字符串 `s`，以及该字符串中的一些「索引对」数组 `pairs`，其中 `pairs[i] = [a, b]` 表示字符串中的两个索引（编号从 0 开始）。

你可以 **任意多次交换** 在 `pairs` 中任意一对索引处的字符。

返回在经过若干次交换后，`s` 可以变成的按字典序最小的字符串。

**示例 1:**

```
输入：s = "dcab", pairs = [[0,3],[1,2]]
输出："bacd"
解释： 
交换 s[0] 和 s[3], s = "bcad"
交换 s[1] 和 s[2], s = "bacd"
```

C++版本

```c++
// 方法一：并查集
class DisjointSetUnion {
private:
    vector<int> f, rank;
    int n;

public:
    DisjointSetUnion(int _n) {
        n = _n;
        rank.resize(n, 1);
        f.resize(n);
        for (int i = 0; i < n; i++) {
            f[i] = i;
        }
    }

    int find(int x) {
        return f[x] == x ? x : f[x] = find(f[x]);
    }

    void unionSet(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) {
            return;
        }
        if (rank[fx] < rank[fy]) {
            swap(fx, fy);
        }
        rank[fx] += rank[fy];
        f[fy] = fx;
    }
};

class Solution {
public:
    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        DisjointSetUnion dsu(s.length());
        for (auto& it : pairs) {
            dsu.unionSet(it[0], it[1]);
        }
        unordered_map<int, vector<int>> mp;
        for (int i = 0; i < s.length(); i++) {
            mp[dsu.find(i)].emplace_back(s[i]);
        }
        for (auto& [x, vec] : mp) {
            sort(vec.begin(), vec.end(), greater<int>());
        }
        for (int i = 0; i < s.length(); i++) {
            int x = dsu.find(i);
            s[i] = mp[x].back();
            mp[x].pop_back();
        }
        return s;
    }
};
```

Java版本

```java
// 方法一：并查集
class Solution {
    public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
        DisjointSetUnion dsu = new DisjointSetUnion(s.length());
        for (List<Integer> pair : pairs) {
            dsu.unionSet(pair.get(0), pair.get(1));
        }
        Map<Integer, List<Character>> map = new HashMap<Integer, List<Character>>();
        for (int i = 0; i < s.length(); i++) {
            int parent = dsu.find(i);
            if (!map.containsKey(parent)) {
                map.put(parent, new ArrayList<Character>());
            }
            map.get(parent).add(s.charAt(i));
        }
        for (Map.Entry<Integer, List<Character>> entry : map.entrySet()) {
            Collections.sort(entry.getValue(), new Comparator<Character>() {
                public int compare(Character c1, Character c2) {
                    return c2 - c1;
                }
            });
        }
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < s.length(); i++) {
            int x = dsu.find(i);
            List<Character> list = map.get(x);
            sb.append(list.remove(list.size() - 1));
        }
        return sb.toString();
    }
}

class DisjointSetUnion {
    int[] f;
    int[] rank;
    int n;

    public DisjointSetUnion(int n) {
        this.n = n;
        rank = new int[n];
        Arrays.fill(rank, 1);
        f = new int[n];
        for (int i = 0; i < n; i++) {
            f[i] = i;
        }
    }

    public int find(int x) {
        return f[x] == x ? x : (f[x] = find(f[x]));
    }

    public void unionSet(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) {
            return;
        }
        if (rank[fx] < rank[fy]) {
            int temp = fx;
            fx = fy;
            fy = temp;
        }
        rank[fx] += rank[fy];
        f[fy] = fx;
    }
}

// 并查集（）
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.PriorityQueue;

public class Solution {

    public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
        if (pairs.size() == 0) {
            return s;
        }

        // 第 1 步：将任意交换的结点对输入并查集
        int len = s.length();
        UnionFind unionFind = new UnionFind(len);
        for (List<Integer> pair : pairs) {
            int index1 = pair.get(0);
            int index2 = pair.get(1);
            unionFind.union(index1, index2);
        }

        // 第 2 步：构建映射关系
        char[] charArray = s.toCharArray();
        // key：连通分量的代表元，value：同一个连通分量的字符集合（保存在一个优先队列中）
        Map<Integer, PriorityQueue<Character>> hashMap = new HashMap<>(len);
        for (int i = 0; i < len; i++) {
            int root = unionFind.find(i);
//            if (hashMap.containsKey(root)) {
//                hashMap.get(root).offer(charArray[i]);
//            } else {
//                PriorityQueue<Character> minHeap = new PriorityQueue<>();
//                minHeap.offer(charArray[i]);
//                hashMap.put(root, minHeap);
//            }
            // 上面六行代码等价于下面一行代码，JDK 1.8 以及以后支持下面的写法
            hashMap.computeIfAbsent(root, key -> new PriorityQueue<>()).offer(charArray[i]);
        }

        // 第 3 步：重组字符串
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < len; i++) {
            int root = unionFind.find(i);
            stringBuilder.append(hashMap.get(root).poll());
        }
        return stringBuilder.toString();
    }

    private class UnionFind {

        private int[] parent;
        /**
         * 以 i 为根结点的子树的高度（引入了路径压缩以后该定义并不准确）
         */
        private int[] rank;

        public UnionFind(int n) {
            this.parent = new int[n];
            this.rank = new int[n];
            for (int i = 0; i < n; i++) {
                this.parent[i] = i;
                this.rank[i] = 1;
            }
        }

        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return;
            }

            if (rank[rootX] == rank[rootY]) {
                parent[rootX] = rootY;
                // 此时以 rootY 为根结点的树的高度仅加了 1
                rank[rootY]++;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
                // 此时以 rootY 为根结点的树的高度不变
            } else {
                // 同理，此时以 rootX 为根结点的树的高度不变
                parent[rootY] = rootX;
            }
        }

        public int find(int x) {
            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }
    }
}

// 并查集
class Solution {
    public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
        char[] cs = s.toCharArray();
        int n = cs.length;
        UnionFind uf = new UnionFind(n);
 
        for (List<Integer> pair : pairs) 
             uf.union(pair.get(0), pair.get(1));

        Node[] nodes = new Node[n];
        for (int i = 0; i < n; i++) {
            int root = uf.find(i);
            if (nodes[root] == null) 
                 nodes[root] = new Node();
            nodes[root].update(cs[i]);
        }

        for (int i = 0; i < n; i++) {
            int root = uf.find(i);
            cs[i] = nodes[root].getChar();
        }
        return new String(cs);
    }

    class UnionFind {
        int n;
        int[] parent;
        int[] rank;

        public UnionFind(int n) {
            this.n = n;
            this.parent = new int[n];
            this.rank = new int[n];

            for (int i = 0; i < n; i++) {
                parent[i] = i;
                rank[i] = 1;
            }
        }

        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);

            if (rootX == rootY) return;

            if (rank[rootX] >= rank[rootY]) {
                parent[rootY] = rootX;
            } else {
                parent[rootX] = rootY;
            }

            rank[parent[rootX]] = rank[rootX] + rank[rootY];
        }

        public int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }
    }

    class Node {
        int curIndex;
        int[] count;

        public Node() {
            this.curIndex = 26;
            this.count = new int[26];
        }

        public void update(char c) {
            int index = c - 'a';
            count[index]++;
            if(index < curIndex)
                curIndex = index;
        }

        public char getChar() {
            while (count[curIndex] == 0) curIndex++;
            count[curIndex]--;
            return (char) (curIndex + 'a');
        }
    }
}
```



### [947. 移除最多的同行或同列石头](https://leetcode.cn/problems/most-stones-removed-with-same-row-or-column/)

中等

`n` 块石头放置在二维平面中的一些整数坐标点上。每个坐标点上最多只能有一块石头。

如果一块石头的 **同行或者同列** 上有其他石头存在，那么就可以移除这块石头。

给你一个长度为 `n` 的数组 `stones` ，其中 `stones[i] = [xi, yi]` 表示第 `i` 块石头的位置，返回 **可以移除的石子** 的最大数量。

**示例 1：**

```
输入：stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
输出：5
解释：一种移除 5 块石头的方法如下所示：
1. 移除石头 [2,2] ，因为它和 [2,1] 同行。
2. 移除石头 [2,1] ，因为它和 [0,1] 同列。
3. 移除石头 [1,2] ，因为它和 [1,0] 同行。
4. 移除石头 [1,0] ，因为它和 [0,0] 同列。
5. 移除石头 [0,1] ，因为它和 [0,0] 同行。
石头 [0,0] 不能移除，因为它没有与另一块石头同行/列。
```

C++版本

```c++
// 方法：并查集
#include <unordered_map>
#include <vector>

using namespace std;

class Solution {
public:
    int removeStones(vector<vector<int>>& stones) {
        UnionFind unionFind;

        for (auto& stone : stones) {
            unionFind.union_(~stone[0], stone[1]);
        }
        return stones.size() - unionFind.getCount();
    }

private:
    class UnionFind {
    private:
        unordered_map<int, int> parent;
        int count;

    public:
        UnionFind() : count(0) {}

        int getCount() {
            return count;
        }

        int find(int x) {
            if (!parent.count(x)) {
                parent[x] = x;
                count++;
            }

            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }

        void union_(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return;
            }

            parent[rootX] = rootY;
            count--;
        }
    };
};

// 方法一：深度优先搜索
class Solution {
public:
    void dfs(int x, vector<vector<int>> &edge, vector<int> &vis) {
        vis[x] = true;
        for (auto &y : edge[x]) {
            if (!vis[y]) {
                dfs(y, edge, vis);
            }
        }
    }

    int removeStones(vector<vector<int>> &stones) {
        int n = stones.size();
        vector<vector<int>> edge(n);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (stones[i][0] == stones[j][0] || stones[i][1] == stones[j][1]) {
                    edge[i].push_back(j);
                }
            }
        }
        vector<int> vis(n);
        int num = 0;
        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                num++;
                dfs(i, edge, vis);
            }
        }
        return n - num;
    }
};

// 方法二：优化建图 + 深度优先搜索
class Solution {
public:
    void dfs(int x, vector<vector<int>> &edge, vector<int> &vis) {
        vis[x] = true;
        for (auto &y : edge[x]) {
            if (!vis[y]) {
                dfs(y, edge, vis);
            }
        }
    }

    int removeStones(vector<vector<int>> &stones) {
        int n = stones.size();
        vector<vector<int>> edge(n);
        unordered_map<int, vector<int>> rec;
        for (int i = 0; i < n; i++) {
            rec[stones[i][0]].push_back(i);
            rec[stones[i][1] + 10001].push_back(i);
        }
        for (auto &[_, vec] : rec) {
            int k = vec.size();
            for (int i = 1; i < k; i++) {
                edge[vec[i - 1]].push_back(vec[i]);
                edge[vec[i]].push_back(vec[i - 1]);
            }
        }

        vector<int> vis(n);
        int num = 0;
        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                num++;
                dfs(i, edge, vis);
            }
        }
        return n - num;
    }
};

// 方法三：优化建图 + 并查集
class DisjointSetUnion {
private:
    unordered_map<int, int> f, rank;

public:
    int find(int x) {
        if (!f.count(x)) {
            f[x] = x;
            rank[x] = 1;
        }
        return f[x] == x ? x : f[x] = find(f[x]);
    }

    void unionSet(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) {
            return;
        }
        if (rank[fx] < rank[fy]) {
            swap(fx, fy);
        }
        rank[fx] += rank[fy];
        f[fy] = fx;
    }

    int numberOfConnectedComponent() {
        int num = 0;
        for (auto &[x, fa] : f) {
            if (x == fa) {
                num++;
            }
        }
        return num;
    }
};

class Solution {
public:
    int removeStones(vector<vector<int>> &stones) {
        int n = stones.size();
        DisjointSetUnion dsu;
        for (int i = 0; i < n; i++) {
            dsu.unionSet(stones[i][0], stones[i][1] + 10001);
        }

        return n - dsu.numberOfConnectedComponent();
    }
};
```

Java版本

```java
// 方法：并查集
import java.util.HashMap;
import java.util.Map;

public class Solution {

    public int removeStones(int[][] stones) {
        UnionFind unionFind = new UnionFind();

        for (int[] stone : stones) {
            // 下面这三种写法任选其一
            // unionFind.union(~stone[0], stone[1]);
            // unionFind.union(stone[0] - 10001, stone[1]);
            unionFind.union(stone[0] + 10001, stone[1]);
        }
        return stones.length - unionFind.getCount();
    }

    private class UnionFind {

        private Map<Integer, Integer> parent;
        private int count;

        public UnionFind() {
            this.parent = new HashMap<>();
            this.count = 0;
        }

        public int getCount() {
            return count;
        }

        public int find(int x) {
            if (!parent.containsKey(x)) {
                parent.put(x, x);
                // 并查集集中新加入一个结点，结点的父亲结点是它自己，所以连通分量的总数 +1
                count++;
            }

            if (x != parent.get(x)) {
                parent.put(x, find(parent.get(x)));
            }
            return parent.get(x);
        }

        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return;
            }

            parent.put(rootX, rootY);
            // 两个连通分量合并成为一个，连通分量的总数 -1
            count--;
        }
    }
}

// 方法一：深度优先搜索
class Solution {
    public int removeStones(int[][] stones) {
        int n = stones.length;
        List<List<Integer>> edge = new ArrayList<List<Integer>>();
        for (int i = 0; i < n; i++) {
            edge.add(new ArrayList<Integer>());
            for (int j = 0; j < n; j++) {
                if (stones[i][0] == stones[j][0] || stones[i][1] == stones[j][1]) {
                    edge.get(i).add(j);
                }
            }
        }
        boolean[] vis = new boolean[n];
        int num = 0;
        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                num++;
                dfs(i, edge, vis);
            }
        }
        return n - num;
    }

    public void dfs(int x, List<List<Integer>> edge, boolean[] vis) {
        vis[x] = true;
        for (int y : edge.get(x)) {
            if (!vis[y]) {
                dfs(y, edge, vis);
            }
        }
    }
}

// 方法二：优化建图 + 深度优先搜索
class Solution {
    public int removeStones(int[][] stones) {
        int n = stones.length;
        List<List<Integer>> edge = new ArrayList<List<Integer>>();
        for (int i = 0; i < n; i++) {
            edge.add(new ArrayList<Integer>());
        }

        Map<Integer, List<Integer>> rec = new HashMap<Integer, List<Integer>>();
        for (int i = 0; i < n; i++) {
            if (!rec.containsKey(stones[i][0])) {
                rec.put(stones[i][0], new ArrayList<Integer>());
            }
            rec.get(stones[i][0]).add(i);
            if (!rec.containsKey(stones[i][1] + 10001)) {
                rec.put(stones[i][1] + 10001, new ArrayList<Integer>());
            }
            rec.get(stones[i][1] + 10001).add(i);
        }
        for (Map.Entry<Integer, List<Integer>> entry : rec.entrySet()) {
            List<Integer> list = entry.getValue();
            int k = list.size();
            for (int i = 1; i < k; i++) {
                edge.get(list.get(i - 1)).add(list.get(i));
                edge.get(list.get(i)).add(list.get(i - 1));
            }
        }

        boolean[] vis = new boolean[n];
        int num = 0;
        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                num++;
                dfs(i, edge, vis);
            }
        }
        return n - num;
    }

    public void dfs(int x, List<List<Integer>> edge, boolean[] vis) {
        vis[x] = true;
        for (int y : edge.get(x)) {
            if (!vis[y]) {
                dfs(y, edge, vis);
            }
        }
    }
}

// 方法三：优化建图 + 并查集
class Solution {
    public int removeStones(int[][] stones) {
        int n = stones.length;
        DisjointSetUnion dsu = new DisjointSetUnion();
        for (int i = 0; i < n; i++) {
            dsu.unionSet(stones[i][0], stones[i][1] + 10001);
        }

        return n - dsu.numberOfConnectedComponent();
    }
}

class DisjointSetUnion {
    int[] f;
    int[] rank;

    public DisjointSetUnion() {
        f = new int[20001];
        rank = new int[20001];
        Arrays.fill(f, -1);
        Arrays.fill(rank, -1);
    }

    public int find(int x) {
        if (f[x] < 0) {
            f[x] = x;
            rank[x] = 1;
        }
        return f[x] == x ? x : (f[x] = find(f[x]));
    }

    public void unionSet(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) {
            return;
        }
        if (rank[fx] < rank[fy]) {
            int temp = fx;
            fx = fy;
            fy = temp;
        }
        rank[fx] += rank[fy];
        f[fy] = fx;
    }

    public int numberOfConnectedComponent() {
        int num = 0;
        for (int i = 0; i < 20000; i++) {
            if (f[i] == i) {
                num++;
            }
        }
        return num;
    }
}
```



### [803. 打砖块](https://leetcode.cn/problems/bricks-falling-when-hit/)

困难

有一个 `m x n` 的二元网格 `grid` ，其中 `1` 表示砖块，`0` 表示空白。砖块 **稳定**（不会掉落）的前提是：

- 一块砖直接连接到网格的顶部，或者
- 至少有一块相邻（4 个方向之一）砖块 **稳定** 不会掉落时

给你一个数组 `hits` ，这是需要依次消除砖块的位置。每当消除 `hits[i] = (rowi, coli)` 位置上的砖块时，对应位置的砖块（若存在）会消失，然后其他的砖块可能因为这一消除操作而 **掉落** 。一旦砖块掉落，它会 **立即** 从网格 `grid` 中消失（即，它不会落在其他稳定的砖块上）。

返回一个数组 `result` ，其中 `result[i]` 表示第 `i` 次消除操作对应掉落的砖块数目。

**注意**，消除可能指向是没有砖块的空白位置，如果发生这种情况，则没有砖块掉落。

**示例 1：**

```
输入：grid = [[1,0,0,0],[1,1,1,0]], hits = [[1,0]]
输出：[2]
解释：网格开始为：
[[1,0,0,0]，
 [1,1,1,0]]
消除 (1,0) 处加粗的砖块，得到网格：
[[1,0,0,0]
 [0,1,1,0]]
两个加粗的砖不再稳定，因为它们不再与顶部相连，也不再与另一个稳定的砖相邻，因此它们将掉落。得到网格：
[[1,0,0,0],
 [0,0,0,0]]
因此，结果为 [2] 。
```

C++版本

```c++
#include <vector>
#include <unordered_map>
#include <utility>
#include <algorithm>

using namespace std;

class Solution {
private:
    int rows;
    int cols;
    
    // 定义常量数组
    static constexpr int DIRECTIONS[4][2] = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};
    
    // 判断坐标是否越界
    bool inArea(int x, int y) {
        return x >= 0 && x < rows && y >= 0 && y < cols;
    }

    // 将二维坐标转换为一维坐标
    int getIndex(int x, int y) {
        return x * cols + y;
    }

    // 并查集类
    class UnionFind {
    private:
        vector<int> parent;
        vector<int> size;
    
    public:
        // 构造函数
        UnionFind(int n) : parent(n), size(n, 1) {
            // 初始化并查集
            for (int i = 0; i < n; ++i) {
                parent[i] = i;
            }
        }

        // 查找根节点
        int find(int x) {
            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }

        // 合并两个节点
        void union_(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);

            if (rootX != rootY) {
                parent[rootX] = rootY;
                size[rootY] += size[rootX];
            }
        }

        // 获取当前节点的子树节点总数
        int getSize(int x) {
            return size[find(x)];
        }
    };

public:
    vector<int> hitBricks(vector<vector<int>>& grid, vector<vector<int>>& hits) {
        // 获取行数和列数
        rows = grid.size();
        cols = grid[0].size();

        // 复制grid到copy
        vector<vector<int>> copy = grid;

        // 根据hits，把copy中对应坐标置为0
        for (auto& hit : hits) {
            copy[hit[0]][hit[1]] = 0;
        }

        // 建图，并将砖块和屋顶相连
        int size = rows * cols;
        UnionFind unionFind(size + 1);

        for (int j = 0; j < cols; ++j) {
            if (copy[0][j] == 1) {
                unionFind.union_(j, size);
            }
        }

        // 遍历copy，将砖块的相邻砖块合并到一起
        for (int i = 1; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (copy[i][j] == 1) {
                    if (copy[i - 1][j] == 1) {
                        unionFind.union_(getIndex(i - 1, j), getIndex(i, j));
                    }
                    if (j > 0 && copy[i][j - 1] == 1) {
                        unionFind.union_(getIndex(i, j - 1), getIndex(i, j));
                    }
                }
            }
        }

        // 逆序处理hits
        int hitsLen = hits.size();
        vector<int> res(hitsLen);
        for (int i = hitsLen - 1; i >= 0; --i) {
            int x = hits[i][0];
            int y = hits[i][1];

            if (grid[x][y] == 0) {
                continue;
            }

            int origin = unionFind.getSize(size);

            if (x == 0) {
                unionFind.union_(y, size);
            }

            for (auto& direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                if (inArea(newX, newY) && copy[newX][newY] == 1) {
                    unionFind.union_(getIndex(x, y), getIndex(newX, newY));
                }
            }

            int current = unionFind.getSize(size);
            res[i] = max(0, current - origin - 1);

            copy[x][y] = 1;
        }
        return res;
    }
};
```

Java版本

```java
// 并查集
public class Solution {

    private int rows;
    private int cols;

    public static final int[][] DIRECTIONS = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};

    public int[] hitBricks(int[][] grid, int[][] hits) {
        this.rows = grid.length;
        this.cols = grid[0].length;

        // 第 1 步：把 grid 中的砖头全部击碎，通常算法问题不能修改输入数据，这一步非必需，可以认为是一种答题规范
        int[][] copy = new int[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                copy[i][j] = grid[i][j];
            }
        }

        // 把 copy 中的砖头全部击碎
        for (int[] hit : hits) {
            copy[hit[0]][hit[1]] = 0;
        }

        // 第 2 步：建图，把砖块和砖块的连接关系输入并查集，size 表示二维网格的大小，也表示虚拟的「屋顶」在并查集中的编号
        int size = rows * cols;
        UnionFind unionFind = new UnionFind(size + 1);

        // 将下标为 0 的这一行的砖块与「屋顶」相连
        for (int j = 0; j < cols; j++) {
            if (copy[0][j] == 1) {
                unionFind.union(j, size);
            }
        }

        // 其余网格，如果是砖块向上、向左看一下，如果也是砖块，在并查集中进行合并
        for (int i = 1; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (copy[i][j] == 1) {
                    // 如果上方也是砖块
                    if (copy[i - 1][j] == 1) {
                        unionFind.union(getIndex(i - 1, j), getIndex(i, j));
                    }
                    // 如果左边也是砖块
                    if (j > 0 && copy[i][j - 1] == 1) {
                        unionFind.union(getIndex(i, j - 1), getIndex(i, j));
                    }
                }
            }
        }

        // 第 3 步：按照 hits 的逆序，在 copy 中补回砖块，把每一次因为补回砖块而与屋顶相连的砖块的增量记录到 res 数组中
        int hitsLen = hits.length;
        int[] res = new int[hitsLen];
        for (int i = hitsLen - 1; i >= 0; i--) {
            int x = hits[i][0];
            int y = hits[i][1];

            // 注意：这里不能用 copy，语义上表示，如果原来在 grid 中，这一块是空白，这一步不会产生任何砖块掉落
            // 逆向补回的时候，与屋顶相连的砖块数量也肯定不会增加
            if (grid[x][y] == 0) {
                continue;
            }

            // 补回之前与屋顶相连的砖块数
            int origin = unionFind.getSize(size);

            // 注意：如果补回的这个结点在第 1 行，要告诉并查集它与屋顶相连（逻辑同第 2 步）
            if (x == 0) {
                unionFind.union(y, size);
            }

            // 在 4 个方向上看一下，如果相邻的 4 个方向有砖块，合并它们
            for (int[] direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                if (inArea(newX, newY) && copy[newX][newY] == 1) {
                    unionFind.union(getIndex(x, y), getIndex(newX, newY));
                }
            }

            // 补回之后与屋顶相连的砖块数
            int current = unionFind.getSize(size);
            // 减去的 1 是逆向补回的砖块（正向移除的砖块），与 0 比较大小，是因为存在一种情况，添加当前砖块，不会使得与屋顶连接的砖块数更多
            res[i] = Math.max(0, current - origin - 1);

            // 真正补上这个砖块
            copy[x][y] = 1;
        }
        return res;
    }

    /**
     * 输入坐标在二维网格中是否越界
     *
     * @param x
     * @param y
     * @return
     */
    private boolean inArea(int x, int y) {
        return x >= 0 && x < rows && y >= 0 && y < cols;
    }

    /**
     * 二维坐标转换为一维坐标
     *
     * @param x
     * @param y
     * @return
     */
    private int getIndex(int x, int y) {
        return x * cols + y;
    }

    private class UnionFind {

        /**
         * 当前结点的父亲结点
         */
        private int[] parent;
        /**
         * 以当前结点为根结点的子树的结点总数
         */
        private int[] size;

        public UnionFind(int n) {
            parent = new int[n];
            size = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
        }

        /**
         * 路径压缩，只要求每个不相交集合的「根结点」的子树包含的结点总数数值正确即可，因此在路径压缩的过程中不用维护数组 size
         *
         * @param x
         * @return
         */
        public int find(int x) {
            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }

        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);

            if (rootX == rootY) {
                return;
            }
            parent[rootX] = rootY;
            // 在合并的时候维护数组 size
            size[rootY] += size[rootX];
        }

        /**
         * @param x
         * @return x 在并查集的根结点的子树包含的结点总数
         */
        public int getSize(int x) {
            int root = find(x);
            return size[root];
        }
    }
}
```



### [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

中等

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

C++版本

```c++
// 方法一：哈希表(并查集思路)
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        // 使用 unordered_set 存储数组中的所有数字
        unordered_set<int> numSet;
        for (int num : nums) {
            numSet.insert(num);
        }

        // 初始化最长连续序列的长度为 0
        int longestStreak = 0;

        // 遍历 unordered_set 中的每个数字
        for (int num : numSet) {
            // 如果 unordered_set 中不包含当前数字的前一个数字，则当前数字是连续序列的起点
            if (!numSet.count(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                // 依次检查当前数字的下一个数字是否在 unordered_set 中，直到不在集合中或者连续序列结束
                while (numSet.count(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                // 更新最长连续序列的长度
                longestStreak = max(longestStreak, currentStreak);
            }
        }

        // 返回最长连续序列的长度
        return longestStreak;
    }
};
```

Java版本

```java
// 方法一：哈希表(并查集思路)
public class Solution {
    public int longestConsecutive(int[] nums) {
        // 使用哈希集合存储数组中的所有数字
        HashSet<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }

        // 初始化最长连续序列的长度为 0
        int longestStreak = 0;

        // 遍历哈希集合中的每个数字
        for (int num : numSet) {
            // 如果哈希集合中不包含当前数字的前一个数字，则当前数字是连续序列的起点
            if (!numSet.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                // 依次检查当前数字的下一个数字是否在哈希集合中，直到不在集合中或者连续序列结束
                while (numSet.contains(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                // 更新最长连续序列的长度
                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        // 返回最长连续序列的长度
        return longestStreak;
    }
}
```



