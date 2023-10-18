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



### [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

简单

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

**示例 1：**

```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

C++版本

```c++
// 方法一：暴力匹配
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(), m = needle.size();
        for (int i = 0; i + m <= n; i++) {
            bool flag = true;
            for (int j = 0; j < m; j++) {
                if (haystack[i + j] != needle[j]) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                return i;
            }
        }
        return -1;
    }
};

// 方法二：Knuth-Morris-Pratt 算法
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(), m = needle.size();
        if (m == 0) {
            return 0;
        }
        vector<int> pi(m);
        for (int i = 1, j = 0; i < m; i++) {
            while (j > 0 && needle[i] != needle[j]) {
                j = pi[j - 1];
            }
            if (needle[i] == needle[j]) {
                j++;
            }
            pi[i] = j;
        }
        for (int i = 0, j = 0; i < n; i++) {
            while (j > 0 && haystack[i] != needle[j]) {
                j = pi[j - 1];
            }
            if (haystack[i] == needle[j]) {
                j++;
            }
            if (j == m) {
                return i - m + 1;
            }
        }
        return -1;
    }
};
```

Java版本

```java
// 方法一：暴力匹配
class Solution {
    public int strStr(String haystack, String needle) {
        int n = haystack.length(), m = needle.length();
        for (int i = 0; i + m <= n; i++) {
            boolean flag = true;
            for (int j = 0; j < m; j++) {
                if (haystack.charAt(i + j) != needle.charAt(j)) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                return i;
            }
        }
        return -1;
    }
}

// 方法二：Knuth-Morris-Pratt 算法
class Solution {
    public int strStr(String haystack, String needle) {
        int n = haystack.length(), m = needle.length();
        if (m == 0) {
            return 0;
        }
        int[] pi = new int[m];
        for (int i = 1, j = 0; i < m; i++) {
            while (j > 0 && needle.charAt(i) != needle.charAt(j)) {
                j = pi[j - 1];
            }
            if (needle.charAt(i) == needle.charAt(j)) {
                j++;
            }
            pi[i] = j;
        }
        for (int i = 0, j = 0; i < n; i++) {
            while (j > 0 && haystack.charAt(i) != needle.charAt(j)) {
                j = pi[j - 1];
            }
            if (haystack.charAt(i) == needle.charAt(j)) {
                j++;
            }
            if (j == m) {
                return i - m + 1;
            }
        }
        return -1;
    }
}
```



### [1408. 数组中的字符串匹配](https://leetcode.cn/problems/string-matching-in-an-array/)

简单

给你一个字符串数组 `words` ，数组中的每个字符串都可以看作是一个单词。请你按 **任意** 顺序返回 `words` 中是其他单词的子字符串的所有单词。

如果你可以删除 `words[j]` 最左侧和/或最右侧的若干字符得到 `words[i]` ，那么字符串 `words[i]` 就是 `words[j]` 的一个子字符串。

**示例 1：**

```
输入：words = ["mass","as","hero","superhero"]
输出：["as","hero"]
解释："as" 是 "mass" 的子字符串，"hero" 是 "superhero" 的子字符串。
["hero","as"] 也是有效的答案。
```

C++版本

```c++
// 方法一：暴力枚举
class Solution {
public:
    vector<string> stringMatching(vector<string>& words) {
        vector<string> ret;
        for (int i = 0; i < words.size(); i++) {
            for (int j = 0; j < words.size(); j++) {
                if (i != j && words[j].find(words[i]) != string::npos) {
                    ret.push_back(words[i]);
                    break;
                }
            }
        }
        return ret;
    }
};
```

Java版本

```java
// 方法一：暴力枚举
class Solution {
    public List<String> stringMatching(String[] words) {
        List<String> ret = new ArrayList<String>();
        for (int i = 0; i < words.length; i++) {
            for (int j = 0; j < words.length; j++) {
                if (i != j && words[j].contains(words[i])) {
                    ret.add(words[i]);
                    break;
                }
            }
        }
        return ret;
    }
}
```



### [686. 重复叠加字符串匹配](https://leetcode.cn/problems/repeated-string-match/)

中等

给定两个字符串 `a` 和 `b`，寻找重复叠加字符串 `a` 的最小次数，使得字符串 `b` 成为叠加后的字符串 `a` 的子串，如果不存在则返回 `-1`。

**注意：**字符串 `"abc"` 重复叠加 0 次是 `""`，重复叠加 1 次是 `"abc"`，重复叠加 2 次是 `"abcabc"`。

**示例 1：**

```
输入：a = "abcd", b = "cdabcdab"
输出：3
解释：a 重复叠加三遍后为 "abcdabcdabcd", 此时 b 是其子串。
```

C++版本

```c++
// 方法一：Rabin-Karp 算法
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(), m = needle.size();
        if (m == 0) {
            return 0;
        }

        long long k1 = 1e9 + 7;
        long long k2 = 1337;
        srand((unsigned)time(NULL));
        long long kMod1 = rand() % k1 + k1;
        long long kMod2 = rand() % k2 + k2;

        long long hash_needle = 0;
        for (auto c : needle) {
            hash_needle = (hash_needle * kMod2 + c) % kMod1;
        }
        long long hash_haystack = 0, extra = 1;
        for (int i = 0; i < m - 1; i++) {
            hash_haystack = (hash_haystack * kMod2 + haystack[i % n]) % kMod1;
            extra = (extra * kMod2) % kMod1;
        }
        for (int i = m - 1; (i - m + 1) < n; i++) {
            hash_haystack = (hash_haystack * kMod2 + haystack[i % n]) % kMod1;
            if (hash_haystack == hash_needle) {
                return i - m + 1;
            }
            hash_haystack = (hash_haystack - extra * haystack[(i - m + 1) % n]) % kMod1;
            hash_haystack = (hash_haystack + kMod1) % kMod1;
        }
        return -1;
    }

    int repeatedStringMatch(string a, string b) {
        int an = a.size(), bn = b.size();
        int index = strStr(a, b);
        if (index == -1) {
            return -1;
        }
        if (an - index >= bn) {
            return 1;
        }
        return (bn + index - an - 1) / an + 2;
    }
};

// 方法二：Knuth-Morris-Pratt 算法
class Solution {
public:
    int strStr(string &haystack, string &needle) {
        int n = haystack.size(), m = needle.size();
        if (m == 0) {
            return 0;
        }
        vector<int> pi(m);
        for (int i = 1, j = 0; i < m; i++) {
            while (j > 0 && needle[i] != needle[j]) {
                j = pi[j - 1];
            }
            if (needle[i] == needle[j]) {
                j++;
            }
            pi[i] = j;
        }
        for (int i = 0, j = 0; i - j < n; i++) { // b 开始匹配的位置是否超过第一个叠加的 a
            while (j > 0 && haystack[i % n] != needle[j]) { // haystack 是循环叠加的字符串，所以取 i % n
                j = pi[j - 1];
            }
            if (haystack[i % n] == needle[j]) {
                j++;
            }
            if (j == m) {
                return i - m + 1;
            }
        }
        return -1;
    }

    int repeatedStringMatch(string a, string b) {
        int an = a.size(), bn = b.size();
        int index = strStr(a, b);
        if (index == -1) {
            return -1;
        }
        if (an - index >= bn) {
            return 1;
        }
        return (bn + index - an - 1) / an + 2;
    }
};
```

Java版本

```java
// 方法一：Rabin-Karp 算法
class Solution {
    static final int kMod1 = 1000000007;
    static final int kMod2 = 1337;

    public int repeatedStringMatch(String a, String b) {
        int an = a.length(), bn = b.length();
        int index = strStr(a, b);
        if (index == -1) {
            return -1;
        }
        if (an - index >= bn) {
            return 1;
        }
        return (bn + index - an - 1) / an + 2;
    }

    public int strStr(String haystack, String needle) {
        int n = haystack.length(), m = needle.length();
        if (m == 0) {
            return 0;
        }

        int k1 = 1000000009;
        int k2 = 1337;
        Random random = new Random();
        int kMod1 = random.nextInt(k1) + k1;
        int kMod2 = random.nextInt(k2) + k2;

        long hashNeedle = 0;
        for (int i = 0; i < m; i++) {
            char c = needle.charAt(i);
            hashNeedle = (hashNeedle * kMod2 + c) % kMod1;
        }
        long hashHaystack = 0, extra = 1;
        for (int i = 0; i < m - 1; i++) {
            hashHaystack = (hashHaystack * kMod2 + haystack.charAt(i % n)) % kMod1;
            extra = (extra * kMod2) % kMod1;
        }
        for (int i = m - 1; (i - m + 1) < n; i++) {
            hashHaystack = (hashHaystack * kMod2 + haystack.charAt(i % n)) % kMod1;
            if (hashHaystack == hashNeedle) {
                return i - m + 1;
            }
            hashHaystack = (hashHaystack - extra * haystack.charAt((i - m + 1) % n)) % kMod1;
            hashHaystack = (hashHaystack + kMod1) % kMod1;
        }
        return -1;
    }
}

// 方法二：Knuth-Morris-Pratt 算法
class Solution {
    public int repeatedStringMatch(String a, String b) {
        int an = a.length(), bn = b.length();
        int index = strStr(a, b);
        if (index == -1) {
            return -1;
        }
        if (an - index >= bn) {
            return 1;
        }
        return (bn + index - an - 1) / an + 2;
    }

    public int strStr(String haystack, String needle) {
        int n = haystack.length(), m = needle.length();
        if (m == 0) {
            return 0;
        }
        int[] pi = new int[m];
        for (int i = 1, j = 0; i < m; i++) {
            while (j > 0 && needle.charAt(i) != needle.charAt(j)) {
                j = pi[j - 1];
            }
            if (needle.charAt(i) == needle.charAt(j)) {
                j++;
            }
            pi[i] = j;
        }
        for (int i = 0, j = 0; i - j < n; i++) { // b 开始匹配的位置是否超过第一个叠加的 a
            while (j > 0 && haystack.charAt(i % n) != needle.charAt(j)) { // haystack 是循环叠加的字符串，所以取 i % n
                j = pi[j - 1];
            }
            if (haystack.charAt(i % n) == needle.charAt(j)) {
                j++;
            }
            if (j == m) {
                return i - m + 1;
            }
        }
        return -1;
    }
}
```



### [2156. 查找给定哈希值的子串](https://leetcode.cn/problems/find-substring-with-given-hash-value/)

困难

给定整数 `p` 和 `m` ，一个长度为 `k` 且下标从 **0** 开始的字符串 `s` 的哈希值按照如下函数计算：

- `hash(s, p, m) = (val(s[0]) * p0 + val(s[1]) * p1 + ... + val(s[k-1]) * pk-1) mod m`.

其中 `val(s[i])` 表示 `s[i]` 在字母表中的下标，从 `val('a') = 1` 到 `val('z') = 26` 。

给你一个字符串 `s` 和整数 `power`，`modulo`，`k` 和 `hashValue` 。请你返回 `s` 中 **第一个** 长度为 `k` 的 **子串** `sub` ，满足 `hash(sub, power, modulo) == hashValue` 。

测试数据保证一定 **存在** 至少一个这样的子串。

**子串** 定义为一个字符串中连续非空字符组成的序列。

**示例 1：**

```
输入：s = "leetcode", power = 7, modulo = 20, k = 2, hashValue = 0
输出："ee"
解释："ee" 的哈希值为 hash("ee", 7, 20) = (5 * 1 + 5 * 7) mod 20 = 40 mod 20 = 0 。
"ee" 是长度为 2 的第一个哈希值为 0 的子串，所以我们返回 "ee" 。
```

C++版本

```c++
// 方法一：数学
class Solution {
public:
    string subStrHash(string s, int power, int modulo, int k, int hashValue) {
        int mult = 1;   // power^k mod modulo
        int n = s.size();
        int pos = -1;   // 第一个符合要求子串的起始下标
        int h = 0;   // 子串哈希值
        // 预处理计算最后一个子串的哈希值和 power^k mod modulo
        for (int i = n - 1; i >= n - k; --i) {
            h = ((long long)h * power + (s[i] - 'a' + 1)) % modulo;
            if (i != n - k) {
                mult = (long long)mult * power % modulo;
            }
        }
        if (h == hashValue) {
            pos = n - k;
        }
        // 向前计算哈希值并尝试更新下标
        for (int i = n - k - 1; i >= 0; --i) {
            h = ((h - (long long)(s[i+k] - 'a' + 1) * mult % modulo + modulo) * power + (s[i] - 'a' + 1)) % modulo;
            if (h == hashValue) {
                pos = i;
            }
        }
        return s.substr(pos, k);
    }
};
```

Java版本

```java
// 方法一：数学
public class Solution {
    public String subStrHash(String s, int power, int modulo, int k, int hashValue) {
        int mult = 1;   // power^k mod modulo
        int n = s.length();
        int pos = -1;   // 第一个符合要求子串的起始下标
        int h = 0;      // 子串哈希值
        
        // 预处理计算最后一个子串的哈希值和 power^k mod modulo
        for (int i = n - 1; i >= n - k; i--) {
            h = (int) (((long) h * power + (s.charAt(i) - 'a' + 1)) % modulo);
            if (i != n - k) {
                mult = (int) ((long) mult * power % modulo);
            }
        }
        if (h == hashValue) {
            pos = n - k;
        }
        
        // 向前计算哈希值并尝试更新下标
        for (int i = n - k - 1; i >= 0; i--) {
            h = (int) (((h - (long) (s.charAt(i + k) - 'a' + 1) * mult % modulo + modulo) * power + (s.charAt(i) - 'a' + 1)) % modulo);
            if (h == hashValue) {
                pos = i;
            }
        }
        
        return s.substring(pos, pos + k);
    }
}
```



