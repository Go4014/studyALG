## 树状数组

### [315. 计算右侧小于当前元素的个数](https://leetcode.cn/problems/count-of-smaller-numbers-after-self/)

困难

给你一个整数数组 `nums` ，按要求返回一个新数组 `counts` 。数组 `counts` 有该性质： `counts[i]` 的值是 `nums[i]` 右侧小于 `nums[i]` 的元素的数量。

**示例 1：**

```
输入：nums = [5,2,6,1]
输出：[2,1,1,0] 
解释：
5 的右侧有 2 个更小的元素 (2 和 1)
2 的右侧仅有 1 个更小的元素 (1)
6 的右侧有 1 个更小的元素 (1)
1 的右侧有 0 个更小的元素
```

C++版本

```c++
// 方法一：离散化树状数组
class Solution {
private:
    vector<int> c;
    vector<int> a;

    void Init(int length) {
        c.resize(length, 0);
    }

    int LowBit(int x) {
        return x & (-x);
    }

    void Update(int pos) {
        while (pos < c.size()) {
            c[pos] += 1;
            pos += LowBit(pos);
        }
    }

    int Query(int pos) {
        int ret = 0;

        while (pos > 0) {
            ret += c[pos];
            pos -= LowBit(pos);
        }

        return ret;
    }

    void Discretization(vector<int>& nums) {
        a.assign(nums.begin(), nums.end());
        sort(a.begin(), a.end());
        a.erase(unique(a.begin(), a.end()), a.end());
    }
    
    int getId(int x) {
        return lower_bound(a.begin(), a.end(), x) - a.begin() + 1;
    }
public:
    vector<int> countSmaller(vector<int>& nums) {
        vector<int> resultList;

        Discretization(nums);

        Init(nums.size() + 5);
        
        for (int i = (int)nums.size() - 1; i >= 0; --i) {
            int id = getId(nums[i]);
            resultList.push_back(Query(id - 1));
            Update(id);
        }

        reverse(resultList.begin(), resultList.end());

        return resultList;
    }
};

// 方法二：归并排序

```

Java版本

```java
// 方法一：离散化树状数组
class Solution {
    private int[] c;
    private int[] a;

    public List<Integer> countSmaller(int[] nums) {
        List<Integer> resultList = new ArrayList<Integer>(); 
        discretization(nums);
        init(nums.length + 5);
        for (int i = nums.length - 1; i >= 0; --i) {
            int id = getId(nums[i]);
            resultList.add(query(id - 1));
            update(id);
        }
        Collections.reverse(resultList);
        return resultList;
    }

    private void init(int length) {
        c = new int[length];
        Arrays.fill(c, 0);
    }

    private int lowBit(int x) {
        return x & (-x);
    }

    private void update(int pos) {
        while (pos < c.length) {
            c[pos] += 1;
            pos += lowBit(pos);
        }
    }

    private int query(int pos) {
        int ret = 0;
        while (pos > 0) {
            ret += c[pos];
            pos -= lowBit(pos);
        }

        return ret;
    }

    private void discretization(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        for (int num : nums) {
            set.add(num);
        }
        int size = set.size();
        a = new int[size];
        int index = 0;
        for (int num : set) {
            a[index++] = num;
        }
        Arrays.sort(a);
    }

    private int getId(int x) {
        return Arrays.binarySearch(a, x) + 1;
    }
}

// 方法二：归并排序
class Solution {
    private int[] index;
    private int[] temp;
    private int[] tempIndex;
    private int[] ans;

    public List<Integer> countSmaller(int[] nums) {
        this.index = new int[nums.length];
        this.temp = new int[nums.length];
        this.tempIndex = new int[nums.length];
        this.ans = new int[nums.length];
        for (int i = 0; i < nums.length; ++i) {
            index[i] = i;
        }
        int l = 0, r = nums.length - 1;
        mergeSort(nums, l, r);
        List<Integer> list = new ArrayList<Integer>();
        for (int num : ans) {
            list.add(num);
        }
        return list;
    }

    public void mergeSort(int[] a, int l, int r) {
        if (l >= r) {
            return;
        }
        int mid = (l + r) >> 1;
        mergeSort(a, l, mid);
        mergeSort(a, mid + 1, r);
        merge(a, l, mid, r);
    }

    public void merge(int[] a, int l, int mid, int r) {
        int i = l, j = mid + 1, p = l;
        while (i <= mid && j <= r) {
            if (a[i] <= a[j]) {
                temp[p] = a[i];
                tempIndex[p] = index[i];
                ans[index[i]] += (j - mid - 1);
                ++i;
                ++p;
            } else {
                temp[p] = a[j];
                tempIndex[p] = index[j];
                ++j;
                ++p;
            }
        }
        while (i <= mid)  {
            temp[p] = a[i];
            tempIndex[p] = index[i];
            ans[index[i]] += (j - mid - 1);
            ++i;
            ++p;
        }
        while (j <= r) {
            temp[p] = a[j];
            tempIndex[p] = index[j];
            ++j;
            ++p;
        }
        for (int k = l; k <= r; ++k) {
            index[k] = tempIndex[k];
            a[k] = temp[k];
        }
    }
}
```



### [1893. 检查是否区域内所有整数都被覆盖](https://leetcode.cn/problems/check-if-all-the-integers-in-a-range-are-covered/)

简单

给你一个二维整数数组 `ranges` 和两个整数 `left` 和 `right` 。每个 `ranges[i] = [starti, endi]` 表示一个从 `starti` 到 `endi` 的 **闭区间** 。

如果闭区间 `[left, right]` 内每个整数都被 `ranges` 中 **至少一个** 区间覆盖，那么请你返回 `true` ，否则返回 `false` 。

已知区间 `ranges[i] = [starti, endi]` ，如果整数 `x` 满足 `starti <= x <= endi` ，那么我们称整数`x` 被覆盖了。

**示例 1：**

```
输入：ranges = [[1,2],[3,4],[5,6]], left = 2, right = 5
输出：true
解释：2 到 5 的每个整数都被覆盖了：
- 2 被第一个区间覆盖。
- 3 和 4 被第二个区间覆盖。
- 5 被第三个区间覆盖。
```

C++版本

```c++
// 方法一：差分数组
class Solution {
public:
    bool isCovered(vector<vector<int>>& ranges, int left, int right) {
        vector<int> diff(52, 0);   // 差分数组
        for (auto&& range: ranges) {
            ++diff[range[0]];
            --diff[range[1]+1];
        }
        // 前缀和
        int curr = 0;
        for (int i = 1; i <= 50; ++i) {
            curr += diff[i];
            if (i >= left && i <= right && curr <= 0) {
                return false;
            }
        }
        return true;
    }
};
```

Java版本

```java
// 方法一：差分数组
class Solution {
    public boolean isCovered(int[][] ranges, int left, int right) {
        int[] diff = new int[52];   // 差分数组
        for (int[] range : ranges) {
            ++diff[range[0]];
            --diff[range[1] + 1];
        }
        // 前缀和
        int curr = 0;
        for (int i = 1; i <= 50; ++i) {
            curr += diff[i];
            if (i >= left && i <= right && curr <= 0) {
                return false;
            }
        }
        return true;
    }
}
```



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

```

Java版本

```java

```



### [1450. 在既定时间做作业的学生人数](https://leetcode.cn/problems/number-of-students-doing-homework-at-a-given-time/)

简单

给你两个整数数组 `startTime`（开始时间）和 `endTime`（结束时间），并指定一个整数 `queryTime` 作为查询时间。

已知，第 `i` 名学生在 `startTime[i]` 时开始写作业并于 `endTime[i]` 时完成作业。

请返回在查询时间 `queryTime` 时正在做作业的学生人数。形式上，返回能够使 `queryTime` 处于区间 `[startTime[i], endTime[i]]`（含）的学生人数。

**示例 1：**

```
输入：startTime = [1,2,3], endTime = [3,2,7], queryTime = 4
输出：1
解释：一共有 3 名学生。
第一名学生在时间 1 开始写作业，并于时间 3 完成作业，在时间 4 没有处于做作业的状态。
第二名学生在时间 2 开始写作业，并于时间 2 完成作业，在时间 4 没有处于做作业的状态。
第三名学生在时间 3 开始写作业，预计于时间 7 完成作业，这是是唯一一名在时间 4 时正在做作业的学生。
```

C++版本

```c++
// 方法一：枚举
class Solution {
public:
    int busyStudent(vector<int>& startTime, vector<int>& endTime, int queryTime) {
        int n = startTime.size();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (startTime[i] <= queryTime && endTime[i] >= queryTime) {
                ans++;
            }
        }
        return ans;
    }
};

// 方法二：差分数组
    class Solution {
public:
    int busyStudent(vector<int>& startTime, vector<int>& endTime, int queryTime) {
        int n = startTime.size();
        int maxEndTime = *max_element(endTime.begin(), endTime.end());
        if (queryTime > maxEndTime) {
            return 0;
        }
        vector<int> cnt(maxEndTime + 2);
        for (int i = 0; i < n; i++) {
            cnt[startTime[i]]++;
            cnt[endTime[i] + 1]--;
        }
        int ans = 0;
        for (int i = 0; i <= queryTime; i++) {
            ans += cnt[i];
        }
        return ans;
    }
};

// 方法三：二分查找
    class Solution {
public: 
    int busyStudent(vector<int>& startTime, vector<int>& endTime, int queryTime) {
        sort(startTime.begin(), startTime.end());
        sort(endTime.begin(), endTime.end());
        int lessStart = upper_bound(startTime.begin(), startTime.end(), queryTime) - startTime.begin();
        int lessEnd = lower_bound(endTime.begin(), endTime.end(), queryTime) - endTime.begin();
        return lessStart - lessEnd;
    }
};
```

Java版本

```java
// 方法一：枚举
class Solution {
    public int busyStudent(int[] startTime, int[] endTime, int queryTime) {
        int n = startTime.length;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (startTime[i] <= queryTime && endTime[i] >= queryTime) {
                ans++;
            }
        }
        return ans;
    }
}

// 方法二：差分数组
class Solution {
    public int busyStudent(int[] startTime, int[] endTime, int queryTime) {
        int n = startTime.length;
        int maxEndTime = Arrays.stream(endTime).max().getAsInt();
        if (queryTime > maxEndTime) {
            return 0;
        }
        int[] cnt = new int[maxEndTime + 2];
        for (int i = 0; i < n; i++) {
            cnt[startTime[i]]++;
            cnt[endTime[i] + 1]--;
        }
        int ans = 0;
        for (int i = 0; i <= queryTime; i++) {
            ans += cnt[i];
        }
        return ans;
    }
}

// 方法三：二分查找
class Solution {
    public int busyStudent(int[] startTime, int[] endTime, int queryTime) {
        Arrays.sort(startTime);
        Arrays.sort(endTime);
        int lessStart = upperbound(startTime, 0, startTime.length - 1, queryTime);
        int lessEnd = lowerbound(endTime, 0, endTime.length - 1, queryTime);
        return lessStart - lessEnd;
    }

    public static int upperbound(int[] arr, int l, int r, int target) {
        int ans = r + 1;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (arr[mid] > target) {
                ans = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return ans;
    }

    public static int lowerbound(int[] arr, int l, int r, int target) {
        int ans = r + 1;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (arr[mid] >= target) {
                ans = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return ans;
    }
}
```



### [354. 俄罗斯套娃信封问题](https://leetcode.cn/problems/russian-doll-envelopes/)

困难

给你一个二维整数数组 `envelopes` ，其中 `envelopes[i] = [wi, hi]` ，表示第 `i` 个信封的宽度和高度。

当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算 **最多能有多少个** 信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

**注意**：不允许旋转信封。

**示例 1：**

```
输入：envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出：3
解释：最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        if (envelopes.empty()) {
            return 0;
        }
        
        int n = envelopes.size();
        sort(envelopes.begin(), envelopes.end(), [](const auto& e1, const auto& e2) {
            return e1[0] < e2[0] || (e1[0] == e2[0] && e1[1] > e2[1]);
        });

        vector<int> f(n, 1);
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (envelopes[j][1] < envelopes[i][1]) {
                    f[i] = max(f[i], f[j] + 1);
                }
            }
        }
        return *max_element(f.begin(), f.end());
    }
};

// 方法二：基于二分查找的动态规划
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        if (envelopes.empty()) {
            return 0;
        }
        
        int n = envelopes.size();
        sort(envelopes.begin(), envelopes.end(), [](const auto& e1, const auto& e2) {
            return e1[0] < e2[0] || (e1[0] == e2[0] && e1[1] > e2[1]);
        });

        vector<int> f = {envelopes[0][1]};
        for (int i = 1; i < n; ++i) {
            if (int num = envelopes[i][1]; num > f.back()) {
                f.push_back(num);
            }
            else {
                auto it = lower_bound(f.begin(), f.end(), num);
                *it = num;
            }
        }
        return f.size();
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if (envelopes.length == 0) {
            return 0;
        }
        
        int n = envelopes.length;
        Arrays.sort(envelopes, new Comparator<int[]>() {
            public int compare(int[] e1, int[] e2) {
                if (e1[0] != e2[0]) {
                    return e1[0] - e2[0];
                } else {
                    return e2[1] - e1[1];
                }
            }
        });

        int[] f = new int[n];
        Arrays.fill(f, 1);
        int ans = 1;
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (envelopes[j][1] < envelopes[i][1]) {
                    f[i] = Math.max(f[i], f[j] + 1);
                }
            }
            ans = Math.max(ans, f[i]);
        }
        return ans;
    }
}

// 方法二：基于二分查找的动态规划
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if (envelopes.length == 0) {
            return 0;
        }
        
        int n = envelopes.length;
        Arrays.sort(envelopes, new Comparator<int[]>() {
            public int compare(int[] e1, int[] e2) {
                if (e1[0] != e2[0]) {
                    return e1[0] - e2[0];
                } else {
                    return e2[1] - e1[1];
                }
            }
        });

        List<Integer> f = new ArrayList<Integer>();
        f.add(envelopes[0][1]);
        for (int i = 1; i < n; ++i) {
            int num = envelopes[i][1];
            if (num > f.get(f.size() - 1)) {
                f.add(num);
            } else {
                int index = binarySearch(f, num);
                f.set(index, num);
            }
        }
        return f.size();
    }

    public int binarySearch(List<Integer> f, int target) {
        int low = 0, high = f.size() - 1;
        while (low < high) {
            int mid = (high - low) / 2 + low;
            if (f.get(mid) < target) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}
```



### [673. 最长递增子序列的个数](https://leetcode.cn/problems/number-of-longest-increasing-subsequence/)

中等

给定一个未排序的整数数组 `nums` ， *返回最长递增子序列的个数* 。

**注意** 这个数列必须是 **严格** 递增的。

**示例 1:**

```
输入: [1,3,5,4,7]
输出: 2
解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int findNumberOfLIS(vector<int> &nums) {
        int n = nums.size(), maxLen = 0, ans = 0;
        vector<int> dp(n), cnt(n);
        for (int i = 0; i < n; ++i) {
            dp[i] = 1;
            cnt[i] = 1;
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    if (dp[j] + 1 > dp[i]) {
                        dp[i] = dp[j] + 1;
                        cnt[i] = cnt[j]; // 重置计数
                    } else if (dp[j] + 1 == dp[i]) {
                        cnt[i] += cnt[j];
                    }
                }
            }
            if (dp[i] > maxLen) {
                maxLen = dp[i];
                ans = cnt[i]; // 重置计数
            } else if (dp[i] == maxLen) {
                ans += cnt[i];
            }
        }
        return ans;
    }
};

// 方法二：贪心 + 前缀和 + 二分查找
class Solution {
    int binarySearch(int n, function<bool(int)> f) {
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r) / 2;
            if (f(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

public:
    int findNumberOfLIS(vector<int> &nums) {
        vector<vector<int>> d, cnt;
        for (int v : nums) {
            int i = binarySearch(d.size(), [&](int i) { return d[i].back() >= v; });
            int c = 1;
            if (i > 0) {
                int k = binarySearch(d[i - 1].size(), [&](int k) { return d[i - 1][k] < v; });
                c = cnt[i - 1].back() - cnt[i - 1][k];
            }
            if (i == d.size()) {
                d.push_back({v});
                cnt.push_back({0, c});
            } else {
                d[i].push_back(v);
                cnt[i].push_back(cnt[i].back() + c);
            }
        }
        return cnt.back().back();
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int n = nums.length, maxLen = 0, ans = 0;
        int[] dp = new int[n];
        int[] cnt = new int[n];
        for (int i = 0; i < n; ++i) {
            dp[i] = 1;
            cnt[i] = 1;
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    if (dp[j] + 1 > dp[i]) {
                        dp[i] = dp[j] + 1;
                        cnt[i] = cnt[j]; // 重置计数
                    } else if (dp[j] + 1 == dp[i]) {
                        cnt[i] += cnt[j];
                    }
                }
            }
            if (dp[i] > maxLen) {
                maxLen = dp[i];
                ans = cnt[i]; // 重置计数
            } else if (dp[i] == maxLen) {
                ans += cnt[i];
            }
        }
        return ans;
    }
}

// 方法二：贪心 + 前缀和 + 二分查找
class Solution {
    public int findNumberOfLIS(int[] nums) {
        List<List<Integer>> d = new ArrayList<List<Integer>>();
        List<List<Integer>> cnt = new ArrayList<List<Integer>>();
        for (int v : nums) {
            int i = binarySearch1(d.size(), d, v);
            int c = 1;
            if (i > 0) {
                int k = binarySearch2(d.get(i - 1).size(), d.get(i - 1), v);
                c = cnt.get(i - 1).get(cnt.get(i - 1).size() - 1) - cnt.get(i - 1).get(k);
            }
            if (i == d.size()) {
                List<Integer> dList = new ArrayList<Integer>();
                dList.add(v);
                d.add(dList);
                List<Integer> cntList = new ArrayList<Integer>();
                cntList.add(0);
                cntList.add(c);
                cnt.add(cntList);
            } else {
                d.get(i).add(v);
                int cntSize = cnt.get(i).size();
                cnt.get(i).add(cnt.get(i).get(cntSize - 1) + c);
            }
        }

        int size1 = cnt.size(), size2 = cnt.get(size1 - 1).size();
        return cnt.get(size1 - 1).get(size2 - 1);
    }

    public int binarySearch1(int n, List<List<Integer>> d, int target) {
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r) / 2;
            List<Integer> list = d.get(mid);
            if (list.get(list.size() - 1) >= target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    public int binarySearch2(int n, List<Integer> list, int target) {
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r) / 2;
            if (list.get(mid) < target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```



### [1310. 子数组异或查询](https://leetcode.cn/problems/xor-queries-of-a-subarray/)

中等

有一个正整数数组 `arr`，现给你一个对应的查询数组 `queries`，其中 `queries[i] = [Li, Ri]`。

对于每个查询 `i`，请你计算从 `Li` 到 `Ri` 的 **XOR** 值（即 `arr[Li] **xor** arr[Li+1] **xor** ... **xor** arr[Ri]`）作为本次查询的结果。

并返回一个包含给定查询 `queries` 所有结果的数组。

**示例 1：**

```
输入：arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]
输出：[2,7,14,8] 
解释：
数组中元素的二进制表示形式是：
1 = 0001 
3 = 0011 
4 = 0100 
8 = 1000 
查询的 XOR 值为：
[0,1] = 1 xor 3 = 2 
[1,2] = 3 xor 4 = 7 
[0,3] = 1 xor 3 xor 4 xor 8 = 14 
[3,3] = 8
```

 C++版本

```c++
// 方法一：前缀异或
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
        int n = arr.size();
        vector<int> xors(n + 1);
        for (int i = 0; i < n; i++) {
            xors[i + 1] = xors[i] ^ arr[i];
        }
        int m = queries.size();
        vector<int> ans(m);
        for (int i = 0; i < m; i++) {
            ans[i] = xors[queries[i][0]] ^ xors[queries[i][1] + 1];
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：前缀异或
class Solution {
    public int[] xorQueries(int[] arr, int[][] queries) {
        int n = arr.length;
        int[] xors = new int[n + 1];
        for (int i = 0; i < n; i++) {
            xors[i + 1] = xors[i] ^ arr[i];
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; i++) {
            ans[i] = xors[queries[i][0]] ^ xors[queries[i][1] + 1];
        }
        return ans;
    }
}
```







