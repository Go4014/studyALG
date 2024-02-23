# 最小生成树

## 什么是最小生成树？

在图论中，最小生成树（Minimum Spanning Tree，MST）是一个连通图中的生成树，它包含图中所有顶点，并且具有最小的总权值。在最小生成树中，任意两点之间有且仅有一条路径，且这条路径的权值是最小的。

## Prim 算法 

从一个顶点开始，选择与当前树集合相邻且权值最小的边加入树中，直到所有顶点都被加入树中为止。Prim算法是一种贪心算法。

### 代码实现

C++版本

```c++
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

struct Edge {
    int from;
    int to;
    int weight;

    Edge(int from, int to, int weight) : from(from), to(to), weight(weight) {}

    friend bool operator<(const Edge& e1, const Edge& e2) {
        return e1.weight > e2.weight;
    }
};

vector<Edge> prim(vector<vector<Edge>>& graph) {
    int n = graph.size();
    vector<Edge> mst;
    vector<bool> visited(n, false);
    visited[0] = true; // 首先从“0”顶点作为起点开始遍历

    priority_queue<Edge> pq;
    for (Edge e : graph[0]) {
        pq.push(e); // 将起点的所有邻边添加进队列
    }

    while (!pq.empty()) {
        Edge e = pq.top(); // 将已选中顶点中最小权值的边弹出
        pq.pop();
        int u = e.from; // 起点
        int v = e.to; // 结点
        int w = e.weight; // 边的权值

        if (visited[u] && !visited[v]) { // 满足的条件：起点必须已加入最小生成树集合，结点若尚未加入该集合
            visited[v] = true; // 该顶点已加入最小生成树集合，设为已访问状态
            mst.push_back(e); // 将该边加入最小生成树集合
            for (Edge edge : graph[v]) {
                pq.push(edge); // 将该顶点的所有邻边添加进队列
            }
        }
    }
    return mst;
}
```

Java版本

```java
import java.util.*;

public class PrimAlgorithm {
    static class Edge {
        int from;
        int to;
        int weight;

        public Edge(int from, int to, int weight) {
            this.from = from;
            this.to = to;
            this.weight = weight;
        }

        @Override
        public String toString() {
            return "(" + from + ", " + to + ", " + weight + ")";
        }
    }
    
    public static List<Edge> prim(List<Edge>[] graph) {
        int n = graph.length;
        List<Edge> mst = new ArrayList<>();
        boolean[] visited = new boolean[n];
        visited[0] = true; // 首先从“0”顶点作为起点开始遍历

        PriorityQueue<Edge> pq = new PriorityQueue<>((e1, e2) -> e1.weight - e2.weight);

        for (Edge e : graph[0]) {
            pq.add(e); // 将起点的所有邻边添加进队列
        }

        while (!pq.isEmpty()) {
            Edge e = pq.poll(); // 将已选中顶点中最小权值的边弹出
            int u = e.from; // 起点
            int v = e.to; // 结点
            int w = e.weight; // 边的权值

            if (visited[u] && !visited[v]) { // 满足的条件：起点必须已加入最小生成树集合，结点若尚未加入该集合
                visited[v] = true; // 该顶点已加入最小生成树集合，设为已访问状态
                mst.add(e); // 将该边加入最小生成树集合
                for (Edge edge : graph[v]) {
                    pq.add(edge); // 将该顶点的所有邻边添加进队列
                }
            }
        }
        return mst;
    }
}
```



## Kruskal 算法

先将所有边按权值排序，然后依次选择权值最小的边，将其加入树中，如果这条边不会形成回路，就将其加入到最小生成树中，直到所有顶点都被加入树中为止。

### 代码实现

C++版本

```c++
#include <iostream>
#include <vector>
#include <algorithm>

class Kruskal {
    struct Edge {
        int from, to, weight;
        Edge(int from, int to, int weight) : from(from), to(to), weight(weight) {}
        bool operator<(const Edge& other) const {
            return weight < other.weight;
        }
    };

    int find(std::vector<int>& parent, int u) {
        if (parent[u] != u) {
            parent[u] = find(parent, parent[u]);
        }
        return parent[u];
    }

    void unite(std::vector<int>& parent, int u, int v) {
        parent[find(parent, v)] = find(parent, u);
    }

public:
    int kruskal(int n, std::vector<Edge>& edges) {
        std::sort(edges.begin(), edges.end()); // 将所有边进行顺序排序
        int ans = 0;
        std::vector<int> parent(n);
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
        for (Edge& edge : edges) {  // 每次去除权值最小的边
            int u = edge.from, v = edge.to, weight = edge.weight;
            if (find(parent, u) != find(parent, v)) { // 该边的两个顶点不在同一个集合，即加入后不会形成环路
                unite(parent, u, v);
                ans += weight;
            }
        }
        return ans;
    }
};
```

Java版本

```java
import java.util.Arrays;

public class Kruskal {
    private static class Edge implements Comparable<Edge> {
        int from, to, weight;
        public Edge(int from, int to, int weight) {
            this.from = from;
            this.to = to;
            this.weight = weight;
        }
        
        @Override
        public int compareTo(Edge o) {
            return this.weight - o.weight;
        }
    }

    private static int find(int[] parent, int u) {
        if (parent[u] != u) {
            parent[u] = find(parent, parent[u]);
        }
        return parent[u];
    }

    private static void union(int[] parent, int u, int v) {
        parent[find(parent, v)] = find(parent, u);
    }

    public static int kruskal(int n, Edge[] edges) {
        Arrays.sort(edges); // 将所有边进行顺序排序
        int ans = 0;
        int[] parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
        for (Edge edge : edges) { // 每次去除权值最小的边
            int u = edge.from, v = edge.to, weight = edge.weight;
            if (find(parent, u) != find(parent, v)) { // 该边的两个顶点不在同一个集合，即加入后不会形成环路
                union(parent, u, v);
                ans += weight;
            }
        }
        return ans;
    }
}
```



