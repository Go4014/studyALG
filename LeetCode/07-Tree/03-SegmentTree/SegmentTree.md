# 树

## 线段树

### [303. 区域和检索 - 数组不可变](https://leetcode.cn/problems/range-sum-query-immutable/)

简单

给定一个整数数组  `nums`，处理以下类型的多个查询:

1. 计算索引 `left` 和 `right` （包含 `left` 和 `right`）之间的 `nums` 元素的 **和** ，其中 `left <= right`

实现 `NumArray` 类：

- `NumArray(int[] nums)` 使用数组 `nums` 初始化对象
- `int sumRange(int i, int j)` 返回数组 `nums` 中索引 `left` 和 `right` 之间的元素的 **总和** ，包含 `left` 和 `right` 两点（也就是 `nums[left] + nums[left + 1] + ... + nums[right]` )

**示例 1：**

```
输入：
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
输出：
[null, 1, -1, -3]

解释：
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))
```

C++版本

```c++
// 方法一：前缀和
class NumArray {
public:
    vector<int> sums;

    NumArray(vector<int>& nums) {
        int n = nums.size();
        sums.resize(n + 1);
        for (int i = 0; i < n; i++) {
            sums[i + 1] = sums[i] + nums[i];
        }
    }

    int sumRange(int i, int j) {
        return sums[j + 1] - sums[i];
    }
};
```

Java版本

```java
// 方法一：前缀和
class NumArray {
    int[] sums;

    public NumArray(int[] nums) {
        int n = nums.length;
        sums = new int[n + 1];
        for (int i = 0; i < n; i++) {
            sums[i + 1] = sums[i] + nums[i];
        }
    }
    
    public int sumRange(int i, int j) {
        return sums[j + 1] - sums[i];
    }
}
```



### [307. 区域和检索 - 数组可修改](https://leetcode.cn/problems/range-sum-query-mutable/)

中等

给你一个数组 `nums` ，请你完成两类查询。

1. 其中一类查询要求 **更新** 数组 `nums` 下标对应的值
2. 另一类查询要求返回数组 `nums` 中索引 `left` 和索引 `right` 之间（ **包含** ）的nums元素的 **和** ，其中 `left <= right`

实现 `NumArray` 类：

- `NumArray(int[] nums)` 用整数数组 `nums` 初始化对象
- `void update(int index, int val)` 将 `nums[index]` 的值 **更新** 为 `val`
- `int sumRange(int left, int right)` 返回数组 `nums` 中索引 `left` 和索引 `right` 之间（ **包含** ）的nums元素的 **和** （即，`nums[left] + nums[left + 1], ..., nums[right]`）

**示例 1：**

```
输入：
["NumArray", "sumRange", "update", "sumRange"]
[[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]
输出：
[null, 9, null, 8]

解释：
NumArray numArray = new NumArray([1, 3, 5]);
numArray.sumRange(0, 2); // 返回 1 + 3 + 5 = 9
numArray.update(1, 2);   // nums = [1,2,5]
numArray.sumRange(0, 2); // 返回 1 + 2 + 5 = 8
```

C++版本

```c++
// 方法一：分块处理
class NumArray {
private:
    vector<int> sum; // sum[i] 表示第 i 个块的元素和
    int size; // 块的大小
    vector<int> &nums;
public:
    NumArray(vector<int>& nums) : nums(nums) {
        int n = nums.size();
        size = sqrt(n);
        sum.resize((n + size - 1) / size); // n/size 向上取整
        for (int i = 0; i < n; i++) {
            sum[i / size] += nums[i];
        }
    }

    void update(int index, int val) {
        sum[index / size] += val - nums[index];
        nums[index] = val;
    }

    int sumRange(int left, int right) {
        int b1 = left / size, i1 = left % size, b2 = right / size, i2 = right % size;
        if (b1 == b2) { // 区间 [left, right] 在同一块中
            return accumulate(nums.begin() + b1 * size + i1, nums.begin() + b1 * size + i2 + 1, 0);
        }
        int sum1 = accumulate(nums.begin() + b1 * size + i1, nums.begin() + b1 * size + size, 0);
        int sum2 = accumulate(nums.begin() + b2 * size, nums.begin() + b2 * size + i2 + 1, 0);
        int sum3 = accumulate(sum.begin() + b1 + 1, sum.begin() + b2, 0);
        return sum1 + sum2 + sum3;
    }
};

// 方法二：线段树
class NumArray {
private:
    vector<int> segmentTree;
    int n;

    void build(int node, int s, int e, vector<int> &nums) {
        if (s == e) {
            segmentTree[node] = nums[s];
            return;
        }
        int m = s + (e - s) / 2;
        build(node * 2 + 1, s, m, nums);
        build(node * 2 + 2, m + 1, e, nums);
        segmentTree[node] = segmentTree[node * 2 + 1] + segmentTree[node * 2 + 2];
    }

    void change(int index, int val, int node, int s, int e) {
        if (s == e) {
            segmentTree[node] = val;
            return;
        }
        int m = s + (e - s) / 2;
        if (index <= m) {
            change(index, val, node * 2 + 1, s, m);
        } else {
            change(index, val, node * 2 + 2, m + 1, e);
        }
        segmentTree[node] = segmentTree[node * 2 + 1] + segmentTree[node * 2 + 2];
    }

    int range(int left, int right, int node, int s, int e) {
        if (left == s && right == e) {
            return segmentTree[node];
        }
        int m = s + (e - s) / 2;
        if (right <= m) {
            return range(left, right, node * 2 + 1, s, m);
        } else if (left > m) {
            return range(left, right, node * 2 + 2, m + 1, e);
        } else {
            return range(left, m, node * 2 + 1, s, m) + range(m + 1, right, node * 2 + 2, m + 1, e);
        }
    }

public:
    NumArray(vector<int>& nums) : n(nums.size()), segmentTree(nums.size() * 4) {
        build(0, 0, n - 1, nums);
    }

    void update(int index, int val) {
        change(index, val, 0, 0, n - 1);
    }

    int sumRange(int left, int right) {
        return range(left, right, 0, 0, n - 1);
    }
};

// 方法三：树状数组
class NumArray {
private:
    vector<int> tree;
    vector<int> &nums;

    int lowBit(int x) {
        return x & -x;
    }

    void add(int index, int val) {
        while (index < tree.size()) {
            tree[index] += val;
            index += lowBit(index);
        }
    }

    int prefixSum(int index) {
        int sum = 0;
        while (index > 0) {
            sum += tree[index];
            index -= lowBit(index);
        }
        return sum;
    }

public:
    NumArray(vector<int>& nums) : tree(nums.size() + 1), nums(nums) {
        for (int i = 0; i < nums.size(); i++) {
            add(i + 1, nums[i]);
        }
    }

    void update(int index, int val) {
        add(index + 1, val - nums[index]);
        nums[index] = val;
    }

    int sumRange(int left, int right) {
        return prefixSum(right + 1) - prefixSum(left);
    }
};
```

Java版本

```java
// 方法一：分块处理
class NumArray {
    private int[] sum; // sum[i] 表示第 i 个块的元素和
    private int size; // 块的大小
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
        int n = nums.length;
        size = (int) Math.sqrt(n);
        sum = new int[(n + size - 1) / size]; // n/size 向上取整
        for (int i = 0; i < n; i++) {
            sum[i / size] += nums[i];
        }
    }

    public void update(int index, int val) {
        sum[index / size] += val - nums[index];
        nums[index] = val;
    }

    public int sumRange(int left, int right) {
        int b1 = left / size, i1 = left % size, b2 = right / size, i2 = right % size;
        if (b1 == b2) { // 区间 [left, right] 在同一块中
            int sum = 0;
            for (int j = i1; j <= i2; j++) {
                sum += nums[b1 * size + j];
            }
            return sum;
        }
        int sum1 = 0;
        for (int j = i1; j < size; j++) {
            sum1 += nums[b1 * size + j];
        }
        int sum2 = 0;
        for (int j = 0; j <= i2; j++) {
            sum2 += nums[b2 * size + j];
        }
        int sum3 = 0;
        for (int j = b1 + 1; j < b2; j++) {
            sum3 += sum[j];
        }
        return sum1 + sum2 + sum3;
    }
}

// 方法二：线段树
class NumArray {
    private int[] segmentTree;
    private int n;

    public NumArray(int[] nums) {
        n = nums.length;
        segmentTree = new int[nums.length * 4];
        build(0, 0, n - 1, nums);
    }

    public void update(int index, int val) {
        change(index, val, 0, 0, n - 1);
    }

    public int sumRange(int left, int right) {
        return range(left, right, 0, 0, n - 1);
    }

    private void build(int node, int s, int e, int[] nums) {
        if (s == e) {
            segmentTree[node] = nums[s];
            return;
        }
        int m = s + (e - s) / 2;
        build(node * 2 + 1, s, m, nums);
        build(node * 2 + 2, m + 1, e, nums);
        segmentTree[node] = segmentTree[node * 2 + 1] + segmentTree[node * 2 + 2];
    }

    private void change(int index, int val, int node, int s, int e) {
        if (s == e) {
            segmentTree[node] = val;
            return;
        }
        int m = s + (e - s) / 2;
        if (index <= m) {
            change(index, val, node * 2 + 1, s, m);
        } else {
            change(index, val, node * 2 + 2, m + 1, e);
        }
        segmentTree[node] = segmentTree[node * 2 + 1] + segmentTree[node * 2 + 2];
    }

    private int range(int left, int right, int node, int s, int e) {
        if (left == s && right == e) {
            return segmentTree[node];
        }
        int m = s + (e - s) / 2;
        if (right <= m) {
            return range(left, right, node * 2 + 1, s, m);
        } else if (left > m) {
            return range(left, right, node * 2 + 2, m + 1, e);
        } else {
            return range(left, m, node * 2 + 1, s, m) + range(m + 1, right, node * 2 + 2, m + 1, e);
        }
    }
}

// 方法三：树状数组
class NumArray {
    private int[] tree;
    private int[] nums;

    public NumArray(int[] nums) {
        this.tree = new int[nums.length + 1];
        this.nums = nums;
        for (int i = 0; i < nums.length; i++) {
            add(i + 1, nums[i]);
        }
    }

    public void update(int index, int val) {
        add(index + 1, val - nums[index]);
        nums[index] = val;
    }

    public int sumRange(int left, int right) {
        return prefixSum(right + 1) - prefixSum(left);
    }

    private int lowBit(int x) {
        return x & -x;
    }

    private void add(int index, int val) {
        while (index < tree.length) {
            tree[index] += val;
            index += lowBit(index);
        }
    }

    private int prefixSum(int index) {
        int sum = 0;
        while (index > 0) {
            sum += tree[index];
            index -= lowBit(index);
        }
        return sum;
    }
}
```







