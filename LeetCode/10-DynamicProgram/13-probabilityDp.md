# 动态规划

## 概率Dp问题

### [688. 骑士在棋盘上的概率](https://leetcode.cn/problems/knight-probability-in-chessboard/)

中等

在一个 `n x n` 的国际象棋棋盘上，一个骑士从单元格 `(row, column)` 开始，并尝试进行 `k` 次移动。行和列是 **从 0 开始** 的，所以左上单元格是 `(0,0)` ，右下单元格是 `(n - 1, n - 1)` 。

象棋骑士有8种可能的走法，如下图所示。每次移动在基本方向上是两个单元格，然后在正交方向上是一个单元格。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/knight.png)

每次骑士要移动时，它都会随机从8种可能的移动中选择一种(即使棋子会离开棋盘)，然后移动到那里。

骑士继续移动，直到它走了 `k` 步或离开了棋盘。

返回 *骑士在棋盘停止移动后仍留在棋盘上的概率* 。

**示例 1：**

```
输入: n = 3, k = 2, row = 0, column = 0
输出: 0.0625
解释: 有两步(到(1,2)，(2,1))可以让骑士留在棋盘上。
在每一个位置上，也有两种移动可以让骑士留在棋盘上。
骑士留在棋盘上的总概率是0.0625。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    vector<vector<int>> dirs = {{-2, -1}, {-2, 1}, {2, -1}, {2, 1}, {-1, -2}, {-1, 2}, {1, -2}, {1, 2}};

    double knightProbability(int n, int k, int row, int column) {
        vector<vector<vector<double>>> dp(k + 1, vector<vector<double>>(n, vector<double>(n)));
        for (int step = 0; step <= k; step++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (step == 0) {
                        dp[step][i][j] = 1;
                    } else {
                        for (auto & dir : dirs) {
                            int ni = i + dir[0], nj = j + dir[1];
                            if (ni >= 0 && ni < n && nj >= 0 && nj < n) {
                                dp[step][i][j] += dp[step - 1][ni][nj] / 8;
                            }
                        }
                    }
                }
            }
        }
        return dp[k][row][column];
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    static int[][] dirs = {{-2, -1}, {-2, 1}, {2, -1}, {2, 1}, {-1, -2}, {-1, 2}, {1, -2}, {1, 2}};

    public double knightProbability(int n, int k, int row, int column) {
        double[][][] dp = new double[k + 1][n][n];
        for (int step = 0; step <= k; step++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (step == 0) {
                        dp[step][i][j] = 1;
                    } else {
                        for (int[] dir : dirs) {
                            int ni = i + dir[0], nj = j + dir[1];
                            if (ni >= 0 && ni < n && nj >= 0 && nj < n) {
                                dp[step][i][j] += dp[step - 1][ni][nj] / 8;
                            }
                        }
                    }
                }
            }
        }
        return dp[k][row][column];
    }
}
```



### [808. 分汤](https://leetcode.cn/problems/soup-servings/)

中等

有 **A 和 B 两种类型** 的汤。一开始每种类型的汤有 `n` 毫升。有四种分配操作：

1. 提供 `100ml` 的 **汤A** 和 `0ml` 的 **汤B** 。
2. 提供 `75ml` 的 **汤A** 和 `25ml` 的 **汤B** 。
3. 提供 `50ml` 的 **汤A** 和 `50ml` 的 **汤B** 。
4. 提供 `25ml` 的 **汤A** 和 `75ml` 的 **汤B** 。

当我们把汤分配给某人之后，汤就没有了。每个回合，我们将从四种概率同为 `0.25` 的操作中进行分配选择。如果汤的剩余量不足以完成某次操作，我们将尽可能分配。当两种类型的汤都分配完时，停止操作。

**注意** 不存在先分配 `100` ml **汤B** 的操作。

需要返回的值： **汤A** 先分配完的概率 + **汤A和汤B** 同时分配完的概率 / 2。返回值在正确答案 `10-5` 的范围内将被认为是正确的。

**示例 1:**

```
输入: n = 50
输出: 0.62500
解释:如果我们选择前两个操作，A 首先将变为空。
对于第三个操作，A 和 B 会同时变为空。
对于第四个操作，B 首先将变为空。
所以 A 变为空的总概率加上 A 和 B 同时变为空的概率的一半是 0.25 *(1 + 1 + 0.5 + 0)= 0.625。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    double soupServings(int n) {
        n = ceil((double) n / 25);
        if (n >= 179) {
            return 1.0;
        }
        vector<vector<double>> dp(n + 1, vector<double>(n + 1));
        dp[0][0] = 0.5;
        for (int i = 1; i <= n; i++) {
            dp[0][i] = 1.0;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = (dp[max(0, i - 4)][j] + dp[max(0, i - 3)][max(0, j - 1)] +
                           dp[max(0, i - 2)][max(0, j - 2)] + dp[max(0, i - 1)][max(0, j - 3)]) / 4.0;
            }
        }
        return dp[n][n];
    }
};

// 方法二：记忆化搜索
class Solution {
public:
    double soupServings(int n) {
        n = ceil((double) n / 25);
        if (n >= 179) {
            return 1.0;
        }
        memo = vector<vector<double>>(n + 1, vector<double>(n + 1));
        return dfs(n, n);
    }

    double dfs(int a, int b) {
        if (a <= 0 && b <= 0) {
            return 0.5;
        } else if (a <= 0) {
            return 1;
        } else if (b <= 0) {
            return 0;
        }
        if (memo[a][b] > 0) {
            return memo[a][b];
        }
        memo[a][b] = 0.25 * (dfs(a - 4, b) + dfs(a - 3, b - 1) + 
                             dfs(a - 2, b - 2) + dfs(a - 1, b - 3));
        return memo[a][b];
    }
private:
    vector<vector<double>> memo;
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public double soupServings(int n) {
        n = (int) Math.ceil((double) n / 25);
        if (n >= 179) {
            return 1.0;
        }
        double[][] dp = new double[n + 1][n + 1];
        dp[0][0] = 0.5;
        for (int i = 1; i <= n; i++) {
            dp[0][i] = 1.0;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = (dp[Math.max(0, i - 4)][j] + dp[Math.max(0, i - 3)][Math.max(0, j - 1)] + dp[Math.max(0, i - 2)][Math.max(0, j - 2)] + dp[Math.max(0, i - 1)][Math.max(0, j - 3)]) / 4.0;
            }
        }
        return dp[n][n];
    }
}

// 方法二：记忆化搜索
class Solution {
    private double[][] memo;

    public double soupServings(int n) {
        n = (int) Math.ceil((double) n / 25);
        if (n >= 179) {
            return 1.0;
        }
        memo = new double[n + 1][n + 1];
        return dfs(n, n);
    }

    public double dfs(int a, int b) {
        if (a <= 0 && b <= 0) {
            return 0.5;
        } else if (a <= 0) {
            return 1;
        } else if (b <= 0) {
            return 0;
        }
        if (memo[a][b] == 0) {
            memo[a][b] = 0.25 * (dfs(a - 4, b) + dfs(a - 3, b - 1) + dfs(a - 2, b - 2) + dfs(a - 1, b - 3));
        }
        return memo[a][b];
    }
}
```



### [837. 新 21 点](https://leetcode.cn/problems/new-21-game/)

中等

爱丽丝参与一个大致基于纸牌游戏 **“21点”** 规则的游戏，描述如下：

爱丽丝以 `0` 分开始，并在她的得分少于 `k` 分时抽取数字。 抽取时，她从 `[1, maxPts]` 的范围中随机获得一个整数作为分数进行累计，其中 `maxPts` 是一个整数。 每次抽取都是独立的，其结果具有相同的概率。

当爱丽丝获得 `k` 分 **或更多分** 时，她就停止抽取数字。

爱丽丝的分数不超过 `n` 的概率是多少？

与实际答案误差不超过 `10-5` 的答案将被视为正确答案。

**示例 1：**

```
输入：n = 10, k = 1, maxPts = 10
输出：1.00000
解释：爱丽丝得到一张牌，然后停止。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    double new21Game(int n, int k, int maxPts) {
        if (k == 0) {
            return 1.0;
        }
        vector<double> dp(k + maxPts);
        for (int i = k; i <= n && i < k + maxPts; i++) {
            dp[i] = 1.0;
        }
        for (int i = k - 1; i >= 0; i--) {
            for (int j = 1; j <= maxPts; j++) {
                dp[i] += dp[i + j] / maxPts;
            }
        }
        return dp[0];
    }
};

// 优化
class Solution {
public:
    double new21Game(int n, int k, int maxPts) {
        if (k == 0) {
            return 1.0;
        }
        vector<double> dp(k + maxPts);
        for (int i = k; i <= n && i < k + maxPts; i++) {
            dp[i] = 1.0;
        }
        dp[k - 1] = 1.0 * min(n - k + 1, maxPts) / maxPts;
        for (int i = k - 2; i >= 0; i--) {
            dp[i] = dp[i + 1] - (dp[i + maxPts + 1] - dp[i + 1]) / maxPts;
        }
        return dp[0];
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public double new21Game(int n, int k, int maxPts) {
        if (k == 0) {
            return 1.0;
        }
        double[] dp = new double[k + maxPts];
        for (int i = k; i <= n && i < k + maxPts; i++) {
            dp[i] = 1.0;
        }
        for (int i = k - 1; i >= 0; i--) {
            for (int j = 1; j <= maxPts; j++) {
                dp[i] += dp[i + j] / maxPts;
            }
        }
        return dp[0];
    }
}

// 优化
class Solution {
    public double new21Game(int n, int k, int maxPts) {
        if (k == 0) {
            return 1.0;
        }
        double[] dp = new double[k + maxPts];
        for (int i = k; i <= n && i < k + maxPts; i++) {
            dp[i] = 1.0;
        }
        dp[k - 1] = 1.0 * Math.min(n - k + 1, maxPts) / maxPts;
        for (int i = k - 2; i >= 0; i--) {
            dp[i] = dp[i + 1] - (dp[i + maxPts + 1] - dp[i + 1]) / maxPts;
        }
        return dp[0];
    }
}
```



### [1467. 两个盒子中球的颜色数相同的概率](https://leetcode.cn/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls/)

困难

桌面上有 `2n` 个颜色不完全相同的球，球上的颜色共有 `k` 种。给你一个大小为 `k` 的整数数组 `balls` ，其中 `balls[i]` 是颜色为 `i` 的球的数量。

所有的球都已经 **随机打乱顺序** ，前 `n` 个球放入第一个盒子，后 `n` 个球放入另一个盒子（请认真阅读示例 2 的解释部分）。

**注意：**这两个盒子是不同的。例如，两个球颜色分别为 `a` 和 `b`，盒子分别为 `[]` 和 `()`，那么 `[a] (b)` 和 `[b] (a)` 这两种分配方式是不同的（请认真阅读示例的解释部分）。

请返回「两个盒子中球的颜色数相同」的情况的概率。答案与真实值误差在 `10^-5` 以内，则被视为正确答案

**示例 1：**

```
输入：balls = [1,1]
输出：1.00000
解释：球平均分配的方式只有两种：
- 颜色为 1 的球放入第一个盒子，颜色为 2 的球放入第二个盒子
- 颜色为 2 的球放入第一个盒子，颜色为 1 的球放入第二个盒子
这两种分配，两个盒子中球的颜色数都相同。所以概率为 2/2 = 1 。
```

C++版本

```c++
class Solution {
public:
    double getProbability(vector<int>& balls) {
        int k = balls.size();

        int sum = 0;
        for (int x : balls)
            sum += x;
        int n = sum / 2;

        vector<double> fact(2 * n + 1);
        fact[0] = 1.0;
        for (int i = 1; i <= 2 * n; ++i) {
            fact[i] = fact[i - 1] * i;
        }

        double total = fact[2 * n];
        for (int ball : balls) {
            total /= fact[ball];
        }

        vector<vector<double>> dp(2 * n + 1, vector<double>(2 * k + 1, 0.0));
        dp[0][k] = 1.0;
        int num = 0;
        for (int i = 0; i < k; ++i) {
            vector<vector<double>> next(2 * n + 1, vector<double>(2 * k + 1, 0.0));
            for (int j = 0; j <= balls[i]; ++j) {
                int trans = j == 0 ? -1 : 0;
                trans = j == balls[i] ? 1 : trans;
                for (int front = 0; front <= 2 * n; ++front) {
                    for (int color = 0; color <= 2 * k; ++color) {
                        if (dp[front][color] == 0) continue;
                        double ways = dp[front][color];
                        ways *= fact[front + j] / (fact[front] * fact[j]);
                        ways *= fact[num - front + balls[i] - j] / (fact[num - front] * fact[balls[i] - j]);
                        next[front + j][color + trans] += ways;
                    }
                }
            }
            dp = next;
            num += balls[i];
        }
        return dp[n][k] / total;
    }
};
```

Java版本

```java
class Solution {
    public double getProbability(int[] balls) {

        int k = balls.length;
        
        int sum = 0;
        for(int x:balls)
            sum += x;
        int n = sum / 2;
       
        double[] fact = new double[2 * n + 1];
        
        fact[0] = 1.0;
        for(int i=1;i<=2*n;++i){
            fact[i] = fact[i-1] * i;
        }
       
        double total = fact[2*n];
        for(int ball:balls){ total /= fact[ball]; }
   
        double[][] dp  = new double[2 * n + 1][2*k+1];        
        dp[0][k] = 1.0;
        int num = 0;
        for(int i=0;i<k;++i) {
            double[][] next  = new double[2 * n + 1][2*k+1];
            for(int j=0;j<=balls[i];++j){
                
                int trans = j==0?-1: 0;
                trans = j==balls[i]?1:trans;
                for(int front = 0; front <= 2*n; ++front)
                for(int color = 0; color <= 2*k; ++color){
                    if(dp[front][color] == 0) continue;
                    double ways = dp[front][color];
                    ways *= fact[front+j] / (fact[front] * fact[j]);
                    ways *= fact[num-front+balls[i]-j] / (fact[num-front] * fact[balls[i]-j]);
                    next[front+j][color+trans] += ways;
                }
            }
            dp = next;
            num += balls[i];
        }
        return dp[n][k]/total;
   
    }
}
```



### [1227. 飞机座位分配概率](https://leetcode.cn/problems/airplane-seat-assignment-probability/)

中等

有 `n` 位乘客即将登机，飞机正好有 `n` 个座位。第一位乘客的票丢了，他随便选了一个座位坐下。

剩下的乘客将会：

- 如果他们自己的座位还空着，就坐到自己的座位上，
- 当他们自己的座位被占用时，随机选择其他座位

第 `n` 位乘客坐在自己的座位上的概率是多少？

**示例 1：**

```
输入：n = 1
输出：1.00000
解释：第一个人只会坐在自己的位置上。
```

C++版本

```c++
// 方法一：数学
class Solution {
public:
    double nthPersonGetsNthSeat(int n) {
        return n == 1 ? 1.0 : 0.5;
    }
};

// 方法二：动态规划
class Solution {
public:
    double nthPersonGetsNthSeat(int n) {
        if (n <= 2) {
            return 1.0 / n;
        }
        double prob = 0.5;
        for (int i = 3; i <= n; i++) {
            prob = (1.0 + (i - 2) * prob) / i;
        }
        return prob;
    }
};
```



Java版本

```java
// 方法一：数学
class Solution {
    public double nthPersonGetsNthSeat(int n) {
        return n == 1 ? 1.0 : 0.5;
    }
}

// 方法二：动态规划
class Solution {
    public double nthPersonGetsNthSeat(int n) {
        if (n <= 2) {
            return 1.0 / n;
        }
        double prob = 0.5;
        for (int i = 3; i <= n; i++) {
            prob = (1.0 + (i - 2) * prob) / i;
        }
        return prob;
    }
}
```



### [1377. T 秒后青蛙的位置](https://leetcode.cn/problems/frog-position-after-t-seconds/)

困难

给你一棵由 `n` 个顶点组成的无向树，顶点编号从 `1` 到 `n`。青蛙从 **顶点 1** 开始起跳。规则如下：

- 在一秒内，青蛙从它所在的当前顶点跳到另一个 **未访问** 过的顶点（如果它们直接相连）。
- 青蛙无法跳回已经访问过的顶点。
- 如果青蛙可以跳到多个不同顶点，那么它跳到其中任意一个顶点上的机率都相同。
- 如果青蛙不能跳到任何未访问过的顶点上，那么它每次跳跃都会停留在原地。

无向树的边用数组 `edges` 描述，其中 `edges[i] = [ai, bi]` 意味着存在一条直接连通 `ai` 和 `bi` 两个顶点的边。

返回青蛙在 *`t`* 秒后位于目标顶点 *`target`* 上的概率。与实际答案相差不超过 `10-5` 的结果将被视为正确答案。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/12/21/frog1.jpg)

```
输入：n = 7, edges = [[1,2],[1,3],[1,7],[2,4],[2,6],[3,5]], t = 2, target = 4
输出：0.16666666666666666 
解释：上图显示了青蛙的跳跃路径。青蛙从顶点 1 起跳，第 1 秒 有 1/3 的概率跳到顶点 2 ，然后第 2 秒 有 1/2 的概率跳到顶点 4，因此青蛙在 2 秒后位于顶点 4 的概率是 1/3 * 1/2 = 1/6 = 0.16666666666666666 。 
```

C++版本

```c++
// 方法一：深度优先搜索
class Solution {
public:
    double frogPosition(int n, vector<vector<int>>& edges, int t, int target) {
        vector<vector<int>> G(n + 1);
        for (int i = 0; i < edges.size(); ++i) {
            G[edges[i][0]].push_back(edges[i][1]);
            G[edges[i][1]].push_back(edges[i][0]);
        }
        vector<bool> visited(n + 1, false);
        return dfs(G, visited, 1, t, target);
    }

    double dfs(vector<vector<int>>& G, vector<bool>& visited, int i, int t, int target) {
        int nxt = i == 1 ? G[i].size() : G[i].size() - 1;
        if (t == 0 || nxt == 0) {
            return i == target ? 1.0 : 0.0;
        }
        visited[i] = true;
        double ans = 0.0;
        for (int j : G[i]) {
            if (!visited[j]) {
                ans += dfs(G, visited, j, t - 1, target);
            }
        }
        return ans / nxt;
    }
};
```

Java版本

```java
// 方法一：深度优先搜索
public class Solution {
    public double frogPosition(int n, int[][] edges, int t, int target) {
        List<Integer>[] G = new ArrayList[n + 1];
        for (int i = 1; i <= n; ++i)
            G[i] = new ArrayList<>();
        for (int[] e : edges) {
            G[e[0]].add(e[1]);
            G[e[1]].add(e[0]);
        }
        boolean[] seen = new boolean[n + 1];
        return dfs(G, seen, 1, t, target);
    }

    private double dfs(List<Integer>[] G, boolean[] seen, int i, int t, int target) {
        int nxt = i == 1 ? G[i].size() : G[i].size() - 1;
        if (t == 0 || nxt == 0) {
            return i == target ? 1.0 : 0.0;
        }
        seen[i] = true;
        double ans = 0.0;
        for (int j : G[i]) {
            if (!seen[j]) {
                ans += dfs(G, seen, j, t - 1, target);
            }
        }
        return ans / nxt;
    }
}
```



### [LCR 185. 统计结果概率](https://leetcode.cn/problems/nge-tou-zi-de-dian-shu-lcof/)

中等

你选择掷出 `num` 个色子，请返回所有点数总和的概率。

你需要用一个浮点数数组返回答案，其中第 `i` 个元素代表这 `num` 个骰子所能掷出的点数集合中第 `i` 小的那个的概率。

**示例 1：**

```
输入：num = 3
输出：[0.00463,0.01389,0.02778,0.04630,0.06944,0.09722,0.11574,0.12500,0.12500,0.11574,0.09722,0.06944,0.04630,0.02778,0.01389,0.00463]
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    vector<double> statisticsProbability(int num) {
        vector<double> dp(6, 1.0 / 6.0);
        for (int i = 2; i <= num; i++) {
            vector<double> tmp(5 * i + 1, 0);
            for (int j = 0; j < dp.size(); j++) {
                for (int k = 0; k < 6; k++) {
                    tmp[j + k] += dp[j] / 6.0;
                }
            }
            dp = tmp;
        }
        return dp;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public double[] statisticsProbability(int num) {
        double[] dp = new double[6];
        Arrays.fill(dp, 1.0 / 6.0);
        for (int i = 2; i <= num; i++) {
            double[] tmp = new double[5 * i + 1];
            for (int j = 0; j < dp.length; j++) {
                for (int k = 0; k < 6; k++) {
                    tmp[j + k] += dp[j] / 6.0;
                }
            }
            dp = tmp;
        }
        return dp;
    }
}
```

