# 图的遍历

## 广度遍历搜索（BFS）

### [958. 二叉树的完全性检验](https://leetcode.cn/problems/check-completeness-of-a-binary-tree/)

中等

给你一棵二叉树的根节点 `root` ，请你判断这棵树是否是一棵 **完全二叉树** 。

在一棵 **[完全二叉树](https://baike.baidu.com/item/完全二叉树/7773232?fr=aladdin)** 中，除了最后一层外，所有层都被完全填满，并且最后一层中的所有节点都尽可能靠左。最后一层（第 `h` 层）中可以包含 `1` 到 `2h` 个节点。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/complete-binary-tree-1.png)

```
输入：root = [1,2,3,4,5,6]
输出：true
解释：最后一层前的每一层都是满的（即，节点值为 {1} 和 {2,3} 的两层），且最后一层中的所有节点（{4,5,6}）尽可能靠左。
```

C++版本

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
// 方法 1：广度优先搜索
class Solution {
public:
    bool isCompleteTree(TreeNode* root) {
        std::queue<TreeNode*> queue;
        queue.push(root);
        TreeNode* cur;
        while ((cur = queue.front()) != nullptr) {
            queue.push(cur->left);
            queue.push(cur->right);
            queue.pop();
        }
        while (!queue.empty()) {
            if (queue.front() != nullptr) {
                return false;
            }
            queue.pop();
        }
        return true;
    }
};
```

Java版本

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
// 方法 1：广度优先搜索
class Solution {
    public boolean isCompleteTree(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        TreeNode cur;
        while ((cur = queue.poll()) != null) {
            queue.offer(cur.left);
            queue.offer(cur.right);
        }
        while (!queue.isEmpty()) {
            if (queue.poll() != null) {
                return false;
            }
        }
        return true;
    }
}
```





