# 动态规划

## 记忆化搜索

### [375. 猜数字大小 II](https://leetcode.cn/problems/guess-number-higher-or-lower-ii/)

中等

我们正在玩一个猜数游戏，游戏规则如下：

1. 我从 `1` 到 `n` 之间选择一个数字。
2. 你来猜我选了哪个数字。
3. 如果你猜到正确的数字，就会 **赢得游戏** 。
4. 如果你猜错了，那么我会告诉你，我选的数字比你的 **更大或者更小** ，并且你需要继续猜数。
5. 每当你猜了数字 `x` 并且猜错了的时候，你需要支付金额为 `x` 的现金。如果你花光了钱，就会 **输掉游戏** 。

给你一个特定的数字 `n` ，返回能够 **确保你获胜** 的最小现金数，**不管我选择那个数字** 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/10/graph.png)

```
输入：n = 10
输出：16
解释：制胜策略如下：
- 数字范围是 [1,10] 。你先猜测数字为 7 。
    - 如果这是我选中的数字，你的总费用为 $0 。否则，你需要支付 $7 。
    - 如果我的数字更大，则下一步需要猜测的数字范围是 [8,10] 。你可以猜测数字为 9 。
        - 如果这是我选中的数字，你的总费用为 $7 。否则，你需要支付 $9 。
        - 如果我的数字更大，那么这个数字一定是 10 。你猜测数字为 10 并赢得游戏，总费用为 $7 + $9 = $16 。
        - 如果我的数字更小，那么这个数字一定是 8 。你猜测数字为 8 并赢得游戏，总费用为 $7 + $9 = $16 。
    - 如果我的数字更小，则下一步需要猜测的数字范围是 [1,6] 。你可以猜测数字为 3 。
        - 如果这是我选中的数字，你的总费用为 $7 。否则，你需要支付 $3 。
        - 如果我的数字更大，则下一步需要猜测的数字范围是 [4,6] 。你可以猜测数字为 5 。
            - 如果这是我选中的数字，你的总费用为 $7 + $3 = $10 。否则，你需要支付 $5 。
            - 如果我的数字更大，那么这个数字一定是 6 。你猜测数字为 6 并赢得游戏，总费用为 $7 + $3 + $5 = $15 。
            - 如果我的数字更小，那么这个数字一定是 4 。你猜测数字为 4 并赢得游戏，总费用为 $7 + $3 + $5 = $15 。
        - 如果我的数字更小，则下一步需要猜测的数字范围是 [1,2] 。你可以猜测数字为 1 。
            - 如果这是我选中的数字，你的总费用为 $7 + $3 = $10 。否则，你需要支付 $1 。
            - 如果我的数字更大，那么这个数字一定是 2 。你猜测数字为 2 并赢得游戏，总费用为 $7 + $3 + $1 = $11 。
在最糟糕的情况下，你需要支付 $16 。因此，你只需要 $16 就可以确保自己赢得游戏。
```

$$
f(1, n) = \min\limits_{1 \le x \le n} \{x + \max(f(1, x - 1), f(x + 1, n))\} \\
f(i, j) = \min\limits_{i \le k \le j} \{k + \max(f(i, k - 1), f(k + 1, j))\}
$$

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int getMoneyAmount(int n) {
        vector<vector<int>> f(n+1,vector<int>(n+1));
        for (int i = n - 1; i >= 1; i--) {
            for (int j = i + 1; j <= n; j++) {
                f[i][j] = j + f[i][j - 1];
                for (int k = i; k < j; k++) {
                    f[i][j] = min(f[i][j], k + max(f[i][k - 1], f[k + 1][j]));
                }
            }
        }
        return f[1][n];
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int getMoneyAmount(int n) {
        int[][] f = new int[n + 1][n + 1];
        for (int i = n - 1; i >= 1; i--) {
            for (int j = i + 1; j <= n; j++) {
                f[i][j] = j + f[i][j - 1];
                for (int k = i; k < j; k++) {
                    f[i][j] = Math.min(f[i][j], k + Math.max(f[i][k - 1], f[k + 1][j]));
                }
            }
        }
        return f[1][n];
    }
}
```



### [1137. 第 N 个泰波那契数](https://leetcode.cn/problems/n-th-tribonacci-number/)

简单

泰波那契序列 Tn 定义如下： 

T0 = 0, T1 = 1, T2 = 1, 且在 n >= 0 的条件下 Tn+3 = Tn + Tn+1 + Tn+2

给你整数 `n`，请返回第 n 个泰波那契数 Tn 的值。

**示例 1：**

```
输入：n = 4
输出：4
解释：
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int tribonacci(int n) {
        if (n == 0) {
            return 0;
        }
        if (n <= 2) {
            return 1;
        }
        int p = 0, q = 0, r = 1, s = 1;
        for (int i = 3; i <= n; ++i) {
            p = q;
            q = r;
            r = s;
            s = p + q + r;
        }
        return s;
    }
};

// 方法二：矩阵快速幂
class Solution {
public:
    int tribonacci(int n) {
        if (n == 0) {
            return 0;
        }
        if (n <= 2) {
            return 1;
        }
        vector<vector<long>> q = {{1, 1, 1}, {1, 0, 0}, {0, 1, 0}};
        vector<vector<long>> res = pow(q, n);
        return res[0][2];
    }

    vector<vector<long>> pow(vector<vector<long>>& a, long n) {
        vector<vector<long>> ret = {{1, 0, 0}, {0, 1, 0}, {0, 0, 1}};
        while (n > 0) {
            if ((n & 1) == 1) {
                ret = multiply(ret, a);
            }
            n >>= 1;
            a = multiply(a, a);
        }
        return ret;
    }

    vector<vector<long>> multiply(vector<vector<long>>& a, vector<vector<long>>& b) {
        vector<vector<long>> c(3, vector<long>(3));
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j] + a[i][2] * b[2][j];
            }
        }
        return c;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int tribonacci(int n) {
        if (n == 0) {
            return 0;
        }
        if (n <= 2) {
            return 1;
        }
        int p = 0, q = 0, r = 1, s = 1;
        for (int i = 3; i <= n; ++i) {
            p = q;
            q = r;
            r = s;
            s = p + q + r;
        }
        return s;
    }
}

// 方法二：矩阵快速幂
class Solution {
    public int tribonacci(int n) {
        if (n == 0) {
            return 0;
        }
        if (n <= 2) {
            return 1;
        }
        int[][] q = {{1, 1, 1}, {1, 0, 0}, {0, 1, 0}};
        int[][] res = pow(q, n);
        return res[0][2];
    }

    public int[][] pow(int[][] a, int n) {
        int[][] ret = {{1, 0, 0}, {0, 1, 0}, {0, 0, 1}};
        while (n > 0) {
            if ((n & 1) == 1) {
                ret = multiply(ret, a);
            }
            n >>= 1;
            a = multiply(a, a);
        }
        return ret;
    }

    public int[][] multiply(int[][] a, int[][] b) {
        int[][] c = new int[3][3];
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j] + a[i][2] * b[2][j];
            }
        }
        return c;
    }
}
```



### [494. 目标和](https://leetcode.cn/problems/target-sum/)

中等

给你一个非负整数数组 `nums` 和一个整数 `target` 。

向数组中的每个整数前添加 `'+'` 或 `'-'` ，然后串联起所有整数，可以构造一个 **表达式** ：

- 例如，`nums = [2, 1]` ，可以在 `2` 之前添加 `'+'` ，在 `1` 之前添加 `'-'` ，然后串联起来得到表达式 `"+2-1"` 。

返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

**示例 1：**

```
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

$$
动态规划：\text{dp}[i][j] = \begin{cases} 
\text{dp}[i-1][j], & \text j < \text{nums}[i] \\
\text{dp}[i-1][j] + \text{dp}[i-1][j-\text{nums}[i]], & \text j \geq \text{nums}[i]
\end{cases}
$$

C++版本

```c++
// 方法一：回溯
class Solution {
public:
    int count = 0;

    int findTargetSumWays(vector<int>& nums, int target) {
        backtrack(nums, target, 0, 0);
        return count;
    }

    void backtrack(vector<int>& nums, int target, int index, int sum) {
        if (index == nums.size()) {
            if (sum == target) {
                count++;
            }
        } else {
            backtrack(nums, target, index + 1, sum + nums[index]);
            backtrack(nums, target, index + 1, sum - nums[index]);
        }
    }
};

// 方法二：动态规划
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for (int& num : nums) {
            sum += num;
        }
        int diff = sum - target;
        if (diff < 0 || diff % 2 != 0) {
            return 0;
        }
        int n = nums.size(), neg = diff / 2;
        vector<vector<int>> dp(n + 1, vector<int>(neg + 1));
        dp[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            int num = nums[i - 1];
            for (int j = 0; j <= neg; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j >= num) {
                    dp[i][j] += dp[i - 1][j - num];
                }
            }
        }
        return dp[n][neg];
    }
};

// 或者
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for (int& num : nums) {
            sum += num;
        }
        int diff = sum - target;
        if (diff < 0 || diff % 2 != 0) {
            return 0;
        }
        int neg = diff / 2;
        vector<int> dp(neg + 1);
        dp[0] = 1;
        for (int& num : nums) {
            for (int j = neg; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        return dp[neg];
    }
};
```

Java版本

```java
// 方法一：回溯
class Solution {
    int count = 0;

    public int findTargetSumWays(int[] nums, int target) {
        backtrack(nums, target, 0, 0);
        return count;
    }

    public void backtrack(int[] nums, int target, int index, int sum) {
        if (index == nums.length) {
            if (sum == target) {
                count++;
            }
        } else {
            backtrack(nums, target, index + 1, sum + nums[index]);
            backtrack(nums, target, index + 1, sum - nums[index]);
        }
    }
}

// 方法二：动态规划
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        int diff = sum - target;
        if (diff < 0 || diff % 2 != 0) {
            return 0;
        }
        int n = nums.length, neg = diff / 2;
        int[][] dp = new int[n + 1][neg + 1];
        dp[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            int num = nums[i - 1];
            for (int j = 0; j <= neg; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j >= num) {
                    dp[i][j] += dp[i - 1][j - num];
                }
            }
        }
        return dp[n][neg];
    }
}

// 或者
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        int diff = sum - target;
        if (diff < 0 || diff % 2 != 0) {
            return 0;
        }
        int neg = diff / 2;
        int[] dp = new int[neg + 1];
        dp[0] = 1;
        for (int num : nums) {
            for (int j = neg; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        return dp[neg];
    }
}
```



### [576. 出界的路径数](https://leetcode.cn/problems/out-of-boundary-paths/)

中等

给你一个大小为 `m x n` 的网格和一个球。球的起始坐标为 `[startRow, startColumn]` 。你可以将球移到在四个方向上相邻的单元格内（可以穿过网格边界到达网格之外）。你 **最多** 可以移动 `maxMove` 次球。

给你五个整数 `m`、`n`、`maxMove`、`startRow` 以及 `startColumn` ，找出并返回可以将球移出边界的路径数量。因为答案可能非常大，返回对 `109 + 7` **取余** 后的结果。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png)

```
输入：m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0
输出：6
```

$$
动态规划：dp[i][j][k] = \begin{cases}
1, &  i = 0 \text{ \&\& } (j,k) = (\text{startRow}, \text{startColumn}) \\
0, &  i = 0 \text{ \&\& } (j,k) \neq (\text{startRow}, \text{startColumn}) \\
\text{dp}[i][j][k] + \text{dp}[i-1][j'][k'], &  0 \leq j' < m \text{ \&\& } 0 \leq k' < n \\
\text{dp}[i][j][k] + 1, & \text{otherwise}
\end{cases}
$$

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    static constexpr int MOD = 1'000'000'007;

    int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        vector<vector<int>> directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        int outCounts = 0;
        vector<vector<vector<int>>> dp(maxMove + 1, vector<vector<int>>(m, vector<int>(n)));
        dp[0][startRow][startColumn] = 1;
        for (int i = 0; i < maxMove; i++) {
            for (int j = 0; j < m; j++) {
                for (int k = 0; k < n; k++) {
                    int count = dp[i][j][k];
                    if (count > 0) {
                        for (auto &direction : directions) {
                            int j1 = j + direction[0], k1 = k + direction[1];
                            if (j1 >= 0 && j1 < m && k1 >= 0 && k1 < n) {
                                dp[i + 1][j1][k1] = (dp[i + 1][j1][k1] + count) % MOD;
                            } else {
                                outCounts = (outCounts + count) % MOD;
                            }
                        }
                    }
                }
            }
        }
        return outCounts;
    }
};

// 或者
class Solution {
public:
    static constexpr int MOD = 1'000'000'007;

    int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        vector<vector<int>> directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        int outCounts = 0;
        vector<vector<int>> dp(m, vector<int>(n));
        dp[startRow][startColumn] = 1;
        for (int i = 0; i < maxMove; i++) {
            vector<vector<int>> dpNew(m, vector<int>(n));
            for (int j = 0; j < m; j++) {
                for (int k = 0; k < n; k++) {
                    int count = dp[j][k];
                    if (count > 0) {
                        for (auto& direction : directions) {
                            int j1 = j + direction[0], k1 = k + direction[1];
                            if (j1 >= 0 && j1 < m && k1 >= 0 && k1 < n) {
                                dpNew[j1][k1] = (dpNew[j1][k1] + count) % MOD;
                            } else {
                                outCounts = (outCounts + count) % MOD;
                            }
                        }
                    }
                }
            }
            dp = dpNew;
        }
        return outCounts;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        final int MOD = 1000000007;
        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        int outCounts = 0;
        int[][][] dp = new int[maxMove + 1][m][n];
        dp[0][startRow][startColumn] = 1;
        for (int i = 0; i < maxMove; i++) {
            for (int j = 0; j < m; j++) {
                for (int k = 0; k < n; k++) {
                    int count = dp[i][j][k];
                    if (count > 0) {
                        for (int[] direction : directions) {
                            int j1 = j + direction[0], k1 = k + direction[1];
                            if (j1 >= 0 && j1 < m && k1 >= 0 && k1 < n) {
                                dp[i + 1][j1][k1] = (dp[i + 1][j1][k1] + count) % MOD;
                            } else {
                                outCounts = (outCounts + count) % MOD;
                            }
                        }
                    }
                }
            }
        }
        return outCounts;
    }
}

// 或者
class Solution {
    public int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        final int MOD = 1000000007;
        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        int outCounts = 0;
        int[][] dp = new int[m][n];
        dp[startRow][startColumn] = 1;
        for (int i = 0; i < maxMove; i++) {
            int[][] dpNew = new int[m][n];
            for (int j = 0; j < m; j++) {
                for (int k = 0; k < n; k++) {
                    int count = dp[j][k];
                    if (count > 0) {
                        for (int[] direction : directions) {
                            int j1 = j + direction[0], k1 = k + direction[1];
                            if (j1 >= 0 && j1 < m && k1 >= 0 && k1 < n) {
                                dpNew[j1][k1] = (dpNew[j1][k1] + count) % MOD;
                            } else {
                                outCounts = (outCounts + count) % MOD;
                            }
                        }
                    }
                }
            }
            dp = dpNew;
        }
        return outCounts;
    }
}
```



### [87. 扰乱字符串](https://leetcode.cn/problems/scramble-string/)

困难

使用下面描述的算法可以扰乱字符串 `s` 得到字符串 `t` ：

1. 如果字符串的长度为 1 ，算法停止
2. 如果字符串的长度 > 1 ，执行下述步骤：
   - 在一个随机下标处将字符串分割成两个非空的子字符串。即，如果已知字符串 `s` ，则可以将其分成两个子字符串 `x` 和 `y` ，且满足 `s = x + y` 。
   - **随机** 决定是要「交换两个子字符串」还是要「保持这两个子字符串的顺序不变」。即，在执行这一步骤之后，`s` 可能是 `s = x + y` 或者 `s = y + x` 。
   - 在 `x` 和 `y` 这两个子字符串上继续从步骤 1 开始递归执行此算法。

给你两个 **长度相等** 的字符串 `s1` 和 `s2`，判断 `s2` 是否是 `s1` 的扰乱字符串。如果是，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：s1 = "great", s2 = "rgeat"
输出：true
解释：s1 上可能发生的一种情形是：
"great" --> "gr/eat" // 在一个随机下标处分割得到两个子字符串
"gr/eat" --> "gr/eat" // 随机决定：「保持这两个子字符串的顺序不变」
"gr/eat" --> "g/r / e/at" // 在子字符串上递归执行此算法。两个子字符串分别在随机下标处进行一轮分割
"g/r / e/at" --> "r/g / e/at" // 随机决定：第一组「交换两个子字符串」，第二组「保持这两个子字符串的顺序不变」
"r/g / e/at" --> "r/g / e/ a/t" // 继续递归执行此算法，将 "at" 分割得到 "a/t"
"r/g / e/ a/t" --> "r/g / e/ a/t" // 随机决定：「保持这两个子字符串的顺序不变」
算法终止，结果字符串和 s2 相同，都是 "rgeat"
这是一种能够扰乱 s1 得到 s2 的情形，可以认为 s2 是 s1 的扰乱字符串，返回 true
```

C++版本

```c++
// 方法一：动态规划
class Solution {
private:
    // 记忆化搜索存储状态的数组
    // -1 表示 false，1 表示 true，0 表示未计算
    int memo[30][30][31];
    string s1, s2;

public:
    bool checkIfSimilar(int i1, int i2, int length) {
        unordered_map<int, int> freq;
        for (int i = i1; i < i1 + length; ++i) {
            ++freq[s1[i]];
        }
        for (int i = i2; i < i2 + length; ++i) {
            --freq[s2[i]];
        }
        if (any_of(freq.begin(), freq.end(), [](const auto& entry) {return entry.second != 0;})) {
            return false;
        }
        return true;
    }

    // 第一个字符串从 i1 开始，第二个字符串从 i2 开始，子串的长度为 length，是否和谐
    bool dfs(int i1, int i2, int length) {
        if (memo[i1][i2][length]) {
            return memo[i1][i2][length] == 1;
        }

        // 判断两个子串是否相等
        if (s1.substr(i1, length) == s2.substr(i2, length)) {
            memo[i1][i2][length] = 1;
            return true;
        }

        // 判断是否存在字符 c 在两个子串中出现的次数不同
        if (!checkIfSimilar(i1, i2, length)) {
            memo[i1][i2][length] = -1;
            return false;
        }
        
        // 枚举分割位置
        for (int i = 1; i < length; ++i) {
            // 不交换的情况
            if (dfs(i1, i2, i) && dfs(i1 + i, i2 + i, length - i)) {
                memo[i1][i2][length] = 1;
                return true;
            }
            // 交换的情况
            if (dfs(i1, i2 + length - i, i) && dfs(i1 + i, i2, length - i)) {
                memo[i1][i2][length] = 1;
                return true;
            }
        }

        memo[i1][i2][length] = -1;
        return false;
    }

    bool isScramble(string s1, string s2) {
        memset(memo, 0, sizeof(memo));
        this->s1 = s1;
        this->s2 = s2;
        return dfs(0, 0, s1.size());
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    // 记忆化搜索存储状态的数组
    // -1 表示 false，1 表示 true，0 表示未计算
    int[][][] memo;
    String s1, s2;

    public boolean isScramble(String s1, String s2) {
        int length = s1.length();
        this.memo = new int[length][length][length + 1];
        this.s1 = s1;
        this.s2 = s2;
        return dfs(0, 0, length);
    }

    // 第一个字符串从 i1 开始，第二个字符串从 i2 开始，子串的长度为 length，是否和谐
    public boolean dfs(int i1, int i2, int length) {
        if (memo[i1][i2][length] != 0) {
            return memo[i1][i2][length] == 1;
        }

        // 判断两个子串是否相等
        if (s1.substring(i1, i1 + length).equals(s2.substring(i2, i2 + length))) {
            memo[i1][i2][length] = 1;
            return true;
        }

        // 判断是否存在字符 c 在两个子串中出现的次数不同
        if (!checkIfSimilar(i1, i2, length)) {
            memo[i1][i2][length] = -1;
            return false;
        }
        
        // 枚举分割位置
        for (int i = 1; i < length; ++i) {
            // 不交换的情况
            if (dfs(i1, i2, i) && dfs(i1 + i, i2 + i, length - i)) {
                memo[i1][i2][length] = 1;
                return true;
            }
            // 交换的情况
            if (dfs(i1, i2 + length - i, i) && dfs(i1 + i, i2, length - i)) {
                memo[i1][i2][length] = 1;
                return true;
            }
        }

        memo[i1][i2][length] = -1;
        return false;
    }

    public boolean checkIfSimilar(int i1, int i2, int length) {
        Map<Character, Integer> freq = new HashMap<Character, Integer>();
        for (int i = i1; i < i1 + length; ++i) {
            char c = s1.charAt(i);
            freq.put(c, freq.getOrDefault(c, 0) + 1);
        }
        for (int i = i2; i < i2 + length; ++i) {
            char c = s2.charAt(i);
            freq.put(c, freq.getOrDefault(c, 0) - 1);
        }
        for (Map.Entry<Character, Integer> entry : freq.entrySet()) {
            int value = entry.getValue();
            if (value != 0) {
                return false;
            }
        }
        return true;
    }
}
```



### [403. 青蛙过河](https://leetcode.cn/problems/frog-jump/)

困难

一只青蛙想要过河。 假定河流被等分为若干个单元格，并且在每一个单元格内都有可能放有一块石子（也有可能没有）。 青蛙可以跳上石子，但是不可以跳入水中。

给你石子的位置列表 `stones`（用单元格序号 **升序** 表示）， 请判定青蛙能否成功过河（即能否在最后一步跳至最后一块石子上）。开始时， 青蛙默认已站在第一块石子上，并可以假定它第一步只能跳跃 `1` 个单位（即只能从单元格 1 跳至单元格 2 ）。

如果青蛙上一步跳跃了 `k` 个单位，那么它接下来的跳跃距离只能选择为 `k - 1`、`k` 或 `k + 1` 个单位。 另请注意，青蛙只能向前方（终点的方向）跳跃。

**示例 1：**

```
输入：stones = [0,1,3,5,6,8,12,17]
输出：true
解释：青蛙可以成功过河，按照如下方案跳跃：跳 1 个单位到第 2 块石子, 然后跳 2 个单位到第 3 块石子, 接着 跳 2 个单位到第 4 块石子, 然后跳 3 个单位到第 6 块石子, 跳 4 个单位到第 7 块石子, 最后，跳 5 个单位到第 8 个石子（即最后一块石子）。
```

C++版本

```c++
// 方法一：记忆化搜索 + 二分查找
class Solution {
public:
    vector<unordered_map<int, int>> rec;

    bool dfs(vector<int>& stones, int i, int lastDis) {
        if (i == stones.size() - 1) {
            return true;
        }
        if (rec[i].count(lastDis)) {
            return rec[i][lastDis];
        }
        for (int curDis = lastDis - 1; curDis <= lastDis + 1; curDis++) {
            if (curDis > 0) {
                int j = lower_bound(stones.begin(), stones.end(), curDis + stones[i]) - stones.begin();
                if (j != stones.size() && stones[j] == curDis + stones[i] && dfs(stones, j, curDis)) {
                    return rec[i][lastDis] = true;
                }
            }
        }
        return rec[i][lastDis] = false;
    }

    bool canCross(vector<int>& stones) {
        int n = stones.size();
        rec.resize(n);
        return dfs(stones, 0, 0);
    }
};

// 方法二：动态规划
class Solution {
public:
    bool canCross(vector<int>& stones) {
        int n = stones.size();
        vector<vector<int>> dp(n, vector<int>(n));
        dp[0][0] = true;
        for (int i = 1; i < n; ++i) {
            if (stones[i] - stones[i - 1] > i) {
                return false;
            }
        }
        for (int i = 1; i < n; ++i) {
            for (int j = i - 1; j >= 0; --j) {
                int k = stones[i] - stones[j];
                if (k > j + 1) {
                    break;
                }
                dp[i][k] = dp[j][k - 1] || dp[j][k] || dp[j][k + 1];
                if (i == n - 1 && dp[i][k]) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

Java版本

```java
// 方法一：记忆化搜索 + 二分查找
class Solution {
    private Boolean[][] rec;

    public boolean canCross(int[] stones) {
        int n = stones.length;
        rec = new Boolean[n][n];
        return dfs(stones, 0, 0);
    }

    private boolean dfs(int[] stones, int i, int lastDis) {
        if (i == stones.length - 1) {
            return true;
        }
        if (rec[i][lastDis] != null) {
            return rec[i][lastDis];
        }

        for (int curDis = lastDis - 1; curDis <= lastDis + 1; curDis++) {
            if (curDis > 0) {
                int j = Arrays.binarySearch(stones, i + 1, stones.length, curDis + stones[i]);
                if (j >= 0 && dfs(stones, j, curDis)) {
                    return rec[i][lastDis] = true;
                }
            }
        }
        return rec[i][lastDis] = false;
    }
}

// 方法二：动态规划
class Solution {
    public boolean canCross(int[] stones) {
        int n = stones.length;
        boolean[][] dp = new boolean[n][n];
        dp[0][0] = true;
        for (int i = 1; i < n; ++i) {
            if (stones[i] - stones[i - 1] > i) {
                return false;
            }
        }
        for (int i = 1; i < n; ++i) {
            for (int j = i - 1; j >= 0; --j) {
                int k = stones[i] - stones[j];
                if (k > j + 1) {
                    break;
                }
                dp[i][k] = dp[j][k - 1] || dp[j][k] || dp[j][k + 1];
                if (i == n - 1 && dp[i][k]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```



### [552. 学生出勤记录 II](https://leetcode.cn/problems/student-attendance-record-ii/)

困难

可以用字符串表示一个学生的出勤记录，其中的每个字符用来标记当天的出勤情况（缺勤、迟到、到场）。记录中只含下面三种字符：

- `'A'`：Absent，缺勤
- `'L'`：Late，迟到
- `'P'`：Present，到场

如果学生能够 **同时** 满足下面两个条件，则可以获得出勤奖励：

- 按 **总出勤** 计，学生缺勤（`'A'`）**严格** 少于两天。
- 学生 **不会** 存在 **连续** 3 天或 **连续** 3 天以上的迟到（`'L'`）记录。

给你一个整数 `n` ，表示出勤记录的长度（次数）。请你返回记录长度为 `n` 时，可能获得出勤奖励的记录情况 **数量** 。答案可能很大，所以返回对 `109 + 7` **取余** 的结果。

**示例 1：**

```
输入：n = 2
输出：8
解释：
有 8 种长度为 2 的记录将被视为可奖励：
"PP" , "AP", "PA", "LP", "PL", "AL", "LA", "LL" 
只有"AA"不会被视为可奖励，因为缺勤次数为 2 次（需要少于 2 次）。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    static constexpr int MOD = 1'000'000'007;

    int checkRecord(int n) {
        vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(2, vector<int>(3)));  // 长度，A 的数量，结尾连续 L 的数量
        dp[0][0][0] = 1;
        for (int i = 1; i <= n; i++) {
            // 以 P 结尾的数量
            for (int j = 0; j <= 1; j++) {
                for (int k = 0; k <= 2; k++) {
                    dp[i][j][0] = (dp[i][j][0] + dp[i - 1][j][k]) % MOD;
                }
            }
            // 以 A 结尾的数量
            for (int k = 0; k <= 2; k++) {
                dp[i][1][0] = (dp[i][1][0] + dp[i - 1][0][k]) % MOD;
            }
            // 以 L 结尾的数量
            for (int j = 0; j <= 1; j++) {
                for (int k = 1; k <= 2; k++) {
                    dp[i][j][k] = (dp[i][j][k] + dp[i - 1][j][k - 1]) % MOD;
                }
            }
        }
        int sum = 0;
        for (int j = 0; j <= 1; j++) {
            for (int k = 0; k <= 2; k++) {
                sum = (sum + dp[n][j][k]) % MOD;
            }
        }
        return sum;
    }
};

// 或者
class Solution {
public:
    static constexpr int MOD = 1'000'000'007;

    int checkRecord(int n) {
        int dp[2][3];  // A 的数量，结尾连续 L 的数量
        memset(dp, 0, sizeof(dp));
        dp[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            int dpNew[2][3];  // A 的数量，结尾连续 L 的数量
            memset(dpNew, 0, sizeof(dpNew));
            // 以 P 结尾的数量
            for (int j = 0; j <= 1; j++) {
                for (int k = 0; k <= 2; k++) {
                    dpNew[j][0] = (dpNew[j][0] + dp[j][k]) % MOD;
                }
            }
            // 以 A 结尾的数量
            for (int k = 0; k <= 2; k++) {
                dpNew[1][0] = (dpNew[1][0] + dp[0][k]) % MOD;
            }
            // 以 L 结尾的数量
            for (int j = 0; j <= 1; j++) {
                for (int k = 1; k <= 2; k++) {
                    dpNew[j][k] = (dpNew[j][k] + dp[j][k - 1]) % MOD;
                }
            }
            memcpy(dp, dpNew, sizeof(dp));
        }
        int sum = 0;
        for (int j = 0; j <= 1; j++) {
            for (int k = 0; k <= 2; k++) {
                sum = (sum + dp[j][k]) % MOD;
            }
        }
        return sum;
    }
};

// 方法二：矩阵快速幂
class Solution {
public:
    static constexpr int MOD = 1'000'000'007;

    vector<vector<long>> pow(vector<vector<long>> mat, int n) {
        vector<vector<long>> ret = {{1, 0, 0, 0, 0, 0}};
        while (n > 0) {
            if ((n & 1) == 1) {
                ret = multiply(ret, mat);
            }
            n >>= 1;
            mat = multiply(mat, mat);
        }
        return ret;
    }

    vector<vector<long>> multiply(vector<vector<long>> a, vector<vector<long>> b) {
        int rows = a.size(), columns = b[0].size(), temp = b.size();
        vector<vector<long>> c(rows, vector<long>(columns));
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                for (int k = 0; k < temp; k++) {
                    c[i][j] += a[i][k] * b[k][j];
                    c[i][j] %= MOD;
                }
            }
        }
        return c;
    }

    int checkRecord(int n) {
        vector<vector<long>> mat = {{1, 1, 0, 1, 0, 0}, {1, 0, 1, 1, 0, 0}, {1, 0, 0, 1, 0, 0}, {0, 0, 0, 1, 1, 0}, {0, 0, 0, 1, 0, 1}, {0, 0, 0, 1, 0, 0}};
        vector<vector<long>> res = pow(mat, n);
        long sum = accumulate(res[0].begin(), res[0].end(), 0ll);
        return (int)(sum % MOD);
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int checkRecord(int n) {
        final int MOD = 1000000007;
        int[][][] dp = new int[n + 1][2][3]; // 长度，A 的数量，结尾连续 L 的数量
        dp[0][0][0] = 1;
        for (int i = 1; i <= n; i++) {
            // 以 P 结尾的数量
            for (int j = 0; j <= 1; j++) {
                for (int k = 0; k <= 2; k++) {
                    dp[i][j][0] = (dp[i][j][0] + dp[i - 1][j][k]) % MOD;
                }
            }
            // 以 A 结尾的数量
            for (int k = 0; k <= 2; k++) {
                dp[i][1][0] = (dp[i][1][0] + dp[i - 1][0][k]) % MOD;
            }
            // 以 L 结尾的数量
            for (int j = 0; j <= 1; j++) {
                for (int k = 1; k <= 2; k++) {
                    dp[i][j][k] = (dp[i][j][k] + dp[i - 1][j][k - 1]) % MOD;
                }
            }
        }
        int sum = 0;
        for (int j = 0; j <= 1; j++) {
            for (int k = 0; k <= 2; k++) {
                sum = (sum + dp[n][j][k]) % MOD;
            }
        }
        return sum;
    }
}

// 或者
class Solution {
    public int checkRecord(int n) {
        final int MOD = 1000000007;
        int[][] dp = new int[2][3]; // A 的数量，结尾连续 L 的数量
        dp[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            int[][] dpNew = new int[2][3];
            // 以 P 结尾的数量
            for (int j = 0; j <= 1; j++) {
                for (int k = 0; k <= 2; k++) {
                    dpNew[j][0] = (dpNew[j][0] + dp[j][k]) % MOD;
                }
            }
            // 以 A 结尾的数量
            for (int k = 0; k <= 2; k++) {
                dpNew[1][0] = (dpNew[1][0] + dp[0][k]) % MOD;
            }
            // 以 L 结尾的数量
            for (int j = 0; j <= 1; j++) {
                for (int k = 1; k <= 2; k++) {
                    dpNew[j][k] = (dpNew[j][k] + dp[j][k - 1]) % MOD;
                }
            }
            dp = dpNew;
        }
        int sum = 0;
        for (int j = 0; j <= 1; j++) {
            for (int k = 0; k <= 2; k++) {
                sum = (sum + dp[j][k]) % MOD;
            }
        }
        return sum;
    }
}

// 方法二：矩阵快速幂
class Solution {
    static final int MOD = 1000000007;

    public int checkRecord(int n) {
        long[][] mat = {{1, 1, 0, 1, 0, 0},
                        {1, 0, 1, 1, 0, 0},
                        {1, 0, 0, 1, 0, 0},
                        {0, 0, 0, 1, 1, 0},
                        {0, 0, 0, 1, 0, 1},
                        {0, 0, 0, 1, 0, 0}};
        long[][] res = pow(mat, n);
        long sum = Arrays.stream(res[0]).sum();
        return (int) (sum % MOD);
    }

    public long[][] pow(long[][] mat, int n) {
        long[][] ret = {{1, 0, 0, 0, 0, 0}};
        while (n > 0) {
            if ((n & 1) == 1) {
                ret = multiply(ret, mat);
            }
            n >>= 1;
            mat = multiply(mat, mat);
        }
        return ret;
    }

    public long[][] multiply(long[][] a, long[][] b) {
        int rows = a.length, columns = b[0].length, temp = b.length;
        long[][] c = new long[rows][columns];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                for (int k = 0; k < temp; k++) {
                    c[i][j] += a[i][k] * b[k][j];
                    c[i][j] %= MOD;
                }
            }
        }
        return c;
    }
}
```



### [913. 猫和老鼠](https://leetcode.cn/problems/cat-and-mouse/)

困难

两位玩家分别扮演猫和老鼠，在一张 **无向** 图上进行游戏，两人轮流行动。

图的形式是：`graph[a]` 是一个列表，由满足 `ab` 是图中的一条边的所有节点 `b` 组成。

老鼠从节点 `1` 开始，第一个出发；猫从节点 `2` 开始，第二个出发。在节点 `0` 处有一个洞。

在每个玩家的行动中，他们 **必须** 沿着图中与所在当前位置连通的一条边移动。例如，如果老鼠在节点 `1` ，那么它必须移动到 `graph[1]` 中的任一节点。

此外，猫无法移动到洞中（节点 `0`）。

然后，游戏在出现以下三种情形之一时结束：

- 如果猫和老鼠出现在同一个节点，猫获胜。
- 如果老鼠到达洞中，老鼠获胜。
- 如果某一位置重复出现（即，玩家的位置和移动顺序都与上一次行动相同），游戏平局。

给你一张图 `graph` ，并假设两位玩家都都以最佳状态参与游戏：

- 如果老鼠获胜，则返回 `1`；
- 如果猫获胜，则返回 `2`；
- 如果平局，则返回 `0` 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/17/cat1.jpg)

```
输入：graph = [[2,5],[3],[0,4,5],[1,4,5],[2,3],[0,2,3]]
输出：0
```

C++版本

```c++
// 方法一：拓扑排序
class Solution {
public:
    const int MOUSE_TURN = 0, CAT_TURN = 1;
    const int DRAW = 0, MOUSE_WIN = 1, CAT_WIN = 2;
    vector<vector<int>> graph;
    vector<vector<vector<int>>> degrees;
    vector<vector<vector<int>>> results;

    int catMouseGame(vector<vector<int>>& graph) {
        int n = graph.size();
        this->graph = graph;
        this->degrees = vector<vector<vector<int>>>(n, vector<vector<int>>(n, vector<int>(2)));
        this->results = vector<vector<vector<int>>>(n, vector<vector<int>>(n, vector<int>(2)));
        queue<tuple<int, int, int>> qu;

        for (int i = 0; i < n; i++) {
            for (int j = 1; j < n; j++) {
                degrees[i][j][MOUSE_TURN] = graph[i].size();
                degrees[i][j][CAT_TURN] = graph[j].size();
            }
        }
        for (int node : graph[0]) {
            for (int i = 0; i < n; i++) {
                degrees[i][node][CAT_TURN]--;
            }
        }
        for (int j = 1; j < n; j++) {
            results[0][j][MOUSE_TURN] = MOUSE_WIN;
            results[0][j][CAT_TURN] = MOUSE_WIN;
            qu.emplace(0, j, MOUSE_TURN);
            qu.emplace(0, j, CAT_TURN);
        }
        for (int i = 1; i < n; i++) {
            results[i][i][MOUSE_TURN] = CAT_WIN;
            results[i][i][CAT_TURN] = CAT_WIN;
            qu.emplace(i, i, MOUSE_TURN);
            qu.emplace(i, i, CAT_TURN);
        }
        while (!qu.empty()) {
            auto [mouse, cat, turn] = qu.front();
            qu.pop();
            int result = results[mouse][cat][turn];
            vector<tuple<int, int, int>> prevStates = GetPrevStates(mouse, cat, turn);
            for (auto & [prevMouse, prevCat, prevTurn] : prevStates) {
                if (results[prevMouse][prevCat][prevTurn] == DRAW) {
                    bool canWin = (result == MOUSE_WIN && prevTurn == MOUSE_TURN) || (result == CAT_WIN && prevTurn == CAT_TURN);
                    if (canWin) {
                        results[prevMouse][prevCat][prevTurn] = result;
                        qu.emplace(prevMouse, prevCat, prevTurn);
                    } else if (--degrees[prevMouse][prevCat][prevTurn] == 0) {
                        int loseResult = prevTurn == MOUSE_TURN ? CAT_WIN : MOUSE_WIN;
                        results[prevMouse][prevCat][prevTurn] = loseResult;
                        qu.emplace(prevMouse, prevCat, prevTurn);
                    }
                }
            }
        }
        return results[1][2][MOUSE_TURN];
    }

    vector<tuple<int, int, int>> GetPrevStates(int mouse, int cat, int turn) {
        vector<tuple<int, int, int>> prevStates;
        int prevTurn = turn == MOUSE_TURN ? CAT_TURN : MOUSE_TURN;
        if (prevTurn == MOUSE_TURN) {
            for (int & prev : graph[mouse]) {
                prevStates.emplace_back(prev, cat, prevTurn);
            }
        } else {
            for (int & prev : graph[cat]) {
                if (prev != 0) {
                    prevStates.emplace_back(mouse, prev, prevTurn);
                }
            }
        }
        return prevStates;
    }
};
```

Java版本

```java
// 方法一：拓扑排序
class Solution {
    static final int MOUSE_TURN = 0, CAT_TURN = 1;
    static final int DRAW = 0, MOUSE_WIN = 1, CAT_WIN = 2;
    int[][] graph;
    int[][][] degrees;
    int[][][] results;

    public int catMouseGame(int[][] graph) {
        int n = graph.length;
        this.graph = graph;
        this.degrees = new int[n][n][2];
        this.results = new int[n][n][2];
        Queue<int[]> queue = new ArrayDeque<int[]>();
        for (int i = 0; i < n; i++) {
            for (int j = 1; j < n; j++) {
                degrees[i][j][MOUSE_TURN] = graph[i].length;
                degrees[i][j][CAT_TURN] = graph[j].length;
            }
        }
        for (int node : graph[0]) {
            for (int i = 0; i < n; i++) {
                degrees[i][node][CAT_TURN]--;
            }
        }
        for (int j = 1; j < n; j++) {
            results[0][j][MOUSE_TURN] = MOUSE_WIN;
            results[0][j][CAT_TURN] = MOUSE_WIN;
            queue.offer(new int[]{0, j, MOUSE_TURN});
            queue.offer(new int[]{0, j, CAT_TURN});
        }
        for (int i = 1; i < n; i++) {
            results[i][i][MOUSE_TURN] = CAT_WIN;
            results[i][i][CAT_TURN] = CAT_WIN;
            queue.offer(new int[]{i, i, MOUSE_TURN});
            queue.offer(new int[]{i, i, CAT_TURN});
        }
        while (!queue.isEmpty()) {
            int[] state = queue.poll();
            int mouse = state[0], cat = state[1], turn = state[2];
            int result = results[mouse][cat][turn];
            List<int[]> prevStates = getPrevStates(mouse, cat, turn);
            for (int[] prevState : prevStates) {
                int prevMouse = prevState[0], prevCat = prevState[1], prevTurn = prevState[2];
                if (results[prevMouse][prevCat][prevTurn] == DRAW) {
                    boolean canWin = (result == MOUSE_WIN && prevTurn == MOUSE_TURN) || (result == CAT_WIN && prevTurn == CAT_TURN);
                    if (canWin) {
                        results[prevMouse][prevCat][prevTurn] = result;
                        queue.offer(new int[]{prevMouse, prevCat, prevTurn});
                    } else {
                        degrees[prevMouse][prevCat][prevTurn]--;
                        if (degrees[prevMouse][prevCat][prevTurn] == 0) {
                            int loseResult = prevTurn == MOUSE_TURN ? CAT_WIN : MOUSE_WIN;
                            results[prevMouse][prevCat][prevTurn] = loseResult;
                            queue.offer(new int[]{prevMouse, prevCat, prevTurn});
                        }
                    }
                }
            }
        }
        return results[1][2][MOUSE_TURN];
    }

    public List<int[]> getPrevStates(int mouse, int cat, int turn) {
        List<int[]> prevStates = new ArrayList<int[]>();
        int prevTurn = turn == MOUSE_TURN ? CAT_TURN : MOUSE_TURN;
        if (prevTurn == MOUSE_TURN) {
            for (int prev : graph[mouse]) {
                prevStates.add(new int[]{prev, cat, prevTurn});
            }
        } else {
            for (int prev : graph[cat]) {
                if (prev != 0) {
                    prevStates.add(new int[]{mouse, prev, prevTurn});
                }
            }
        }
        return prevStates;
    }
}
```



### [329. 矩阵中的最长递增路径](https://leetcode.cn/problems/longest-increasing-path-in-a-matrix/)

困难

给定一个 `m x n` 整数矩阵 `matrix` ，找出其中 **最长递增路径** 的长度。

对于每个单元格，你可以往上，下，左，右四个方向移动。 你 **不能** 在 **对角线** 方向上移动或移动到 **边界外**（即不允许环绕）。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

```
输入：matrix = [[9,9,4],[6,6,8],[2,1,1]]
输出：4 
解释：最长递增路径为 [1, 2, 6, 9]。
```

C++版本

```c++
// 方法一：记忆化深度优先搜索
class Solution {
public:
    static constexpr int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    int rows, columns;

    int longestIncreasingPath(vector< vector<int> > &matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return 0;
        }
        rows = matrix.size();
        columns = matrix[0].size();
        auto memo = vector< vector<int> > (rows, vector <int> (columns));
        int ans = 0;
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < columns; ++j) {
                ans = max(ans, dfs(matrix, i, j, memo));
            }
        }
        return ans;
    }

    int dfs(vector< vector<int> > &matrix, int row, int column, vector< vector<int> > &memo) {
        if (memo[row][column] != 0) {
            return memo[row][column];
        }
        ++memo[row][column];
        for (int i = 0; i < 4; ++i) {
            int newRow = row + dirs[i][0], newColumn = column + dirs[i][1];
            if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns && matrix[newRow][newColumn] > matrix[row][column]) {
                memo[row][column] = max(memo[row][column], dfs(matrix, newRow, newColumn, memo) + 1);
            }
        }
        return memo[row][column];
    }
};

// 方法二：拓扑排序
class Solution {
public:
    static constexpr int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    int rows, columns;

    int longestIncreasingPath(vector< vector<int> > &matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return 0;
        }
        rows = matrix.size();
        columns = matrix[0].size();
        auto outdegrees = vector< vector<int> > (rows, vector <int> (columns));
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < columns; ++j) {
                for (int k = 0; k < 4; ++k) {
                    int newRow = i + dirs[k][0], newColumn = j + dirs[k][1];
                    if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns && matrix[newRow][newColumn] > matrix[i][j]) {
                        ++outdegrees[i][j];
                    }
                }
            }
        }
        queue < pair<int, int> > q;
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < columns; ++j) {
                if (outdegrees[i][j] == 0) {
                    q.push({i, j});
                }
            }
        }
        int ans = 0;
        while (!q.empty()) {
            ++ans;
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                auto cell = q.front(); q.pop();
                int row = cell.first, column = cell.second;
                for (int k = 0; k < 4; ++k) {
                    int newRow = row + dirs[k][0], newColumn = column + dirs[k][1];
                    if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns && matrix[newRow][newColumn] < matrix[row][column]) {
                        --outdegrees[newRow][newColumn];
                        if (outdegrees[newRow][newColumn] == 0) {
                            q.push({newRow, newColumn});
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：记忆化深度优先搜索
class Solution {
    public int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    public int rows, columns;

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        rows = matrix.length;
        columns = matrix[0].length;
        int[][] memo = new int[rows][columns];
        int ans = 0;
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < columns; ++j) {
                ans = Math.max(ans, dfs(matrix, i, j, memo));
            }
        }
        return ans;
    }

    public int dfs(int[][] matrix, int row, int column, int[][] memo) {
        if (memo[row][column] != 0) {
            return memo[row][column];
        }
        ++memo[row][column];
        for (int[] dir : dirs) {
            int newRow = row + dir[0], newColumn = column + dir[1];
            if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns && matrix[newRow][newColumn] > matrix[row][column]) {
                memo[row][column] = Math.max(memo[row][column], dfs(matrix, newRow, newColumn, memo) + 1);
            }
        }
        return memo[row][column];
    }
}

// 方法二：拓扑排序
class Solution {
    public int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    public int rows, columns;

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        rows = matrix.length;
        columns = matrix[0].length;
        int[][] outdegrees = new int[rows][columns];
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < columns; ++j) {
                for (int[] dir : dirs) {
                    int newRow = i + dir[0], newColumn = j + dir[1];
                    if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns && matrix[newRow][newColumn] > matrix[i][j]) {
                        ++outdegrees[i][j];
                    }
                }
            }
        }
        Queue<int[]> queue = new LinkedList<int[]>();
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < columns; ++j) {
                if (outdegrees[i][j] == 0) {
                    queue.offer(new int[]{i, j});
                }
            }
        }
        int ans = 0;
        while (!queue.isEmpty()) {
            ++ans;
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                int[] cell = queue.poll();
                int row = cell[0], column = cell[1];
                for (int[] dir : dirs) {
                    int newRow = row + dir[0], newColumn = column + dir[1];
                    if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns && matrix[newRow][newColumn] < matrix[row][column]) {
                        --outdegrees[newRow][newColumn];
                        if (outdegrees[newRow][newColumn] == 0) {
                            queue.offer(new int[]{newRow, newColumn});
                        }
                    }
                }
            }
        }
        return ans;
    }
}
```

