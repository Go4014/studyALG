# 树

## 二叉搜索树

### [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

中等

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
输入：root = [2,1,3]
输出：true
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
// 方法一: 递归
class Solution {
public:
    bool helper(TreeNode* root, long long lower, long long upper) {
        if (root == nullptr) {
            return true;
        }
        if (root -> val <= lower || root -> val >= upper) {
            return false;
        }
        return helper(root -> left, lower, root -> val) && helper(root -> right, root -> val, upper);
    }
    bool isValidBST(TreeNode* root) {
        return helper(root, LONG_MIN, LONG_MAX);
    }
};

// 方法二：中序遍历
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> stack;
        long long inorder = (long long)INT_MIN - 1;

        while (!stack.empty() || root != nullptr) {
            while (root != nullptr) {
                stack.push(root);
                root = root -> left;
            }
            root = stack.top();
            stack.pop();
            // 如果中序遍历得到的节点的值小于等于前一个 inorder，说明不是二叉搜索树
            if (root -> val <= inorder) {
                return false;
            }
            inorder = root -> val;
            root = root -> right;
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
// 方法一: 递归
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean isValidBST(TreeNode node, long lower, long upper) {
        if (node == null) {
            return true;
        }
        if (node.val <= lower || node.val >= upper) {
            return false;
        }
        return isValidBST(node.left, lower, node.val) && isValidBST(node.right, node.val, upper);
    }
}

// 方法二：中序遍历
class Solution {
    public boolean isValidBST(TreeNode root) {
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        double inorder = -Double.MAX_VALUE;

        while (!stack.isEmpty() || root != null) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
              // 如果中序遍历得到的节点的值小于等于前一个 inorder，说明不是二叉搜索树
            if (root.val <= inorder) {
                return false;
            }
            inorder = root.val;
            root = root.right;
        }
        return true;
    }
}
```



### [173. 二叉搜索树迭代器](https://leetcode.cn/problems/binary-search-tree-iterator/)

中等

实现一个二叉搜索树迭代器类`BSTIterator` ，表示一个按中序遍历二叉搜索树（BST）的迭代器：

- `BSTIterator(TreeNode root)` 初始化 `BSTIterator` 类的一个对象。BST 的根节点 `root` 会作为构造函数的一部分给出。指针应初始化为一个不存在于 BST 中的数字，且该数字小于 BST 中的任何元素。
- `boolean hasNext()` 如果向指针右侧遍历存在数字，则返回 `true` ；否则返回 `false` 。
- `int next()`将指针向右移动，然后返回指针处的数字。

注意，指针初始化为一个不存在于 BST 中的数字，所以对 `next()` 的首次调用将返回 BST 中的最小元素。

你可以假设 `next()` 调用总是有效的，也就是说，当调用 `next()` 时，BST 的中序遍历中至少存在一个下一个数字。

**示例：**

![img](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)

```
输入
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
输出
[null, 3, 7, true, 9, true, 15, true, 20, false]

解释
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // 返回 3
bSTIterator.next();    // 返回 7
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 9
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 15
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 20
bSTIterator.hasNext(); // 返回 False
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
// 方法一：扁平化
class BSTIterator {
private:
    void inorder(TreeNode* root, vector<int>& res) {
        if (!root) {
            return;
        }
        inorder(root->left, res);
        res.push_back(root->val);
        inorder(root->right, res);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        inorder(root, res);
        return res;
    }
    
    vector<int> arr;
    int idx;
public:
    BSTIterator(TreeNode* root): idx(0), arr(inorderTraversal(root)) {}
    
    int next() {
        return arr[idx++];
    }
    
    bool hasNext() {
        return (idx < arr.size());
    }
};

// 方法二：迭代
class BSTIterator {
private:
    TreeNode* cur;
    stack<TreeNode*> stk;
public:
    BSTIterator(TreeNode* root): cur(root) {}
    
    int next() {
        while (cur != nullptr) {
            stk.push(cur);
            cur = cur->left;
        }
        cur = stk.top();
        stk.pop();
        int ret = cur->val;
        cur = cur->right;
        return ret;
    }
    
    bool hasNext() {
        return cur != nullptr || !stk.empty();
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
// 方法一：扁平化
class BSTIterator {
    private int idx;
    private List<Integer> arr;

    public BSTIterator(TreeNode root) {
        idx = 0;
        arr = new ArrayList<Integer>();
        inorderTraversal(root, arr);
    }

    public int next() {
        return arr.get(idx++);
    }

    public boolean hasNext() {
        return idx < arr.size();
    }

    private void inorderTraversal(TreeNode root, List<Integer> arr) {
        if (root == null) {
            return;
        }
        inorderTraversal(root.left, arr);
        arr.add(root.val);
        inorderTraversal(root.right, arr);
    }
}

// 方法二：迭代
class BSTIterator {
    private TreeNode cur;
    private Deque<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        cur = root;
        stack = new LinkedList<TreeNode>();
    }
    
    public int next() {
        while (cur != null) {
            stack.push(cur);
            cur = cur.left;
        }
        cur = stack.pop();
        int ret = cur.val;
        cur = cur.right;
        return ret;
    }
    
    public boolean hasNext() {
        return cur != null || !stack.isEmpty();
    }
}
```



### [700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)

简单

给定二叉搜索树（BST）的根节点 `root` 和一个整数值 `val`。

你需要在 BST 中找到节点值等于 `val` 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 `null` 。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/01/12/tree1.jpg)

```
输入：root = [4,2,7,1,3], val = 2
输出：[2,1,3]
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
// 方法一：递归
class Solution {
public:
    TreeNode *searchBST(TreeNode *root, int val) {
        if (root == nullptr) {
            return nullptr;
        }
        if (val == root->val) {
            return root;
        }
        return searchBST(val < root->val ? root->left : root->right, val);
    }
};

// 方法二：迭代
class Solution {
public:
    TreeNode *searchBST(TreeNode *root, int val) {
        while (root) {
            if (val == root->val) {
                return root;
            }
            root = val < root->val ? root->left : root->right;
        }
        return nullptr;
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
// 方法一：递归
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) {
            return null;
        }
        if (val == root.val) {
            return root;
        }
        return searchBST(val < root.val ? root.left : root.right, val);
    }
}

// 方法二：迭代
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        while (root != null) {
            if (val == root.val) {
                return root;
            }
            root = val < root.val ? root.left : root.right;
        }
        return null;
    }
}
```



### [701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

中等

给定二叉搜索树（BST）的根节点 `root` 和要插入树中的值 `value` ，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 **保证** ，新值和原始二叉搜索树中的任意节点值都不同。

**注意**，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 **任意有效的结果** 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/05/insertbst.jpg)

```
输入：root = [4,2,7,1,3], val = 5
输出：[4,2,7,1,3,5]
```

解释：另一个满足题目要求可以通过的树是：

![解释](https://assets.leetcode.com/uploads/2020/10/05/bst.jpg)

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
// 方法一：模拟
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == nullptr) {
            return new TreeNode(val);
        }
        TreeNode* pos = root;
        while (pos != nullptr) {
            if (val < pos->val) {
                if (pos->left == nullptr) {
                    pos->left = new TreeNode(val);
                    break;
                } else {
                    pos = pos->left;
                }
            } else {
                if (pos->right == nullptr) {
                    pos->right = new TreeNode(val);
                    break;
                } else {
                    pos = pos->right;
                }
            }
        }
        return root;
    }
};
```

Java版本

```java
// 方法一：模拟
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
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }
        TreeNode pos = root;
        while (pos != null) {
            if (val < pos.val) {
                if (pos.left == null) {
                    pos.left = new TreeNode(val);
                    break;
                } else {
                    pos = pos.left;
                }
            } else {
                if (pos.right == null) {
                    pos.right = new TreeNode(val);
                    break;
                } else {
                    pos = pos.right;
                }
            }
        }
        return root;
    }
}
```



### [450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)

中等

给定一个二叉搜索树的根节点 **root** 和一个值 **key**，删除二叉搜索树中的 **key** 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg)

```
输入：root = [5,3,6,2,4,null,7], key = 3
输出：[5,4,6,2,null,null,7]
解释：给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。
一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。
另一个正确答案是 [5,2,6,null,4,null,7]。
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
// 方法一：递归
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == nullptr) {
            return nullptr;
        }
        if (root->val > key) {
            root->left = deleteNode(root->left, key);
            return root;
        }
        if (root->val < key) {
            root->right = deleteNode(root->right, key);
            return root;
        }
        if (root->val == key) {
            if (!root->left && !root->right) {
                return nullptr;
            }
            if (!root->right) {
                return root->left;
            }
            if (!root->left) {
                return root->right;
            }
            TreeNode *successor = root->right;
            while (successor->left) {
                successor = successor->left;
            }
            root->right = deleteNode(root->right, successor->val);
            successor->right = root->right;
            successor->left = root->left;
            return successor;
        }
        return root;
    }
};

// 方法二：迭代
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        TreeNode *cur = root, *curParent = nullptr;
        while (cur && cur->val != key) {
            curParent = cur;
            if (cur->val > key) {
                cur = cur->left;
            } else {
                cur = cur->right;
            }
        }
        if (!cur) {
            return root;
        }
        if (!cur->left && !cur->right) {
            cur = nullptr;
        } else if (!cur->right) {
            cur = cur->left;
        } else if (!cur->left) {
            cur = cur->right;
        } else {
            TreeNode *successor = cur->right, *successorParent = cur;
            while (successor->left) {
                successorParent = successor;
                successor = successor->left;
            }
            if (successorParent->val == cur->val) {
                successorParent->right = successor->right;
            } else {
                successorParent->left = successor->right;
            }
            successor->right = cur->right;
            successor->left = cur->left;
            cur = successor;
        }
        if (!curParent) {
            return cur;
        } else {
            if (curParent->left && curParent->left->val == key) {
                curParent->left = cur;
            } else {
                curParent->right = cur;
            }
            return root;
        }
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
// 方法一：递归
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) {
            return null;
        }
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
            return root;
        }
        if (root.val < key) {
            root.right = deleteNode(root.right, key);
            return root;
        }
        if (root.val == key) {
            if (root.left == null && root.right == null) {
                return null;
            }
            if (root.right == null) {
                return root.left;
            }
            if (root.left == null) {
                return root.right;
            }
            TreeNode successor = root.right;
            while (successor.left != null) {
                successor = successor.left;
            }
            root.right = deleteNode(root.right, successor.val);
            successor.right = root.right;
            successor.left = root.left;
            return successor;
        }
        return root;
    }
}

// 方法二：迭代
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        TreeNode cur = root, curParent = null;
        while (cur != null && cur.val != key) {
            curParent = cur;
            if (cur.val > key) {
                cur = cur.left;
            } else {
                cur = cur.right;
            }
        }
        if (cur == null) {
            return root;
        }
        if (cur.left == null && cur.right == null) {
            cur = null;
        } else if (cur.right == null) {
            cur = cur.left;
        } else if (cur.left == null) {
            cur = cur.right;
        } else {
            TreeNode successor = cur.right, successorParent = cur;
            while (successor.left != null) {
                successorParent = successor;
                successor = successor.left;
            }
            if (successorParent.val == cur.val) {
                successorParent.right = successor.right;
            } else {
                successorParent.left = successor.right;
            }
            successor.right = cur.right;
            successor.left = cur.left;
            cur = successor;
        }
        if (curParent == null) {
            return cur;
        } else {
            if (curParent.left != null && curParent.left.val == key) {
                curParent.left = cur;
            } else {
                curParent.right = cur;
            }
            return root;
        }
    }
}
```



### [703. 数据流中的第 K 大元素](https://leetcode.cn/problems/kth-largest-element-in-a-stream/)

简单

设计一个找到数据流中第 `k` 大元素的类（class）。注意是排序后的第 `k` 大元素，不是第 `k` 个不同的元素。

请实现 `KthLargest` 类：

- `KthLargest(int k, int[] nums)` 使用整数 `k` 和整数流 `nums` 初始化对象。
- `int add(int val)` 将 `val` 插入数据流 `nums` 后，返回当前数据流中第 `k` 大的元素。

**示例：**

```
输入：
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
输出：
[null, 4, 5, 5, 8, 8]

解释：
KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(3);   // return 4
kthLargest.add(5);   // return 5
kthLargest.add(10);  // return 5
kthLargest.add(9);   // return 8
kthLargest.add(4);   // return 8
```

C++版本

```c++
/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
// 方法一：优先队列
class KthLargest {
public:
    priority_queue<int, vector<int>, greater<int>> q;
    int k;
    KthLargest(int k, vector<int>& nums) {
        this->k = k;
        for (auto& x: nums) {
            add(x);
        }
    }
    
    int add(int val) {
        q.push(val);
        if (q.size() > k) {
            q.pop();
        }
        return q.top();
    }
};


```

Java版本

```java
/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.add(val);
 */
// 方法一：优先队列
class KthLargest {
    PriorityQueue<Integer> pq;
    int k;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        pq = new PriorityQueue<Integer>();
        for (int x : nums) {
            add(x);
        }
    }
    
    public int add(int val) {
        pq.offer(val);
        if (pq.size() > k) {
            pq.poll();
        }
        return pq.peek();
    }
}
```



### [LCR 174. 寻找二叉搜索树中的目标节点](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

简单

某公司组织架构以二叉搜索树形式记录，节点值为处于该职位的员工编号。请返回第 `cnt` 大的员工编号。

**示例 1：**

![img](https://pic.leetcode.cn/1695101634-kzHKZW-image.png)

```
输入：root = [7, 3, 9, 1, 5], cnt = 2
       7
      / \
     3   9
    / \
   1   5
输出：7
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
class Solution {
public:
    int findTargetNode(TreeNode* root, int cnt) {
        this->cnt = cnt;
        dfs(root);
        return res;
    }
private:
    int res, cnt;
    void dfs(TreeNode* root) {
        if(root == nullptr) return;
        dfs(root->right);
        if(cnt == 0) return;
        if(--cnt == 0) res = root->val;
        dfs(root->left);
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
class Solution {
    int res, cnt;
    public int findTargetNode(TreeNode root, int cnt) {
        this.cnt = cnt;
        dfs(root);
        return res;
    }
    void dfs(TreeNode root) {
        if(root == null) return;
        dfs(root.right);
        if(cnt == 0) return;
        if(--cnt == 0) res = root.val;
        dfs(root.left);
    }
}
```



### [230. 二叉搜索树中第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)

中等

给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k` 个最小元素（从 1 开始计数）。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

```
输入：root = [3,1,4,null,2], k = 1
输出：1
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
// 方法一：中序遍历
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        stack<TreeNode *> stack;
        while (root != nullptr || stack.size() > 0) {
            while (root != nullptr) {
                stack.push(root);
                root = root->left;
            }
            root = stack.top();
            stack.pop();
            --k;
            if (k == 0) {
                break;
            }
            root = root->right;
        }
        return root->val;
    }
};

// 方法二：记录子树的结点数
class MyBst {
public:
    MyBst(TreeNode *root) {
        this->root = root;
        countNodeNum(root);
    }

    // 返回二叉搜索树中第k小的元素
    int kthSmallest(int k) {
        TreeNode *node = root;
        while (node != nullptr) {
            int left = getNodeNum(node->left);
            if (left < k - 1) {
                node = node->right;
                k -= left + 1;
            } else if (left == k - 1) {
                break;
            } else {
                node = node->left;
            }
        }
        return node->val;
    }

private:
    TreeNode *root;
    unordered_map<TreeNode *, int> nodeNum;

    // 统计以node为根结点的子树的结点数
    int countNodeNum(TreeNode * node) {
        if (node == nullptr) {
            return 0;
        }
        nodeNum[node] = 1 + countNodeNum(node->left) + countNodeNum(node->right);
        return nodeNum[node];
    }

    // 获取以node为根结点的子树的结点数
    int getNodeNum(TreeNode * node) {
        if (node != nullptr && nodeNum.count(node)) {
            return nodeNum[node];
        }else{
            return 0;
        }
    }
};

class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        MyBst bst(root);
        return bst.kthSmallest(k);
    }
};

// 方法三：平衡二叉搜索树
// 平衡二叉搜索树结点
struct Node {
    int val;
    Node * parent;
    Node * left;
    Node * right;
    int size;
    int height;

    Node(int val) {
        this->val = val;
        this->parent = nullptr;
        this->left = nullptr;
        this->right = nullptr;
        this->height = 0; // 结点高度：以node为根节点的子树的高度（高度定义：叶结点的高度是0）
        this->size = 1; // 结点元素数：以node为根节点的子树的节点总数
    }

    Node(int val, Node * parent) {
        this->val = val;
        this->parent = parent;
        this->left = nullptr;
        this->right = nullptr;
        this->height = 0; // 结点高度：以node为根节点的子树的高度（高度定义：叶结点的高度是0）
        this->size = 1; // 结点元素数：以node为根节点的子树的节点总数
    }

    Node(int val, Node * parent, Node * left, Node * right) {
        this->val = val;
        this->parent = parent;
        this->left = left;
        this->right = right;
        this->height = 0; // 结点高度：以node为根节点的子树的高度（高度定义：叶结点的高度是0）
        this->size = 1; // 结点元素数：以node为根节点的子树的节点总数
    }
};


// 平衡二叉搜索树（AVL树）：允许重复值
class AVL {
public:
    AVL(vector<int> & vals) {
        if (!vals.empty()) {
            root = build(vals, 0, vals.size() - 1, nullptr);
        }
    }

    // 根据vals[l:r]构造平衡二叉搜索树 -> 返回根结点
    Node * build(vector<int> & vals, int l, int r, Node * parent) {
        int m = (l + r) >> 1;
        Node * node = new Node(vals[m], parent);
        if (l <= m - 1) {
            node->left = build(vals, l, m - 1, node);
        }
        if (m + 1 <= r) {
            node->right = build(vals, m + 1, r, node);
        }
        recompute(node);
        return node;
    }

    // 返回二叉搜索树中第k小的元素
    int kthSmallest(int k) {
        Node * node = root;
        while (node != nullptr) {
            int left = getSize(node->left);
            if (left < k - 1) {
                node = node->right;
                k -= left + 1;
            } else if (left == k - 1) {
                break;
            } else {
                node = node->left;
            }
        }
        return node->val;
    }

    void insert(int v) {
        if (root == nullptr) {
            root = new Node(v);
        } else {
            // 计算新结点的添加位置
            Node * node = subtreeSearch(root, v);
            bool isAddLeft = v <= node->val; // 是否将新结点添加到node的左子结点
            if (node->val == v) { // 如果值为v的结点已存在
                if (node->left != nullptr) { // 值为v的结点存在左子结点，则添加到其左子树的最右侧
                    node = subtreeLast(node->left);
                    isAddLeft = false;
                } else { // 值为v的结点不存在左子结点，则添加到其左子结点
                    isAddLeft = true;
                }
            }

            // 添加新结点
            Node * leaf = new Node(v, node);
            if (isAddLeft) {
                node->left = leaf;
            } else {
                node->right = leaf;
            }

            rebalance(leaf);
        }
    }

    // 删除值为v的结点 -> 返回是否成功删除结点
    bool Delete(int v) {
        if (root == nullptr) {
            return false;
        }

        Node * node = subtreeSearch(root, v);
        if (node->val != v) { // 没有找到需要删除的结点
            return false;
        }

        // 处理当前结点既有左子树也有右子树的情况
        // 若左子树比右子树高度低，则将当前结点替换为右子树最左侧的结点，并移除右子树最左侧的结点
        // 若右子树比左子树高度低，则将当前结点替换为左子树最右侧的结点，并移除左子树最右侧的结点
        if (node->left != nullptr && node->right != nullptr) {
            Node * replacement = nullptr;
            if (node->left->height <= node->right->height) {
                replacement = subtreeFirst(node->right);
            } else {
                replacement = subtreeLast(node->left);
            }
            node->val = replacement->val;
            node = replacement;
        }

        Node * parent = node->parent;
        Delete(node);
        rebalance(parent);
        return true;
    }

private:
    Node * root;

    // 删除结点p并用它的子结点代替它，结点p至多只能有1个子结点
    void Delete(Node * node) {
        if (node->left != nullptr && node->right != nullptr) {
            return;
            // throw new Exception("Node has two children");
        }
        Node * child = node->left != nullptr ? node->left : node->right;
        if (child != nullptr) {
            child->parent = node->parent;
        }
        if (node == root) {
            root = child;
        } else {
            Node * parent = node->parent;
            if (node == parent->left) {
                parent->left = child;
            } else {
                parent->right = child;
            }
        }
        node->parent = node;
    }

    // 在以node为根结点的子树中搜索值为v的结点，如果没有值为v的结点，则返回值为v的结点应该在的位置的父结点
    Node * subtreeSearch(Node * node, int v) {
        if (node->val < v && node->right != nullptr) {
            return subtreeSearch(node->right, v);
        } else if (node->val > v && node->left != nullptr) {
            return subtreeSearch(node->left, v);
        } else {
            return node;
        }
    }

    // 重新计算node结点的高度和元素数
    void recompute(Node * node) {
        node->height = 1 + max(getHeight(node->left), getHeight(node->right));
        node->size = 1 + getSize(node->left) + getSize(node->right);
    }

    // 从node结点开始（含node结点）逐个向上重新平衡二叉树，并更新结点高度和元素数
    void rebalance(Node * node) {
        while (node != nullptr) {
            int oldHeight = node->height, oldSize = node->size;
            if (!isBalanced(node)) {
                node = restructure(tallGrandchild(node));
                recompute(node->left);
                recompute(node->right);
            }
            recompute(node);
            if (node->height == oldHeight && node->size == oldSize) {
                node = nullptr; // 如果结点高度和元素数都没有变化则不需要再继续向上调整
            } else {
                node = node->parent;
            }
        }
    }

    // 判断node结点是否平衡
    bool isBalanced(Node * node) {
        return abs(getHeight(node->left) - getHeight(node->right)) <= 1;
    }

    // 获取node结点更高的子树
    Node * tallChild(Node * node) {
        if (getHeight(node->left) > getHeight(node->right)) {
            return node->left;
        } else {
            return node->right;
        }
    }

    // 获取node结点更高的子树中的更高的子树
    Node * tallGrandchild(Node * node) {
        Node * child = tallChild(node);
        return tallChild(child);
    }

    // 重新连接父结点和子结点（子结点允许为空）
    static void relink(Node * parent, Node * child, bool isLeft) {
        if (isLeft) {
            parent->left = child;
        } else {
            parent->right = child;
        }
        if (child != nullptr) {
            child->parent = parent;
        }
    }

    // 旋转操作
    void rotate(Node * node) {
        Node * parent = node->parent;
        Node * grandparent = parent->parent;
        if (grandparent == nullptr) {
            root = node;
            node->parent = nullptr;
        } else {
            relink(grandparent, node, parent == grandparent->left);
        }

        if (node == parent->left) {
            relink(parent, node->right, true);
            relink(node, parent, false);
        } else {
            relink(parent, node->left, false);
            relink(node, parent, true);
        }
    }

    // trinode操作
    Node * restructure(Node * node) {
        Node * parent = node->parent;
        Node * grandparent = parent->parent;

        if ((node == parent->right) == (parent == grandparent->right)) { // 处理需要一次旋转的情况
            rotate(parent);
            return parent;
        } else { // 处理需要两次旋转的情况：第1次旋转后即成为需要一次旋转的情况
            rotate(node);
            rotate(node);
            return node;
        }
    }

    // 返回以node为根结点的子树的第1个元素
    static Node * subtreeFirst(Node * node) {
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }

    // 返回以node为根结点的子树的最后1个元素
    static Node * subtreeLast(Node * node) {
        while (node->right != nullptr) {
            node = node->right;
        }
        return node;
    }

    // 获取以node为根结点的子树的高度
    static int getHeight(Node * node) {
        return node != nullptr ? node->height : 0;
    }

    // 获取以node为根结点的子树的结点数
    static int getSize(Node * node) {
        return node != nullptr ? node->size : 0;
    }
};

class Solution {
public:
    int kthSmallest(TreeNode * root, int k) {
        // 中序遍历生成数值列表
        vector<int> inorderList;
        inorder(root, inorderList);
        // 构造平衡二叉搜索树
        AVL avl(inorderList);

        // 模拟1000次插入和删除操作
        vector<int> randomNums(1000);
        std::random_device rd;
        for (int i = 0; i < 1000; ++i) {
            randomNums[i] = rd()%(10001);
            avl.insert(randomNums[i]);
        }
        shuffle(randomNums); // 列表乱序
        for (int i = 0; i < 1000; ++i) {
            avl.Delete(randomNums[i]);
        }

        return avl.kthSmallest(k);
    }

private:
    void inorder(TreeNode * node, vector<int> & inorderList) {
        if (node->left != nullptr) {
            inorder(node->left, inorderList);
        }
        inorderList.push_back(node->val);
        if (node->right != nullptr) {
            inorder(node->right, inorderList);
        }
    }

    void shuffle(vector<int> & arr) {
        std::random_device rd;
        int length = arr.size();
        for (int i = 0; i < length; i++) {
            int randIndex = rd()%length;
            swap(arr[i],arr[randIndex]);
        }
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
// 方法一：中序遍历
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            --k;
            if (k == 0) {
                break;
            }
            root = root.right;
        }
        return root.val;
    }
}

// 方法二：记录子树的结点数
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        MyBst bst = new MyBst(root);
        return bst.kthSmallest(k);
    }
}

class MyBst {
    TreeNode root;
    Map<TreeNode, Integer> nodeNum;

    public MyBst(TreeNode root) {
        this.root = root;
        this.nodeNum = new HashMap<TreeNode, Integer>();
        countNodeNum(root);
    }

    // 返回二叉搜索树中第k小的元素
    public int kthSmallest(int k) {
        TreeNode node = root;
        while (node != null) {
            int left = getNodeNum(node.left);
            if (left < k - 1) {
                node = node.right;
                k -= left + 1;
            } else if (left == k - 1) {
                break;
            } else {
                node = node.left;
            }
        }
        return node.val;
    }

    // 统计以node为根结点的子树的结点数
    private int countNodeNum(TreeNode node) {
        if (node == null) {
            return 0;
        }
        nodeNum.put(node, 1 + countNodeNum(node.left) + countNodeNum(node.right));
        return nodeNum.get(node);
    }

    // 获取以node为根结点的子树的结点数
    private int getNodeNum(TreeNode node) {
        return nodeNum.getOrDefault(node, 0);
    }
}

// 方法三：平衡二叉搜索树
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        // 中序遍历生成数值列表
        List<Integer> inorderList = new ArrayList<Integer>();
        inorder(root, inorderList);

        // 构造平衡二叉搜索树
        AVL avl = new AVL(inorderList);

        // 模拟1000次插入和删除操作
        int[] randomNums = new int[1000];
        Random random = new Random();
        for (int i = 0; i < 1000; ++i) {
            randomNums[i] = random.nextInt(10001);
            avl.insert(randomNums[i]);
        }
        shuffle(randomNums); // 列表乱序
        for (int i = 0; i < 1000; ++i) {
            avl.delete(randomNums[i]);
        }

        return avl.kthSmallest(k);
    }

    private void inorder(TreeNode node, List<Integer> inorderList) {
        if (node.left != null) {
            inorder(node.left, inorderList);
        }
        inorderList.add(node.val);
        if (node.right != null) {
            inorder(node.right, inorderList);
        }
    }

    private void shuffle(int[] arr) {
        Random random = new Random();
        int length = arr.length;
        for (int i = 0; i < length; i++) {
            int randIndex = random.nextInt(length);
            int temp = arr[i];
            arr[i] = arr[randIndex];
            arr[randIndex] = temp;
        }
    }
}

// 平衡二叉搜索树（AVL树）：允许重复值
class AVL {
    Node root;

    // 平衡二叉搜索树结点
    class Node {
        int val;
        Node parent;
        Node left;
        Node right;
        int size;
        int height;

        public Node(int val) {
            this(val, null);
        }

        public Node(int val, Node parent) {
            this(val, parent, null, null);
        }

        public Node(int val, Node parent, Node left, Node right) {
            this.val = val;
            this.parent = parent;
            this.left = left;
            this.right = right;
            this.height = 0; // 结点高度：以node为根节点的子树的高度（高度定义：叶结点的高度是0）
            this.size = 1; // 结点元素数：以node为根节点的子树的节点总数
        }
    }

    public AVL(List<Integer> vals) {
        if (vals != null) {
            this.root = build(vals, 0, vals.size() - 1, null);
        }
    }

    // 根据vals[l:r]构造平衡二叉搜索树 -> 返回根结点
    private Node build(List<Integer> vals, int l, int r, Node parent) {
        int m = (l + r) >> 1;
        Node node = new Node(vals.get(m), parent);
        if (l <= m - 1) {
            node.left = build(vals, l, m - 1, node);
        }
        if (m + 1 <= r) {
            node.right = build(vals, m + 1, r, node);
        }
        recompute(node);
        return node;
    }

    // 返回二叉搜索树中第k小的元素
    public int kthSmallest(int k) {
        Node node = root;
        while (node != null) {
            int left = getSize(node.left);
            if (left < k - 1) {
                node = node.right;
                k -= left + 1;
            } else if (left == k - 1) {
                break;
            } else {
                node = node.left;
            }
        }
        return node.val;
    }

    public void insert(int v) {
        if (root == null) {
            root = new Node(v);
        } else {
            // 计算新结点的添加位置
            Node node = subtreeSearch(root, v);
            boolean isAddLeft = v <= node.val; // 是否将新结点添加到node的左子结点
            if (node.val == v) { // 如果值为v的结点已存在
                if (node.left != null) { // 值为v的结点存在左子结点，则添加到其左子树的最右侧
                    node = subtreeLast(node.left);
                    isAddLeft = false;
                } else { // 值为v的结点不存在左子结点，则添加到其左子结点
                    isAddLeft = true;
                }
            }

            // 添加新结点
            Node leaf = new Node(v, node);
            if (isAddLeft) {
                node.left = leaf;
            } else {
                node.right = leaf;
            }

            rebalance(leaf);
        }
    }

    // 删除值为v的结点 -> 返回是否成功删除结点
    public boolean delete(int v) {
        if (root == null) {
            return false;
        }

        Node node = subtreeSearch(root, v);
        if (node.val != v) { // 没有找到需要删除的结点
            return false;
        }

        // 处理当前结点既有左子树也有右子树的情况
        // 若左子树比右子树高度低，则将当前结点替换为右子树最左侧的结点，并移除右子树最左侧的结点
        // 若右子树比左子树高度低，则将当前结点替换为左子树最右侧的结点，并移除左子树最右侧的结点
        if (node.left != null && node.right != null) {
            Node replacement = null;
            if (node.left.height <= node.right.height) {
                replacement = subtreeFirst(node.right);
            } else {
                replacement = subtreeLast(node.left);
            }
            node.val = replacement.val;
            node = replacement;
        }

        Node parent = node.parent;
        delete(node);
        rebalance(parent);
        return true;
    }

    // 删除结点p并用它的子结点代替它，结点p至多只能有1个子结点
    private void delete(Node node) {
        if (node.left != null && node.right != null) {
            return;
            // throw new Exception("Node has two children");
        }
        Node child = node.left != null ? node.left : node.right;
        if (child != null) {
            child.parent = node.parent;
        }
        if (node == root) {
            root = child;
        } else {
            Node parent = node.parent;
            if (node == parent.left) {
                parent.left = child;
            } else {
                parent.right = child;
            }
        }
        node.parent = node;
    }

    // 在以node为根结点的子树中搜索值为v的结点，如果没有值为v的结点，则返回值为v的结点应该在的位置的父结点
    private Node subtreeSearch(Node node, int v) {
        if (node.val < v && node.right != null) {
            return subtreeSearch(node.right, v);
        } else if (node.val > v && node.left != null) {
            return subtreeSearch(node.left, v);
        } else {
            return node;
        }
    }

    // 重新计算node结点的高度和元素数
    private void recompute(Node node) {
        node.height = 1 + Math.max(getHeight(node.left), getHeight(node.right));
        node.size = 1 + getSize(node.left) + getSize(node.right);
    }

    // 从node结点开始（含node结点）逐个向上重新平衡二叉树，并更新结点高度和元素数
    private void rebalance(Node node) {
        while (node != null) {
            int oldHeight = node.height, oldSize = node.size;
            if (!isBalanced(node)) {
                node = restructure(tallGrandchild(node));
                recompute(node.left);
                recompute(node.right);
            }
            recompute(node);
            if (node.height == oldHeight && node.size == oldSize) {
                node = null; // 如果结点高度和元素数都没有变化则不需要再继续向上调整
            } else {
                node = node.parent;
            }
        }
    }

    // 判断node结点是否平衡
    private boolean isBalanced(Node node) {
        return Math.abs(getHeight(node.left) - getHeight(node.right)) <= 1;
    }

    // 获取node结点更高的子树
    private Node tallChild(Node node) {
        if (getHeight(node.left) > getHeight(node.right)) {
            return node.left;
        } else {
            return node.right;
        }
    }

    // 获取node结点更高的子树中的更高的子树
    private Node tallGrandchild(Node node) {
        Node child = tallChild(node);
        return tallChild(child);
    }

    // 重新连接父结点和子结点（子结点允许为空）
    private static void relink(Node parent, Node child, boolean isLeft) {
        if (isLeft) {
            parent.left = child;
        } else {
            parent.right = child;
        }
        if (child != null) {
            child.parent = parent;
        }
    }

    // 旋转操作
    private void rotate(Node node) {
        Node parent = node.parent;
        Node grandparent = parent.parent;
        if (grandparent == null) {
            root = node;
            node.parent = null;
        } else {
            relink(grandparent, node, parent == grandparent.left);
        }

        if (node == parent.left) {
            relink(parent, node.right, true);
            relink(node, parent, false);
        } else {
            relink(parent, node.left, false);
            relink(node, parent, true);
        }
    }

    // trinode操作
    private Node restructure(Node node) {
        Node parent = node.parent;
        Node grandparent = parent.parent;

        if ((node == parent.right) == (parent == grandparent.right)) { // 处理需要一次旋转的情况
            rotate(parent);
            return parent;
        } else { // 处理需要两次旋转的情况：第1次旋转后即成为需要一次旋转的情况
            rotate(node);
            rotate(node);
            return node;
        }
    }

    // 返回以node为根结点的子树的第1个元素
    private static Node subtreeFirst(Node node) {
        while (node.left != null) {
            node = node.left;
        }
        return node;
    }

    // 返回以node为根结点的子树的最后1个元素
    private static Node subtreeLast(Node node) {
        while (node.right != null) {
            node = node.right;
        }
        return node;
    }

    // 获取以node为根结点的子树的高度
    private static int getHeight(Node node) {
        return node != null ? node.height : 0;
    }

    // 获取以node为根结点的子树的结点数
    private static int getSize(Node node) {
        return node != null ? node.size : 0;
    }
}
```



