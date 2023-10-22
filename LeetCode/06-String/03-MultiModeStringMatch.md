# 字符串

## 多模式串匹配

### 字典树

#### [208. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)

中等

**[Trie](https://baike.baidu.com/item/字典树/9825209?fr=aladdin)**（发音类似 "try"）或者说 **前缀树** 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。

请你实现 Trie 类：

- `Trie()` 初始化前缀树对象。
- `void insert(String word)` 向前缀树中插入字符串 `word` 。
- `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
- `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。

**示例：**

```
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]

解释
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True
```

C++版本

```c++
// 方法一：字典树
class Trie {
private:
    vector<Trie*> children;
    bool isEnd;

    Trie* searchPrefix(string prefix) {
        Trie* node = this;
        for (char ch : prefix) {
            ch -= 'a';
            if (node->children[ch] == nullptr) {
                return nullptr;
            }
            node = node->children[ch];
        }
        return node;
    }

public:
    Trie() : children(26), isEnd(false) {}

    void insert(string word) {
        Trie* node = this;
        for (char ch : word) {
            ch -= 'a';
            if (node->children[ch] == nullptr) {
                node->children[ch] = new Trie();
            }
            node = node->children[ch];
        }
        node->isEnd = true;
    }

    bool search(string word) {
        Trie* node = this->searchPrefix(word);
        return node != nullptr && node->isEnd;
    }

    bool startsWith(string prefix) {
        return this->searchPrefix(prefix) != nullptr;
    }
};
```

Java版本

```java
// 方法一：字典树
class Trie {
    private Trie[] children;
    private boolean isEnd;

    public Trie() {
        children = new Trie[26];
        isEnd = false;
    }
    
    public void insert(String word) {
        Trie node = this;
        for (int i = 0; i < word.length(); i++) {
            char ch = word.charAt(i);
            int index = ch - 'a';
            if (node.children[index] == null) {
                node.children[index] = new Trie();
            }
            node = node.children[index];
        }
        node.isEnd = true;
    }
    
    public boolean search(String word) {
        Trie node = searchPrefix(word);
        return node != null && node.isEnd;
    }
    
    public boolean startsWith(String prefix) {
        return searchPrefix(prefix) != null;
    }

    private Trie searchPrefix(String prefix) {
        Trie node = this;
        for (int i = 0; i < prefix.length(); i++) {
            char ch = prefix.charAt(i);
            int index = ch - 'a';
            if (node.children[index] == null) {
                return null;
            }
            node = node.children[index];
        }
        return node;
    }
}
```



#### [677. 键值映射](https://leetcode.cn/problems/map-sum-pairs/)

中等

设计一个 map ，满足以下几点:

- 字符串表示键，整数表示值
- 返回具有前缀等于给定字符串的键的值的总和

实现一个 `MapSum` 类：

- `MapSum()` 初始化 `MapSum` 对象
- `void insert(String key, int val)` 插入 `key-val` 键值对，字符串表示键 `key` ，整数表示值 `val` 。如果键 `key` 已经存在，那么原来的键值对 `key-value` 将被替代成新的键值对。
- `int sum(string prefix)` 返回所有以该前缀 `prefix` 开头的键 `key` 的值的总和。

**示例 1：**

```
输入：
["MapSum", "insert", "sum", "insert", "sum"]
[[], ["apple", 3], ["ap"], ["app", 2], ["ap"]]
输出：
[null, null, 3, null, 5]

解释：
MapSum mapSum = new MapSum();
mapSum.insert("apple", 3);  
mapSum.sum("ap");           // 返回 3 (apple = 3)
mapSum.insert("app", 2);    
mapSum.sum("ap");           // 返回 5 (apple + app = 3 + 2 = 5)
```

C++版本

```c++
// 方法一：暴力扫描
class MapSum {
public:
    MapSum() {

    }
    
    void insert(string key, int val) {
        cnt[key] = val;
    }
    
    int sum(string prefix) {
        int res = 0;
        for (auto & [key,val] : cnt) {
            if (key.substr(0, prefix.size()) == prefix) {
                res += val;
            }
        }
        return res;
    }
private:
    unordered_map<string, int> cnt;
};

// 方法二：前缀哈希映射
class MapSum {
public:
    MapSum() {

    }
    
    void insert(string key, int val) {
        int delta = val;
        if (map.count(key)) {
            delta -= map[key];
        }
        map[key] = val;
        for (int i = 1; i <= key.size(); ++i) {
            prefixmap[key.substr(0, i)] += delta;
        }
    }
    
    int sum(string prefix) {
        return prefixmap[prefix];
    }
private:
    unordered_map<string, int> map;
    unordered_map<string, int> prefixmap;
};

// 方法三：字典树
struct TrieNode {
    int val;
    TrieNode * next[26];
    TrieNode() {
        this->val = 0;
        for (int i = 0; i < 26; ++i) {
            this->next[i] = nullptr;
        }
    }
};

class MapSum {
public:
    MapSum() {
        this->root = new TrieNode();
    }
    
    void insert(string key, int val) {
        int delta = val;
        if (cnt.count(key)) {
            delta -= cnt[key];
        }
        cnt[key] = val;
        TrieNode * node = root;
        for (auto c : key) {
            if (node->next[c - 'a'] == nullptr) {
                node->next[c - 'a'] = new TrieNode();
            }
            node = node->next[c - 'a'];
            node->val += delta;
        }
    }
    
    int sum(string prefix) {
        TrieNode * node = root;
        for (auto c : prefix) {
            if (node->next[c - 'a'] == nullptr) {
                return 0;
            } else {
                node = node->next[c - 'a'];
            }
        }
        return node->val;
    }
private:
    TrieNode * root;
    unordered_map<string, int> cnt;
};
```

Java版本

```java
// 方法一：暴力扫描
class MapSum {
    Map<String, Integer> map;

    public MapSum() {
        map = new HashMap<>();
    }
    
    public void insert(String key, int val) {
        map.put(key,val);
    }
    
    public int sum(String prefix) {
        int res = 0;
        for (String s : map.keySet()) {
            if (s.startsWith(prefix)) {
                res += map.get(s);
            }
        }
        return res;
    }
}

// 方法二：前缀哈希映射
class MapSum {
    Map<String, Integer> map;
    Map<String, Integer> prefixmap;

    public MapSum() {
        map = new HashMap<>();
        prefixmap = new HashMap<>();
    }
    
    public void insert(String key, int val) {
        int delta = val - map.getOrDefault(key, 0);
        map.put(key, val);
        for (int i = 1; i <= key.length(); ++i) {
            String currprefix = key.substring(0, i);
            prefixmap.put(currprefix, prefixmap.getOrDefault(currprefix, 0) + delta);
        }
    }
    
    public int sum(String prefix) {
        return prefixmap.getOrDefault(prefix, 0);
    }
}

// 方法三：字典树
class MapSum {
    class TrieNode {
        int val = 0;
        TrieNode[] next = new TrieNode[26];
    }

    TrieNode root;
    Map<String, Integer> map;

    public MapSum() {
        root = new TrieNode();
        map = new HashMap<>();
    }
    
    public void insert(String key, int val) {        
        int delta = val - map.getOrDefault(key, 0);
        map.put(key, val);
        TrieNode node = root;
        for (char c : key.toCharArray()) {
            if (node.next[c - 'a'] == null) {
                node.next[c - 'a'] = new TrieNode();
            }
            node = node.next[c - 'a'];
            node.val += delta;
        }
    }
    
    public int sum(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            if (node.next[c - 'a'] == null) {
                return 0;
            }
            node = node.next[c - 'a'];
        }
        return node.val;
    }
}
```



#### [648. 单词替换](https://leetcode.cn/problems/replace-words/)

中等

在英语中，我们有一个叫做 `词根`(root) 的概念，可以词根**后面**添加其他一些词组成另一个较长的单词——我们称这个词为 `继承词`(successor)。例如，词根`an`，跟随着单词 `other`(其他)，可以形成新的单词 `another`(另一个)。

现在，给定一个由许多**词根**组成的词典 `dictionary` 和一个用空格分隔单词形成的句子 `sentence`。你需要将句子中的所有**继承词**用**词根**替换掉。如果**继承词**有许多可以形成它的**词根**，则用**最短**的词根替换它。

你需要输出替换之后的句子。

**示例 1：**

```
输入：dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"
输出："the cat was rat by the bat"
```

C++版本

```c++
// 方法一：哈希集合
class Solution {
public:
    vector<string_view> split(string &str, char ch) {
        int pos = 0;
        int start = 0;
        string_view s(str);
        vector<string_view> ret;
        while (pos < s.size()) {
            while (pos < s.size() && s[pos] == ch) {
                pos++;
            }
            start = pos;
            while (pos < s.size() && s[pos] != ch) {
                pos++;
            }
            if (start < s.size()) {
                ret.emplace_back(s.substr(start, pos - start));
            }
        }
        return ret;
    }

    string replaceWords(vector<string>& dictionary, string sentence) {
        unordered_set<string_view> dictionarySet;
        for (auto &root : dictionary) {
            dictionarySet.emplace(root);
        }
        vector<string_view> words = split(sentence, ' ');
        for (auto &word : words) {
            for (int j = 0; j < word.size(); j++) {
                if (dictionarySet.count(word.substr(0, 1 + j))) {
                    word = word.substr(0, 1 + j);
                    break;
                }
            }
        }
        string ans;
        for (int i = 0; i < words.size() - 1; i++) {
            ans.append(words[i]);
            ans.append(" ");
        }
        ans.append(words.back());
        return ans;
    }
};

// 方法二：字典树
struct Trie {
    unordered_map<char, Trie *> children;
};

class Solution {
public:
    string replaceWords(vector<string>& dictionary, string sentence) {
        Trie *trie = new Trie();
        for (auto &word : dictionary) {
            Trie *cur = trie;
            for (char &c: word) {
                if (!cur->children.count(c)) {
                    cur->children[c] = new Trie();
                }
                cur = cur->children[c];
            }
            cur->children['#'] = new Trie();
        }
        vector<string> words = split(sentence, ' ');
        for (auto &word : words) {
            word = findRoot(word, trie);
        }
        string ans;
        for (int i = 0; i < words.size() - 1; i++) {
            ans.append(words[i]);
            ans.append(" ");
        }
        ans.append(words.back());
        return ans;
    }

    vector<string> split(string &str, char ch) {
        int pos = 0;
        int start = 0;
        vector<string> ret;
        while (pos < str.size()) {
            while (pos < str.size() && str[pos] == ch) {
                pos++;
            }
            start = pos;
            while (pos < str.size() && str[pos] != ch) {
                pos++;
            }
            if (start < str.size()) {
                ret.emplace_back(str.substr(start, pos - start));
            }
        }
        return ret;
    }

    string findRoot(string &word, Trie *trie) {
        string root;
        Trie *cur = trie;
        for (char &c : word) {
            if (cur->children.count('#')) {
                return root;
            }
            if (!cur->children.count(c)) {
                return word;
            }
            root.push_back(c);
            cur = cur->children[c];
        }
        return root;
    }
};
```

Java版本

```java
// 方法一：哈希集合
class Solution {
    public String replaceWords(List<String> dictionary, String sentence) {
        Set<String> dictionarySet = new HashSet<String>();
        for (String root : dictionary) {
            dictionarySet.add(root);
        }
        String[] words = sentence.split(" ");
        for (int i = 0; i < words.length; i++) {
            String word = words[i];
            for (int j = 0; j < word.length(); j++) {
                if (dictionarySet.contains(word.substring(0, 1 + j))) {
                    words[i] = word.substring(0, 1 + j);
                    break;
                }
            }
        }
        return String.join(" ", words);
    }
}

// 方法二：字典树
class Solution {
    public String replaceWords(List<String> dictionary, String sentence) {
        Trie trie = new Trie();
        for (String word : dictionary) {
            Trie cur = trie;
            for (int i = 0; i < word.length(); i++) {
                char c = word.charAt(i);
                cur.children.putIfAbsent(c, new Trie());
                cur = cur.children.get(c);
            }
            cur.children.put('#', new Trie());
        }
        String[] words = sentence.split(" ");
        for (int i = 0; i < words.length; i++) {
            words[i] = findRoot(words[i], trie);
        }
        return String.join(" ", words);
    }

    public String findRoot(String word, Trie trie) {
        StringBuffer root = new StringBuffer();
        Trie cur = trie;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (cur.children.containsKey('#')) {
                return root.toString();
            }
            if (!cur.children.containsKey(c)) {
                return word;
            }
            root.append(c);
            cur = cur.children.get(c);
        }
        return root.toString();
    }
}

class Trie {
    Map<Character, Trie> children;

    public Trie() {
        children = new HashMap<Character, Trie>();
    }
}
```



#### [211. 添加与搜索单词 - 数据结构设计](https://leetcode.cn/problems/design-add-and-search-words-data-structure/)

中等

请你设计一个数据结构，支持 添加新单词 和 查找字符串是否与任何先前添加的字符串匹配 。

实现词典类 `WordDictionary` ：

- `WordDictionary()` 初始化词典对象
- `void addWord(word)` 将 `word` 添加到数据结构中，之后可以对它进行匹配
- `bool search(word)` 如果数据结构中存在字符串与 `word` 匹配，则返回 `true` ；否则，返回 `false` 。`word` 中可能包含一些 `'.'` ，每个 `.` 都可以表示任何一个字母。

**示例：**

```
输入：
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
输出：
[null,null,null,null,false,true,true,true]

解释：
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // 返回 False
wordDictionary.search("bad"); // 返回 True
wordDictionary.search(".ad"); // 返回 True
wordDictionary.search("b.."); // 返回 True
```

C++版本

```c++
// 方法一：字典树
struct TrieNode{
    vector<TrieNode *> child;
    bool isEnd;
    TrieNode() {
        this->child = vector<TrieNode *>(26,nullptr);
        this->isEnd = false;
    }
};

void insert(TrieNode * root, const string & word) {
    TrieNode * node = root;
    for (auto c : word) {
        if (node->child[c - 'a'] == nullptr) {
            node->child[c - 'a'] = new TrieNode();
        }
        node = node->child[c - 'a'];
    }
    node->isEnd = true;
}

class WordDictionary {
public:
    WordDictionary() {
        trie = new TrieNode();
    }
    
    void addWord(string word) {
        insert(trie,word);
    }
    
    bool search(string word) {
        return dfs(word, 0, trie);
    }

    bool dfs(const string & word,int index,TrieNode * node) {
　　　　if (index == word.size()) {
            return node->isEnd;    
        }
        char ch = word[index];
        if (ch >= 'a' && ch <= 'z') {
            TrieNode * child = node->child[ch - 'a'];
            if (child != nullptr && dfs(word, index + 1, child)) {
                return true;
            }
        } else if (ch == '.') {
            for (int i = 0; i < 26; i++) {
                TrieNode * child = node->child[i];
                if (child != nullptr && dfs(word, index + 1, child)) {
                    return true;
                }
            }
        }
        return false;
    }
private:
    TrieNode * trie;
};
```

Java版本

```java
// 方法一：字典树
class WordDictionary {
    private Trie root;

    public WordDictionary() {
        root = new Trie();
    }
    
    public void addWord(String word) {
        root.insert(word);
    }
    
    public boolean search(String word) {
        return dfs(word, 0, root);
    }

    private boolean dfs(String word, int index, Trie node) {
        if (index == word.length()) {
            return node.isEnd();
        }
        char ch = word.charAt(index);
        if (Character.isLetter(ch)) {
            int childIndex = ch - 'a';
            Trie child = node.getChildren()[childIndex];
            if (child != null && dfs(word, index + 1, child)) {
                return true;
            }
        } else {
            for (int i = 0; i < 26; i++) {
                Trie child = node.getChildren()[i];
                if (child != null && dfs(word, index + 1, child)) {
                    return true;
                }
            }
        }
        return false;
    }
}

class Trie {
    private Trie[] children;
    private boolean isEnd;

    public Trie() {
        children = new Trie[26];
        isEnd = false;
    }
    
    public void insert(String word) {
        Trie node = this;
        for (int i = 0; i < word.length(); i++) {
            char ch = word.charAt(i);
            int index = ch - 'a';
            if (node.children[index] == null) {
                node.children[index] = new Trie();
            }
            node = node.children[index];
        }
        node.isEnd = true;
    }

    public Trie[] getChildren() {
        return children;
    }

    public boolean isEnd() {
        return isEnd;
    }
}
```



#### [421. 数组中两个数的最大异或值](https://leetcode.cn/problems/maximum-xor-of-two-numbers-in-an-array/)

中等

给你一个整数数组 `nums` ，返回 `nums[i] XOR nums[j]` 的最大运算结果，其中 `0 ≤ i ≤ j < n` 。

**示例 1：**

```
输入：nums = [3,10,5,25,2,8]
输出：28
解释：最大运算结果是 5 XOR 25 = 28.
```

C++版本

```c++
// 方法一：哈希表
class Solution {
private:
    // 最高位的二进制位编号为 30
    static constexpr int HIGH_BIT = 30;

public:
    int findMaximumXOR(vector<int>& nums) {
        int x = 0;
        for (int k = HIGH_BIT; k >= 0; --k) {
            unordered_set<int> seen;
            // 将所有的 pre^k(a_j) 放入哈希表中
            for (int num: nums) {
                // 如果只想保留从最高位开始到第 k 个二进制位为止的部分
                // 只需将其右移 k 位
                seen.insert(num >> k);
            }

            // 目前 x 包含从最高位开始到第 k+1 个二进制位为止的部分
            // 我们将 x 的第 k 个二进制位置为 1，即为 x = x*2+1
            int x_next = x * 2 + 1;
            bool found = false;
            
            // 枚举 i
            for (int num: nums) {
                if (seen.count(x_next ^ (num >> k))) {
                    found = true;
                    break;
                }
            }

            if (found) {
                x = x_next;
            }
            else {
                // 如果没有找到满足等式的 a_i 和 a_j，那么 x 的第 k 个二进制位只能为 0
                // 即为 x = x*2
                x = x_next - 1;
            }
        }
        return x;
    }
};

// 方法二：字典树
struct Trie {
    // 左子树指向表示 0 的子节点
    Trie* left = nullptr;
    // 右子树指向表示 1 的子节点
    Trie* right = nullptr;

    Trie() {}
};

class Solution {
private:
    // 字典树的根节点
    Trie* root = new Trie();
    // 最高位的二进制位编号为 30
    static constexpr int HIGH_BIT = 30;

public:
    void add(int num) {
        Trie* cur = root;
        for (int k = HIGH_BIT; k >= 0; --k) {
            int bit = (num >> k) & 1;
            if (bit == 0) {
                if (!cur->left) {
                    cur->left = new Trie();
                }
                cur = cur->left;
            }
            else {
                if (!cur->right) {
                    cur->right = new Trie();
                }
                cur = cur->right;
            }
        }
    }

    int check(int num) {
        Trie* cur = root;
        int x = 0;
        for (int k = HIGH_BIT; k >= 0; --k) {
            int bit = (num >> k) & 1;
            if (bit == 0) {
                // a_i 的第 k 个二进制位为 0，应当往表示 1 的子节点 right 走
                if (cur->right) {
                    cur = cur->right;
                    x = x * 2 + 1;
                }
                else {
                    cur = cur->left;
                    x = x * 2;
                }
            }
            else {
                // a_i 的第 k 个二进制位为 1，应当往表示 0 的子节点 left 走
                if (cur->left) {
                    cur = cur->left;
                    x = x * 2 + 1;
                }
                else {
                    cur = cur->right;
                    x = x * 2;
                }
            }
        }
        return x;
    }

    int findMaximumXOR(vector<int>& nums) {
        int n = nums.size();
        int x = 0;
        for (int i = 1; i < n; ++i) {
            // 将 nums[i-1] 放入字典树，此时 nums[0 .. i-1] 都在字典树中
            add(nums[i - 1]);
            // 将 nums[i] 看作 ai，找出最大的 x 更新答案
            x = max(x, check(nums[i]));
        }
        return x;
    }
};
```

Java版本

```java
// 方法一：哈希表
class Solution {
    // 最高位的二进制位编号为 30
    static final int HIGH_BIT = 30;

    public int findMaximumXOR(int[] nums) {
        int x = 0;
        for (int k = HIGH_BIT; k >= 0; --k) {
            Set<Integer> seen = new HashSet<Integer>();
            // 将所有的 pre^k(a_j) 放入哈希表中
            for (int num : nums) {
                // 如果只想保留从最高位开始到第 k 个二进制位为止的部分
                // 只需将其右移 k 位
                seen.add(num >> k);
            }

            // 目前 x 包含从最高位开始到第 k+1 个二进制位为止的部分
            // 我们将 x 的第 k 个二进制位置为 1，即为 x = x*2+1
            int xNext = x * 2 + 1;
            boolean found = false;
            
            // 枚举 i
            for (int num : nums) {
                if (seen.contains(xNext ^ (num >> k))) {
                    found = true;
                    break;
                }
            }

            if (found) {
                x = xNext;
            } else {
                // 如果没有找到满足等式的 a_i 和 a_j，那么 x 的第 k 个二进制位只能为 0
                // 即为 x = x*2
                x = xNext - 1;
            }
        }
        return x;
    }
}

// 方法二：字典树
class Solution {
    // 字典树的根节点
    Trie root = new Trie();
    // 最高位的二进制位编号为 30
    static final int HIGH_BIT = 30;

    public int findMaximumXOR(int[] nums) {
        int n = nums.length;
        int x = 0;
        for (int i = 1; i < n; ++i) {
            // 将 nums[i-1] 放入字典树，此时 nums[0 .. i-1] 都在字典树中
            add(nums[i - 1]);
            // 将 nums[i] 看作 ai，找出最大的 x 更新答案
            x = Math.max(x, check(nums[i]));
        }
        return x;
    }

    public void add(int num) {
        Trie cur = root;
        for (int k = HIGH_BIT; k >= 0; --k) {
            int bit = (num >> k) & 1;
            if (bit == 0) {
                if (cur.left == null) {
                    cur.left = new Trie();
                }
                cur = cur.left;
            }
            else {
                if (cur.right == null) {
                    cur.right = new Trie();
                }
                cur = cur.right;
            }
        }
    }

    public int check(int num) {
        Trie cur = root;
        int x = 0;
        for (int k = HIGH_BIT; k >= 0; --k) {
            int bit = (num >> k) & 1;
            if (bit == 0) {
                // a_i 的第 k 个二进制位为 0，应当往表示 1 的子节点 right 走
                if (cur.right != null) {
                    cur = cur.right;
                    x = x * 2 + 1;
                } else {
                    cur = cur.left;
                    x = x * 2;
                }
            } else {
                // a_i 的第 k 个二进制位为 1，应当往表示 0 的子节点 left 走
                if (cur.left != null) {
                    cur = cur.left;
                    x = x * 2 + 1;
                } else {
                    cur = cur.right;
                    x = x * 2;
                }
            }
        }
        return x;
    }
}

class Trie {
    // 左子树指向表示 0 的子节点
    Trie left = null;
    // 右子树指向表示 1 的子节点
    Trie right = null;
}
```



#### [212. 单词搜索 II](https://leetcode.cn/problems/word-search-ii/)

困难

给定一个 `m x n` 二维字符网格 `board` 和一个单词（字符串）列表 `words`， *返回所有二维网格上的单词* 。

单词必须按照字母顺序，通过 **相邻的单元格** 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

```
输入：board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
输出：["eat","oath"]
```

C++版本

```c++
// 方法一：回溯 + 字典树
struct TrieNode {
    string word;
    unordered_map<char,TrieNode *> children;
    TrieNode() {
        this->word = "";
    }   
};

void insertTrie(TrieNode * root,const string & word) {
    TrieNode * node = root;
    for (auto c : word){
        if (!node->children.count(c)) {
            node->children[c] = new TrieNode();
        }
        node = node->children[c];
    }
    node->word = word;
}

class Solution {
public:
    int dirs[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    bool dfs(vector<vector<char>>& board, int x, int y, TrieNode * root, set<string> & res) {
        char ch = board[x][y];        
        if (!root->children.count(ch)) {
            return false;
        }
        root = root->children[ch];
        if (root->word.size() > 0) {
            res.insert(root->word);
        }

        board[x][y] = '#';
        for (int i = 0; i < 4; ++i) {
            int nx = x + dirs[i][0];
            int ny = y + dirs[i][1];
            if (nx >= 0 && nx < board.size() && ny >= 0 && ny < board[0].size()) {
                if (board[nx][ny] != '#') {
                    dfs(board, nx, ny, root,res);
                }
            }
        }
        board[x][y] = ch;

        return true;      
    }

    vector<string> findWords(vector<vector<char>> & board, vector<string> & words) {
        TrieNode * root = new TrieNode();
        set<string> res;
        vector<string> ans;

        for (auto & word: words){
            insertTrie(root,word);
        }
        for (int i = 0; i < board.size(); ++i) {
            for (int j = 0; j < board[0].size(); ++j) {
                dfs(board, i, j, root, res);
            }
        }        
        for (auto & word: res) {
            ans.emplace_back(word);
        }
        return ans;        
    }
};

// 方法二：删除被匹配的单词
struct TrieNode {
    string word;
    unordered_map<char, TrieNode *> children;
    TrieNode() {
        this->word = "";
    }   
};

void insertTrie(TrieNode * root, const string & word) {
    TrieNode * node = root;

    for (auto c : word) {
        if (!node->children.count(c)) {
            node->children[c] = new TrieNode();
        }
        node = node->children[c];
    }

    node->word = word;
}

class Solution {
public:
    int dirs[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    bool dfs(vector<vector<char>>& board, int x, int y, TrieNode * root, set<string> & res) {
        char ch = board[x][y];   
     
        if (root == nullptr || !root->children.count(ch)) {
            return false;
        }

        TrieNode * nxt = root->children[ch];
        if (nxt->word.size() > 0) {
            res.insert(nxt->word);
            nxt->word = "";
        }
        if (!nxt->children.empty()) {
            board[x][y] = '#';
            for (int i = 0; i < 4; ++i) {
                int nx = x + dirs[i][0];
                int ny = y + dirs[i][1];
                if (nx >= 0 && nx < board.size() && ny >= 0 && ny < board[0].size()) {
                    if (board[nx][ny] != '#') {
                        dfs(board, nx, ny, nxt,res);
                    }
                }
            }
            board[x][y] = ch;
        }
        if (nxt->children.empty()) {
            root->children.erase(ch);
        }

        return true;      
    }

    vector<string> findWords(vector<vector<char>> & board, vector<string> & words) {
        TrieNode * root = new TrieNode();
        set<string> res;
        vector<string> ans;

        for (auto & word: words) {
            insertTrie(root,word);
        }
        for (int i = 0; i < board.size(); ++i) {
            for(int j = 0; j < board[0].size(); ++j) {
                dfs(board, i, j, root, res);
            }
        }        
        for (auto & word: res) {
            ans.emplace_back(word);
        }
        
        return ans;        
    }
};
```

Java版本

```java
// 方法一：回溯 + 字典树
class Solution {
    int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public List<String> findWords(char[][] board, String[] words) {
        Trie trie = new Trie();
        for (String word : words) {
            trie.insert(word);
        }

        Set<String> ans = new HashSet<String>();
        for (int i = 0; i < board.length; ++i) {
            for (int j = 0; j < board[0].length; ++j) {
                dfs(board, trie, i, j, ans);
            }
        }

        return new ArrayList<String>(ans);
    }

    public void dfs(char[][] board, Trie now, int i1, int j1, Set<String> ans) {
        if (!now.children.containsKey(board[i1][j1])) {
            return;
        }
        char ch = board[i1][j1];
        now = now.children.get(ch);
        if (!"".equals(now.word)) {
            ans.add(now.word);
        }

        board[i1][j1] = '#';
        for (int[] dir : dirs) {
            int i2 = i1 + dir[0], j2 = j1 + dir[1];
            if (i2 >= 0 && i2 < board.length && j2 >= 0 && j2 < board[0].length) {
                dfs(board, now, i2, j2, ans);
            }
        }
        board[i1][j1] = ch;
    }
}

class Trie {
    String word;
    Map<Character, Trie> children;
    boolean isWord;

    public Trie() {
        this.word = "";
        this.children = new HashMap<Character, Trie>();
    }

    public void insert(String word) {
        Trie cur = this;
        for (int i = 0; i < word.length(); ++i) {
            char c = word.charAt(i);
            if (!cur.children.containsKey(c)) {
                cur.children.put(c, new Trie());
            }
            cur = cur.children.get(c);
        }
        cur.word = word;
    }
}

// 方法二：删除被匹配的单词
class Solution {
    int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public List<String> findWords(char[][] board, String[] words) {
        Trie trie = new Trie();
        for (String word : words) {
            trie.insert(word);
        }

        Set<String> ans = new HashSet<String>();
        for (int i = 0; i < board.length; ++i) {
            for (int j = 0; j < board[0].length; ++j) {
                dfs(board, trie, i, j, ans);
            }
        }

        return new ArrayList<String>(ans);
    }

    public void dfs(char[][] board, Trie now, int i1, int j1, Set<String> ans) {
        if (!now.children.containsKey(board[i1][j1])) {
            return;
        }
        char ch = board[i1][j1];
        Trie nxt = now.children.get(ch);
        if (!"".equals(nxt.word)) {
            ans.add(nxt.word);
            nxt.word = "";
        }

        if (!nxt.children.isEmpty()) {
            board[i1][j1] = '#';
            for (int[] dir : dirs) {
                int i2 = i1 + dir[0], j2 = j1 + dir[1];
                if (i2 >= 0 && i2 < board.length && j2 >= 0 && j2 < board[0].length) {
                    dfs(board, nxt, i2, j2, ans);
                }
            }
            board[i1][j1] = ch;
        }

        if (nxt.children.isEmpty()) {
            now.children.remove(ch);
        }
    }
}

class Trie {
    String word;
    Map<Character, Trie> children;
    boolean isWord;

    public Trie() {
        this.word = "";
        this.children = new HashMap<Character, Trie>();
    }

    public void insert(String word) {
        Trie cur = this;
        for (int i = 0; i < word.length(); ++i) {
            char c = word.charAt(i);
            if (!cur.children.containsKey(c)) {
                cur.children.put(c, new Trie());
            }
            cur = cur.children.get(c);
        }
        cur.word = word;
    }
}
```













