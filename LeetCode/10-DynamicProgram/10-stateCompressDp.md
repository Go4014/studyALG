# 动态规划

## 状态压缩DP题目

### [1879. 两个数组最小的异或值之和](https://leetcode.cn/problems/minimum-xor-sum-of-two-arrays/)

困难

给你两个整数数组 `nums1` 和 `nums2` ，它们长度都为 `n` 。

两个数组的 **异或值之和** 为 `(nums1[0] XOR nums2[0]) + (nums1[1] XOR nums2[1]) + ... + (nums1[n - 1] XOR nums2[n - 1])` （**下标从 0 开始**）。

- 比方说，`[1,2,3]` 和 `[3,2,1]` 的 **异或值之和** 等于 `(1 XOR 3) + (2 XOR 2) + (3 XOR 1) = 2 + 0 + 2 = 4` 。

请你将 `nums2` 中的元素重新排列，使得 **异或值之和** **最小** 。

请你返回重新排列之后的 **异或值之和** 。

**示例 1：**

```
输入：nums1 = [1,2], nums2 = [2,3]
输出：2
解释：将 nums2 重新排列得到 [3,2] 。
异或值之和为 (1 XOR 3) + (2 XOR 2) = 2 + 0 = 2 。
```

C++版本

```c++
// 方法一：状态压缩动态规划
class Solution {
public:
    int minimumXORSum(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        vector<int> f(1 << n, INT_MAX);
        f[0] = 0;
        for (int mask = 1; mask < (1 << n); ++mask) {
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {
                    f[mask] = min(f[mask], f[mask ^ (1 << i)] + (nums1[__builtin_popcount(mask) - 1] ^ nums2[i]));
                }
            }
        }
        return f[(1 << n) - 1];
    }
};
```

Java版本

```java
// 方法一：状态压缩动态规划
class Solution {
    public int minimumXORSum(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int[] f = new int[1 << n];
        Arrays.fill(f, Integer.MAX_VALUE);
        f[0] = 0;

        for (int mask = 1; mask < (1 << n); ++mask) {
            int count = Integer.bitCount(mask);
            for (int i = 0; i < n; ++i) {
                if ((mask & (1 << i)) != 0) {
                    f[mask] = Math.min(f[mask], f[mask ^ (1 << i)] + (nums1[count - 1] ^ nums2[i]));
                }
            }
        }
        
        return f[(1 << n) - 1];
    }
}
```



### [2172. 数组的最大与和](https://leetcode.cn/problems/maximum-and-sum-of-array/)

困难

给你一个长度为 `n` 的整数数组 `nums` 和一个整数 `numSlots` ，满足`2 * numSlots >= n` 。总共有 `numSlots` 个篮子，编号为 `1` 到 `numSlots` 。

你需要把所有 `n` 个整数分到这些篮子中，且每个篮子 **至多** 有 2 个整数。一种分配方案的 **与和** 定义为每个数与它所在篮子编号的 **按位与运算** 结果之和。

- 比方说，将数字 `[1, 3]` 放入篮子 ***`1`\*** 中，`[4, 6]` 放入篮子 ***`2`\*** 中，这个方案的与和为 `(1 AND ***1\***) + (3 AND ***1\***) + (4 AND ***2***) + (6 AND ***2***) = 1 + 1 + 0 + 2 = 4` 。

请你返回将 `nums` 中所有数放入 `numSlots` 个篮子中的最大与和。

**示例 1：**

```
输入：nums = [1,2,3,4,5,6], numSlots = 3
输出：9
解释：一个可行的方案是 [1, 4] 放入篮子 1 中，[2, 6] 放入篮子 2 中，[3, 5] 放入篮子 3 中。
最大与和为 (1 AND 1) + (4 AND 1) + (2 AND 2) + (6 AND 2) + (3 AND 3) + (5 AND 3) = 1 + 0 + 2 + 2 + 3 + 1 = 9 。
```

C++版本

```c++
// 方法一：三进制状态压缩动态规划
class Solution {
public:
    int maximumANDSum(vector<int>& nums, int numSlots) {
        int n = nums.size();
        int mask_max = 1;
        for (int i = 0; i < numSlots; ++i) {
            mask_max *= 3;
        }
        
        vector<int> f(mask_max);
        for (int mask = 1; mask < mask_max; ++mask) {
            int cnt = 0;
            for (int i = 0, dummy = mask; i < numSlots; ++i, dummy /= 3) {
                cnt += dummy % 3;
            }
            if (cnt > n) {
                continue;
            }
            for (int i = 0, dummy = mask, w = 1; i < numSlots; ++i, dummy /= 3, w *= 3) {
                int has = dummy % 3;
                if (has) {
                    f[mask] = max(f[mask], f[mask - w] + (nums[cnt - 1] & (i + 1)));
                }
            }
        }
        
        return *max_element(f.begin(), f.end());
    }
};
```

Java版本

```java
// 方法一：三进制状态压缩动态规划
class Solution {
    public int maximumANDSum(int[] nums, int numSlots) {
        int n = nums.length;
        int maskMax = 1;
        for (int i = 0; i < numSlots; ++i) {
            maskMax *= 3;
        }
        
        int[] f = new int[maskMax];
        
        for (int mask = 1; mask < maskMax; ++mask) {
            int cnt = 0;
            for (int i = 0, dummy = mask; i < numSlots; ++i, dummy /= 3) {
                cnt += dummy % 3;
            }
            if (cnt > n) {
                continue;
            }
            for (int i = 0, dummy = mask, w = 1; i < numSlots; ++i, dummy /= 3, w *= 3) {
                int has = dummy % 3;
                if (has != 0) {
                    f[mask] = Math.max(f[mask], f[mask - w] + (nums[cnt - 1] & (i + 1)));
                }
            }
        }
        
        return Arrays.stream(f).max().getAsInt();
    }
}
```



### [1947. 最大兼容性评分和](https://leetcode.cn/problems/maximum-compatibility-score-sum/)

中等

有一份由 `n` 个问题组成的调查问卷，每个问题的答案要么是 `0`（no，否），要么是 `1`（yes，是）。

这份调查问卷被分发给 `m` 名学生和 `m` 名导师，学生和导师的编号都是从 `0` 到 `m - 1` 。学生的答案用一个二维整数数组 `students` 表示，其中 `students[i]` 是一个整数数组，包含第 `i` 名学生对调查问卷给出的答案（**下标从 0 开始**）。导师的答案用一个二维整数数组 `mentors` 表示，其中 `mentors[j]` 是一个整数数组，包含第 `j` 名导师对调查问卷给出的答案（**下标从 0 开始**）。

每个学生都会被分配给 **一名** 导师，而每位导师也会分配到 **一名** 学生。配对的学生与导师之间的兼容性评分等于学生和导师答案相同的次数。

- 例如，学生答案为`[1, ***0\***, ***1\***]` 而导师答案为 `[0, ***0\***, ***1\***]` ，那么他们的兼容性评分为 2 ，因为只有第二个和第三个答案相同。

请你找出最优的学生与导师的配对方案，以 **最大程度上** 提高 **兼容性评分和** 。

给你 `students` 和 `mentors` ，返回可以得到的 **最大兼容性评分和** 。

**示例 1：**

```
输入：students = [[1,1,0],[1,0,1],[0,0,1]], mentors = [[1,0,0],[0,0,1],[1,1,0]]
输出：8
解释：按下述方式分配学生和导师：
- 学生 0 分配给导师 2 ，兼容性评分为 3 。
- 学生 1 分配给导师 0 ，兼容性评分为 2 。
- 学生 2 分配给导师 1 ，兼容性评分为 3 。
最大兼容性评分和为 3 + 2 + 3 = 8 。
```

C++版本

```c++
// 方法一：枚举排列
class Solution {
public:
    int maxCompatibilitySum(vector<vector<int>>& students, vector<vector<int>>& mentors) {
        int m = students.size();
        int n = students[0].size();
        vector<vector<int>> g(m, vector<int>(m));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < m; ++j) {
                for (int k = 0; k < n; ++k) {
                    g[i][j] += (students[i][k] == mentors[j][k]);
                }
            }
        }

        vector<int> p(m);
        iota(p.begin(), p.end(), 0);
        int ans = 0;
        do {
            int cur = 0;
            for (int i = 0; i < m; ++i) {
                cur += g[i][p[i]];
            }
            ans = max(ans, cur);
        } while (next_permutation(p.begin(), p.end()));
        return ans;
    }
};

// 方法二：状态压缩动态规划
class Solution {
public:
    int maxCompatibilitySum(vector<vector<int>>& students, vector<vector<int>>& mentors) {
        int m = students.size();
        int n = students[0].size();
        vector<vector<int>> g(m, vector<int>(m));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < m; ++j) {
                for (int k = 0; k < n; ++k) {
                    g[i][j] += (students[i][k] == mentors[j][k]);
                }
            }
        }

        vector<int> f(1 << m);
        for (int mask = 1; mask < (1 << m); ++mask) {
            int c = __builtin_popcount(mask);
            for (int i = 0; i < m; ++i) {
                // 判断 mask 的第 i 位是否为 1
                if (mask & (1 << i)) {
                    f[mask] = max(f[mask], f[mask ^ (1 << i)] + g[c - 1][i]);
                }
            }
        }
        return f[(1 << m) - 1];
    }
};
```

Java版本

```java
// 方法一：枚举排列
class Solution {
    public int maxCompatibilitySum(int[][] students, int[][] mentors) {
        int m = students.length;
        int n = students[0].length;
        int[][] g = new int[m][m];

        // Calculate compatibility scores
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < m; ++j) {
                for (int k = 0; k < n; ++k) {
                    g[i][j] += (students[i][k] == mentors[j][k]) ? 1 : 0;
                }
            }
        }

        // Generate all permutations
        List<Integer> p = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            p.add(i);
        }

        int ans = 0;
        do {
            int cur = 0;
            for (int i = 0; i < m; ++i) {
                cur += g[i][p.get(i)];
            }
            ans = Math.max(ans, cur);
        } while (nextPermutation(p));
        return ans;
    }

    private boolean nextPermutation(List<Integer> p) {
        int n = p.size();
        int i = n - 2;
        while (i >= 0 && p.get(i) >= p.get(i + 1)) {
            i--;
        }
        if (i < 0) {
            return false;
        }
        int j = n - 1;
        while (p.get(j) <= p.get(i)) {
            j--;
        }
        Collections.swap(p, i, j);
        Collections.reverse(p.subList(i + 1, n));
        return true;
    }
}


// 方法二：状态压缩动态规划
class Solution {
    public int maxCompatibilitySum(int[][] students, int[][] mentors) {
        int m = students.length;
        int n = students[0].length;
        int[][] g = new int[m][m];

        // Calculate compatibility scores
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < m; ++j) {
                for (int k = 0; k < n; ++k) {
                    g[i][j] += (students[i][k] == mentors[j][k]) ? 1 : 0;
                }
            }
        }

        int[] f = new int[1 << m];
        for (int mask = 1; mask < (1 << m); ++mask) {
            int c = Integer.bitCount(mask);
            for (int i = 0; i < m; ++i) {
                if ((mask & (1 << i)) != 0) {
                    f[mask] = Math.max(f[mask], f[mask ^ (1 << i)] + g[c - 1][i]);
                }
            }
        }
        return f[(1 << m) - 1];
    }
}
```



### [1595. 连通两组点的最小成本](https://leetcode.cn/problems/minimum-cost-to-connect-two-groups-of-points/)

困难

给你两组点，其中第一组中有 `size1` 个点，第二组中有 `size2` 个点，且 `size1 >= size2` 。

任意两点间的连接成本 `cost` 由大小为 `size1 x size2` 矩阵给出，其中 `cost[i][j]` 是第一组中的点 `i` 和第二组中的点 `j` 的连接成本。**如果两个组中的每个点都与另一组中的一个或多个点连接，则称这两组点是连通的。**换言之，第一组中的每个点必须至少与第二组中的一个点连接，且第二组中的每个点必须至少与第一组中的一个点连接。

返回连通两组点所需的最小成本。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/20/ex1.jpg)

```
输入：cost = [[15, 96], [36, 2]]
输出：17
解释：连通两组点的最佳方法是：
1--A
2--B
总成本为 17 。
```

C++版本

```c++
// 方法一：状态压缩 + 动态规划
class Solution {
public:
    int connectTwoGroups(vector<vector<int>>& cost) {
        int size1 = cost.size(), size2 = cost[0].size(), m = 1 << size2;
        vector<vector<int>> dp(size1 + 1, vector<int>(m, INT_MAX / 2));
        dp[0][0] = 0;
        for (int i = 1; i <= size1; i++) {
            for (int s = 0; s < m; s++) {
                for (int k = 0; k < size2; k++) {
                    if ((s & (1 << k)) == 0) {
                        continue;
                    }
                    dp[i][s] = min(dp[i][s], dp[i][s ^ (1 << k)] + cost[i - 1][k]);
                    dp[i][s] = min(dp[i][s], dp[i - 1][s] + cost[i - 1][k]);
                    dp[i][s] = min(dp[i][s], dp[i - 1][s ^ (1 << k)] + cost[i - 1][k]);
                }
            }
        }
        return dp[size1][m - 1];
    }
};

// 优化空间
class Solution {
public:
    int connectTwoGroups(vector<vector<int>>& cost) {
        int size1 = cost.size(), size2 = cost[0].size(), m = 1 << size2;
        vector<int> dp1(m, INT_MAX / 2), dp2(m);
        dp1[0] = 0;
        for (int i = 1; i <= size1; i++) {
            for (int s = 0; s < m; s++) {
                dp2[s] = INT_MAX / 2;
                for (int k = 0; k < size2; k++) {
                    if ((s & (1 << k)) == 0) {
                        continue;
                    }
                    dp2[s] = min(dp2[s], dp2[s ^ (1 << k)] + cost[i - 1][k]);
                    dp2[s] = min(dp2[s], dp1[s] + cost[i - 1][k]);
                    dp2[s] = min(dp2[s], dp1[s ^ (1 << k)] + cost[i - 1][k]);
                }
            }
            dp1.swap(dp2);
        }
        return dp1[m - 1];
    }
};
```

Java版本

```java
// 方法一：状态压缩 + 动态规划
class Solution {
    public int connectTwoGroups(List<List<Integer>> cost) {
        int size1 = cost.size(), size2 = cost.get(0).size(), m = 1 << size2;
        int[][] dp = new int[size1 + 1][m];
        for (int i = 0; i <= size1; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE / 2);
        }
        dp[0][0] = 0;
        for (int i = 1; i <= size1; i++) {
            for (int s = 0; s < m; s++) {
                for (int k = 0; k < size2; k++) {
                    if ((s & (1 << k)) == 0) {
                        continue;
                    }
                    dp[i][s] = Math.min(dp[i][s], dp[i][s ^ (1 << k)] + cost.get(i - 1).get(k));
                    dp[i][s] = Math.min(dp[i][s], dp[i - 1][s] + cost.get(i - 1).get(k));
                    dp[i][s] = Math.min(dp[i][s], dp[i - 1][s ^ (1 << k)] + cost.get(i - 1).get(k));
                }
            }
        }
        return dp[size1][m - 1];
    }
}

// 优化空间
class Solution {
    public int connectTwoGroups(List<List<Integer>> cost) {
        int size1 = cost.size(), size2 = cost.get(0).size(), m = 1 << size2;
        int[] dp1 = new int[m];
        Arrays.fill(dp1, Integer.MAX_VALUE / 2);
        int[] dp2 = new int[m];
        dp1[0] = 0;
        for (int i = 1; i <= size1; i++) {
            for (int s = 0; s < m; s++) {
                dp2[s] = Integer.MAX_VALUE / 2;
                for (int k = 0; k < size2; k++) {
                    if ((s & (1 << k)) == 0) {
                        continue;
                    }
                    dp2[s] = Math.min(dp2[s], dp2[s ^ (1 << k)] + cost.get(i - 1).get(k));
                    dp2[s] = Math.min(dp2[s], dp1[s] + cost.get(i - 1).get(k));
                    dp2[s] = Math.min(dp2[s], dp1[s ^ (1 << k)] + cost.get(i - 1).get(k));
                }
            }
            System.arraycopy(dp2, 0, dp1, 0, m);
        }
        return dp1[m - 1];
    }
}
```



