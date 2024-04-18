# 动态规划

## 双串线性 DP 问题

### [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

中等

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。

一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

- 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。

两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

**示例 1：**

```
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.length(), n = text2.length();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; i++) {
            char c1 = text1.at(i - 1);
            for (int j = 1; j <= n; j++) {
                char c2 = text2.at(j - 1);
                if (c1 == c2) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            char c1 = text1.charAt(i - 1);
            for (int j = 1; j <= n; j++) {
                char c2 = text2.charAt(j - 1);
                if (c1 == c2) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
}
```



### [712. 两个字符串的最小ASCII删除和](https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings/)

中等

给定两个字符串`s1` 和 `s2`，返回 *使两个字符串相等所需删除字符的 **ASCII** 值的最小和* 。

**示例 1:**

```
输入: s1 = "sea", s2 = "eat"
输出: 231
解释: 在 "sea" 中删除 "s" 并将 "s" 的值(115)加入总和。
在 "eat" 中删除 "t" 并将 116 加入总和。
结束时，两个字符串相等，115 + 116 = 231 就是符合条件的最小和。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        int m = s1.size();
        int n = s2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));

        for (int i = 1; i <= m; ++i) {
            dp[i][0] = dp[i - 1][0] + s1[i - 1];
        }
        for (int j = 1; j <= n; ++j) {
            dp[0][j] = dp[0][j - 1] + s2[j - 1];
        }
        for (int i = 1; i <= m; i++) {
            char c1 = s1[i - 1];
            for (int j = 1; j <= n; j++) {
                char c2 = s2[j - 1];
                if (c1 == c2) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = min(dp[i - 1][j] + s1[i - 1], dp[i][j - 1] + s2[j - 1]);
                }
            }
        }

        return dp[m][n];
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            dp[i][0] = dp[i - 1][0] + s1.codePointAt(i - 1);
        }
        for (int j = 1; j <= n; j++) {
            dp[0][j] = dp[0][j - 1] + s2.codePointAt(j - 1);
        }
        for (int i = 1; i <= m; i++) {
            int code1 = s1.codePointAt(i - 1);
            for (int j = 1; j <= n; j++) {
                int code2 = s2.codePointAt(j - 1);
                if (code1 == code2) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(dp[i - 1][j] + code1, dp[i][j - 1] + code2);
                }
            }
        }
        return dp[m][n];
    }
}

// 或者
//求出 s1 和 s2 的 ASCII值最大的公共子序列的ASCII值 dp
//用 s1 和 s2 总共的ASCII值 - 2 * dp
//即 删除字符的 ASCII 值的最小和
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        char[] c1 = s1.toCharArray();
        char[] c2 = s2.toCharArray();
        int n1 = c1.length, n2 = c2.length;
        
        int[] dp = new int[n2 + 1];
        for (int k = 1; k <= n1; k++) {
            int leftUp = dp[0];
            int left = dp[0];
            for (int i = 1; i <= n2; i++) {
                int up = dp[i];
                dp[i] = Math.max(left, up);
                if (c1[k-1] == c2[i-1]) {
                    dp[i] = Math.max(dp[i], leftUp + c1[k-1]);
                }
                leftUp = up;
                left = dp[i];
            }
           
        }
        int sum = 0;
        for (char c : c1) {
            sum += c;
        }
        for (char c : c2) {
            sum += c;
        }
        return sum - 2 * dp[n2];
    }
}
```

