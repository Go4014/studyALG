# 字符串

## 单模式串匹配

### [796. 旋转字符串](https://leetcode.cn/problems/rotate-string/)

简单

给定两个字符串, `s` 和 `goal`。如果在若干次旋转操作之后，`s` 能变成 `goal` ，那么返回 `true` 。

`s` 的 **旋转操作** 就是将 `s` 最左边的字符移动到最右边。 

- 例如, 若 `s = 'abcde'`，在旋转一次之后结果就是`'bcdea'` 。

**示例 1:**

```
输入: s = "abcde", goal = "cdeab"
输出: true
```

C++版本

```c++
// 方法一：模拟
class Solution {
public:
    bool rotateString(string s, string goal) {
        int m = s.size(), n = goal.size();
        if (m != n) {
            return false;
        }
        for (int i = 0; i < n; i++) {
            bool flag = true;
            for (int j = 0; j < n; j++) {
                if (s[(i + j) % n] != goal[j]) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                return true;
            }
        }
        return false;
    }
};

// 方法二：搜索子字符串
class Solution {
public:
    bool rotateString(string s, string goal) {
        return s.size() == goal.size() && (s + s).find(goal) != string::npos;
    }
};
```

Java版本

```java
// 方法一：模拟
class Solution {
    public boolean rotateString(String s, String goal) {
        int m = s.length(), n = goal.length();
        if (m != n) {
            return false;
        }
        for (int i = 0; i < n; i++) {
            boolean flag = true;
            for (int j = 0; j < n; j++) {
                if (s.charAt((i + j) % n) != goal.charAt(j)) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                return true;
            }
        }
        return false;
    }
}

// 方法二：搜索子字符串
class Solution {
    public boolean rotateString(String s, String goal) {
        return s.length() == goal.length() && (s + s).contains(goal);
    }
}
```



### [1668. 最大重复子字符串](https://leetcode.cn/problems/maximum-repeating-substring/)

简单

给你一个字符串 `sequence` ，如果字符串 `word` 连续重复 `k` 次形成的字符串是 `sequence` 的一个子字符串，那么单词 `word` 的 **重复值为 `k`** 。单词 `word` 的 **最****大重复值** 是单词 `word` 在 `sequence` 中最大的重复值。如果 `word` 不是 `sequence` 的子串，那么重复值 `k` 为 `0` 。

给你一个字符串 `sequence` 和 `word` ，请你返回 **最大重复值 `k`** 。

**示例 1：**

```
输入：sequence = "ababc", word = "ab"
输出：2
解释："abab" 是 "ababc" 的子字符串。
```

C++版本

```c++
// 方法一：简单枚举 + 动态规划
class Solution {
public:
    int maxRepeating(string sequence, string word) {
        int n = sequence.size(), m = word.size();
        if (n < m) {
            return 0;
        }

        vector<int> f(n);
        for (int i = m - 1; i < n; ++i) {
            bool valid = true;
            for (int j = 0; j < m; ++j) {
                if (sequence[i - m + j + 1] != word[j]) {
                    valid = false;
                    break;
                }
            }
            if (valid) {
                f[i] = (i == m - 1 ? 0 : f[i - m]) + 1;
            }
        }
        
        return *max_element(f.begin(), f.end());
    }
};

// 方法二：KMP 算法 + 动态规划
class Solution {
public:
    int maxRepeating(string sequence, string word) {
        int n = sequence.size(), m = word.size();
        if (n < m) {
            return 0;
        }

        vector<int> fail(m, -1);
        for (int i = 1; i < m; ++i) {
            int j = fail[i - 1];
            while (j != -1 && word[j + 1] != word[i]) {
                j = fail[j];
            }
            if (word[j + 1] == word[i]) {
                fail[i] = j + 1;
            }
        }

        vector<int> f(n);
        int j = -1;
        for (int i = 0; i < n; ++i) {
            while (j != -1 && word[j + 1] != sequence[i]) {
                j = fail[j];
            }
            if (word[j + 1] == sequence[i]) {
                ++j;
                if (j == m - 1) {
                    f[i] = (i >= m ? f[i - m] : 0) + 1;
                    j = fail[j];
                }
            }
        }

        return *max_element(f.begin(), f.end());
    }
};
```

Java版本

```java
// 方法一：简单枚举 + 动态规划
class Solution {
    public int maxRepeating(String sequence, String word) {
        int n = sequence.length(), m = word.length();
        if (n < m) {
            return 0;
        }

        int[] f = new int[n];
        for (int i = m - 1; i < n; ++i) {
            boolean valid = true;
            for (int j = 0; j < m; ++j) {
                if (sequence.charAt(i - m + j + 1) != word.charAt(j)) {
                    valid = false;
                    break;
                }
            }
            if (valid) {
                f[i] = (i == m - 1 ? 0 : f[i - m]) + 1;
            }
        }

        return Arrays.stream(f).max().getAsInt();
    }
}

// 方法二：KMP 算法 + 动态规划
class Solution {
    public int maxRepeating(String sequence, String word) {
        int n = sequence.length(), m = word.length();
        if (n < m) {
            return 0;
        }

        int[] fail = new int[m];
        Arrays.fill(fail, -1);
        for (int i = 1; i < m; ++i) {
            int j = fail[i - 1];
            while (j != -1 && word.charAt(j + 1) != word.charAt(i)) {
                j = fail[j];
            }
            if (word.charAt(j + 1) == word.charAt(i)) {
                fail[i] = j + 1;
            }
        }

        int[] f = new int[n];
        int j = -1;
        for (int i = 0; i < n; ++i) {
            while (j != -1 && word.charAt(j + 1) != sequence.charAt(i)) {
                j = fail[j];
            }
            if (word.charAt(j + 1) == sequence.charAt(i)) {
                ++j;
                if (j == m - 1) {
                    f[i] = (i >= m ? f[i - m] : 0) + 1;
                    j = fail[j];
                }
            }
        }

        return Arrays.stream(f).max().getAsInt();
    }
}
```



### [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

简单

给定一个非空的字符串 `s` ，检查是否可以通过由它的一个子串重复多次构成。

**示例 1:**

```
输入: s = "abab"
输出: true
解释: 可由子串 "ab" 重复两次构成。
```

C++版本

```c++
// 方法一：枚举
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int n = s.size();
        for (int i = 1; i * 2 <= n; ++i) {
            if (n % i == 0) {
                bool match = true;
                for (int j = i; j < n; ++j) {
                    if (s[j] != s[j - i]) {
                        match = false;
                        break;
                    }
                }
                if (match) {
                    return true;
                }
            }
        }
        return false;
    }
};

// 方法二：字符串匹配
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        return (s + s).find(s, 1) != s.size();
    }
};

// // 方法三：KMP 算法
class Solution {
public:
    bool kmp(const string& query, const string& pattern) {
        int n = query.size();
        int m = pattern.size();
        vector<int> fail(m, -1);
        for (int i = 1; i < m; ++i) {
            int j = fail[i - 1];
            while (j != -1 && pattern[j + 1] != pattern[i]) {
                j = fail[j];
            }
            if (pattern[j + 1] == pattern[i]) {
                fail[i] = j + 1;
            }
        }
        int match = -1;
        for (int i = 1; i < n - 1; ++i) {
            while (match != -1 && pattern[match + 1] != query[i]) {
                match = fail[match];
            }
            if (pattern[match + 1] == query[i]) {
                ++match;
                if (match == m - 1) {
                    return true;
                }
            }
        }
        return false;
    }

    bool repeatedSubstringPattern(string s) {
        return kmp(s + s, s);
    }
};
```

Java版本

```java
// 方法一：枚举
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();
        for (int i = 1; i * 2 <= n; ++i) {
            if (n % i == 0) {
                boolean match = true;
                for (int j = i; j < n; ++j) {
                    if (s.charAt(j) != s.charAt(j - i)) {
                        match = false;
                        break;
                    }
                }
                if (match) {
                    return true;
                }
            }
        }
        return false;
    }
}

// 方法二：字符串匹配
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return (s + s).indexOf(s, 1) != s.length();
    }
}

// 方法三：KMP 算法
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return kmp(s + s, s);
    }

    public boolean kmp(String query, String pattern) {
        int n = query.length();
        int m = pattern.length();
        int[] fail = new int[m];
        Arrays.fill(fail, -1);
        for (int i = 1; i < m; ++i) {
            int j = fail[i - 1];
            while (j != -1 && pattern.charAt(j + 1) != pattern.charAt(i)) {
                j = fail[j];
            }
            if (pattern.charAt(j + 1) == pattern.charAt(i)) {
                fail[i] = j + 1;
            }
        }
        int match = -1;
        for (int i = 1; i < n - 1; ++i) {
            while (match != -1 && pattern.charAt(match + 1) != query.charAt(i)) {
                match = fail[match];
            }
            if (pattern.charAt(match + 1) == query.charAt(i)) {
                ++match;
                if (match == m - 1) {
                    return true;
                }
            }
        }
        return false;
    }
}
```





