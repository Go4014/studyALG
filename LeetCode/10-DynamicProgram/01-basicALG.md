# 动态规划

## 基础题目

### [509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)

简单

**斐波那契数** （通常用 `F(n)` 表示）形成的序列称为 **斐波那契数列** 。该数列由 `0` 和 `1` 开始，后面的每一项数字都是前面两项数字的和。也就是：

> F(0) = 0，F(1) = 1
> F(n) = F(n - 1) + F(n - 2)，其中 n > 1

给定 `n` ，请计算 `F(n)` 。

**示例 1：**

```
输入：n = 2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int fib(int n) {
        if (n < 2) {
            return n;
        }
        int p = 0, q = 0, r = 1;
        for (int i = 2; i <= n; ++i) {
            p = q; 
            q = r; 
            r = p + q;
        }
        return r;
    }
};

// 方法二：矩阵快速幂
class Solution {
public:
    int fib(int n) {
        if (n < 2) {
            return n;
        }
        vector<vector<int>> q{{1, 1}, {1, 0}};
        vector<vector<int>> res = matrix_pow(q, n - 1);
        return res[0][0];
    }

    vector<vector<int>> matrix_pow(vector<vector<int>>& a, int n) {
        vector<vector<int>> ret{{1, 0}, {0, 1}};
        while (n > 0) {
            if (n & 1) {
                ret = matrix_multiply(ret, a);
            }
            n >>= 1;
            a = matrix_multiply(a, a);
        }
        return ret;
    }

    vector<vector<int>> matrix_multiply(vector<vector<int>>& a, vector<vector<int>>& b) {
        vector<vector<int>> c{{0, 0}, {0, 0}};
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j];
            }
        }
        return c;
    }
};

// 方法三：通项公式
class Solution {
public:
    int fib(int n) {
        double sqrt5 = sqrt(5);
        double fibN = pow((1 + sqrt5) / 2, n) - pow((1 - sqrt5) / 2, n);
        return round(fibN / sqrt5);
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int fib(int n) {
        if (n < 2) {
            return n;
        }
        int p = 0, q = 0, r = 1;
        for (int i = 2; i <= n; ++i) {
            p = q; 
            q = r; 
            r = p + q;
        }
        return r;
    }
}

// 或者
class Solution {
    public int fib(int n) {
        int[] fibs = new int[n+1];
        for(int i = 0; i <= n; i++) {
            if(i == 0 || i == 1) {
                fibs[i] = i;
            } else {
                fibs[i] = fibs[i-1] + fibs[i-2];
            }
        }
        return fibs[n];
    }
}

// 方法二：矩阵快速幂
class Solution {
    public int fib(int n) {
        if (n < 2) {
            return n;
        }
        int[][] q = {{1, 1}, {1, 0}};
        int[][] res = pow(q, n - 1);
        return res[0][0];
    }

    public int[][] pow(int[][] a, int n) {
        int[][] ret = {{1, 0}, {0, 1}};
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
        int[][] c = new int[2][2];
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j];
            }
        }
        return c;
    }
}

// 方法三：通项公式
class Solution {
    public int fib(int n) {
        double sqrt5 = Math.sqrt(5);
        double fibN = Math.pow((1 + sqrt5) / 2, n) - Math.pow((1 - sqrt5) / 2, n);
        return (int) Math.round(fibN / sqrt5);
    }
}
```

