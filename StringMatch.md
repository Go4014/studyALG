# BruteForce

暴力匹配算法，用于在文本中查找模式串的起始位置。算法的主要思想是通过两个指针i和j在文本和模式串上进行遍历，当字符匹配成功时，两个指针同时向后移动一位；当字符匹配失败时，通过回溯的方式重新开始匹配。最终，如果模式串完全匹配成功，则返回模式串在文本中的起始位置；否则，返回匹配失败的标志-1。

Java版本

```java
public class BruteForce {

    public static int bruteForce(String T, String p) {
        int n = T.length();  // 获取文本T的长度
        int m = p.length();  // 获取模式串p的长度

        int i = 0, j = 0;  // 初始化两个指针i和j，分别指向文本T和模式串p的起始位置

        while (i < n && j < m) {  // 循环遍历文本T和模式串p
            if (T.charAt(i) == p.charAt(j)) {  // 如果当前字符匹配成功
                i++;  // 移动文本T的指针i
                j++;  // 移动模式串p的指针j
            } else {  // 如果当前字符匹配失败
                i = i - (j - 1);  // 回溯文本T的指针i，使其回到上一次匹配的后一个位置
                j = 0;  // 重置模式串p的指针j，使其重新从模式串的起始位置开始匹配
            }
        }

        if (j == m) {  // 如果模式串p完全匹配成功
            return i - j;  // 返回模式串p在文本T中的起始位置
        } else {
            return -1;  // 返回匹配失败的标志
        }
    }
}
```

C++版本

```C++
int bruteForce(const string& T, const string& p) {
    int n = T.length();  // 获取文本T的长度
    int m = p.length();  // 获取模式串p的长度

    int i = 0, j = 0;  // 初始化两个指针i和j，分别指向文本T和模式串p的起始位置

    while (i < n && j < m) {  // 循环遍历文本T和模式串p
        if (T[i] == p[j]) {  // 如果当前字符匹配成功
            i++;  // 移动文本T的指针i
            j++;  // 移动模式串p的指针j
        } else {  // 如果当前字符匹配失败
            i = i - (j - 1);  // 回溯文本T的指针i，使其回到上一次匹配的后一个位置
            j = 0;  // 重置模式串p的指针j，使其重新从模式串的起始位置开始匹配
        }
    }

    if (j == m) {  // 如果模式串p完全匹配成功
        return i - j;  // 返回模式串p在文本T中的起始位置
    } else {
        return -1;  // 返回匹配失败的标志
    }
}
```



# Rabin-Karp

Rabin-Karp算法，用于在文本中查找模式串的起始位置。算法的主要思想是通过哈希值的比较来快速判断文本窗口内的字符是否与模式串匹配，从而避免逐个字符比较的开销。具体步骤如下：

> 1. 计算模式串p和文本T的初始哈希值。
> 2. 使用滑动窗口的方式，在文本T中移动窗口，比较窗口内的文本和模式串的哈希值。
> 3. 如果哈希值相等，则逐个字符比较窗口内的文本和模式串，确认是否匹配。
> 4. 如果匹配成功，返回模式串在文本T中的起始位置。
> 5. 如果匹配失败，更新窗口内文本的哈希值，继续移动窗口。
> 6. 重复步骤2-5，直到找到匹配或遍历完整个文本T。
>
> 其中，d是进制数，q是模数，用于计算哈希值。通过不断更新哈希值和利用进制运算。

Java版本

```java
public class RabinKarp {

    public static int rabinKarp(String T, String p, int d, int q) {
        int n = T.length();  // 获取文本T的长度
        int m = p.length();  // 获取模式串p的长度
        if (n < m) {
            return -1;  // 如果文本长度小于模式串长度，直接返回匹配失败的标志
        }

        int hash_p = 0, hash_t = 0;  // 初始化模式串p和文本T的哈希值

        for (int i = 0; i < m; i++) {  // 计算模式串p的哈希值和文本T的前m个字符的哈希值
            hash_p = (hash_p * d + (int) p.charAt(i)) % q;  // 使用进制d计算模式串p的哈希值
            hash_t = (hash_t * d + (int) T.charAt(i)) % q;  // 使用进制d计算文本T的哈希值
        }

        int power = 1;
        for (int i = 0; i < m - 1; i++) {  // 计算进制d的m-1次方
            power = (power * d) % q;  // 使用取余运算确保结果在模数q范围内
        }

        for (int i = 0; i <= n - m; i++) {  // 在文本T中滑动窗口查找匹配
            if (hash_p == hash_t) {  // 如果模式串p的哈希值和当前窗口内的文本哈希值相等
                boolean match = true;
                for (int j = 0; j < m; j++) {  // 逐个字符比较模式串p和当前窗口内的文本
                    if (T.charAt(i + j) != p.charAt(j)) {
                        match = false;  // 如果有字符不匹配，标记为匹配失败
                        break;
                    }
                }
                if (match) {
                    return i;  // 如果所有字符匹配成功，返回模式串在文本T中的起始位置
                }
            }
            if (i < n - m) {  // 如果窗口还未到达文本末尾
                hash_t = (hash_t - power * T.charAt(i)) % q;  // 更新窗口内文本的哈希值
                hash_t = (hash_t * d + T.charAt(i + m)) % q;  // 加入下一个字符的哈希值
                hash_t = (hash_t + q) % q;  // 使用取余运算确保结果在模数q范围内
            }
        }
        return -1;  // 如果未找到匹配，返回匹配失败的标志
    }
}
```

C++版本

```c++
int rabinKarp(const string& T, const string& p, int d, int q) {
    int n = T.length();  // 获取文本T的长度
    int m = p.length();  // 获取模式串p的长度
    if (n < m) {
        return -1;  // 如果文本长度小于模式串长度，直接返回匹配失败的标志
    }

    int hash_p = 0;  // 初始化模式串p的哈希值
    int hash_t = 0;  // 初始化文本T的哈希值

    for (int i = 0; i < m; i++) {  // 计算模式串p的哈希值和文本T的前m个字符的哈希值
        hash_p = (hash_p * d + p[i]) % q;  // 使用进制d计算模式串p的哈希值
        hash_t = (hash_t * d + T[i]) % q;  // 使用进制d计算文本T的哈希值
    }

    int power = 1;
    for (int i = 0; i < m - 1; i++) {  // 计算进制d的m-1次方
        power = (power * d) % q;  // 使用取余运算确保结果在模数q范围内
    }

    for (int i = 0; i <= n - m; i++) {  // 在文本T中滑动窗口查找匹配
        if (hash_p == hash_t) {  // 如果模式串p的哈希值和当前窗口内的文本哈希值相等
            bool match = true;
            for (int j = 0; j < m; j++) {  // 逐个字符比较模式串p和当前窗口内的文本
                if (T[i + j] != p[j]) {
                    match = false;  // 如果有字符不匹配，标记为匹配失败
                    break;
                }
            }
            if (match) {
                return i;  // 如果所有字符匹配成功，返回模式串在文本T中的起始位置
            }
        }
        if (i < n - m) {  // 如果窗口还未到达文本末尾
            hash_t = (hash_t - power * T[i]) % q;  // 更新窗口内文本的哈希值
            hash_t = (hash_t * d + T[i + m]) % q;  // 加入下一个字符的哈希值
            hash_t = (hash_t + q) % q;  // 使用取余运算确保结果在模数q范围内
        }
    }
    return -1;  // 如果未找到匹配，返回匹配失败的标志
}
```



# KMP

KMP算法的核心是生成模式串的next数组，该数组记录了在匹配过程中需要回溯的位置。在生成next数组时，通过不断比较模式串中的字符，确定回溯位置。然后，在匹配过程中，根据next数组的值进行回溯操作，尽量减少不必要的比较。

> KMP算法的主要步骤：
>
> 1. 生成模式串p的next数组，用于存储匹配失败时的回溯位置。
> 2. 初始化模式串p的匹配指针j为0。
> 3. 遍历文本T的字符，使用i表示当前匹配的文本字符的位置。
> 4. 如果当前文本字符与模式串字符不匹配，根据next数组回溯模式串的指针j。
> 5. 如果当前文本字符与模式串字符匹配，模式串指针j右移。
> 6. 如果模式串指针j达到模式串长度m，表示匹配成功，返回模式串在文本T中的起始位置。
> 7. 如果遍历完整个文本T都没有匹配成功，则返回匹配失败的标志。

KMP算法通过预处理模式串，利用已经匹配过的信息，避免了不必要的回溯操作，从而提高了匹配的效率。

Java版本

```java
public class KMP {

    // 生成模式串p的next数组
    public static int[] generateNext(String p) {
        int m = p.length();
        int[] next = new int[m];  // 创建next数组，用于存储匹配失败时的回溯位置

        int left = 0;  // 左边界，表示当前已匹配的字符数
        for (int right = 1; right < m; right++) {  // 从第二个字符开始计算next数组
            while (left > 0 && p.charAt(left) != p.charAt(right)) {
                left = next[left - 1];  // 回溯到上一个匹配位置
            }
            if (p.charAt(left) == p.charAt(right)) {
                left++;  // 如果当前字符匹配成功，左边界右移
            }
            next[right] = left;  // 记录当前位置的回溯位置
        }
        return next;
    }

    // KMP算法主函数
    public static int kmp(String T, String p) {
        int n = T.length();  // 文本T的长度
        int m = p.length();  // 模式串p的长度

        int[] next = generateNext(p);  // 生成模式串p的next数组

        int j = 0;  // 模式串p的匹配指针
        for (int i = 0; i < n; i++) {  // 遍历文本T
            while (j > 0 && T.charAt(i) != p.charAt(j)) {
                j = next[j - 1];  // 回溯到上一个匹配位置
            }
            if (T.charAt(i) == p.charAt(j)) {
                j++;  // 如果当前字符匹配成功，模式串指针右移
            }
            if (j == m) {
                return i - j + 1;  // 匹配成功，返回模式串在文本T中的起始位置
            }
        }
        return -1;  // 匹配失败，返回失败标志
    }
}
```

C++版本

```c++
vector<int> generateNext(const string& p) {
    int m = p.length();
    vector<int> next(m, 0);  // 创建next数组，用于存储匹配失败时的回溯位置

    int left = 0;  // 左边界，表示当前已匹配的字符数
    for (int right = 1; right < m; right++) {  // 从第二个字符开始计算next数组
        while (left > 0 && p[left] != p[right]) {
            left = next[left - 1];  // 回溯到上一个匹配位置
        }
        if (p[left] == p[right]) {
            left++;  // 如果当前字符匹配成功，左边界右移
        }
        next[right] = left;  // 记录当前位置的回溯位置
    }
    return next;
}

int kmp(const string& T, const string& p) {
    int n = T.length();  // 文本T的长度
    int m = p.length();  // 模式串p的长度

    vector<int> next = generateNext(p);  // 生成模式串p的next数组

    int j = 0;  // 模式串p的匹配指针
    for (int i = 0; i < n; i++) {  // 遍历文本T
        while (j > 0 && T[i] != p[j]) {
            j = next[j - 1];  // 回溯到上一个匹配位置
        }
        if (T[i] == p[j]) {
            j++;  // 如果当前字符匹配成功，模式串指针右移
        }
        if (j == m) {
            return i - j + 1;  // 匹配成功，返回模式串在文本T中的起始位置
        }
    }
    return -1;  // 匹配失败，返回失败标志
}
```



# Boyer-Moore

Boyer-Moore算法是一种高效的字符串匹配算法，它利用了坏字符规则和好后缀规则来进行快速匹配和移动。

Boyer-Moore算法的思路和实现步骤：

> 1. 坏字符规则（Bad Character Rule）：
>    - 首先，生成一个坏字符表（Bad Character Table），用于记录模式串中每个字符最后出现的位置。
>    - 遍历模式串，记录每个字符最后出现的位置。如果一个字符在模式串中多次出现，记录最右边的位置。
>    - 如果在文本中的某个位置发生不匹配，我们将模式串中的最后一个字符与文本中的字符进行比较。
>    - 如果该字符在模式串中存在，我们可以根据坏字符表中记录的最右位置将模式串向右滑动，使得该字符对齐。
>    - 如果该字符在模式串中不存在，我们可以将模式串整体右滑，使得模式串中的最后一个字符与文本中的不匹配字符对齐。
> 2. 好后缀规则（Good Suffix Rule）：
>    - 首先，生成一个好后缀表（Good Suffix Table），用于记录模式串中每个后缀的匹配位置。
>    - 生成后缀数组（Suffix Array），从模式串倒数第二个字符开始，向前遍历，计算每个后缀的长度。
>    - 处理好后缀表：根据后缀数组的值，更新好后缀表中的对应位置。如果好后缀在模式串中只出现一次，则更新对应位置的值。
>    - 如果在文本中的某个位置发生不匹配，我们将模式串的后缀与文本中的子串进行比较。
>    - 如果在模式串的后缀中存在与文本中的子串匹配的部分，我们可以将模式串向右滑动，使得该匹配部分对齐。
>    - 如果不存在这样的匹配部分，我们可以根据好后缀表中记录的值将模式串向右滑动，使得模式串中的某个好后缀与文本中的子串对齐。
> 3. Boyer-Moore算法主函数：
>    - 初始化文本T和模式串p的长度。
>    - 生成坏字符表和好后缀表。
>    - 使用两个指针i和j进行匹配，从文本T的起始位置开始，每次比较模式串p的末尾字符和文本T中对应位置的字符。
>    - 如果匹配成功，继续向前匹配，直到模式串p完全匹配。
>    - 如果匹配失败，根据坏字符移动距离和好后缀移动距离，选择较大的值进行移动。
>    - 重复上述步骤，直到遍历完整个文本T。
>    - 如果找到匹配的位置，返回模式串在文本T中的起始位置；否则，返回失败标志。

通过预处理生成坏字符表和好后缀表，并利用这些表来决定每次匹配失败时的移动距离，Boyer-Moore算法能够快速跳过不匹配的字符和子串，从而提高匹配效率。

Java版本

```java
public class BoyerMoore {

    // Boyer-Moore算法主函数
    public static int boyerMoore(String T, String p) {
        int n = T.length();  // 文本T的长度
        int m = p.length();  // 模式串p的长度

        Map<Character, Integer> bcTable = generateBadCharTable(p);  // 生成坏字符表
        int[] gsList = generateGoodSuffixList(p);  // 生成好后缀表

        int i = 0;
        while (i <= n - m) {
            int j = m - 1;
            while (j >= 0 && T.charAt(i + j) == p.charAt(j)) {  // 从模式串末尾向前匹配
                j--;
            }
            if (j < 0) {
                return i;  // 匹配成功，返回模式串在文本T中的起始位置
            }
            int badMove = j - bcTable.getOrDefault(T.charAt(i + j), -1);  // 计算坏字符移动距离
            int goodMove = gsList[j];  // 计算好后缀移动距离
            i += Math.max(badMove, goodMove);  // 取坏字符移动距离和好后缀移动距离的较大值进行移动
        }
        return -1;  // 匹配失败，返回失败标志
    }

    // 生成坏字符表
    public static Map<Character, Integer> generateBadCharTable(String p) {
        Map<Character, Integer> bcTable = new HashMap<>();
        int m = p.length();
        for (int i = 0; i < m; i++) {
            bcTable.put(p.charAt(i), i);  // 记录模式串中每个字符最后出现的位置
        }
        return bcTable;
    }

    // 生成好后缀表
    public static int[] generateGoodSuffixList(String p) {
        int m = p.length();
        int[] gsList = new int[m];
        Arrays.fill(gsList, m);  // 初始化好后缀表，将所有位置的值设为模式串长度m

        int[] suffix = generateSuffixArray(p);  // 生成后缀数组
        int j = 0;

        for (int i = m - 1; i >= 0; i--) {
            if (suffix[i] == i + 1) {
                while (j < m - 1 - i) {
                    if (gsList[j] == m) {
                        gsList[j] = m - 1 - i;  // 如果好后缀在模式串中只出现一次，则更新好后缀表
                    }
                    j++;
                }
            }
        }

        for (int i = 0; i < m - 1; i++) {
            gsList[m - 1 - suffix[i]] = m - 1 - i;  // 更新好后缀表中的其他位置
        }
        return gsList;
    }

    // 生成后缀数组
    public static int[] generateSuffixArray(String p) {
        int m = p.length();
        int[] suffix = new int[m];
        Arrays.fill(suffix, m);  // 初始化后缀数组，将所有位置的值设为模式串长度m

        for (int i = m - 2; i >= 0; i--) {
            int start = i;
            while (start >= 0 && p.charAt(start) == p.charAt(m - 1 - i + start)) {
                start--;  // 向前匹配，直到不匹配为止
            }
            suffix[i] = i - start;  // 记录后缀的长度
        }
        return suffix;
    }
}
```

C++版本

```c++
// 生成坏字符表
unordered_map<char, int> generateBadCharTable(const string& p) {
    unordered_map<char, int> bcTable;
    int m = p.length();

    // 遍历模式串，记录每个字符最后出现的位置
    for (int i = 0; i < m; i++) {
        bcTable[p[i]] = i;
    }
    return bcTable;
}

// 生成好后缀表
vector<int> generateGoodSuffixList(const string& p) {
    int m = p.length();
    vector<int> gsList(m, m);

    // 生成后缀数组
    vector<int> suffix = generateSuffixArray(p);
    int j = 0;

    // 处理好后缀表
    for (int i = m - 1; i >= 0; i--) {
        if (suffix[i] == i + 1) {
            // 如果好后缀在模式串中只出现一次，则更新好后缀表
            while (j < m - 1 - i) {
                if (gsList[j] == m) {
                    gsList[j] = m - 1 - i;
                }
                j++;
            }
        }
    }

    // 更新好后缀表中的其他位置
    for (int i = 0; i < m - 1; i++) {
        gsList[m - 1 - suffix[i]] = m - 1 - i;
    }
    return gsList;
}

// 生成后缀数组
vector<int> generateSuffixArray(const string& p) {
    int m = p.length();
    vector<int> suffix(m, m);

    // 从模式串倒数第二个字符开始，向前遍历，计算每个后缀的长度
    for (int i = m - 2; i >= 0; i--) {
        int start = i;
        while (start >= 0 && p[start] == p[m - 1 - i + start]) {
            start--;
        }
        suffix[i] = i - start;
    }
    return suffix;
}

// Boyer-Moore算法主函数
int boyerMoore(const string& T, const string& p) {
    int n = T.length();  // 文本T的长度
    int m = p.length();  // 模式串p的长度

    unordered_map<char, int> bcTable = generateBadCharTable(p);  // 生成坏字符表
    vector<int> gsList = generateGoodSuffixList(p);  // 生成好后缀表

    int i = 0;
    while (i <= n - m) {
        int j = m - 1;
        while (j >= 0 && T[i + j] == p[j]) {
            j--;
        }
        if (j < 0) {
            return i;  // 匹配成功，返回模式串在文本T中的起始位置
        }
        int badMove = j - bcTable[T[i + j]];  // 计算坏字符移动距离
        int goodMove = gsList[j];  // 计算好后缀移动距离
        i += std::max(badMove, goodMove);  // 取坏字符移动距离和好后缀移动距离的较大值进行移动
    }
    return -1;  // 匹配失败，返回失败标志
}
```



# Horspool

Java版本

```java
public class Horspool {

    // Horspool算法主函数，用于在文本T中查找模式串p的位置
    public static int horspool(String T, String p) {
        int n = T.length();  // 文本T的长度
        int m = p.length();  // 模式串p的长度

        // 生成坏字符表
        Map<Character, Integer> bcTable = generateBadCharTable(p);

        int i = 0;  // 文本T中当前比较位置的索引
        while (i <= n - m) {
            int j = m - 1;  // 模式串p中当前比较位置的索引
            while (j >= 0 && T.charAt(i + j) == p.charAt(j)) {
                j--;  // 从模式串的末尾向前逐个字符比较
            }
            if (j < 0) {
                return i;  // 匹配成功，返回模式串在文本T中的起始位置
            }
            // 根据坏字符规则，获取坏字符在坏字符表中的移动距离
            i += bcTable.getOrDefault(T.charAt(i + m - 1), m);
        }
        return -1;  // 未找到匹配位置，返回失败标志
    }

    // 生成坏字符表
    public static Map<Character, Integer> generateBadCharTable(String p) {
        int m = p.length();  // 模式串p的长度
        Map<Character, Integer> bcTable = new HashMap<>();  // 坏字符表

        for (int i = 0; i < m - 1; i++) {
            // 将模式串中每个字符与最右边的位置的距离记录到坏字符表中
            bcTable.put(p.charAt(i), m - 1 - i);
        }
        return bcTable;
    }
}
```

C++版本

```c++
int horspool(const string& T, const string& p) {
    int n = T.length();  // 文本T的长度
    int m = p.length();  // 模式串p的长度

    unordered_map<char, int> bcTable = generateBadCharTable(p);  // 生成坏字符表

    int i = 0;  // 文本T中当前比较位置的索引
    while (i <= n - m) {
        int j = m - 1;  // 模式串p中当前比较位置的索引
        while (j >= 0 && T[i + j] == p[j]) {
            j--;  // 从模式串的末尾向前逐个字符比较
        }
        if (j < 0) {
            return i;  // 匹配成功，返回模式串在文本T中的起始位置
        }
        // 根据坏字符规则，获取坏字符在坏字符表中的移动距离
        i += bcTable[T[i + m - 1]];
    }
    return -1;  // 未找到匹配位置，返回失败标志
}

unordered_map<char, int> generateBadCharTable(const string& p) {
    int m = p.length();  // 模式串p的长度
    unordered_map<char, int> bcTable;  // 坏字符表

    for (int i = 0; i < m - 1; i++) {
        // 将模式串中每个字符与最右边的位置的距离记录到坏字符表中
        bcTable[p[i]] = m - 1 - i;
    }
    return bcTable;
}
```



# Sunday

Sunday算法是一种用于字符串匹配的快速算法，主要思想是从前往后匹配，在不匹配的情况下，通过坏字符规则快速滑动模式串。

> - 主要数据结构：
>   - `T`：文本串
>   - `p`：模式串
>   - `bcTable`：坏字符表，记录模式串中每个字符最后出现的位置
> - 主要步骤：
>   1. 生成坏字符表（Bad Character Table）：
>      - 遍历模式串，记录每个字符最右边出现的位置。
>      - 将每个字符与最右边位置的距离记录到坏字符表中。
>   2. Sunday算法主函数：
>      - 初始化文本T和模式串p的长度。
>      - 生成坏字符表。
>      - 使用指针i进行匹配，从文本T的起始位置开始，每次比较模式串p与文本T中对应位置的子串。
>      - 如果匹配成功，返回模式串在文本T中的起始位置。
>      - 如果匹配失败，根据坏字符规则，获取坏字符在坏字符表中的移动距离。
>      - 将指针i向右滑动，移动的距离为坏字符在坏字符表中的值。
>      - 重复上述步骤，直到遍历完整个文本T。

Java版本

```java
public class Sunday {

    // Sunday算法主函数，用于在文本T中查找模式串p的位置
    public static int sunday(String T, String p) {
        int n = T.length();  // 文本T的长度
        int m = p.length();  // 模式串p的长度

        // 生成坏字符表
        Map<Character, Integer> bcTable = generateBadCharTable(p);

        int i = 0;  // 文本T中当前比较位置的索引
        while (i <= n - m) {
            int j = 0;  // 模式串p中当前比较位置的索引
            if (T.substring(i, i + m).equals(p)) {
                return i;  // 匹配成功，返回模式串在文本T中的起始位置
            }
            if (i + m >= n) {
                return -1;  // 超出文本T的范围，未找到匹配位置，返回失败标志
            }
            // 根据坏字符规则，获取坏字符在坏字符表中的移动距离
            i += bcTable.getOrDefault(T.charAt(i + m), m + 1);
        }
        return -1;  // 未找到匹配位置，返回失败标志
    }

    // 生成坏字符表
    public static Map<Character, Integer> generateBadCharTable(String p) {
        int m = p.length();  // 模式串p的长度
        Map<Character, Integer> bcTable = new HashMap<>();  // 坏字符表

        for (int i = 0; i < m; i++) {
            // 将模式串中每个字符与最右边的位置的距离记录到坏字符表中
            bcTable.put(p.charAt(i), m - i);
        }
        return bcTable;
    }
}
```

C++版本

```c++
int sunday(const string& T, const string& p) {
    int n = T.length();  // 文本T的长度
    int m = p.length();  // 模式串p的长度
    
    unordered_map<char, int> bcTable = generateBadCharTable(p);  // 生成坏字符表
    
    int i = 0;  // 文本T中当前比较位置的索引
    while (i <= n - m) {
        int j = 0;  // 模式串p中当前比较位置的索引
        if (T.substr(i, m) == p) {
            return i;  // 匹配成功，返回模式串在文本T中的起始位置
        }
        if (i + m >= n) {
            return -1;  // 超出文本T的范围，未找到匹配位置，返回失败标志
        }
        // 根据坏字符规则，获取坏字符在坏字符表中的移动距离
        i += bcTable[T[i + m]];
    }
    return -1;  // 未找到匹配位置，返回失败标志
}

unordered_map<char, int> generateBadCharTable(const string& p) {
    int m = p.length();  // 模式串p的长度
    unordered_map<char, int> bcTable;  // 坏字符表
    
    for (int i = 0; i < m; i++) {
        // 将模式串中每个字符与最右边的位置的距离记录到坏字符表中
        bcTable[p[i]] = m - i;
    }
    return bcTable;
}
```

