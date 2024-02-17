# 拓扑排序

## 1、基于深度优先搜索实现

### 核心思路

首先从任意一个未访问的顶点开始，递归地遍历其所有未访问的邻居节点，直到遍历到没有未访问邻居节点为止，然后将该节点添加到一个结果列表中。接着从任意一个剩余未访问的顶点开始，重复这个过程，直到所有的节点都被访问到。

**基于深度优先搜索的拓扑排序算法的具体步骤：**

> 1. 初始化一个空的结果列表 `result`，一个空的集合 `visited` 用于记录已访问过的节点。
> 2. 从任意一个未访问的顶点 `u` 开始，调用一个递归的深度优先搜索函数 `dfs(u)`。
> 3. 在 `dfs(u)` 函数中，首先将节点 `u` 添加到 `visited` 中，然后遍历 `u` 的所有邻居节点 `v`，如果 `v` 没有被访问过，就调用 `dfs(v)`。
> 4. 当 `dfs(v)` 返回时，表示 `v` 的所有未访问的邻居节点都已被访问，因此将 `v` 添加到 `result` 中。
> 5. 重复步骤2-4，直到所有节点都被访问到。



### 代码实现

C++版本

```c++
#include <vector>
#include <unordered_map>
#include <unordered_set>

using namespace std;

class Solution {
public:
    // 拓扑排序，graph 中包含所有顶点的有向边关系（包括无边顶点）
    vector<int> topologicalSortingDFS(unordered_map<int, vector<int>>& graph) {
        unordered_set<int> visited;    // 记录当前顶点是否被访问过
        unordered_set<int> onStack;    // 记录同一次深搜时，当前顶点是否被访问过
        vector<int> order;             // 用于存储拓扑序列
        bool hasCycle = false;         // 用于判断是否存在环
        
        function<void(int)> dfs = [&](int u) {
            if (onStack.count(u)) {    // 同一次深度优先搜索时，当前顶点被访问过，说明存在环
                hasCycle = true;
            }
            if (visited.count(u) || hasCycle) {    // 当前节点被访问或者有环时直接返回
                return;
            }
            
            visited.insert(u);      // 标记节点被访问
            onStack.insert(u);      // 标记本次深搜时，当前顶点被访问
    
            for (int v : graph[u]) {    // 遍历顶点 u 的邻接顶点 v
                dfs(v);                 // 递归访问节点 v
            }
                    
            order.push_back(u);         // 后序遍历顺序访问节点 u
            onStack.erase(u);           // 取消本次深搜时的 顶点访问标记
        };
        
        for (auto& entry : graph) {
            int u = entry.first;
            if (!visited.count(u)) {
                dfs(u);    // 递归遍历未访问节点 u
            }
        }
        
        if (hasCycle) {    // 判断是否存在环
            return vector<int>();    // 存在环，无法构成拓扑序列
        }
        reverse(order.begin(), order.end());    // 将后序遍历转为拓扑排序顺序
        return order;                           // 返回拓扑序列
    }
    
    vector<int> findOrder(int n, vector<vector<int>>& edges) {
        // 构建图
        unordered_map<int, vector<int>> graph;
        for (int i = 0; i < n; ++i) {
            graph[i] = vector<int>();
        }
        for (auto& edge : edges) {
            graph[edge[1]].push_back(edge[0]);
        }
        
        return topologicalSortingDFS(graph);
    }
};
```

Java版本

```java
import java.util.*;

class Solution {
    // 拓扑排序，graph 中包含所有顶点的有向边关系（包括无边顶点）
    public List<Integer> topologicalSortingDFS(Map<Integer, List<Integer>> graph) {
        Set<Integer> visited = new HashSet<>(); // 记录当前顶点是否被访问过
        Set<Integer> onStack = new HashSet<>(); // 记录同一次深搜时，当前顶点是否被访问过
        List<Integer> order = new ArrayList<>(); // 用于存储拓扑序列
        boolean[] hasCycle = new boolean[]{false}; // 用于判断是否存在环
        
        // 定义深度优先搜索函数
        Stack<Integer> stack = new Stack<>();
        Consumer<Integer> dfs = u -> {
            if (onStack.contains(u)) { // 同一次深度优先搜索时，当前顶点被访问过，说明存在环
                hasCycle[0] = true;
            }
            if (visited.contains(u) || hasCycle[0]) { // 当前节点被访问或者有环时直接返回
                return;
            }
            
            visited.add(u); // 标记节点被访问
            onStack.add(u); // 标记本次深搜时，当前顶点被访问
    
            for (int v : graph.getOrDefault(u, new ArrayList<>())) { // 遍历顶点 u 的邻接顶点 v
                dfs.accept(v); // 递归访问节点 v
            }
                    
            order.add(u); // 后序遍历顺序访问节点 u
            onStack.remove(u); // 取消本次深搜时的 顶点访问标记
        };
        
        for (int u : graph.keySet()) {
            if (!visited.contains(u)) {
                dfs.accept(u); // 递归遍历未访问节点 u
            }
        }
        
        if (hasCycle[0]) { // 判断是否存在环
            return new ArrayList<>(); // 存在环，无法构成拓扑序列
        }
        Collections.reverse(order); // 将后序遍历转为拓扑排序顺序
        return order; // 返回拓扑序列
    }
    
    public List<Integer> findOrder(int n, int[][] edges) {
        // 构建图
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            graph.put(i, new ArrayList<>());
        }
        for (int[] edge : edges) {
            graph.getOrDefault(edge[1], new ArrayList<>()).add(edge[0]);
        }
        
        return topologicalSortingDFS(graph);
    }
}
```



## 2、Kahn 算法（基于广度优先遍历实现）

### 核心思路

首先从没有入度的顶点开始，将其入队，并在图中删除其所有的出边。然后从队列中取出一个顶点，将其添加到结果列表中，并将其所有邻居节点的入度减 1。重复这个过程，直到队列为空。

**基于广度优先搜索的拓扑排序算法的具体步骤**

> 1. 初始化一个结果列表 `result` 用于存放拓扑排序的顶点序列，一个空的队列 `queue` 用于存放入度为0的顶点。
> 2. 遍历图中所有的顶点，统计每个顶点的入度，并将入度为0的顶点加入到队列 `queue` 中。
> 3. 当队列 `queue` 不为空时，取出一个顶点 `u`。
> 4. 将顶点 `u` 加入到结果列表 `result` 中，并遍历顶点 `u` 的所有邻居节点 `v`。
> 5. 对于每个邻居节点 `v`，将 `v` 的入度减 1。
> 6. 如果顶点 `v` 的入度减为 0，则将其加入到队列 `queue` 中。
> 7. 重复步骤3-6，直到队列 `queue` 为空。
> 8. 如果结果列表 `result` 的长度等于图中所有顶点的个数，则返回结果列表 `result`，否则表示图中存在环，无法进行拓扑排序。



### 代码实现

C++版本

```c++
#include <vector>
#include <unordered_map>
#include <deque>

using namespace std;

class Solution {
public:
    // 拓扑排序，graph 中包含所有顶点的有向边关系（包括无边顶点）
    vector<int> topologicalSortingKahn(unordered_map<int, vector<int>>& graph) {
        unordered_map<int, int> indegrees;    // indegrees 用于记录所有顶点入度
        for (auto& entry : graph) {
            indegrees[entry.first] = 0;
        }
        
        for (auto& entry : graph) {
            for (int v : entry.second) {
                indegrees[v]++;    // 统计所有顶点入度
            }
        }
        
        // 将入度为 0 的顶点存入集合 S 中
        deque<int> S;
        for (auto& entry : indegrees) {
            if (entry.second == 0) {
                S.push_back(entry.first);
            }
        }
        
        vector<int> order;    // order 用于存储拓扑序列
        
        while (!S.empty()) {
            int u = S.back();    // 从集合中选择一个没有前驱的顶点 0
            S.pop_back();
            order.push_back(u);    // 将其输出到拓扑序列 order 中
            for (int v : graph[u]) {    // 遍历顶点 u 的邻接顶点 v
                indegrees[v]--;    // 删除从顶点 u 出发的有向边
                if (indegrees[v] == 0) {    // 如果删除该边后顶点 v 的入度变为 0
                    S.push_back(v);    // 将其放入集合 S 中
                }
            }
        }
        
        if (indegrees.size() != order.size()) {    // 还有顶点未遍历（存在环），无法构成拓扑序列
            return vector<int>();
        }
        return order;    // 返回拓扑序列
    }
    
    vector<int> findOrder(int n, vector<vector<int>>& edges) {
        // 构建图
        unordered_map<int, vector<int>> graph;
        for (int i = 0; i < n; ++i) {
            graph[i] = vector<int>();
        }
        
        for (auto& edge : edges) {
            graph[edge[0]].push_back(edge[1]);
        }
        
        return topologicalSortingKahn(graph);
    }
};
```

Java版本

```java
import java.util.*;

class Solution {
    // 拓扑排序，graph 中包含所有顶点的有向边关系（包括无边顶点）
    public List<Integer> topologicalSortingKahn(Map<Integer, List<Integer>> graph) {
        Map<Integer, Integer> indegrees = new HashMap<>(); // indegrees 用于记录所有顶点入度
        for (int u : graph.keySet()) {
            indegrees.put(u, 0);
        }
        
        for (int u : graph.keySet()) {
            for (int v : graph.get(u)) {
                indegrees.put(v, indegrees.getOrDefault(v, 0) + 1); // 统计所有顶点入度
            }
        }
        
        // 将入度为 0 的顶点存入集合 S 中
        Deque<Integer> S = new ArrayDeque<>();
        for (int u : indegrees.keySet()) {
            if (indegrees.get(u) == 0) {
                S.offer(u);
            }
        }
        
        List<Integer> order = new ArrayList<>(); // order 用于存储拓扑序列
        
        while (!S.isEmpty()) {
            int u = S.poll(); // 从集合中选择一个没有前驱的顶点 0
            order.add(u); // 将其输出到拓扑序列 order 中
            for (int v : graph.get(u)) { // 遍历顶点 u 的邻接顶点 v
                indegrees.put(v, indegrees.get(v) - 1); // 删除从顶点 u 出发的有向边
                if (indegrees.get(v) == 0) { // 如果删除该边后顶点 v 的入度变为 0
                    S.offer(v); // 将其放入集合 S 中
                }
            }
        }
        
        if (indegrees.size() != order.size()) { // 还有顶点未遍历（存在环），无法构成拓扑序列
            return new ArrayList<>();
        }
        return order; // 返回拓扑序列
    }
    
    public List<Integer> findOrder(int n, int[][] edges) {
        // 构建图
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            graph.put(i, new ArrayList<>());
        }
        
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
        }
        
        return topologicalSortingKahn(graph);
    }
}
```







