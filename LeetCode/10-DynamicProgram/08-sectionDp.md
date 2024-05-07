# 动态规划

## 区间Dp问题

常见的区间 DP 问题可以分为两种：

1. 单个区间从中间向两侧更大区间转移的区间 DP 问题。比如从区间 [i+1，j−1] 转移到更大区间 [i，j]
2. 多个小区间转移到大区间的区间 DP 问题。比如从区间 [i，k] 和区间 [k，j] 转移到区间 [i，j]。

C++版本

```c++
// 单个区间从中间向两侧更大区间转移的区间 DP 问题
class Solution {
public:
    int sectionDpvector<vector<int>>& dp, vector<vector<int>>& cost) {
		int size = dp.size();
    	for (int i = size - 1; i >= 0; --i) {    // 枚举区间起点
        	for (int j = i + 1; j < size; ++j) { // 枚举区间终点
            	// 状态转移方程，计算转移到更大区间后的最优值
            	dp[i][j] = max({dp[i + 1][j - 1], dp[i + 1][j], dp[i][j - 1]}) + cost[i][j];
        	}
    	}
    }
};

// 多个小区间转移到大区间的区间 DP 问题
class Solution {
public:
    int sectionDp(vector<vector<int>>& dp, vector<vector<int>>& cost) {
		int n = dp.size();
    
    	for (int l = 1; l < n; ++l) {          // 枚举区间长度
        	for (int i = 0; i < n; ++i) {      // 枚举区间起点
            	int j = i + l - 1;             // 根据起点和长度得到终点
            	if (j >= n) {
                	break;
            	}
            	dp[i][j] = INT_MIN;            // 初始化 dp[i][j]
            	for (int k = i; k <= j; ++k) { // 枚举区间分割点
                	// 状态转移方程，计算合并区间后的最优值
                	dp[i][j] = max(dp[i][j], dp[i][k] + dp[k + 1][j] + cost[i][j]);
            	}
        	}
    	}
    }
};

```

Java版本

```java
// 单个区间从中间向两侧更大区间转移的区间 DP 问题
class Solution {
    public int sectionDp(int[][] dp, int[][] cost) {
        // 假设 dp 和 cost 是已经定义好的二维数组
        int size = dp.length;
        
        for (int i = size - 1; i >= 0; --i) {     // 枚举区间起点
            for (int j = i + 1; j < size; ++j) {  // 枚举区间终点
                // 状态转移方程，计算转移到更大区间后的最优值
                dp[i][j] = Math.max(Math.max(dp[i + 1][j - 1], dp[i + 1][j]), dp[i][j - 1]) + cost[i][j];
            }
        }
    }
}

// 多个小区间转移到大区间的区间 DP 问题
class Solution {
    public int sectionDp(int[][] dp, int[][] cost) {
        int n = dp.length;
        
        for (int l = 1; l < n; ++l) {          // 枚举区间长度
            for (int i = 0; i < n; ++i) {      // 枚举区间起点
                int j = i + l - 1;             // 根据起点和长度得到终点
                if (j >= n) {
                    break;
                }
                dp[i][j] = Integer.MIN_VALUE;  // 初始化 dp[i][j]
                for (int k = i; k <= j; ++k) { // 枚举区间分割点
                    // 状态转移方程，计算合并区间后的最优值
                    dp[i][j] = Math.max(dp[i][j], dp[i][k] + dp[k + 1][j] + cost[i][j]);
                }
            }
        }
    }
}
```



### [486. 预测赢家](https://leetcode.cn/problems/predict-the-winner/)

中等

给你一个整数数组 `nums` 。玩家 1 和玩家 2 基于这个数组设计了一个游戏。

玩家 1 和玩家 2 轮流进行自己的回合，玩家 1 先手。开始时，两个玩家的初始分值都是 `0` 。每一回合，玩家从数组的任意一端取一个数字（即，`nums[0]` 或 `nums[nums.length - 1]`），取到的数字将会从数组中移除（数组长度减 `1` ）。玩家选中的数字将会加到他的得分上。当数组中没有剩余数字可取时，游戏结束。

如果玩家 1 能成为赢家，返回 `true` 。如果两个玩家得分相等，同样认为玩家 1 是游戏的赢家，也返回 `true` 。你可以假设每个玩家的玩法都会使他的分数最大化。

**示例 1：**

```
输入：nums = [1,5,2]
输出：false
解释：一开始，玩家 1 可以从 1 和 2 中进行选择。
如果他选择 2（或者 1 ），那么玩家 2 可以从 1（或者 2 ）和 5 中进行选择。如果玩家 2 选择了 5 ，那么玩家 1 则只剩下 1（或者 2 ）可选。 
所以，玩家 1 的最终分数为 1 + 2 = 3，而玩家 2 为 5 。
因此，玩家 1 永远不会成为赢家，返回 false 。
```

C++版本

```c++
// 方法一：递归
class Solution {
public:
    bool predictTheWinner(vector<int>& nums) {
        return total(nums, 0, nums.size() - 1, 1) >= 0;
    }

    int total(vector<int>& nums, int start, int end, int turn) {
        if (start == end) {
            return nums[start] * turn;
        }
        int scoreStart = nums[start] * turn + total(nums, start + 1, end, -turn);
        int scoreEnd = nums[end] * turn + total(nums, start, end - 1, -turn);
        return max(scoreStart * turn, scoreEnd * turn) * turn;
    }
};

// 方法二：动态规划
class Solution {
public:
    bool predictTheWinner(vector<int>& nums) {
        int length = nums.size();
        auto dp = vector<vector<int>> (length, vector<int>(length));
        for (int i = 0; i < length; i++) {
            dp[i][i] = nums[i];
        }
        for (int i = length - 2; i >= 0; i--) {
            for (int j = i + 1; j < length; j++) {
                dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
            }
        }
        return dp[0][length - 1] >= 0;
    }
};

// 优化
class Solution {
public:
    bool predictTheWinner(vector<int>& nums) {
        int length = nums.size();
        auto dp = vector<int>(length);
        for (int i = 0; i < length; i++) {
            dp[i] = nums[i];
        }
        for (int i = length - 2; i >= 0; i--) {
            for (int j = i + 1; j < length; j++) {
                dp[j] = max(nums[i] - dp[j], nums[j] - dp[j - 1]);
            }
        }
        return dp[length - 1] >= 0;
    }
};
```

Java版本

```java
// 方法一：递归
class Solution {
    public boolean predictTheWinner(int[] nums) {
        return total(nums, 0, nums.length - 1, 1) >= 0;
    }

    public int total(int[] nums, int start, int end, int turn) {
        if (start == end) {
            return nums[start] * turn;
        }
        int scoreStart = nums[start] * turn + total(nums, start + 1, end, -turn);
        int scoreEnd = nums[end] * turn + total(nums, start, end - 1, -turn);
        return Math.max(scoreStart * turn, scoreEnd * turn) * turn;
    }
}

// 方法二：动态规划
class Solution {
    public boolean predictTheWinner(int[] nums) {
        int length = nums.length;
        int[][] dp = new int[length][length];
        for (int i = 0; i < length; i++) {
            dp[i][i] = nums[i];
        }
        for (int i = length - 2; i >= 0; i--) {
            for (int j = i + 1; j < length; j++) {
                dp[i][j] = Math.max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
            }
        }
        return dp[0][length - 1] >= 0;
    }
}

// 优化
class Solution {
    public boolean predictTheWinner(int[] nums) {
        int length = nums.length;
        int[] dp = new int[length];
        for (int i = 0; i < length; i++) {
            dp[i] = nums[i];
        }

        for (int i = length - 2; i >= 0; i--) {
            for (int j = i + 1; j < length; j++) {
                dp[j] = Math.max(nums[i] - dp[j], nums[j] - dp[j - 1]);
            }
        }
        
        return dp[length - 1] >= 0;
    }
}
```



### [312. 戳气球](https://leetcode.cn/problems/burst-balloons/)

困难

有 `n` 个气球，编号为`0` 到 `n - 1`，每个气球上都标有一个数字，这些数字存在数组 `nums` 中。

现在要求你戳破所有的气球。戳破第 `i` 个气球，你可以获得 `nums[i - 1] * nums[i] * nums[i + 1]` 枚硬币。 这里的 `i - 1` 和 `i + 1` 代表和 `i` 相邻的两个气球的序号。如果 `i - 1`或 `i + 1` 超出了数组的边界，那么就当它是一个数字为 `1` 的气球。

求所能获得硬币的最大数量。

**示例 1：**

```
输入：nums = [3,1,5,8]
输出：167
解释：
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

C++版本

```c++
// 方法一：记忆化搜索
class Solution {
public:
    vector<vector<int>> rec;
    vector<int> val;

public:
    int solve(int left, int right) {
        if (left >= right - 1) {
            return 0;
        }
        if (rec[left][right] != -1) {
            return rec[left][right];
        }
        for (int i = left + 1; i < right; i++) {
            int sum = val[left] * val[i] * val[right];
            sum += solve(left, i) + solve(i, right);
            rec[left][right] = max(rec[left][right], sum);
        }
        return rec[left][right];
    }

    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        val.resize(n + 2);
        for (int i = 1; i <= n; i++) {
            val[i] = nums[i - 1];
        }
        val[0] = val[n + 1] = 1;
        rec.resize(n + 2, vector<int>(n + 2, -1));
        return solve(0, n + 1);
    }
};

// 方法二：动态规划
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> rec(n + 2, vector<int>(n + 2));
        vector<int> val(n + 2);
        val[0] = val[n + 1] = 1;
        for (int i = 1; i <= n; i++) {
            val[i] = nums[i - 1];
        }
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 2; j <= n + 1; j++) {
                for (int k = i + 1; k < j; k++) {
                    int sum = val[i] * val[k] * val[j];
                    sum += rec[i][k] + rec[k][j];
                    rec[i][j] = max(rec[i][j], sum);
                }
            }
        }
        return rec[0][n + 1];
    }
};
```

Java版本

```java
// 方法一：记忆化搜索
class Solution {
    public int[][] rec;
    public int[] val;

    public int maxCoins(int[] nums) {
        int n = nums.length;
        val = new int[n + 2];
        for (int i = 1; i <= n; i++) {
            val[i] = nums[i - 1];
        }
        val[0] = val[n + 1] = 1;
        rec = new int[n + 2][n + 2];
        for (int i = 0; i <= n + 1; i++) {
            Arrays.fill(rec[i], -1);
        }
        return solve(0, n + 1);
    }

    public int solve(int left, int right) {
        if (left >= right - 1) {
            return 0;
        }
        if (rec[left][right] != -1) {
            return rec[left][right];
        }
        for (int i = left + 1; i < right; i++) {
            int sum = val[left] * val[i] * val[right];
            sum += solve(left, i) + solve(i, right);
            rec[left][right] = Math.max(rec[left][right], sum);
        }
        return rec[left][right];
    }
}

// 方法二：动态规划
class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;
        int[][] rec = new int[n + 2][n + 2];
        int[] val = new int[n + 2];
        val[0] = val[n + 1] = 1;
        for (int i = 1; i <= n; i++) {
            val[i] = nums[i - 1];
        }
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 2; j <= n + 1; j++) {
                for (int k = i + 1; k < j; k++) {
                    int sum = val[i] * val[k] * val[j];
                    sum += rec[i][k] + rec[k][j];
                    rec[i][j] = Math.max(rec[i][j], sum);
                }
            }
        }
        return rec[0][n + 1];
    }
}
```



### [877. 石子游戏](https://leetcode.cn/problems/stone-game/)

中等

Alice 和 Bob 用几堆石子在做游戏。一共有偶数堆石子，**排成一行**；每堆都有 **正** 整数颗石子，数目为 `piles[i]` 。

游戏以谁手中的石子最多来决出胜负。石子的 **总数** 是 **奇数** ，所以没有平局。

Alice 和 Bob 轮流进行，**Alice 先开始** 。 每回合，玩家从行的 **开始** 或 **结束** 处取走整堆石头。 这种情况一直持续到没有更多的石子堆为止，此时手中 **石子最多** 的玩家 **获胜** 。

假设 Alice 和 Bob 都发挥出最佳水平，当 Alice 赢得比赛时返回 `true` ，当 Bob 赢得比赛时返回 `false` 。

**示例 1：**

```
输入：piles = [5,3,4,5]
输出：true
解释：
Alice 先开始，只能拿前 5 颗或后 5 颗石子 。
假设他取了前 5 颗，这一行就变成了 [3,4,5] 。
如果 Bob 拿走前 3 颗，那么剩下的是 [4,5]，Alice 拿走后 5 颗赢得 10 分。
如果 Bob 拿走后 5 颗，那么剩下的是 [3,4]，Alice 拿走后 4 颗赢得 9 分。
这表明，取前 5 颗石子对 Alice 来说是一个胜利的举动，所以返回 true 。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int length = piles.size();
        auto dp = vector<vector<int>>(length, vector<int>(length));
        for (int i = 0; i < length; i++) {
            dp[i][i] = piles[i];
        }
        for (int i = length - 2; i >= 0; i--) {
            for (int j = i + 1; j < length; j++) {
                dp[i][j] = max(piles[i] - dp[i + 1][j], piles[j] - dp[i][j - 1]);
            }
        }
        return dp[0][length - 1] > 0;
    }
};

// 优化
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int length = piles.size();
        auto dp = vector<int>(length);
        for (int i = 0; i < length; i++) {
            dp[i] = piles[i];
        }
        for (int i = length - 2; i >= 0; i--) {
            for (int j = i + 1; j < length; j++) {
                dp[j] = max(piles[i] - dp[j], piles[j] - dp[j - 1]);
            }
        }
        return dp[length - 1] > 0;
    }
};

// 方法二：数学
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        return true;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public boolean stoneGame(int[] piles) {
        int length = piles.length;
        int[][] dp = new int[length][length];
        for (int i = 0; i < length; i++) {
            dp[i][i] = piles[i];
        }
        for (int i = length - 2; i >= 0; i--) {
            for (int j = i + 1; j < length; j++) {
                dp[i][j] = Math.max(piles[i] - dp[i + 1][j], piles[j] - dp[i][j - 1]);
            }
        }
        return dp[0][length - 1] > 0;
    }
}

// 优化
class Solution {
    public boolean stoneGame(int[] piles) {
        int length = piles.length;
        int[] dp = new int[length];
        for (int i = 0; i < length; i++) {
            dp[i] = piles[i];
        }
        for (int i = length - 2; i >= 0; i--) {
            for (int j = i + 1; j < length; j++) {
                dp[j] = Math.max(piles[i] - dp[j], piles[j] - dp[j - 1]);
            }
        }
        return dp[length - 1] > 0;
    }
}

// 方法二：数学
class Solution {
    public boolean stoneGame(int[] piles) {
        return true;
    }
}
```



### [1000. 合并石头的最低成本](https://leetcode.cn/problems/minimum-cost-to-merge-stones/)

困难

有 `n` 堆石头排成一排，第 `i` 堆中有 `stones[i]` 块石头。

每次 **移动** 需要将 **连续的** `k` 堆石头合并为一堆，而这次移动的成本为这 `k` 堆中石头的总数。

返回把所有石头合并成一堆的最低成本。如果无法合并成一堆，返回 `-1` 。

**示例 1：**

```
输入：stones = [3,2,4,1], K = 2
输出：20
解释：
从 [3, 2, 4, 1] 开始。
合并 [3, 2]，成本为 5，剩下 [5, 4, 1]。
合并 [4, 1]，成本为 5，剩下 [5, 5]。
合并 [5, 5]，成本为 10，剩下 [10]。
总成本 20，这是可能的最小值。
```

C++版本

```c++
// 方法一：动态规划
// 记忆化搜索
class Solution {
    static constexpr int inf = 0x3f3f3f3f;
    vector<vector<vector<int>>> d;
    vector<int> sum;
    int k;
public:
    int get(int l, int r, int t) {
        // 若 d[l][r][t] 不为 -1，表示已经在之前的递归被求解过，直接返回答案
        if (d[l][r][t] != -1) {
            return d[l][r][t];
        }
        // 当石头堆数小于 t 时，一定无解
        if (t > r - l + 1) {
            return inf;
        }
        if (t == 1) {
            int res = get(l, r, k);
            if (res == inf) {
                return d[l][r][t] = inf;
            }
            return d[l][r][t] = res + (sum[r] - (l == 0 ? 0 : sum[l - 1]));
        }
        int val = inf;
        for (int p = l; p < r; p += (k - 1)) {
            val = min(val, get(l, p, 1) + get(p + 1, r, t - 1));
        }
        return d[l][r][t] = val;
    }
    int mergeStones(vector<int>& stones, int k) {
        int n = stones.size();
        if ((n - 1) % (k - 1) != 0) {
            return -1;
        }
        this->k = k;
        d = vector(n, vector(n, vector<int>(k + 1, -1)));
        sum = vector<int>(n, 0);

        // 初始化
        for (int i = 0, s = 0; i < n; i++) {
            d[i][i][1] = 0;
            s += stones[i];
            sum[i] = s;
        }
        int res = get(0, n - 1, 1);
        return res;
    }
};

// 递推
class Solution {
    static constexpr int inf = 0x3f3f3f3f;
public:
    int mergeStones(vector<int>& stones, int k) {
        int n = stones.size();
        if ((n - 1) % (k - 1) != 0) {
            return -1;
        }

        vector d(n, vector(n, vector<int>(k + 1, inf)));
        vector<int> sum(n, 0);

        for (int i = 0, s = 0; i < n; i++) {
            d[i][i][1] = 0;
            s += stones[i];
            sum[i] = s;
        }

        for (int len = 2; len <= n; len++) {
            for (int l = 0; l < n && l + len - 1 < n; l++) {
                int r = l + len - 1;
                for (int t = 2; t <= k; t++) {
                    for (int p = l; p < r; p += k - 1) {
                        d[l][r][t] = min(d[l][r][t], d[l][p][1] + d[p + 1][r][t - 1]);
                    }
                }
                d[l][r][1] = min(d[l][r][1], d[l][r][k] + 
                                sum[r] - (l == 0 ? 0 : sum[l - 1]));
            }
        }
        return d[0][n - 1][1];
    }
};

// 方法二：状态优化
class Solution {
    static constexpr int inf = 0x3f3f3f3f;
public:
    int mergeStones(vector<int>& stones, int k) {
        int n = stones.size();
        if ((n - 1) % (k - 1) != 0) {
            return -1;
        }

        vector d(n, vector<int>(n, inf));
        vector<int> sum(n, 0);

        for (int i = 0, s = 0; i < n; i++) {
            d[i][i] = 0;
            s += stones[i];
            sum[i] = s;
        }

        for (int len = 2; len <= n; len++) {
            for (int l = 0; l < n && l + len - 1 < n; l++) {
                int r = l + len - 1;
                for (int p = l; p < r; p += k - 1) {
                    d[l][r] = min(d[l][r], d[l][p] + d[p + 1][r]);
                }
                if ((r - l) % (k - 1) == 0) {
                    d[l][r] += sum[r] - (l == 0 ? 0 : sum[l - 1]);
                }
            }
        }
        return d[0][n - 1];
    }
};
```

Java版本

```java
// 方法一：动态规划
// 记忆化搜索
class Solution {
    static final int INF = 0x3f3f3f3f;
    int[][][] d;
    int[] sum;
    int k;

    public int mergeStones(int[] stones, int k) {
        int n = stones.length;
        if ((n - 1) % (k - 1) != 0) {
            return -1;
        }
        this.k = k;
        d = new int[n][n][k + 1];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                Arrays.fill(d[i][j], -1);
            }
        }
        sum = new int[n];

        // 初始化
        for (int i = 0, s = 0; i < n; i++) {
            d[i][i][1] = 0;
            s += stones[i];
            sum[i] = s;
        }
        int res = get(0, n - 1, 1);
        return res;
    }

    public int get(int l, int r, int t) {
        // 若 d[l][r][t] 不为 -1，表示已经在之前的递归被求解过，直接返回答案
        if (d[l][r][t] != -1) {
            return d[l][r][t];
        }
        // 当石头堆数小于 t 时，一定无解
        if (t > r - l + 1) {
            return INF;
        }
        if (t == 1) {
            int res = get(l, r, k);
            if (res == INF) {
                return d[l][r][t] = INF;
            }
            return d[l][r][t] = res + (sum[r] - (l == 0 ? 0 : sum[l - 1]));
        }
        int val = INF;
        for (int p = l; p < r; p += (k - 1)) {
            val = Math.min(val, get(l, p, 1) + get(p + 1, r, t - 1));
        }
        return d[l][r][t] = val;
    }
}

// 递推
class Solution {
    static final int INF = 0x3f3f3f3f;

    public int mergeStones(int[] stones, int k) {
        int n = stones.length;
        if ((n - 1) % (k - 1) != 0) {
            return -1;
        }

        int[][][] d = new int[n][n][k + 1];
        int[] sum = new int[n];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                Arrays.fill(d[i][j], INF);
            }
        }

        for (int i = 0, s = 0; i < n; i++) {
            d[i][i][1] = 0;
            s += stones[i];
            sum[i] = s;
        }

        for (int len = 2; len <= n; len++) {
            for (int l = 0; l < n && l + len - 1 < n; l++) {
                int r = l + len - 1;
                for (int t = 2; t <= k; t++) {
                    for (int p = l; p < r; p += k - 1) {
                        d[l][r][t] = Math.min(d[l][r][t], d[l][p][1] + d[p + 1][r][t - 1]);
                    }
                }
                d[l][r][1] = Math.min(d[l][r][1], d[l][r][k] + sum[r] - (l == 0 ? 0 : sum[l - 1]));
            }
        }
        return d[0][n - 1][1];
    }
}

// 方法二：状态优化
class Solution {
    static final int INF = 0x3f3f3f3f;

    public int mergeStones(int[] stones, int k) {
        int n = stones.length;
        if ((n - 1) % (k - 1) != 0) {
            return -1;
        }

        int[][] d = new int[n][n];
        for (int i = 0; i < n; i++) {
            Arrays.fill(d[i], INF);
        }
        int[] sum = new int[n];

        for (int i = 0, s = 0; i < n; i++) {
            d[i][i] = 0;
            s += stones[i];
            sum[i] = s;
        }

        for (int len = 2; len <= n; len++) {
            for (int l = 0; l < n && l + len - 1 < n; l++) {
                int r = l + len - 1;
                for (int p = l; p < r; p += k - 1) {
                    d[l][r] = Math.min(d[l][r], d[l][p] + d[p + 1][r]);
                }
                if ((r - l) % (k - 1) == 0) {
                    d[l][r] += sum[r] - (l == 0 ? 0 : sum[l - 1]);
                }
            }
        }
        return d[0][n - 1];
    }
}

// 其他
class Solution {
    private int[] sum;
    private int k;
    private int[][] memo;

    public int mergeStones(int[] stones, int k) {
        int n = stones.length;
        if ((n - 1) % (k - 1) != 0) {
            return -1;
        }

        sum = new int[n + 1];
        this.k = k;
        for (int i = 0; i < n; i++) {
            sum[i + 1] = stones[i] + sum[i];
        }

        memo = new int[n][n];

        for (int i = 0; i < n; i++) {
            Arrays.fill(memo[i], -1);
        }

        return dfs(0, n - 1);

    }

    private int dfs(int l, int r) {
        if(l == r){
            return 0;
        }

        if (memo[l][r] != -1) {
            return memo[l][r];
        }

        int res = Integer.MAX_VALUE/2;
        for (int i = l ; i < r; i += k - 1) {
            res = Math.min(res, dfs(l, i) + dfs(i + 1, r));
        }
        if ((r - l) % (k - 1) == 0) {
            res += sum[r + 1] - sum[l];
        }

        return memo[l][r] = res;
    }
}
```



### [1547. 切棍子的最小成本](https://leetcode.cn/problems/minimum-cost-to-cut-a-stick/)

困难

有一根长度为 `n` 个单位的木棍，棍上从 `0` 到 `n` 标记了若干位置。例如，长度为 **6** 的棍子可以标记如下：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/09/statement.jpg)

给你一个整数数组 `cuts` ，其中 `cuts[i]` 表示你需要将棍子切开的位置。

你可以按顺序完成切割，也可以根据需要更改切割的顺序。

每次切割的成本都是当前要切割的棍子的长度，切棍子的总成本是历次切割成本的总和。对棍子进行切割将会把一根木棍分成两根较小的木棍（这两根木棍的长度和就是切割前木棍的长度）。请参阅第一个示例以获得更直观的解释。

返回切棍子的 **最小总成本** 。

**示例 1：**

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/09/e1.jpg" alt="img" style="zoom: 80%;" />

```
输入：n = 7, cuts = [1,3,4,5]
输出：16
解释：按 [1, 3, 4, 5] 的顺序切割的情况如下所示：

第一次切割长度为 7 的棍子，成本为 7 。第二次切割长度为 6 的棍子（即第一次切割得到的第二根棍子），第三次切割为长度 4 的棍子，最后切割长度为 3 的棍子。总成本为 7 + 6 + 4 + 3 = 20 。
而将切割顺序重新排列为 [3, 5, 1, 4] 后，总成本 = 16（如示例图中 7 + 4 + 3 + 2 = 16）。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int minCost(int n, vector<int>& cuts) {
        int m = cuts.size();
        sort(cuts.begin(), cuts.end());
        cuts.insert(cuts.begin(), 0);
        cuts.push_back(n);
        vector<vector<int>> f(m + 2, vector<int>(m + 2));
        for (int i = m; i >= 1; --i) {
            for (int j = i; j <= m; ++j) {
                f[i][j] = (i == j ? 0 : INT_MAX);
                for (int k = i; k <= j; ++k) {
                    f[i][j] = min(f[i][j], f[i][k - 1] + f[k + 1][j]);
                }
                f[i][j] += cuts[j + 1] - cuts[i - 1];
            }
        }
        return f[1][m];
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int minCost(int n, int[] cuts) {
        int m = cuts.length;
        Arrays.sort(cuts);
        int[] newCuts = new int[m + 2];
        newCuts[0] = 0;
        for (int i = 1; i <= m; ++i) {
            newCuts[i] = cuts[i - 1];
        }
        newCuts[m + 1] = n;
        int[][] f = new int[m + 2][m + 2];
        for (int i = m; i >= 1; --i) {
            for (int j = i; j <= m; ++j) {
                f[i][j] = i == j ? 0 : Integer.MAX_VALUE;
                for (int k = i; k <= j; ++k) {
                    f[i][j] = Math.min(f[i][j], f[i][k - 1] + f[k + 1][j]);
                }
                f[i][j] += newCuts[j + 1] - newCuts[i - 1];
            }
        }
        return f[1][m];
    }
}

// 或者
class Solution {
    public int minCost(int n, int[] cuts) {
        int m = cuts.length;
        int[] arr = new int[m + 2];
        Arrays.sort(cuts);
        for(int i = 0 ; i < m ; i++) {
            arr[i + 1] = cuts[i];
        }
        arr[m + 1] = n;
        int[][] dp = new int[m + 2][m + 2];
        for(int i = 1 ; i <= m ; i++) {
            dp[i][i] = arr[i + 1] - arr[i - 1];
        }
        for(int l = m - 1, next ; l >= 1 ; l--) {
            for(int r = l + 1 ; r <= m ; r++) {
                next = Integer.MAX_VALUE;
                for(int k = l ; k <= r ; k++) {
                    next = Math.min(next, dp[l][k - 1] + dp[k + 1][r]);
                }
                dp[l][r] = next + arr[r + 1] - arr[l - 1];
            }
        }
        return dp[1][m];
    }
}
```



### [664. 奇怪的打印机](https://leetcode.cn/problems/strange-printer/)

困难

有台奇怪的打印机有以下两个特殊要求：

- 打印机每次只能打印由 **同一个字符** 组成的序列。
- 每次可以在从起始到结束的任意位置打印新字符，并且会覆盖掉原来已有的字符。

给你一个字符串 `s` ，你的任务是计算这个打印机打印它需要的最少打印次数。

**示例 1：**

```
输入：s = "aaabbb"
输出：2
解释：首先打印 "aaa" 然后打印 "bbb"。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int strangePrinter(string s) {
        int n = s.length();
        vector<vector<int>> f(n, vector<int>(n));
        for (int i = n - 1; i >= 0; i--) {
            f[i][i] = 1;
            for (int j = i + 1; j < n; j++) {
                if (s[i] == s[j]) {
                    f[i][j] = f[i][j - 1];
                } else {
                    int minn = INT_MAX;
                    for (int k = i; k < j; k++) {
                        minn = min(minn, f[i][k] + f[k + 1][j]);
                    }
                    f[i][j] = minn;
                }
            }
        }
        return f[0][n - 1];
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int strangePrinter(String s) {
        int n = s.length();
        int[][] f = new int[n][n];
        for (int i = n - 1; i >= 0; i--) {
            f[i][i] = 1;
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = f[i][j - 1];
                } else {
                    int minn = Integer.MAX_VALUE;
                    for (int k = i; k < j; k++) {
                        minn = Math.min(minn, f[i][k] + f[k + 1][j]);
                    }
                    f[i][j] = minn;
                }
            }
        }
        return f[0][n - 1];
    }
}

// 或者
class Solution {
    public int strangePrinter(String s) {
        char[] ch = s.toCharArray();
        int n = ch.length;
        int[][] dp = new int[n][n];
        dp[n - 1][n - 1] = 1;
        for(int i = 0 ; i < n - 1 ; i++) {
            dp[i][i] = 1;
            dp[i][i + 1] = ch[i] == ch[i + 1] ? 1 : 2; 
        }
        for(int l = n - 3 ; l >= 0 ; l--) {
            for(int r = l + 2 ; r < n ; r++) {
                if(ch[l] == ch[r]) {
                    dp[l][r] = dp[l + 1][r];
                }else {
                    int ans = Integer.MAX_VALUE;
                    for(int i = l ; i < r ; i++) {
                        ans = Math.min(ans, dp[l][i] + dp[i + 1][r]);
                    }
                    dp[l][r] = ans;
                }
            }
        }
        return dp[0][n - 1];
    }
}
```



### [1039. 多边形三角剖分的最低得分](https://leetcode.cn/problems/minimum-score-triangulation-of-polygon/)

中等

你有一个凸的`n`边形，其每个顶点都有一个整数值。给定一个整数数组`values`，其中`values[i]`是第`i`个顶点的值（即 **顺时针顺序** ）。

假设将多边形 **剖分** 为 `n - 2` 个三角形。对于每个三角形，该三角形的值是顶点标记的**乘积**，三角剖分的分数是进行三角剖分后所有 `n - 2` 个三角形的值之和。

返回 *多边形进行三角剖分后可以得到的最低分* 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/25/shape1.jpg)

```
输入：values = [1,2,3]
输出：6
解释：多边形已经三角化，唯一三角形的分数为 6。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int minScoreTriangulation(vector<int>& values) {
        unordered_map<int, int> memo;
        int n = values.size();
        function<int(int, int)> dp = [&](int i, int j) -> int {
             if (i + 2 > j) {
                return 0;
            }
            if (i + 2 == j) {
                return values[i] * values[i + 1] * values[j];
            }
            int key = i * n + j;
            if (!memo.count(key)) {
                int minScore = INT_MAX;
                for (int k = i + 1; k < j; k++) {
                    minScore = min(minScore, values[i] * values[k] * values[j] + dp(i, k) + dp(k, j));
                }
                memo[key] = minScore;
            }
            return memo[key];
        };
        return dp(0, n - 1);
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    int n;
    int[] values;
    Map<Integer, Integer> memo = new HashMap<Integer, Integer>();

    public int minScoreTriangulation(int[] values) {
        this.n = values.length;
        this.values = values;
        return dp(0, n - 1);
    }

    public int dp(int i, int j) {
        if (i + 2 > j) {
            return 0;
        }
        if (i + 2 == j) {
            return values[i] * values[i + 1] * values[j];
        }
        int key = i * n + j;
        if (!memo.containsKey(key)) {
            int minScore = Integer.MAX_VALUE;
            for (int k = i + 1; k < j; k++) {
                minScore = Math.min(minScore, values[i] * values[k] * values[j] + dp(i, k) + dp(k, j));
            }
            memo.put(key, minScore);
        }
        return memo.get(key);
    }
}
```



### [546. 移除盒子](https://leetcode.cn/problems/remove-boxes/)

困难

给出一些不同颜色的盒子 `boxes` ，盒子的颜色由不同的正数表示。

你将经过若干轮操作去去掉盒子，直到所有的盒子都去掉为止。每一轮你可以移除具有相同颜色的连续 `k` 个盒子（`k >= 1`），这样一轮之后你将得到 `k * k` 个积分。

返回 *你能获得的最大积分和* 。

**示例 1：**

```
输入：boxes = [1,3,2,2,2,3,4,3,1]
输出：23
解释：
[1, 3, 2, 2, 2, 3, 4, 3, 1] 
----> [1, 3, 3, 4, 3, 1] (3*3=9 分) 
----> [1, 3, 3, 3, 1] (1*1=1 分) 
----> [1, 1] (3*3=9 分) 
----> [] (2*2=4 分)
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int dp[100][100][100];

    int removeBoxes(vector<int>& boxes) {
        memset(dp, 0, sizeof dp);
        return calculatePoints(boxes, 0, boxes.size() - 1, 0);
    }

    int calculatePoints(vector<int>& boxes, int l, int r, int k) {
        if (l > r) {
            return 0;
        }
        if (dp[l][r][k] == 0) {
            dp[l][r][k] = calculatePoints(boxes, l, r - 1, 0) + (k + 1) * (k + 1);
            for (int i = l; i < r; i++) {
                if (boxes[i] == boxes[r]) {
                    dp[l][r][k] = max(dp[l][r][k], calculatePoints(boxes, l, i, k + 1) + calculatePoints(boxes, i + 1, r - 1, 0));
                }
            }
        }
        return dp[l][r][k];
    }
};

// 优化
class Solution {
public:
    int dp[100][100][100];

    int removeBoxes(vector<int>& boxes) {
        memset(dp, 0, sizeof dp);
        return calculatePoints(boxes, 0, boxes.size() - 1, 0);
    }

    int calculatePoints(vector<int>& boxes, int l, int r, int k) {
        if (l > r) {
            return 0;
        }
        if (dp[l][r][k] == 0) {
            int r1 = r, k1 = k;
            while (r1 > l && boxes[r1] == boxes[r1 - 1]) {
                r1--;
                k1++;
            }
            dp[l][r][k] = calculatePoints(boxes, l, r1 - 1, 0) + (k1 + 1) * (k1 + 1);
            for (int i = l; i < r1; i++) {
                if (boxes[i] == boxes[r1]) {
                    dp[l][r][k] = max(dp[l][r][k], calculatePoints(boxes, l, i, k1 + 1) + calculatePoints(boxes, i + 1, r1 - 1, 0));
                }
            }
        }
        return dp[l][r][k];
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    int[][][] dp;

    public int removeBoxes(int[] boxes) {
        int length = boxes.length;
        dp = new int[length][length][length];
        return calculatePoints(boxes, 0, length - 1, 0);
    }

    public int calculatePoints(int[] boxes, int l, int r, int k) {
        if (l > r) {
            return 0;
        }
        if (dp[l][r][k] == 0) {
            dp[l][r][k] = calculatePoints(boxes, l, r - 1, 0) + (k + 1) * (k + 1);
            for (int i = l; i < r; i++) {
                if (boxes[i] == boxes[r]) {
                    dp[l][r][k] = Math.max(dp[l][r][k], calculatePoints(boxes, l, i, k + 1) + calculatePoints(boxes, i + 1, r - 1, 0));
                }
            }
        }
        return dp[l][r][k];
    }
}

// 优化
class Solution {
    int[][][] dp;

    public int removeBoxes(int[] boxes) {
        int length = boxes.length;
        dp = new int[length][length][length];
        return calculatePoints(boxes, 0, length - 1, 0);
    }

    public int calculatePoints(int[] boxes, int l, int r, int k) {
        if (l > r) {
            return 0;
        }
        if (dp[l][r][k] == 0) {
            int r1 = r, k1 = k;
            while (r1 > l && boxes[r1] == boxes[r1 - 1]) {
                r1--;
                k1++;
            }
            dp[l][r][k] = calculatePoints(boxes, l, r1 - 1, 0) + (k1 + 1) * (k1 + 1);
            for (int i = l; i < r1; i++) {
                if (boxes[i] == boxes[r1]) {
                    dp[l][r][k] = Math.max(dp[l][r][k], calculatePoints(boxes, l, i, k1 + 1) + calculatePoints(boxes, i + 1, r1 - 1, 0));
                }
            }
        }
        return dp[l][r][k];
    }
}
```



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



### [678. 有效的括号字符串](https://leetcode.cn/problems/valid-parenthesis-string/)

中等

给你一个只包含三种字符的字符串，支持的字符类型分别是 `'('`、`')'` 和 `'*'`。请你检验这个字符串是否为有效字符串，如果是有效字符串返回 `true` 。

有效字符串符合如下规则：

- 任何左括号 `'('` 必须有相应的右括号 `')'`。
- 任何右括号 `')'` 必须有相应的左括号 `'('` 。
- 左括号 `'('` 必须在对应的右括号之前 `')'`。
- `'*'` 可以被视为单个右括号 `')'` ，或单个左括号 `'('` ，或一个空字符串。
- 一个空字符串也被视为有效字符串。

**示例 1：**

```
输入：s = "()"
输出：true
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    bool checkValidString(string s) {
        int n = s.size();
        vector<vector<bool>> dp = vector<vector<bool>>(n,vector<bool>(n,false));

        for (int i = 0; i < n; i++) {
            if (s[i] == '*') {
                dp[i][i] = true;
            }
        }

        for (int i = 1; i < n; i++) {
            char c1 = s[i - 1]; 
            char c2 = s[i];
            dp[i - 1][i] = (c1 == '(' || c1 == '*') && (c2 == ')' || c2 == '*');
        }

        for (int i = n - 3; i >= 0; i--) {
            char c1 = s[i];
            for (int j = i + 2; j < n; j++) {
                char c2 = s[j];
                if ((c1 == '(' || c1 == '*') && (c2 == ')' || c2 == '*')) {
                    dp[i][j] = dp[i + 1][j - 1];
                }
                for (int k = i; k < j && !dp[i][j]; k++) {
                    dp[i][j] = dp[i][k] && dp[k + 1][j];
                }
            }
        }
        return dp[0][n - 1];
    }
};

// 方法二：栈
class Solution {
public:
    bool checkValidString(string s) {
        stack<int> leftStack;
        stack<int> asteriskStack;
        int n = s.size();

        for (int i = 0; i < n; i++) {
            char c = s[i];
            if (c == '(') {
                leftStack.push(i);
            } else if (c == '*') {
                asteriskStack.push(i);
            } else {
                if (!leftStack.empty()) {
                    leftStack.pop();
                } else if (!asteriskStack.empty()) {
                    asteriskStack.pop();
                } else {
                    return false;
                }
            }
        }

        while (!leftStack.empty() && !asteriskStack.empty()) {
            int leftIndex = leftStack.top();
            leftStack.pop();
            int asteriskIndex = asteriskStack.top();
            asteriskStack.pop();
            if (leftIndex > asteriskIndex) {
                return false;
            }
        }
        
        return leftStack.empty();
    }
};

// 方法三：贪心
class Solution {
public:
    bool checkValidString(string s) {
        int minCount = 0, maxCount = 0;
        int n = s.size();
        for (int i = 0; i < n; i++) {
            char c = s[i];
            if (c == '(') {
                minCount++;
                maxCount++;
            } else if (c == ')') {
                minCount = max(minCount - 1, 0);
                maxCount--;
                if (maxCount < 0) {
                    return false;
                }
            } else {
                minCount = max(minCount - 1, 0);
                maxCount++;
            }
        }
        return minCount == 0;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public boolean checkValidString(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '*') {
                dp[i][i] = true;
            }
        }
        for (int i = 1; i < n; i++) {
            char c1 = s.charAt(i - 1), c2 = s.charAt(i);
            dp[i - 1][i] = (c1 == '(' || c1 == '*') && (c2 == ')' || c2 == '*');
        }
        for (int i = n - 3; i >= 0; i--) {
            char c1 = s.charAt(i);
            for (int j = i + 2; j < n; j++) {
                char c2 = s.charAt(j);
                if ((c1 == '(' || c1 == '*') && (c2 == ')' || c2 == '*')) {
                    dp[i][j] = dp[i + 1][j - 1];
                }
                for (int k = i; k < j && !dp[i][j]; k++) {
                    dp[i][j] = dp[i][k] && dp[k + 1][j];
                }
            }
        }
        return dp[0][n - 1]; 
    }
}

// 方法二：栈
class Solution {
    public boolean checkValidString(String s) {
        Deque<Integer> leftStack = new LinkedList<Integer>();
        Deque<Integer> asteriskStack = new LinkedList<Integer>();
        int n = s.length();
        
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            if (c == '(') {
                leftStack.push(i);
            } else if (c == '*') {
                asteriskStack.push(i);
            } else {
                if (!leftStack.isEmpty()) {
                    leftStack.pop();
                } else if (!asteriskStack.isEmpty()) {
                    asteriskStack.pop();
                } else {
                    return false;
                }
            }
        }
        
        while (!leftStack.isEmpty() && !asteriskStack.isEmpty()) {
            int leftIndex = leftStack.pop();
            int asteriskIndex = asteriskStack.pop();
            if (leftIndex > asteriskIndex) {
                return false;
            }
        }
        return leftStack.isEmpty();
    }
}

// 方法三：贪心
class Solution {
    public boolean checkValidString(String s) {
        int minCount = 0, maxCount = 0;
        int n = s.length();
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            if (c == '(') {
                minCount++;
                maxCount++;
            } else if (c == ')') {
                minCount = Math.max(minCount - 1, 0);
                maxCount--;
                if (maxCount < 0) {
                    return false;
                }
            } else {
                minCount = Math.max(minCount - 1, 0);
                maxCount++;
            }
        }
        return minCount == 0;
    }
}
```



### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

中等

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

**示例 1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        if (n < 2) {
            return s;
        }

        int maxLen = 1;
        int begin = 0;
        // dp[i][j] 表示 s[i..j] 是否是回文串
        vector<vector<int>> dp(n, vector<int>(n));
        // 初始化：所有长度为 1 的子串都是回文串
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }
        // 递推开始
        // 先枚举子串长度
        for (int L = 2; L <= n; L++) {
            // 枚举左边界，左边界的上限设置可以宽松一些
            for (int i = 0; i < n; i++) {
                // 由 L 和 i 可以确定右边界，即 j - i + 1 = L 得
                int j = L + i - 1;
                // 如果右边界越界，就可以退出当前循环
                if (j >= n) {
                    break;
                }

                if (s[i] != s[j]) {
                    dp[i][j] = false;
                } else {
                    if (j - i < 3) {
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = dp[i + 1][j - 1];
                    }
                }

                // 只要 dp[i][L] == true 成立，就表示子串 s[i..L] 是回文，此时记录回文长度和起始位置
                if (dp[i][j] && j - i + 1 > maxLen) {
                    maxLen = j - i + 1;
                    begin = i;
                }
            }
        }
        return s.substr(begin, maxLen);
    }
};

// 方法二：中心扩展算法
class Solution {
public:
    pair<int, int> expandAroundCenter(const string& s, int left, int right) {
        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            --left;
            ++right;
        }
        return {left + 1, right - 1};
    }

    string longestPalindrome(string s) {
        int start = 0, end = 0;
        for (int i = 0; i < s.size(); ++i) {
            auto [left1, right1] = expandAroundCenter(s, i, i);
            auto [left2, right2] = expandAroundCenter(s, i, i + 1);
            if (right1 - left1 > end - start) {
                start = left1;
                end = right1;
            }
            if (right2 - left2 > end - start) {
                start = left2;
                end = right2;
            }
        }
        return s.substr(start, end - start + 1);
    }
};

// 方法三：Manacher 算法
class Solution {
public:
    int expand(const string& s, int left, int right) {
        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            --left;
            ++right;
        }
        return (right - left - 2) / 2;
    }

    string longestPalindrome(string s) {
        int start = 0, end = -1;
        string t = "#";
        for (char c: s) {
            t += c;
            t += '#';
        }
        t += '#';
        s = t;

        vector<int> arm_len;
        int right = -1, j = -1;
        for (int i = 0; i < s.size(); ++i) {
            int cur_arm_len;
            if (right >= i) {
                int i_sym = j * 2 - i;
                int min_arm_len = min(arm_len[i_sym], right - i);
                cur_arm_len = expand(s, i - min_arm_len, i + min_arm_len);
            } else {
                cur_arm_len = expand(s, i, i);
            }
            arm_len.push_back(cur_arm_len);
            if (i + cur_arm_len > right) {
                j = i;
                right = i + cur_arm_len;
            }
            if (cur_arm_len * 2 + 1 > end - start) {
                start = i - cur_arm_len;
                end = i + cur_arm_len;
            }
        }

        string ans;
        for (int i = start; i <= end; ++i) {
            if (s[i] != '#') {
                ans += s[i];
            }
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：动态规划
public class Solution {

    public String longestPalindrome(String s) {
        int len = s.length();
        if (len < 2) {
            return s;
        }

        int maxLen = 1;
        int begin = 0;
        // dp[i][j] 表示 s[i..j] 是否是回文串
        boolean[][] dp = new boolean[len][len];
        // 初始化：所有长度为 1 的子串都是回文串
        for (int i = 0; i < len; i++) {
            dp[i][i] = true;
        }

        char[] charArray = s.toCharArray();
        // 递推开始
        // 先枚举子串长度
        for (int L = 2; L <= len; L++) {
            // 枚举左边界，左边界的上限设置可以宽松一些
            for (int i = 0; i < len; i++) {
                // 由 L 和 i 可以确定右边界，即 j - i + 1 = L 得
                int j = L + i - 1;
                // 如果右边界越界，就可以退出当前循环
                if (j >= len) {
                    break;
                }

                if (charArray[i] != charArray[j]) {
                    dp[i][j] = false;
                } else {
                    if (j - i < 3) {
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = dp[i + 1][j - 1];
                    }
                }

                // 只要 dp[i][L] == true 成立，就表示子串 s[i..L] 是回文，此时记录回文长度和起始位置
                if (dp[i][j] && j - i + 1 > maxLen) {
                    maxLen = j - i + 1;
                    begin = i;
                }
            }
        }
        return s.substring(begin, begin + maxLen);
    }
}

// 方法二：中心扩展算法
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) {
            return "";
        }
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            int len1 = expandAroundCenter(s, i, i);
            int len2 = expandAroundCenter(s, i, i + 1);
            int len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }

    public int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            --left;
            ++right;
        }
        return right - left - 1;
    }
}

// 方法三：Manacher 算法
class Solution {
    public String longestPalindrome(String s) {
        int start = 0, end = -1;
        StringBuffer t = new StringBuffer("#");
        for (int i = 0; i < s.length(); ++i) {
            t.append(s.charAt(i));
            t.append('#');
        }
        t.append('#');
        s = t.toString();

        List<Integer> arm_len = new ArrayList<Integer>();
        int right = -1, j = -1;
        for (int i = 0; i < s.length(); ++i) {
            int cur_arm_len;
            if (right >= i) {
                int i_sym = j * 2 - i;
                int min_arm_len = Math.min(arm_len.get(i_sym), right - i);
                cur_arm_len = expand(s, i - min_arm_len, i + min_arm_len);
            } else {
                cur_arm_len = expand(s, i, i);
            }
            arm_len.add(cur_arm_len);
            if (i + cur_arm_len > right) {
                j = i;
                right = i + cur_arm_len;
            }
            if (cur_arm_len * 2 + 1 > end - start) {
                start = i - cur_arm_len;
                end = i + cur_arm_len;
            }
        }

        StringBuffer ans = new StringBuffer();
        for (int i = start; i <= end; ++i) {
            if (s.charAt(i) != '#') {
                ans.append(s.charAt(i));
            }
        }
        return ans.toString();
    }

    public int expand(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            --left;
            ++right;
        }
        return (right - left - 2) / 2;
    }
}
```





