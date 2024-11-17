# 图的路径

## 1、单源最短路径

### 定义

指从图中的一个顶点出发，找到该顶点到图中所有其他顶点的最短路径。

### 1.1、Dijkstra 算法

适用于解决非负权边的单源最短路径问题。基本思路是维护一个优先队列（通常使用最小堆实现），从源点开始依次找到与源点距离最短的顶点，将该顶点加入已访问集合，并更新与该顶点相邻的顶点的距离。重复以上步骤直到所有顶点都被加入已访问集合。



### 1.2、Bellman-Ford 算法

可用于解决带负权边的单源最短路径问题，但要求图中不能有负权回路。基本思路是进行多轮松弛操作，每一轮松弛都会更新与源点的距离。如果某一轮松弛操作没有使得任何顶点的距离发生变化，那么就可以确定没有负权回路，算法可以停止。



### 1.3、Floyd-Warshall 算法:

适用于解决任意两点之间的最短路径问题，包括有负权边和无负权回路的情况。基本思路是动态规划，通过一个二维数组来记录所有顶点之间的最短距离。具体步骤是先初始化二维数组，然后遍历每一对顶点，以某一个顶点为中间节点来更新两个顶点之间的最短距离。

```java
public class FloydWarshall {
    final static int INF = 99999, V = 4;

    void floydWarshall(int graph[][]) {
        int dist[][] = new int[V][V];
        int i, j, k;

        /* 将解决方案矩阵初始化为与输入图矩阵相同的值。或者我们可以说最短距离的初始值是基于不考虑中间顶点的最短路径。 */
        for (i = 0; i < V; i++)
            for (j = 0; j < V; j++)
                dist[i][j] = graph[i][j];

        /*
        将解决方案矩阵初始化为与输入图矩阵相同的值。或者我们可以说最短距离的初始值基于不考虑中间顶点的最短路径。
        将所有顶点逐个添加到中间顶点集合中。
		---> 在迭代开始之前，我们有所有顶点对之间的最短距离，因此最短距离仅将集合 {0, 1, 2, .. k-1} 中的顶点视为中间顶点。
		---> 迭代结束后，顶点编号 k 被添加到中间顶点集合中，集合变为 {0, 1, 2, .. k}
         */
        for (k = 0; k < V; k++) {
            // 逐个选取所有顶点作为源
            for (i = 0; i < V; i++) {
                // 选取所有顶点作为上述选取源的目的地
                for (j = 0; j < V; j++) {
                    // 如果顶点 k 位于从 i 到 j 的最短路径上，则更新 dist[i][j] 的值
                    if (dist[i][k] + dist[k][j] < dist[i][j])
                        dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }

        // 打印最短距离矩阵
        printSolution(dist);
    }

    void printSolution(int dist[][]) {
        System.out.println("下面的矩阵显示了每对顶点之间的最短距离");
        for (int i = 0; i < V; ++i) {
            for (int j = 0; j < V; ++j) {
                if (dist[i][j] == INF)
                    System.out.print("INF ");
                else
                    System.out.print(dist[i][j] + "   ");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        /* 创建以下加权图
               10
          (0)------->(3)
           |         /|\
         5 |          |
           |          | 1
          \|/         |
          (1)------->(2)
               3           */
        int graph[][] = {
            { 0, 5, INF, 10 },
            { INF, 0, 3, INF },
            { INF, INF, 0, 1 },
            { INF, INF, INF, 0 }
        };
        FloydWarshall a = new FloydWarshall();

        // 打印解决方案
        a.floydWarshall(graph);
    }
}
```





### 1.4、SPFA 算法







## 2、多源最短路径

### 定义

指从图中所有顶点到其他所有顶点的最短路径的问题







