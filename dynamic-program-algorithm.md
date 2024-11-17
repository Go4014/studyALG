# 动态规划

判断一个问题是否适合使用动态规划的方法来解决通常可以通过以下几个特征来确定：

1. **最优子结构（Optimal Substructure）**：问题的最优解可以通过其子问题的最优解来构造。
2. **重叠子问题（Overlapping Subproblems）**：问题可以被分解为具有重叠子问题的子问题。
3. **状态转移方程（State Transition）**：问题可以被划分为若干个阶段，每个阶段有多种状态，而当前阶段的状态可以通过之前阶段的状态转移而来。
4. **无后效性（No Overlapping）**：问题的当前状态只与之前的状态相关，与之后的状态无关。





## 背包问题

### 01背包问题





### 完全背包问题

#### 达到某限定值

问题：从给定数组中选择一些元素，计算并返回可以达到某限定值的组合数，并且每个数字可以使用次数无限制。（组合数 < 2^32^）

顺序问题：对于达到限定值的集合，若顺序不同是否考虑视为不同答案？

- 考虑顺序：相同集合但不同顺序的数组视为不同答案
- 不考虑顺序：相同集合但不同顺序的数组视为相同答案

##### 考虑顺序

```java

class Solution {
    public int noOrder(int[] nums, int limit) {
        int[] dp = new int[limit + 1];
        dp[0] = 1;
        for (int i = 1; i <= limit; i++) {
            for (int num : nums) {
                if (num <= i) {
                    dp[i] += dp[i - num];
                }
            }
        }
        return dp[limit];
    }
}
```

##### 不考虑顺序

```java
class Solution {
    public int order(int limit, int[] nums) {
        int[] dp = new int[limit + 1];
        dp[0] = 1;
        for (int num : nums) {
            for (int i = num; i <= limit; i++) {
                dp[i] += dp[i - num];
            }
        }
        return dp[limit];
    }
}
```



