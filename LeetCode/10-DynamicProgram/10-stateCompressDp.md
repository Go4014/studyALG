# 动态规划

## 状态压缩DP题目

### [1879. 两个数组最小的异或值之和](https://leetcode.cn/problems/minimum-xor-sum-of-two-arrays/)

困难

给你两个整数数组 `nums1` 和 `nums2` ，它们长度都为 `n` 。

两个数组的 **异或值之和** 为 `(nums1[0] XOR nums2[0]) + (nums1[1] XOR nums2[1]) + ... + (nums1[n - 1] XOR nums2[n - 1])` （**下标从 0 开始**）。

- 比方说，`[1,2,3]` 和 `[3,2,1]` 的 **异或值之和** 等于 `(1 XOR 3) + (2 XOR 2) + (3 XOR 1) = 2 + 0 + 2 = 4` 。

请你将 `nums2` 中的元素重新排列，使得 **异或值之和** **最小** 。

请你返回重新排列之后的 **异或值之和** 。

**示例 1：**

```
输入：nums1 = [1,2], nums2 = [2,3]
输出：2
解释：将 nums2 重新排列得到 [3,2] 。
异或值之和为 (1 XOR 3) + (2 XOR 2) = 2 + 0 = 2 。
```

C++版本

```c++
// 方法一：状态压缩动态规划
class Solution {
public:
    int minimumXORSum(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        vector<int> f(1 << n, INT_MAX);
        f[0] = 0;
        for (int mask = 1; mask < (1 << n); ++mask) {
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {
                    f[mask] = min(f[mask], f[mask ^ (1 << i)] + (nums1[__builtin_popcount(mask) - 1] ^ nums2[i]));
                }
            }
        }
        return f[(1 << n) - 1];
    }
};
```

Java版本

```java
// 方法一：状态压缩动态规划
class Solution {
    public int minimumXORSum(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int[] f = new int[1 << n];
        Arrays.fill(f, Integer.MAX_VALUE);
        f[0] = 0;

        for (int mask = 1; mask < (1 << n); ++mask) {
            int count = Integer.bitCount(mask);
            for (int i = 0; i < n; ++i) {
                if ((mask & (1 << i)) != 0) {
                    f[mask] = Math.min(f[mask], f[mask ^ (1 << i)] + (nums1[count - 1] ^ nums2[i]));
                }
            }
        }
        
        return f[(1 << n) - 1];
    }
}
```



### [2172. 数组的最大与和](https://leetcode.cn/problems/maximum-and-sum-of-array/)

困难

给你一个长度为 `n` 的整数数组 `nums` 和一个整数 `numSlots` ，满足`2 * numSlots >= n` 。总共有 `numSlots` 个篮子，编号为 `1` 到 `numSlots` 。

你需要把所有 `n` 个整数分到这些篮子中，且每个篮子 **至多** 有 2 个整数。一种分配方案的 **与和** 定义为每个数与它所在篮子编号的 **按位与运算** 结果之和。

- 比方说，将数字 `[1, 3]` 放入篮子 ***`1`\*** 中，`[4, 6]` 放入篮子 ***`2`\*** 中，这个方案的与和为 `(1 AND ***1\***) + (3 AND ***1\***) + (4 AND ***2***) + (6 AND ***2***) = 1 + 1 + 0 + 2 = 4` 。

请你返回将 `nums` 中所有数放入 `numSlots` 个篮子中的最大与和。

**示例 1：**

```
输入：nums = [1,2,3,4,5,6], numSlots = 3
输出：9
解释：一个可行的方案是 [1, 4] 放入篮子 1 中，[2, 6] 放入篮子 2 中，[3, 5] 放入篮子 3 中。
最大与和为 (1 AND 1) + (4 AND 1) + (2 AND 2) + (6 AND 2) + (3 AND 3) + (5 AND 3) = 1 + 0 + 2 + 2 + 3 + 1 = 9 。
```

C++版本

```c++
// 方法一：三进制状态压缩动态规划
class Solution {
public:
    int maximumANDSum(vector<int>& nums, int numSlots) {
        int n = nums.size();
        int mask_max = 1;
        for (int i = 0; i < numSlots; ++i) {
            mask_max *= 3;
        }
        
        vector<int> f(mask_max);
        for (int mask = 1; mask < mask_max; ++mask) {
            int cnt = 0;
            for (int i = 0, dummy = mask; i < numSlots; ++i, dummy /= 3) {
                cnt += dummy % 3;
            }
            if (cnt > n) {
                continue;
            }
            for (int i = 0, dummy = mask, w = 1; i < numSlots; ++i, dummy /= 3, w *= 3) {
                int has = dummy % 3;
                if (has) {
                    f[mask] = max(f[mask], f[mask - w] + (nums[cnt - 1] & (i + 1)));
                }
            }
        }
        
        return *max_element(f.begin(), f.end());
    }
};
```

Java版本

```java
// 方法一：三进制状态压缩动态规划
class Solution {
    public int maximumANDSum(int[] nums, int numSlots) {
        int n = nums.length;
        int maskMax = 1;
        for (int i = 0; i < numSlots; ++i) {
            maskMax *= 3;
        }
        
        int[] f = new int[maskMax];
        
        for (int mask = 1; mask < maskMax; ++mask) {
            int cnt = 0;
            for (int i = 0, dummy = mask; i < numSlots; ++i, dummy /= 3) {
                cnt += dummy % 3;
            }
            if (cnt > n) {
                continue;
            }
            for (int i = 0, dummy = mask, w = 1; i < numSlots; ++i, dummy /= 3, w *= 3) {
                int has = dummy % 3;
                if (has != 0) {
                    f[mask] = Math.max(f[mask], f[mask - w] + (nums[cnt - 1] & (i + 1)));
                }
            }
        }
        
        return Arrays.stream(f).max().getAsInt();
    }
}
```

