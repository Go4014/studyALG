# 图的生成树

## 最小生成树

### [1584. 连接所有点的最小费用](https://leetcode.cn/problems/min-cost-to-connect-all-points/)

中等

给你一个`points` 数组，表示 2D 平面上的一些点，其中 `points[i] = [xi, yi]` 。

连接点 `[xi, yi]` 和点 `[xj, yj]` 的费用为它们之间的 **曼哈顿距离** ：`|xi - xj| + |yi - yj|` ，其中 `|val|` 表示 `val` 的绝对值。

请你返回将所有点连接的最小总费用。只有任意两点之间 **有且仅有** 一条简单路径时，才认为所有点都已连接。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/08/26/d.png)

```
输入：points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
输出：20
解释：

我们可以按照上图所示连接所有点得到最小总费用，总费用为 20 。
注意到任意两个点之间只有唯一一条路径互相到达。
```

C++版本

```c++
// 方法一：Kruskal 算法
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

    int unionSet(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) {
            return false;
        }
        if (rank[fx] < rank[fy]) {
            swap(fx, fy);
        }
        rank[fx] += rank[fy];
        f[fy] = fx;
        return true;
    }
};

struct Edge {
    int len, x, y;
    Edge(int len, int x, int y) : len(len), x(x), y(y) {
    }
};

class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        auto dist = [&](int x, int y) -> int {
            return abs(points[x][0] - points[y][0]) + abs(points[x][1] - points[y][1]);
        };
        int n = points.size();
        DisjointSetUnion dsu(n);
        vector<Edge> edges;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                edges.emplace_back(dist(i, j), i, j);
            }
        }
        sort(edges.begin(), edges.end(), [](Edge a, Edge b) -> int { return a.len < b.len; });
        int ret = 0, num = 1;
        for (auto& [len, x, y] : edges) {
            if (dsu.unionSet(x, y)) {
                ret += len;
                num++;
                if (num == n) {
                    break;
                }
            }
        }
        return ret;
    }
};

// 方法二：建图优化的 Kruskal
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

    int unionSet(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) {
            return false;
        }
        if (rank[fx] < rank[fy]) {
            swap(fx, fy);
        }
        rank[fx] += rank[fy];
        f[fy] = fx;
        return true;
    }
};

class BIT {
public:
    vector<int> tree, idRec;
    int n;

    BIT(int _n) {
        n = _n;
        tree.resize(n, INT_MAX);
        idRec.resize(n, -1);
    }

    int lowbit(int k) {
        return k & (-k);
    }

    void update(int pos, int val, int id) {
        while (pos > 0) {
            if (tree[pos] > val) {
                tree[pos] = val;
                idRec[pos] = id;
            }
            pos -= lowbit(pos);
        }
    }

    int query(int pos) {
        int minval = INT_MAX;
        int j = -1;
        while (pos < n) {
            if (minval > tree[pos]) {
                minval = tree[pos];
                j = idRec[pos];
            }
            pos += lowbit(pos);
        }
        return j;
    }
};

struct Edge {
    int len, x, y;
    Edge(int len, int x, int y) : len(len), x(x), y(y) {
    }
    bool operator<(const Edge& a) const {
        return len < a.len;
    }
};

struct Pos {
    int id, x, y;
    bool operator<(const Pos& a) const {
        return x == a.x ? y < a.y : x < a.x;
    }
};

class Solution {
public:
    vector<Edge> edges;
    vector<Pos> pos;

    void build(int n) {
        sort(pos.begin(), pos.end());
        vector<int> a(n), b(n);
        for (int i = 0; i < n; i++) {
            a[i] = pos[i].y - pos[i].x;
            b[i] = pos[i].y - pos[i].x;
        }
        sort(b.begin(), b.end());
        b.erase(unique(b.begin(), b.end()), b.end());
        int num = b.size();
        BIT bit(num + 1);
        for (int i = n - 1; i >= 0; i--) {
            int poss = lower_bound(b.begin(), b.end(), a[i]) - b.begin() + 1;
            int j = bit.query(poss);
            if (j != -1) {
                int dis = abs(pos[i].x - pos[j].x) + abs(pos[i].y - pos[j].y);
                edges.emplace_back(dis, pos[i].id, pos[j].id);
            }
            bit.update(poss, pos[i].x + pos[i].y, i);
        }
    }

    void solve(vector<vector<int>>& points, int n) {
        pos.resize(n);
        for (int i = 0; i < n; i++) {
            pos[i].x = points[i][0];
            pos[i].y = points[i][1];
            pos[i].id = i;
        }
        build(n);
        for (int i = 0; i < n; i++) {
            swap(pos[i].x, pos[i].y);
        }
        build(n);
        for (int i = 0; i < n; i++) {
            pos[i].x = -pos[i].x;
        }
        build(n);
        for (int i = 0; i < n; i++) {
            swap(pos[i].x, pos[i].y);
        }
        build(n);
    }

    int minCostConnectPoints(vector<vector<int>>& points) {
        int n = points.size();
        solve(points, n);

        DisjointSetUnion dsu(n);
        sort(edges.begin(), edges.end());
        int ret = 0, num = 1;
        for (auto& [len, x, y] : edges) {
            if (dsu.unionSet(x, y)) {
                ret += len;
                num++;
                if (num == n) {
                    break;
                }
            }
        }
        return ret;
    }
};
```

Java版本

```java
// 方法一：Kruskal 算法
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        DisjointSetUnion dsu = new DisjointSetUnion(n);
        List<Edge> edges = new ArrayList<Edge>();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                edges.add(new Edge(dist(points, i, j), i, j));
            }
        }
        Collections.sort(edges, new Comparator<Edge>() {
            public int compare(Edge edge1, Edge edge2) {
                return edge1.len - edge2.len;
            }
        });
        int ret = 0, num = 1;
        for (Edge edge : edges) {
            int len = edge.len, x = edge.x, y = edge.y;
            if (dsu.unionSet(x, y)) {
                ret += len;
                num++;
                if (num == n) {
                    break;
                }
            }
        }
        return ret;
    }

    public int dist(int[][] points, int x, int y) {
        return Math.abs(points[x][0] - points[y][0]) + Math.abs(points[x][1] - points[y][1]);
    }
}

class DisjointSetUnion {
    int[] f;
    int[] rank;
    int n;

    public DisjointSetUnion(int n) {
        this.n = n;
        this.rank = new int[n];
        Arrays.fill(this.rank, 1);
        this.f = new int[n];
        for (int i = 0; i < n; i++) {
            this.f[i] = i;
        }
    }

    public int find(int x) {
        return f[x] == x ? x : (f[x] = find(f[x]));
    }

    public boolean unionSet(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) {
            return false;
        }
        if (rank[fx] < rank[fy]) {
            int temp = fx;
            fx = fy;
            fy = temp;
        }
        rank[fx] += rank[fy];
        f[fy] = fx;
        return true;
    }
}

class Edge {
    int len, x, y;

    public Edge(int len, int x, int y) {
        this.len = len;
        this.x = x;
        this.y = y;
    }
}

// 方法二：建图优化的 Kruskal
class Solution {
    List<Edge> edges = new ArrayList<Edge>();
    Pos[] pos;

    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        solve(points, n);

        DisjointSetUnion dsu = new DisjointSetUnion(n);
        Collections.sort(edges, new Comparator<Edge>() {
            public int compare(Edge edge1, Edge edge2) {
                return edge1.len - edge2.len;
            }
        });
        int ret = 0, num = 1;
        for (Edge edge : edges) {
            int len = edge.len, x = edge.x, y = edge.y;
            if (dsu.unionSet(x, y)) {
                ret += len;
                num++;
                if (num == n) {
                    break;
                }
            }
        }
        return ret;
    }

    public void solve(int[][] points, int n) {
        pos = new Pos[n];
        for (int i = 0; i < n; i++) {
            pos[i] = new Pos(i, points[i][0], points[i][1]);
        }
        build(n);
        for (int i = 0; i < n; i++) {
            int temp = pos[i].x;
            pos[i].x = pos[i].y;
            pos[i].y = temp;
        }
        build(n);
        for (int i = 0; i < n; i++) {
            pos[i].x = -pos[i].x;
        }
        build(n);
        for (int i = 0; i < n; i++) {
            int temp = pos[i].x;
            pos[i].x = pos[i].y;
            pos[i].y = temp;
        }
        build(n);
    }

    public void build(int n) {
        Arrays.sort(pos, new Comparator<Pos>() {
            public int compare(Pos pos1, Pos pos2) {
                return pos1.x == pos2.x ? pos1.y - pos2.y : pos1.x - pos2.x;
            }
        });
        int[] a = new int[n];
        Set<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < n; i++) {
            a[i] = pos[i].y - pos[i].x;
            set.add(pos[i].y - pos[i].x);
        }
        int num = set.size();
        int[] b = new int[num];
        int index = 0;
        for (int element : set) {
            b[index++] = element;
        }
        Arrays.sort(b);
        BIT bit = new BIT(num + 1);
        for (int i = n - 1; i >= 0; i--) {
            int poss = binarySearch(b, a[i]) + 1;
            int j = bit.query(poss);
            if (j != -1) {
                int dis = Math.abs(pos[i].x - pos[j].x) + Math.abs(pos[i].y - pos[j].y);
                edges.add(new Edge(dis, pos[i].id, pos[j].id));
            }
            bit.update(poss, pos[i].x + pos[i].y, i);
        }
    }

    public int binarySearch(int[] array, int target) {
        int low = 0, high = array.length - 1;
        while (low < high) {
            int mid = (high - low) / 2 + low;
            int num = array[mid];
            if (num < target) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}

class DisjointSetUnion {
    int[] f;
    int[] rank;
    int n;

    public DisjointSetUnion(int n) {
        this.n = n;
        this.rank = new int[n];
        Arrays.fill(this.rank, 1);
        this.f = new int[n];
        for (int i = 0; i < n; i++) {
            this.f[i] = i;
        }
    }

    public int find(int x) {
        return f[x] == x ? x : (f[x] = find(f[x]));
    }

    public boolean unionSet(int x, int y) {
        int fx = find(x), fy = find(y);
        if (fx == fy) {
            return false;
        }
        if (rank[fx] < rank[fy]) {
            int temp = fx;
            fx = fy;
            fy = temp;
        }
        rank[fx] += rank[fy];
        f[fy] = fx;
        return true;
    }
}

class BIT {
    int[] tree;
    int[] idRec;
    int n;

    public BIT(int n) {
        this.n = n;
        this.tree = new int[n];
        Arrays.fill(this.tree, Integer.MAX_VALUE);
        this.idRec = new int[n];
        Arrays.fill(this.idRec, -1);
    }

    public int lowbit(int k) {
        return k & (-k);
    }

    public void update(int pos, int val, int id) {
        while (pos > 0) {
            if (tree[pos] > val) {
                tree[pos] = val;
                idRec[pos] = id;
            }
            pos -= lowbit(pos);
        }
    }

    public int query(int pos) {
        int minval = Integer.MAX_VALUE;
        int j = -1;
        while (pos < n) {
            if (minval > tree[pos]) {
                minval = tree[pos];
                j = idRec[pos];
            }
            pos += lowbit(pos);
        }
        return j;
    }
}

class Edge {
    int len, x, y;

    public Edge(int len, int x, int y) {
        this.len = len;
        this.x = x;
        this.y = y;
    }
}

class Pos {
    int id, x, y;

    public Pos(int id, int x, int y) {
        this.id = id;
        this.x = x;
        this.y = y;
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
    static const vector<vector<int>> DIRECTIONS;

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
const vector<vector<int>> Solution::DIRECTIONS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

// 方法二：并查集
class Solution {
private:
    int N;
    static const vector<vector<int>> DIRECTIONS;

    int getIndex(int x, int y) {
        return x * N + y;
    }

    bool inArea(int x, int y) {
        return x >= 0 && x < N && y >= 0 && y < N;
    }

    class UnionFind {
    private:
        vector<int> parent;

    public:
        UnionFind(int n) : parent(n) {
            for (int i = 0; i < n; ++i) {
                parent[i] = i;
            }
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

        void Union(int p, int q) {
            int rootP = root(p);
            int rootQ = root(q);
            if (rootP != rootQ) {
                parent[rootP] = rootQ;
            }
        }
    };

public:
    int swimInWater(vector<vector<int>>& grid) {
        N = grid.size();
        int len = N * N;
        vector<int> index(len);
        for (int i = 0; i < N; ++i) {
            for (int j = 0; j < N; ++j) {
                index[grid[i][j]] = getIndex(i, j);
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
                    uf.Union(index[i], getIndex(newX, newY));
                }

                if (uf.isConnected(0, len - 1)) {
                    return i;
                }
            }
        }
        return -1;
    }
};
const vector<vector<int>> Solution::DIRECTIONS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

// 方法三：Dijkstra 算法
class Solution {
public:
    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();
        priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>> minHeap;
        minHeap.push({grid[0][0], 0, 0});

        vector<vector<bool>> visited(n, vector<bool>(n, false));
        vector<vector<int>> distTo(n, vector<int>(n, n * n));
        distTo[0][0] = grid[0][0];

        const vector<vector<int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        while (!minHeap.empty()) {
            auto front = minHeap.top();
            minHeap.pop();
            int currentX = front[1];
            int currentY = front[2];
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
                    minHeap.push({distTo[newX][newY], newX, newY});
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

// 方法一：堆（优先队列）
// 优先队列中的数据结构。其中 (i,j) 代表坐标，val 代表水位。
struct Entry {
    int i;
    int j;
    int val;
    bool operator<(const Entry& other) const {
        return this->val > other.val;
    }
    Entry(int ii, int jj, int val): i(ii), j(jj), val(val) {}
};

class Solution {
public:
    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();
        priority_queue<Entry, vector<Entry>, function<bool(const Entry& x, const Entry& other)>> pq(&Entry::operator<);
        vector<vector<int>> visited(n, vector<int>(n, 0));

        pq.push(Entry(0, 0, grid[0][0]));
        int ret = 0;
        vector<pair<int, int>> directions{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        while (!pq.empty()) {
            Entry x = pq.top();
            pq.pop();
            if (visited[x.i][x.j] == 1) {
                continue;
            }
            
            visited[x.i][x.j] = 1;
            ret = max(ret, grid[x.i][x.j]);
            if (x.i == n - 1 && x.j == n - 1) {
                break;
            }

            for (const auto [di, dj]: directions) {
                int ni = x.i + di, nj = x.j + dj;
                if (ni >= 0 && ni < n && nj >= 0 && nj < n) {
                    if (visited[ni][nj] == 0) {
                        pq.push(Entry(ni, nj, grid[ni][nj]));
                    }
                }
            }
        }
        return ret;
    }
};

// 二分查找
class Solution {
public:
    bool check(vector<vector<int>>& grid, int threshold) {
        if (grid[0][0] > threshold) {
            return false;
        }
        int n = grid.size();
        vector<vector<int>> visited(n, vector<int>(n, 0));
        visited[0][0] = 1;
        queue<pair<int, int>> q;
        q.push(make_pair(0, 0));

        vector<pair<int, int>> directions{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        while (!q.empty()) {
            auto [i, j] = q.front();
            q.pop();

            for (const auto [di, dj]: directions) {
                int ni = i + di, nj = j + dj;
                if (ni >= 0 && ni < n && nj >= 0 && nj < n) {
                    if (visited[ni][nj] == 0 && grid[ni][nj] <= threshold) {
                        q.push(make_pair(ni, nj));
                        visited[ni][nj] = 1;
                    }
                }
            }
        }
        return visited[n - 1][n - 1] == 1;
    }

    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();
        int left = 0, right = n * n - 1;
        while (left < right) {
            int mid = (left + right) / 2;
            if (check(grid, mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        } 
        return left;
    }
};

// 方法三：并查集
class Solution {
public:
    int find(vector<int>& f, int x) {
        if (f[x] == x) {
            return x;
        }
        int fa = find(f, f[x]);
        f[x] = fa;
        return fa;
    }

    void merge(vector<int>& f, int x, int y) {
        int fx = find(f, x), fy = find(f, y);
        f[fx] = fy;
    }

    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<int> f(n * n);
        for (int i = 0; i < n * n; i++) {
            f[i] = i;
        }

        vector<pair<int, int>> idx(n * n); // 存储每个平台高度对应的位置
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                idx[grid[i][j]] = make_pair(i, j);
            }
        }

        vector<pair<int, int>> directions{{0,1},{0,-1},{1,0},{-1,0}};
        for (int threshold = 0; threshold < n * n; threshold++) {
            auto [i, j] = idx[threshold];
            for (const auto [di, dj]: directions) {
                int ni = i + di, nj = j + dj;
                if (ni >= 0 && ni < n && nj >= 0 && nj < n && grid[ni][nj] <= threshold) {
                    merge(f, i * n + j, ni * n + nj);
                }
            }
            if (find(f, 0) == find(f, n * n - 1)) {
                return threshold;
            }
        }
        return -1; // cannot happen
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
import java.util.Arrays;
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;

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

// 方法一：堆（优先队列）
class Solution {
    public int swimInWater(int[][] grid) {
        int n = grid.length;
        PriorityQueue<Entry> pq = new PriorityQueue<Entry>(new Comparator<Entry>() {
            public int compare(Entry entry1, Entry entry2) {
                return entry1.val - entry2.val;
            }
        });
        boolean[][] visited = new boolean[n][n];

        pq.offer(new Entry(0, 0, grid[0][0]));
        int ret = 0;
        int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        while (!pq.isEmpty()) {
            Entry x = pq.poll();
            if (visited[x.i][x.j]) {
                continue;
            }
            
            visited[x.i][x.j] = true;
            ret = Math.max(ret, grid[x.i][x.j]);
            if (x.i == n - 1 && x.j == n - 1) {
                break;
            }

            for (int[] direction : directions) {
                int ni = x.i + direction[0], nj = x.j + direction[1];
                if (ni >= 0 && ni < n && nj >= 0 && nj < n) {
                    if (!visited[ni][nj]) {
                        pq.offer(new Entry(ni, nj, grid[ni][nj]));
                    }
                }
            }
        }
        return ret;
    }
}

// 优先队列中的数据结构。其中 (i,j) 代表坐标，val 代表水位。
class Entry {
    int i;
    int j;
    int val;

    public Entry(int i, int j, int val) {
        this.i = i;
        this.j = j;
        this.val = val;
    }
};

// 方法二：二分查找
class Solution {
    public int swimInWater(int[][] grid) {
        int n = grid.length;
        int left = 0, right = n * n - 1;
        while (left < right) {
            int mid = (left + right) / 2;
            if (check(grid, mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        } 
        return left;
    }

    public boolean check(int[][] grid, int threshold) {
        if (grid[0][0] > threshold) {
            return false;
        }
        int n = grid.length;
        boolean[][] visited = new boolean[n][n];
        visited[0][0] = true;
        Queue<int[]> queue = new LinkedList<int[]>();
        queue.offer(new int[]{0, 0});

        int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        while (!queue.isEmpty()) {
            int[] square = queue.poll();
            int i = square[0], j = square[1];

            for (int[] direction : directions) {
                int ni = i + direction[0], nj = j + direction[1];
                if (ni >= 0 && ni < n && nj >= 0 && nj < n) {
                    if (!visited[ni][nj] && grid[ni][nj] <= threshold) {
                        queue.offer(new int[]{ni, nj});
                        visited[ni][nj] = true;
                    }
                }
            }
        }
        return visited[n - 1][n - 1];
    }
}

// 方法三：并查集
class Solution {
    public int swimInWater(int[][] grid) {
        int n = grid.length;
        int[] f = new int[n * n];
        for (int i = 0; i < n * n; i++) {
            f[i] = i;
        }

        int[][] idx = new int[n * n][2]; // 存储每个平台高度对应的位置
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                idx[grid[i][j]][0] = i;
                idx[grid[i][j]][1] = j;
            }
        }

        int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for (int threshold = 0; threshold < n * n; threshold++) {
            int i = idx[threshold][0], j = idx[threshold][1];
            for (int[] direction : directions) {
                int ni = i + direction[0], nj = j + direction[1];
                if (ni >= 0 && ni < n && nj >= 0 && nj < n && grid[ni][nj] <= threshold) {
                    merge(f, i * n + j, ni * n + nj);
                }
            }
            if (find(f, 0) == find(f, n * n - 1)) {
                return threshold;
            }
        }
        return -1; // cannot happen
    }

    public int find(int[] f, int x) {
        if (f[x] == x) {
            return x;
        }
        int fa = find(f, f[x]);
        f[x] = fa;
        return fa;
    }

    public void merge(int[] f, int x, int y) {
        int fx = find(f, x), fy = find(f, y);
        f[fx] = fy;
    }
}
```





