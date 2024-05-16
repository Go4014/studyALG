# 动态规划

## 状态压缩DP题目

### [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

简单

给你一棵二叉树的根节点，返回该树的 **直径** 。

二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。这条路径可能经过也可能不经过根节点 `root` 。

两节点之间路径的 **长度** 由它们之间边数表示。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
输入：root = [1,2,3,4,5]
输出：3
解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。
```

C++版本

```c++
// 方法一：深度优先搜索
class Solution {
    int ans;
    int depth(TreeNode* rt){
        if (rt == NULL) {
            return 0; // 访问到空节点了，返回0
        }
        int L = depth(rt->left); // 左儿子为根的子树的深度
        int R = depth(rt->right); // 右儿子为根的子树的深度
        ans = max(ans, L + R + 1); // 计算d_node即L+R+1 并更新ans
        return max(L, R) + 1; // 返回该节点为根的子树的深度
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        ans = 1;
        depth(root);
        return ans - 1;
    }
};
```

Java版本

```java
// 方法一：深度优先搜索
class Solution {
    int ans;
    public int diameterOfBinaryTree(TreeNode root) {
        ans = 1;
        depth(root);
        return ans - 1;
    }
    public int depth(TreeNode node) {
        if (node == null) {
            return 0; // 访问到空节点了，返回0
        }
        int L = depth(node.left); // 左儿子为根的子树的深度
        int R = depth(node.right); // 右儿子为根的子树的深度
        ans = Math.max(ans, L+R+1); // 计算d_node即L+R+1 并更新ans
        return Math.max(L, R) + 1; // 返回该节点为根的子树的深度
    }
}
```



### [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

困难

二叉树中的 **路径** 被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中 **至多出现一次** 。该路径 **至少包含一个** 节点，且不一定经过根节点。

**路径和** 是路径中各节点值的总和。

给你一个二叉树的根节点 `root` ，返回其 **最大路径和** 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

C++版本

```c++
// 方法一：递归
class Solution {
private:
    int maxSum = INT_MIN;

public:
    int maxGain(TreeNode* node) {
        if (node == nullptr) {
            return 0;
        }
        
        // 递归计算左右子节点的最大贡献值
        // 只有在最大贡献值大于 0 时，才会选取对应子节点
        int leftGain = max(maxGain(node->left), 0);
        int rightGain = max(maxGain(node->right), 0);

        // 节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值
        int priceNewpath = node->val + leftGain + rightGain;

        // 更新答案
        maxSum = max(maxSum, priceNewpath);

        // 返回节点的最大贡献值
        return node->val + max(leftGain, rightGain);
    }

    int maxPathSum(TreeNode* root) {
        maxGain(root);
        return maxSum;
    }
};
```

Java版本

```java
// 方法一：递归
class Solution {
    int maxSum = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        maxGain(root);
        return maxSum;
    }

    public int maxGain(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        // 递归计算左右子节点的最大贡献值
        // 只有在最大贡献值大于 0 时，才会选取对应子节点
        int leftGain = Math.max(maxGain(node.left), 0);
        int rightGain = Math.max(maxGain(node.right), 0);

        // 节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值
        int priceNewpath = node.val + leftGain + rightGain;

        // 更新答案
        maxSum = Math.max(maxSum, priceNewpath);

        // 返回节点的最大贡献值
        return node.val + Math.max(leftGain, rightGain);
    }
}
```



### [2246. 相邻字符不同的最长路径](https://leetcode.cn/problems/longest-path-with-different-adjacent-characters/)

困难

给你一棵 **树**（即一个连通、无向、无环图），根节点是节点 `0` ，这棵树由编号从 `0` 到 `n - 1` 的 `n` 个节点组成。用下标从 **0** 开始、长度为 `n` 的数组 `parent` 来表示这棵树，其中 `parent[i]` 是节点 `i` 的父节点，由于节点 `0` 是根节点，所以 `parent[0] == -1` 。

另给你一个字符串 `s` ，长度也是 `n` ，其中 `s[i]` 表示分配给节点 `i` 的字符。

请你找出路径上任意一对相邻节点都没有分配到相同字符的 **最长路径** ，并返回该路径的长度。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2022/03/25/testingdrawio.png)

```
输入：parent = [-1,0,0,1,1,2], s = "abacbe"
输出：3
解释：任意一对相邻节点字符都不同的最长路径是：0 -> 1 -> 3 。该路径的长度是 3 ，所以返回 3 。
可以证明不存在满足上述条件且比 3 更长的路径。 
```

C++版本

```c++
class Solution {
public:
    int longestPath(vector<int>& parent, string s) {
        int n = parent.size();

        vector<pair<int, int>> dp(n, make_pair(0, 0));
        vector<int> degree(n, 0);
        for(auto x: parent){
            if(x == -1) continue;
            ++degree[x];
        }

        int res = 0;
        queue<int> que;
        for(int i = 0; i<n; i++){
            if(degree[i] == 0){
                que.push(i);
                dp[i] = make_pair(0, 0);
            }
        }

        while(!que.empty()){
            int idx = que.front();
            que.pop();
            int pr = parent[idx];
            res = max(res, dp[idx].first + dp[idx].second + 1);

            if(pr != -1){
                degree[pr]--;
                if(degree[pr] <= 0) que.push(pr);
                if(s[pr] != s[idx]){
                    if(dp[idx].second + 1 <= dp[pr].first) continue;
                    else{
                        if(dp[idx].second + 1 < dp[pr].second){
                            dp[pr].first = dp[idx].second + 1;
                        }
                        else{
                            dp[pr].first = dp[pr].second;
                            dp[pr].second = dp[idx].second + 1;
                        }
                    }
                }
            }
            
        }

        return res;
    }
};
```

Java版本

```java
class Solution {
    public int longestPath(int[] parents, String s) {
        char[] cs = s.toCharArray();
        int n = cs.length, result = 1; 
        
        // topo sort, count in-degree first (in=degree here means child -> parent)
        int[] degree = new int[n];
        for (int i = 1; i < n; i++)
            degree[parents[i]]++; // skip root
        
        // stack: for topo sort, start with leaf nodes(in-degree = 0)
        int[] stack = new int[n];
        int top = -1; 
        for (int i = 1; i < n; i++) // skip root
            if (degree[i] == 0)  
                 stack[++top] = i;
        
        int[] path = new int[n]; // max length of path for each node
        Arrays.fill(path, 1); // every node is a path of length 1
        
        while (top >= 0) { // topo sort till the end
            int v = stack[top--]; // c: child index
            int u = parents[v]; // p: parent index
            if (--degree[u] == 0 && u != 0)
                 stack[++top] = u; // do not process root
            
            if (cs[u] == cs[v]) continue; // nothing to update
            
            // we must update res first, otherwise we may double count the same path
            result = Math.max(result, path[u] + path[v]);
            path[u] = Math.max(path[u], path[v] + 1);
        }

        return result;
    }
}
```







