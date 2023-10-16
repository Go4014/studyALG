# BruteForce

Java版本

```java
public class BruteForce {

    public static int bruteForce(String T, String p) {
        int n = T.length();
        int m = p.length();

        int i = 0, j = 0;

        while (i < n && j < m) {
            if (T.charAt(i) == p.charAt(j)) {
                i++;
                j++;
            } else {
                i = i - (j - 1);
                j = 0;
            }
        }

        if (j == m) {
            return i - j;
        } else {
            return -1;
        }
    }
}
```

C++版本

```C++
int bruteForce(const string& T, const string& p) {
    int n = T.length();
    int m = p.length();

    int i = 0, j = 0;

    while (i < n && j < m) {
        if (T[i] == p[j]) {
            i++;
            j++;
        } else {
            i = i - (j - 1);
            j = 0;
        }
    }

    if (j == m) {
        return i - j;
    } else {
        return -1;
    }
}
```



# Rabin-Karp

Java版本

```java
public class RabinKarp {

    public static int rabinKarp(String T, String p, int d, int q) {
        int n = T.length();
        int m = p.length();
        if (n < m) {
            return -1;
        }

        int hash_p = 0, hash_t = 0;

        for (int i = 0; i < m; i++) {
            hash_p = (hash_p * d + (int) p.charAt(i)) % q;
            hash_t = (hash_t * d + (int) T.charAt(i)) % q;
        }

        int power = 1;
        for (int i = 0; i < m - 1; i++) {
            power = (power * d) % q;
        }

        for (int i = 0; i <= n - m; i++) {
            if (hash_p == hash_t) {
                boolean match = true;
                for (int j = 0; j < m; j++) {
                    if (T.charAt(i + j) != p.charAt(j)) {
                        match = false;
                        break;
                    }
                }
                if (match) {
                    return i;
                }
            }
            if (i < n - m) {
                hash_t = (hash_t - power * T.charAt(i)) % q;
                hash_t = (hash_t * d + T.charAt(i + m)) % q;
                hash_t = (hash_t + q) % q;
            }
        }
        return -1;
    }
}
```

C++版本

```c++
int rabinKarp(const string& T, const string& p, int d, int q) {
    int n = T.length();
    int m = p.length();
    if (n < m) {
        return -1;
    }

    int hash_p = 0;
    int hash_t = 0;

    for (int i = 0; i < m; i++) {
        hash_p = (hash_p * d + p[i]) % q;
        hash_t = (hash_t * d + T[i]) % q;
    }

    int power = 1;
    for (int i = 0; i < m - 1; i++) {
        power = (power * d) % q;
    }

    for (int i = 0; i <= n - m; i++) {
        if (hash_p == hash_t) {
            bool match = true;
            for (int j = 0; j < m; j++) {
                if (T[i + j] != p[j]) {
                    match = false;
                    break;
                }
            }
            if (match) {
                return i;
            }
        }
        if (i < n - m) {
            hash_t = (hash_t - power * T[i]) % q;
            hash_t = (hash_t * d + T[i + m]) % q;
            hash_t = (hash_t + q) % q;
        }
    }
    return -1;
}
```



# KMP

Java版本

```java
public class KMP {

    public static int[] generateNext(String p) {
        int m = p.length();
        int[] next = new int[m];

        int left = 0;
        for (int right = 1; right < m; right++) {
            while (left > 0 && p.charAt(left) != p.charAt(right)) {
                left = next[left - 1];
            }
            if (p.charAt(left) == p.charAt(right)) {
                left++;
            }
            next[right] = left;
        }
        return next;
    }

    public static int kmp(String T, String p) {
        int n = T.length();
        int m = p.length();

        int[] next = generateNext(p);

        int j = 0;
        for (int i = 0; i < n; i++) {
            while (j > 0 && T.charAt(i) != p.charAt(j)) {
                j = next[j - 1];
            }
            if (T.charAt(i) == p.charAt(j)) {
                j++;
            }
            if (j == m) {
                return i - j + 1;
            }
        }
        return -1;
    }
}
```

C++版本

```c++
vector<int> generateNext(const string& p) {
    int m = p.length();
    vector<int> next(m, 0);

    int left = 0;
    for (int right = 1; right < m; right++) {
        while (left > 0 && p[left] != p[right]) {
            left = next[left - 1];
        }
        if (p[left] == p[right]) {
            left++;
        }
        next[right] = left;
    }
    return next;
}

int kmp(const string& T, const string& p) {
    int n = T.length();
    int m = p.length();

    vector<int> next = generateNext(p);

    int j = 0;
    for (int i = 0; i < n; i++) {
        while (j > 0 && T[i] != p[j]) {
            j = next[j - 1];
        }
        if (T[i] == p[j]) {
            j++;
        }
        if (j == m) {
            return i - j + 1;
        }
    }
    return -1;
}
```



# Boyer-Moore

Java版本

```java
public class BoyerMoore {

    public static int boyerMoore(String T, String p) {
        int n = T.length();
        int m = p.length();

        Map<Character, Integer> bcTable = generateBadCharTable(p);
        int[] gsList = generateGoodSuffixList(p);

        int i = 0;
        while (i <= n - m) {
            int j = m - 1;
            while (j >= 0 && T.charAt(i + j) == p.charAt(j)) {
                j--;
            }
            if (j < 0) {
                return i;
            }
            int badMove = j - bcTable.getOrDefault(T.charAt(i + j), -1);
            int goodMove = gsList[j];
            i += Math.max(badMove, goodMove);
        }
        return -1;
    }

    public static Map<Character, Integer> generateBadCharTable(String p) {
        Map<Character, Integer> bcTable = new HashMap<>();
        int m = p.length();
        for (int i = 0; i < m; i++) {
            bcTable.put(p.charAt(i), i);
        }
        return bcTable;
    }

    public static int[] generateGoodSuffixList(String p) {
        int m = p.length();
        int[] gsList = new int[m];
        Arrays.fill(gsList, m);

        int[] suffix = generateSuffixArray(p);
        int j = 0;

        for (int i = m - 1; i >= 0; i--) {
            if (suffix[i] == i + 1) {
                while (j < m - 1 - i) {
                    if (gsList[j] == m) {
                        gsList[j] = m - 1 - i;
                    }
                    j++;
                }
            }
        }

        for (int i = 0; i < m - 1; i++) {
            gsList[m - 1 - suffix[i]] = m - 1 - i;
        }
        return gsList;
    }

    public static int[] generateSuffixArray(String p) {
        int m = p.length();
        int[] suffix = new int[m];
        Arrays.fill(suffix, m);

        for (int i = m - 2; i >= 0; i--) {
            int start = i;
            while (start >= 0 && p.charAt(start) == p.charAt(m - 1 - i + start)) {
                start--;
            }
            suffix[i] = i - start;
        }
        return suffix;
    }
}
```

C++版本

```c++
unordered_map<char, int> generateBadCharTable(const string& p) {
    unordered_map<char, int> bcTable;
    int m = p.length();

    for (int i = 0; i < m; i++) {
        bcTable[p[i]] = i;
    }
    return bcTable;
}

vector<int> generateGoodSuffixList(const string& p) {
    int m = p.length();
    vector<int> gsList(m, m);

    vector<int> suffix = generateSuffixArray(p);
    int j = 0;

    for (int i = m - 1; i >= 0; i--) {
        if (suffix[i] == i + 1) {
            while (j < m - 1 - i) {
                if (gsList[j] == m) {
                    gsList[j] = m - 1 - i;
                }
                j++;
            }
        }
    }

    for (int i = 0; i < m - 1; i++) {
        gsList[m - 1 - suffix[i]] = m - 1 - i;
    }
    return gsList;
}

vector<int> generateSuffixArray(const string& p) {
    int m = p.length();
    vector<int> suffix(m, m);

    for (int i = m - 2; i >= 0; i--) {
        int start = i;
        while (start >= 0 && p[start] == p[m - 1 - i + start]) {
            start--;
        }
        suffix[i] = i - start;
    }
    return suffix;
}

int boyerMoore(const string& T, const string& p) {
    int n = T.length();
    int m = p.length();

    unordered_map<char, int> bcTable = generateBadCharTable(p);
    vector<int> gsList = generateGoodSuffixList(p);

    int i = 0;
    while (i <= n - m) {
        int j = m - 1;
        while (j >= 0 && T[i + j] == p[j]) {
            j--;
        }
        if (j < 0) {
            return i;
        }
        int badMove = j - bcTable[T[i + j]];
        int goodMove = gsList[j];
        i += std::max(badMove, goodMove);
    }
    return -1;
}
```



# Horspool

Java版本

```java
public class Horspool {

    public static int horspool(String T, String p) {
        int n = T.length();
        int m = p.length();

        Map<Character, Integer> bcTable = generateBadCharTable(p);

        int i = 0;
        while (i <= n - m) {
            int j = m - 1;
            while (j >= 0 && T.charAt(i + j) == p.charAt(j)) {
                j--;
            }
            if (j < 0) {
                return i;
            }
            i += bcTable.getOrDefault(T.charAt(i + m - 1), m);
        }
        return -1;
    }

    public static Map<Character, Integer> generateBadCharTable(String p) {
        int m = p.length();
        Map<Character, Integer> bcTable = new HashMap<>();

        for (int i = 0; i < m - 1; i++) {
            bcTable.put(p.charAt(i), m - 1 - i);
        }
        return bcTable;
    }
}
```

C++版本

```c++
int horspool(const string& T, const string& p) {
    int n = T.length();
    int m = p.length();

    unordered_map<char, int> bcTable = generateBadCharTable(p);

    int i = 0;
    while (i <= n - m) {
        int j = m - 1;
        while (j >= 0 && T[i + j] == p[j]) {
            j--;
        }
        if (j < 0) {
            return i;
        }
        i += bcTable[T[i + m - 1]];
    }
    return -1;
}

unordered_map<char, int> generateBadCharTable(const string& p) {
    int m = p.length();
    unordered_map<char, int> bcTable;

    for (int i = 0; i < m - 1; i++) {
        bcTable[p[i]] = m - 1 - i;
    }
    return bcTable;
}
```



# Sunday

Java版本

```java
public class Sunday {

    public static int sunday(String T, String p) {
        int n = T.length();
        int m = p.length();
        
        Map<Character, Integer> bcTable = generateBadCharTable(p);
        
        int i = 0;
        while (i <= n - m) {
            int j = 0;
            if (T.substring(i, i + m).equals(p)) {
                return i;
            }
            if (i + m >= n) {
                return -1;
            }
            i += bcTable.getOrDefault(T.charAt(i + m), m + 1);
        }
        return -1;
    }

    public static Map<Character, Integer> generateBadCharTable(String p) {
        int m = p.length();
        Map<Character, Integer> bcTable = new HashMap<>();
        
        for (int i = 0; i < m; i++) {
            bcTable.put(p.charAt(i), m - i);
        }
        return bcTable;
    }
}
```

C++版本

```c++
int sunday(const string& T, const string& p) {
    int n = T.length();
    int m = p.length();
    
    unordered_map<char, int> bcTable = generateBadCharTable(p);
    
    int i = 0;
    while (i <= n - m) {
        int j = 0;
        if (T.substr(i, m) == p) {
            return i;
        }
        if (i + m >= n) {
            return -1;
        }
        i += bcTable[T[i + m]];
    }
    return -1;
}

unordered_map<char, int> generateBadCharTable(const string& p) {
    int m = p.length();
    unordered_map<char, int> bcTable;
    
    for (int i = 0; i < m; i++) {
        bcTable[p[i]] = m - i;
    }
    return bcTable;
}
```

