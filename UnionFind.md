# 并查集算法

## 概念

并查集（Union-Find，Disjoint Set）是一种用于处理集合的数据结构，它支持两个主要操作：查找（Find）和合并（Union）。

并查集通常用于划分元素集合，并在动态连通性的问题中进行快速合并和查找。

每个元素都被分配一个标识符，形成一个集合。初始时，每个元素都是其自己所在集合的根。合并操作将两个集合合并为一个，而查找操作用于确定某个元素所属的集合，通常通过找到该集合的根节点来实现。

并查集的主要优势是通过“路径压缩”和“按秩合并”两种优化技术使其对于合并和查找操作的高效性：

> 1. **路径压缩：** 在查找操作中，通过将元素指向其根节点的路径上的每个节点直接连接到根，可以降低树的高度，提高后续查找操作的效率。
> 2. **按秩合并：** 在合并操作中，将深度较小的树连接到深度较大的树上，以避免由于合并而导致整体树的高度增加，从而减少查找操作的时间复杂度。

应用于：判断无向图中是否存在环、连通性判断、最小生成树算法（如Kruskal算法）等。



## 路径压缩（Path Compression）

### 隔代压缩（Half-Compression）

在 find 操作中，将每个节点的父节点更新为其祖父节点，即每隔一代进行一次压缩。
通过隔代压缩，路径的长度仍然会减半，但相比完全压缩，对整体的路径长度影响较小。

C++版本

```C++
int find(int x, vector<int>& parent) {
    while (parent[x] != x) {
        parent[x] = parent[parent[x]];
    }
    return parent[x];
}
```

Java版本

```java
int find(int x, int[] parent) {
    while (parent[x] != x) {
        parent[x] = parent[parent[x]]; 
    }
    return parent[x];
}
```



### 完全压缩（Full-Compression）

在 find 操作中，将路径上的每个节点都直接连接到根节点，完全消除了路径。
虽然完全压缩的路径长度更短，但在实际应用中，它可能会导致更多的内存写入，因为需要更新更多的节点。

C++版本

```C++
int find(int x, vector<int>& parent) {
    if (parent[x] != x) {
        parent[x] = find(parent[x], parent); 
    }
    return parent[x];
}
```

Java版本

```java
int find(int x, int[] parent) {
    if (parent[x] != x) {
        parent[x] = find(parent[x], parent);
    }
    return parent[x];
}
```



## 按秩合并（Union by Rank）

### 按深度合并（Union by Depth）

在 union 操作中，将深度（或秩）较小的树合并到深度较大的树上。
深度可以理解为从根节点到最深叶节点的路径长度。

C++版本

```C++
void unionSets(int x, int y, vector<int>& parent, vector<int>& depth) {
    int root_x = find(x, parent);
    int root_y = find(y, parent);

    if (root_x != root_y) {
        if (depth[root_x] < depth[root_y]) {
            parent[root_x] = root_y;
        } else if (depth[root_x] > depth[root_y]) {
            parent[root_y] = root_x;
        } else {
            parent[root_x] = root_y;
            depth[root_y]++;
        }
    }
}
```

Java版本

```java
void unionSets(int x, int y, int[] parent, int[] depth) {
    int root_x = find(x, parent);
    int root_y = find(y, parent);

    if (root_x != root_y) {
        if (depth[root_x] < depth[root_y]) {
            parent[root_x] = root_y;
        } else if (depth[root_x] > depth[root_y]) {
            parent[root_y] = root_x;
        } else {
            parent[root_x] = root_y;
            depth[root_y]++;
        }
    }
}
```



### 按大小合并（Union by Size）：

在 union 操作中，将节点数较少的树合并到节点数较多的树上。
这种方式可以避免树的大小过于不平衡，防止树过深。

C++版本

```C++
void unionSets(int x, int y, vector<int>& parent, vector<int>& size) {
    int root_x = find(x, parent);
    int root_y = find(y, parent);

    if (root_x != root_y) {
        if (size[root_x] < size[root_y]) {
            parent[root_x] = root_y;
            size[root_y] += size[root_x];
        } else {
            parent[root_y] = root_x;
            size[root_x] += size[root_y];
        }
    }
}
```

Java版本

```java
void unionSets(int x, int y, int[] parent, int[] size) {
    int root_x = find(x, parent);
    int root_y = find(y, parent);

    if (root_x != root_y) {
        if (size[root_x] < size[root_y]) {
            parent[root_x] = root_y;
            size[root_y] += size[root_x];
        } else {
            parent[root_y] = root_x;
            size[root_x] += size[root_y];
        }
    }
}
```



## 代码实现

C++版本

```c++
#include <vector>

class UnionFind {
private:
    std::vector<int> parent; // 树节点集合
    std::vector<int> rank; // 集合树的深度（层级）

public:
    // 初始化并查集
    UnionFind(int n) {
        parent.resize(n);
        rank.resize(n, 1);
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }

     // 查找
    int find(int x) {
        while (parent[x] != x) {
            parent[x] = parent[parent[x]]; // 路径压缩：隔代压缩
            x = parent[x];
        }
        return x; // 返回集合树的根节点元素（代表值）
    }

    // 合并：按深度合并
    bool unionSets(int x, int y) {
        int root_x = find(x);
        int root_y = find(y);

        // 同一集合树无法再次合并
        if (root_x == root_y) {
            return false;
        }

        // 比较集合树的深度再合并
        if (rank[root_x] < rank[root_y]) {
            parent[root_x] = root_y;
        } else if (rank[root_x] > rank[root_y]) {
            parent[root_y] = root_x;
        } else {
            parent[root_x] = root_y;
            rank[root_y]++; // 当两集合树深度相同时，合并的集合树深度加一
        }

        return true;
    }

    // 判断是否为同一个集合
    bool isConnected(int x, int y) {
        return find(x) == find(y);
    }
};
```

Java版本

```java
public class UnionFind {
    private int[] parent; // 树节点集合
    private int[] rank; // 集合树的深度（层级）

    // 初始化并查集
    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
            rank[i] = 1;
        }
    }
	
    // 查找
    public int find(int x) {
        while (parent[x] != x) {
            parent[x] = parent[parent[x]]; // 路径压缩：隔代压缩
            x = parent[x];
        }
        return x; // 返回集合树的根节点元素（代表值）
    }

    // 合并：按深度合并
    public boolean union(int x, int y) {
        int root_x = find(x);
        int root_y = find(y);

        // 同一集合树无法再次合并
        if (root_x == root_y) {
            return false;
        }
		
        // 比较集合树的深度再合并
        if (rank[root_x] < rank[root_y]) {
            parent[root_x] = root_y;
        } else if (rank[root_x] > rank[root_y]) {
            parent[root_y] = root_x;
        } else {
            parent[root_x] = root_y;
            rank[root_y]++; // 当两集合树深度相同时，合并的集合树深度加一
        }

        return true;
    }

    // 判断是否为同一个集合
    public boolean isConnected(int x, int y) {
        return find(x) == find(y);
    }
}
```

