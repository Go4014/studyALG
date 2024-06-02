# 动态规划

## 数位Dp问题

### [2376. 统计特殊整数](https://leetcode.cn/problems/count-special-integers/)

困难

如果一个正整数每一个数位都是 **互不相同** 的，我们称它是 **特殊整数** 。

给你一个 **正** 整数 `n` ，请你返回区间 `[1, n]` 之间特殊整数的数目。

**示例 1：**

```
输入：n = 20
输出：19
解释：1 到 20 之间所有整数除了 11 以外都是特殊整数。所以总共有 19 个特殊整数。
```

C++版本

```c++
class Solution {
public:
    int countSpecialNumbers(int n) {
        auto s = to_string(n);
        int m = s.length(), memo[m][1 << 10];
        memset(memo, -1, sizeof(memo)); // -1 表示没有计算过
        function<int(int, int, bool, bool)> f = [&](int i, int mask, bool is_limit, bool is_num) -> int {
            if (i == m)
                return is_num; // is_num 为 true 表示得到了一个合法数字
            if (!is_limit && is_num && memo[i][mask] != -1)
                return memo[i][mask];
            int res = 0;
            if (!is_num) // 可以跳过当前数位
                res = f(i + 1, mask, false, false);
            int up = is_limit ? s[i] - '0' : 9; // 如果前面填的数字都和 n 的一样，那么这一位至多填数字 s[i]（否则就超过 n 啦）
            for (int d = 1 - is_num; d <= up; ++d) // 枚举要填入的数字 d
                if ((mask >> d & 1) == 0) // d 不在 mask 中
                    res += f(i + 1, mask | (1 << d), is_limit && d == up, true);
            if (!is_limit && is_num)
                memo[i][mask] = res;
            return res;
        };
        return f(0, 0, true, false);
    }
};
```

Java版本

```java
class Solution {
    char s[];
    int memo[][];

    public int countSpecialNumbers(int n) {
        s = Integer.toString(n).toCharArray();
        int m = s.length;
        memo = new int[m][1 << 10];
        for (int i = 0; i < m; i++) 
            Arrays.fill(memo[i], -1); // -1 表示没有计算过
        return helper(0, 0, true, false);
    }

    int helper(int i, int mask, boolean isLimit, boolean isNum) {
        if (i == s.length)
            return isNum ? 1 : 0; // isNum 为 true 表示得到了一个合法数字
        if (!isLimit && isNum && memo[i][mask] != -1)
            return memo[i][mask];
        int res = 0;
        if (!isNum) // 可以跳过当前数位
            res = helper(i + 1, mask, false, false);
        int up = isLimit ? s[i] - '0' : 9; // 如果前面填的数字都和 n 的一样，那么这一位至多填数字 s[i]（否则就超过 n 啦）
        for (int d = isNum ? 0 : 1; d <= up; ++d) // 枚举要填入的数字 d
            if ((mask >> d & 1) == 0) // d 不在 mask 中
                res += helper(i + 1, mask | (1 << d), isLimit && d == up, true);
        if (!isLimit && isNum)
            memo[i][mask] = res;
        return res;
    }
}
```



### [357. 统计各位数字都不同的数字个数](https://leetcode.cn/problems/count-numbers-with-unique-digits/)

中等

给你一个整数 `n` ，统计并返回各位数字都不同的数字 `x` 的个数，其中 `0 <= x < 10n` 。

**示例 1：**

```
输入：n = 2
输出：91
解释：答案应为除去 11、22、33、44、55、66、77、88、99 外，在 0 ≤ x < 100 范围内的所有数字。 
```

C++版本

```c++
// 方法一：排列组合
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        if (n == 0) {
            return 1;
        }
        if (n == 1) {
            return 10;
        }
        int ans = 10, cur = 9;
        for (int i = 0; i < n - 1; ++i) {
            cur *= 9 - i;
            ans += cur;
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：排列组合
class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        if (n == 0) {
            return 1;
        }
        if (n == 1) {
            return 10;
        }
        int res = 10, cur = 9;
        for (int i = 0; i < n - 1; i++) {
            cur *= 9 - i;
            res += cur;
        }
        return res;
    }
}
```



### [1012. 至少有 1 位重复的数字](https://leetcode.cn/problems/numbers-with-repeated-digits/)

困难

给定正整数 `n`，返回在 `[1, n]` 范围内具有 **至少 1 位** 重复数字的正整数的个数。

**示例 1：**

```
输入：n = 20
输出：1
解释：具有至少 1 位重复数字的正数（<= 20）只有 11 。
```

C++版本

```c++
// 方法一：记忆化搜索
class Solution {
public:
    vector<vector<int>> dp;

    int f(int mask, const string &sn, int i, bool same) {
        if (i == sn.size()) {
            return 1;
        }
        if (!same && dp[i][mask] >= 0) {
            return dp[i][mask];
        }
        int res = 0, t = same ? (sn[i] - '0') : 9;
        for (int k = 0; k <= t; k++) {
            if (mask & (1 << k)) {
                continue;
            }
            res += f(mask == 0 && k == 0 ? mask : mask | (1 << k), sn, i + 1, same && k == t);
        }
        if (!same) {
            dp[i][mask] = res;
        }
        return res;
    }

    int numDupDigitsAtMostN(int n) {
        string sn = to_string(n);
        dp.resize(sn.size(), vector<int>(1 << 10, -1));
        return n + 1 - f(0, sn, 0, true);
    }
};

// 方法二：组合数学
class Solution {
public:
    int A(int x, int y) {
        int res = 1;
        for (int i = 0; i < x; i++) {
            res *= y--;
        }
        return res;
    }

    int f(int mask, const string &sn, int i, bool same) {
        if (i == sn.size()) {
            return 1;
        }
        int t = same ? sn[i] - '0' : 9, res = 0, c = __builtin_popcount(mask) + 1;
        for (int k = 0; k <= t; k++) {
            if (mask & (1 << k)) {
                continue;
            }
            if (same && k == t) {
                res += f(mask | (1 << t), sn, i + 1, true);
            } else if (mask == 0 && k == 0) {
                res += f(0, sn, i + 1, false);
            } else {
                res += A(sn.size() - 1 - i, 10 - c);
            }
        }
        return res;
    }

    int numDupDigitsAtMostN(int n) {
        string sn = to_string(n);
        return n + 1 - f(0, sn, 0, true);
    }
};
```

Java版本

```java
// 方法一：记忆化搜索
class Solution {
    int[][] dp;

    public int numDupDigitsAtMostN(int n) {
        String sn = String.valueOf(n);
        dp = new int[sn.length()][1 << 10];
        for (int i = 0; i < sn.length(); i++) {
            Arrays.fill(dp[i], -1);
        }
        return n + 1 - f(0, sn, 0, true);
    }

    public int f(int mask, String sn, int i, boolean same) {
        if (i == sn.length()) {
            return 1;
        }
        if (!same && dp[i][mask] >= 0) {
            return dp[i][mask];
        }
        int res = 0, t = same ? (sn.charAt(i) - '0') : 9;
        for (int k = 0; k <= t; k++) {
            if ((mask & (1 << k)) != 0) {
                continue;
            }
            res += f(mask == 0 && k == 0 ? mask : mask | (1 << k), sn, i + 1, same && k == t);
        }
        if (!same) {
            dp[i][mask] = res;
        }
        return res;
    }
}

// 方法二：组合数学
class Solution {
    public int numDupDigitsAtMostN(int n) {
        String sn = String.valueOf(n);
        return n + 1 - f(0, sn, 0, true);
    }

    public int f(int mask, String sn, int i, boolean same) {
        if (i == sn.length()) {
            return 1;
        }
        int t = same ? sn.charAt(i) - '0' : 9, res = 0, c = Integer.bitCount(mask) + 1;
        for (int k = 0; k <= t; k++) {
            if ((mask & (1 << k)) != 0) {
                continue;
            }
            if (same && k == t) {
                res += f(mask | (1 << t), sn, i + 1, true);
            } else if (mask == 0 && k == 0) {
                res += f(0, sn, i + 1, false);
            } else {
                res += A(sn.length() - 1 - i, 10 - c);
            }
        }
        return res;
    }

    public int A(int x, int y) {
        int res = 1;
        for (int i = 0; i < x; i++) {
            res *= y--;
        }
        return res;
    }
}
```



### [902. 最大为 N 的数字组合](https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/)

困难

给定一个按 **非递减顺序** 排列的数字数组 `digits` 。你可以用任意次数 `digits[i]` 来写的数字。例如，如果 `digits = ['1','3','5']`，我们可以写数字，如 `'13'`, `'551'`, 和 `'1351315'`。

返回 *可以生成的小于或等于给定整数 `n` 的正整数的个数* 。

**示例 1：**

```
输入：digits = ["1","3","5","7"], n = 100
输出：20
解释：
可写出的 20 个数字是：
1, 3, 5, 7, 11, 13, 15, 17, 31, 33, 35, 37, 51, 53, 55, 57, 71, 73, 75, 77.
```

C++版本

```c++
// 方法一：数位动态规划
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        string s = to_string(n);
        int m = digits.size(), k = s.size();
        vector<vector<int>> dp(k + 1, vector<int>(2));
        dp[0][1] = 1;
        for (int i = 1; i <= k; i++) {
            for (int j = 0; j < m; j++) {
                if (digits[j][0] == s[i - 1]) {
                    dp[i][1] = dp[i - 1][1];
                } else if (digits[j][0] < s[i - 1]) {
                    dp[i][0] += dp[i - 1][1];
                } else {
                    break;
                }
            }
            if (i > 1) {
                dp[i][0] += m + dp[i - 1][0] * m;
            }
        }
        return dp[k][0] + dp[k][1];
    }
};

// 方法二：数学
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        string s = to_string(n);
        int m = digits.size(), k = s.size();
        vector<int> bits;
        bool isLimit = true;
        for (int i = 0; i < k; i++) {
            if (!isLimit) {
                bits.emplace_back(m - 1);
            } else {
                int selectIndex = -1;
                for (int j = 0; j < m; j++) {
                    if (digits[j][0] <= s[i]) {
                        selectIndex = j;
                    } else {
                        break;
                    }
                }
                if (selectIndex >= 0) {
                    bits.emplace_back(selectIndex);
                    if (digits[selectIndex][0] < s[i]) {
                        isLimit = false;
                    }
                } else {
                    int len = bits.size();
                    while (!bits.empty() && bits.back() == 0) {
                        bits.pop_back();
                    }
                    if (!bits.empty()) {
                        bits.back() -= 1;
                    } else {
                        len--;
                    }
                    while ((int)bits.size() <= len) {
                        bits.emplace_back(m - 1);
                    }
                    isLimit = false;
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < bits.size(); i++) {
            ans = ans * m + (bits[i] + 1);
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：数位动态规划
class Solution {
    public int atMostNGivenDigitSet(String[] digits, int n) {
        String s = Integer.toString(n);
        int m = digits.length, k = s.length();
        int[][] dp = new int[k + 1][2];
        dp[0][1] = 1;
        for (int i = 1; i <= k; i++) {
            for (int j = 0; j < m; j++) {
                if (digits[j].charAt(0) == s.charAt(i - 1)) {
                    dp[i][1] = dp[i - 1][1];
                } else if (digits[j].charAt(0) < s.charAt(i - 1)) {
                    dp[i][0] += dp[i - 1][1];
                } else {
                    break;
                }
            }
            if (i > 1) {
                dp[i][0] += m + dp[i - 1][0] * m;
            }
        }
        return dp[k][0] + dp[k][1];
    }
}

// 方法二：数学
class Solution {
    public int atMostNGivenDigitSet(String[] digits, int n) {
        String s = Integer.toString(n);
        int m = digits.length, k = s.length();
        List<Integer> bits = new ArrayList<Integer>();
        boolean isLimit = true;
        for (int i = 0; i < k; i++) {
            if (!isLimit) {
                bits.add(m - 1);
            } else {
                int selectIndex = -1;
                for (int j = 0; j < m; j++) {
                    if (digits[j].charAt(0) <= s.charAt(i)) {
                        selectIndex = j;
                    } else {
                        break;
                    }
                }
                if (selectIndex >= 0) {
                    bits.add(selectIndex);
                    if (digits[selectIndex].charAt(0) < s.charAt(i)) {
                        isLimit = false;
                    }
                } else {
                    int len = bits.size();
                    while (!bits.isEmpty() && bits.get(bits.size() - 1) == 0) {
                        bits.remove(bits.size() - 1);
                    }
                    if (!bits.isEmpty()) {
                        bits.set(bits.size() - 1, bits.get(bits.size() - 1) - 1);
                    } else {
                        len--;
                    }
                    while (bits.size() <= len) {
                        bits.add(m - 1);
                    }
                    isLimit = false;
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < bits.size(); i++) {
            ans = ans * m + (bits.get(i) + 1);
        }
        return ans;
    }
}
```



### [788. 旋转数字](https://leetcode.cn/problems/rotated-digits/)

中等

我们称一个数 X 为好数, 如果它的每位数字逐个地被旋转 180 度后，我们仍可以得到一个有效的，且和 X 不同的数。要求每位数字都要被旋转。

如果一个数的每位数字被旋转以后仍然还是一个数字， 则这个数是有效的。0, 1, 和 8 被旋转后仍然是它们自己；2 和 5 可以互相旋转成对方（在这种情况下，它们以不同的方向旋转，换句话说，2 和 5 互为镜像）；6 和 9 同理，除了这些以外其他的数字旋转以后都不再是有效的数字。

现在我们有一个正整数 `N`, 计算从 `1` 到 `N` 中有多少个数 X 是好数？

**示例：**

```
输入: 10
输出: 4
解释: 
在[1, 10]中有四个好数： 2, 5, 6, 9。
注意 1 和 10 不是好数, 因为他们在旋转之后不变。
```

C++版本

```c++
// 方法一：枚举每一个数
class Solution {
public:
    int rotatedDigits(int n) {
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            string num = to_string(i);
            bool valid = true, diff = false;
            for (char ch: num) {
                if (check[ch - '0'] == -1) {
                    valid = false;
                }
                else if (check[ch - '0'] == 1) {
                    diff = true;
                }
            }
            if (valid && diff) {
                ++ans;
            }
        }
        return ans;
    }

private:
    static constexpr int check[10] = {0, 0, 1, -1, -1, 1, 1, -1, 0, 1};
};

// 方法二：数位动态规划
class Solution {
public:
    int rotatedDigits(int n) {
        vector<int> digits;
        while (n) {
            digits.push_back(n % 10);
            n /= 10;
        }
        reverse(digits.begin(), digits.end());
        
        memset(memo, -1, sizeof(memo));
        function<int(int, bool, bool)> dfs = [&](int pos, bool bound, bool diff) -> int {
            if (pos == digits.size()) {
                return diff;
            }
            if (memo[pos][bound][diff] != -1) {
                return memo[pos][bound][diff];
            }

            int ret = 0;
            for (int i = 0; i <= (bound ? digits[pos] : 9); ++i) {
                if (check[i] != -1) {
                    ret += dfs(
                        pos + 1,
                        bound && (i == digits[pos]),
                        diff || (check[i] == 1)
                    );
                }
            }
            return memo[pos][bound][diff] = ret;
        };
        
        int ans = dfs(0, true, false);
        return ans;
    }

private:
    static constexpr int check[10] = {0, 0, 1, -1, -1, 1, 1, -1, 0, 1};
    int memo[5][2][2];
};
```

Java版本

```java
// 方法一：枚举每一个数
class Solution {
    static int[] check = {0, 0, 1, -1, -1, 1, 1, -1, 0, 1};

    public int rotatedDigits(int n) {
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            String num = String.valueOf(i);
            boolean valid = true, diff = false;
            for (int j = 0; j < num.length(); ++j) {
                char ch = num.charAt(j);
                if (check[ch - '0'] == -1) {
                    valid = false;
                } else if (check[ch - '0'] == 1) {
                    diff = true;
                }
            }
            if (valid && diff) {
                ++ans;
            }
        }
        return ans;
    }
}

// 方法二：数位动态规划
class Solution {
    static int[] check = {0, 0, 1, -1, -1, 1, 1, -1, 0, 1};
    int[][][] memo = new int[5][2][2];
    List<Integer> digits = new ArrayList<Integer>();

    public int rotatedDigits(int n) {
        while (n != 0) {
            digits.add(n % 10);
            n /= 10;
        }
        Collections.reverse(digits);

        for (int i = 0; i < 5; ++i) {
            for (int j = 0; j < 2; ++j) {
                Arrays.fill(memo[i][j], -1);
            }
        }

        int ans = dfs(0, 1, 0);
        return ans;
    }

    public int dfs(int pos, int bound, int diff) {
        if (pos == digits.size()) {
            return diff;
        }
        if (memo[pos][bound][diff] != -1) {
            return memo[pos][bound][diff];
        }

        int ret = 0;
        for (int i = 0; i <= (bound != 0 ? digits.get(pos) : 9); ++i) {
            if (check[i] != -1) {
                ret += dfs(
                    pos + 1,
                    bound != 0 && i == digits.get(pos) ? 1 : 0,
                    diff != 0 || check[i] == 1 ? 1 : 0
                );
            }
        }
        return memo[pos][bound][diff] = ret;
    }
}
```







