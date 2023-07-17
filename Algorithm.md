# 一、分治算法

## 1.1. 分治法基本思想

分治法的基本思想是将一个规模为 n 的问题分解为 k 个规模较小的子问题，这些子 问题互相独立且与原问题相同。递归地求解这些子问题，然后将子问题的解合并（构造） 出原问题的解。

> 分治算法的基本思想：将一个复杂的问题划分为更小的子问题，通过解决子问题来解决原始问题。每个子问题的解决方案是独立的，然后将子问题的解合并起来得到原始问题的解。
>
> - 将问题分解为较小的子问题。
> - 解决子问题并合并子问题的解来解决原始问题。
> - 适用于问题可以被划分为相互独立的子问题，并且可以将子问题的解合并为原问题的解。

## 1.2. 分支算法设计步骤

> **分治算法的设计过程分为三个阶段：**
>
> - 分解（Divide）阶段：将整个问题划分为多个子问题
>
> - 递归求解（Conquer）阶段：求解每个子问题（递归调用正在设计的算法）
>
> - 合并（Combine）阶段：合并子问题的解，形成原始问题的解



## 1.3. 分治算法设计模式

分治法一般算法设计模式如下：

```c++
Divide-and-Conquer(int n){
    //N为问题规模 ,N0为可解子问题的规模
	if(N <= N0){
		解子问题;
		return(子问题的解);
	}
    //分解成较小的子问题p1,p2,...,pk
	for(int i = 1; i <= k; i++){
        //递归解决
		yi = Divide-and-Conquer(|Pi|);
	}
    //合并子问题
	T = MERGE(y1,y2,...yk);
    //返回问题的解
	return(T);
}
```

## 1.4. 例题：

### 1.4.1.  最大子段和问题

```c++
#include<iostream>
using namespace std;

/**
 * 分治算法求最大子段和
 * @param arr
 * @param left
 * @param right
 * @param lIndex
 * @param rIndex
 * @return
 */
int MaxSubSum(int arr[], int left, int right, int &lIndex, int &rIndex) {
    int sum;
    if (left == right) {
        sum = arr[left] > 0 ? arr[left] : 0;
        lIndex = left;
        rIndex = right;
    } else {
        int center = (left + right) / 2;
        int leftSum = MaxSubSum(arr, left, center, lIndex, rIndex);
        int lslIndex = lIndex, lsrIndex = rIndex;
        
        int rightSum = MaxSubSum(arr, center + 1, right, lIndex, rIndex);
        int rslIndex = lIndex, rsrIndex = rIndex;
        
        int lSum = 0, lefts = 0, li = center;
        for (int i = center; i >= left; i--) {
            lefts += arr[i];
            if (lefts > lSum) {
                lSum = lefts;
                li = i;
            }
        }
        int rSum = 0, rights = 0, ri = center;
        for (int i = center + 1; i <= right; i++) {
            rights += arr[i];
            if (rights > rSum) {
                rSum = rights;
                ri = i;
            }
        }
        sum = lSum + rSum;
        lIndex = li;
        rIndex = ri;
        if (sum < leftSum) {
            sum = leftSum;
            lIndex = lslIndex;
            rIndex = lsrIndex;
        }
        if (sum < rightSum) {
            sum = rightSum;
            lIndex = rslIndex;
            rIndex = rsrIndex;
        }
    }
    return sum;
}

/**
12
23 21 -16 14 -43 27 -7 23 -24 21 -34 31
*/

int main() {
    int length = 0;
    cout << "输入数组长度：";
    cin >> length;
    int arr[length];
    cout << "输入数组元素值：";
    for (int i = 0; i < length; ++i) {
        cin >> arr[i];
    }
    int lIndex = 0, rIndex = 0;
    int sum = MaxSubSum(arr, 0, length - 1, lIndex, rIndex);
    cout << "最大子段和：" << sum << "\n起始下标：" << lIndex << "\n结束下标：" << rIndex << endl;
    system("pause");
    return 0;
}
```

### 1.4.2. 众数及重数问题

```c++
#include <iostream>

using namespace std;

/**
 * 分裂函数
 * @param arr
 * @param length
 * @param left
 * @param right
 */
void split(const int arr[], int length, int &left, int &right) {
    int mid = (left + right) / 2;
    for (left = 0; left <= mid; left++) {
        if (arr[left] == arr[mid]) {
            break;
        }
    }
    for (right = left + 1; right < length; right++) {
        if (arr[right] != arr[mid]) {
            break;
        }
    }
}

/**
 * 分治算法求众数及重数
 * @param mode
 * @param sum
 * @param arr
 * @param length
 */
void Mode(int &mode, int &sum, int arr[], int length) {
    int left, right;
    split(arr, length, left, right);
    int mid = length / 2;
    int cnt = right - left;
    if (cnt > sum) {
        sum = cnt;
        mode = arr[mid];
    }
    //取众数最小值
    if (cnt == sum && mode > arr[mid]) {
        mode = arr[mid];
    }

    if (left >= sum) {
        Mode(mode, sum, arr, left);
    }
    if (length - right > sum) {
        Mode(mode, sum, arr + right, length - right);
    }
}

int main() {
    int mode = 0, sum = 0, length;
    cout << "输入数组长度：";
    cin >> length;
    int arr[length];
    cout << "输入数组元素值：";
    for (int i = 0; i < length; ++i) {
        cin >> arr[i];
    }
    Mode(mode, sum, arr, length);
    cout << "众数：" << mode << ", 重数：" << sum << endl;
    system("pause");
    return 0;
}
```



# 二、动态规划

## 2.1. 动态规划的基本思想

动态规划算法的基本思想是将一个大问题分解为多个子问题，并通过解决子问题来求解原始问题。它采用自底向上的递推方式，将问题划分为重叠子问题，并利用子问题的解来构建原始问题的解。

> 动态规划算法的基本思想：将问题划分为一系列重叠的子问题，通过求解子问题的最优解来构建原问题的最优解。动态规划使用记忆化技术来存储子问题的解，避免重复计算。
>
> - 将问题分解为重叠子问题。
> - 使用递归或迭代的方式求解子问题，并将结果保存起来。
> - 适用于问题具有最优子结构和重叠子问题性质的情况。

## 2.2. 动态规划的性质

动态规划法通常用于求一个问题在某种意义下的最优解

> **采用动态规划方法的优化问题必须具备**
>
> - 最优子结构性质
>
> - 子问题重叠性质

适合采用动态规划方法的优化问题必须具备最优子结构性质和子问题重叠性质：

> - 当一个问题的优化解包含了子问题的优化解时，则称该问题具有优化子结构性质
> - 在求解一个问题的过程中，很多子问题的解被多次调用，则称该问题具有子问题的重叠性质



## 2.3. 动态规划算法步骤

### 2.3.1. 设计一个动态规划算法，通常可按以下几个步骤进行：

> - ①定义子问题（用参数表达子问题的边界）
> - 分析最优解的性质，并刻画其结构特征
> - ⑶定义最优解的代价(每个最优解都有一个值称最优值,也称代价最大值或最小值;目标函数、优化函数)
> - ⑷列出关于优化函数的递推式和边界条件（即递推式的初值）
> - ⑸根据递推式分解子问题，直到不可分为止
> - ⑹自底向上计算最优值（迭代实现）以备忘录方式（表格）存储，并记录构造最优解所需的信息
> - ⑺根据计算最优值时得到的信息构造一个最优解



### 2.3.2. 判断题意是否符合动态规划的步骤

> 在解题时，通常可以通过以下几个步骤来判断一个问题是否可以使用动态规划算法：
>
> 1. 确定问题具有最优子结构性质：即问题的最优解包含了其子问题的最优解。
> 2. 确定问题具有重叠子问题性质：即问题的子问题会重复出现。
> 3. 定义状态：确定状态变量和状态表示方法，以及状态之间的转移关系。
> 4. 确定状态转移方程：通过状态之间的转移关系，求解出每个状态的值。
> 5. 求解最终问题的解：最终问题的解通常是由状态变量中的某些状态计算得出的。



### 2.3.3.  具体来说，动态规划算法一般分为以下几个步骤：

> 1. 定义状态：确定状态变量和状态表示方法，以及状态之间的转移关系。
> 2. 初始化状态：给出问题的最小子问题的解。
> 3. 确定状态转移方程：通过状态之间的转移关系，求解出每个状态的值。
> 4. 求解最终问题的解：最终问题的解通常是由状态变量中的某些状态计算得出的。
> 5. 可选：优化空间复杂度。
>



**求解模板：**

```c++
int dp[N][M]; // dp数组，N和M是状态的范围

// 初始化
dp[0][0] = 初始值;

for(int i = 1; i < N; i++) {
    for(int j = 1; j < M; j++) {
        // 状态转移方程
        dp[i][j] = max(dp[i-1][j], dp[i][j-1]) + A[i][j]; // 或其他操作
    }
}

// 返回结果
return dp[N-1][M-1]; // 或其他操作
```



**典型的动态规划算法解题模板**

```c++
// 动态规划算法解题模板
int dynamicProgramming(vector<int>& nums) {
    int n = nums.size();
    
    // 定义并初始化dp数组
    vector<int> dp(n, 0);
    
    // 确定初始状态
    dp[0] = nums[0];
    
    // 递推计算dp数组的值
    for (int i = 1; i < n; i++) {
        // 根据状态转移方程计算dp[i]的值
        dp[i] = max(dp[i-1] + nums[i], nums[i]);
    }
    
    // 找到最优解
    int maxSum = INT_MIN;
    for (int i = 0; i < n; i++) {
        maxSum = max(maxSum, dp[i]);
    }
    
    return maxSum;  // 返回最终结果
}
```

该模板包括以下几个步骤：

> 1. 定义并初始化dp数组：根据问题的要求，定义一个dp数组来存储状态的值，初始化数组的初始值。
> 2. 确定初始状态：根据问题的要求，确定初始状态的值。
> 3. 递推计算dp数组的值：通过状态转移方程，从初始状态开始逐步计算出dp数组中每个状态的值，通常使用循环迭代的方式。
> 4. 找到最优解：根据问题的要求，从dp数组中找到最优解。
> 5. 返回最终结果：根据问题的要求，返回最终结果。

需要根据具体问题进行适当的调整和修改，包括定义状态、确定状态转移方程和最优解的计算方式。



## 2.4. 动态规划的解题方法

> **两种求解的方式**
>
> - 自顶向下的备忘录法、
>
> - 自底向上



## 2.5. 例题

### 2.5.1. 最长公共子序列问题

```c++
#include <iostream>
#include<bits/stdc++.h>
using namespace std;

/**
 * 动态规划求最长公共子序列
 * @param aStr
 * @param bStr
 * @return
 */
char* MaxComSub(string &aStr, string &bStr) {
    int m = aStr.length();
    int n = bStr.length();
    int dp[m+1][n+1];
    memset(dp, 0, sizeof(dp));
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (aStr[i-1] == bStr[j-1]) {
                //当两个字符串右元素相等时，则其左上数组值加一
                dp[i][j] = dp[i-1][j-1] + 1;
            } else {
                //当两个字符串右元素不相等时，则其选择左数组值或上数组值中的最大值
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }

    int index = dp[m][n];
    char* lcs = new char[index+1];
    lcs[index] = '\0';
    int i = m, j = n;
    while (i > 0 && j > 0) {
        if (aStr[i-1] == bStr[j-1]) {
            //当两个字符串右元素相等时，则其数组行列下标同时减一，往左上回溯
            lcs[index-1] = aStr[i-1];
            i--;
            j--;
            index--;
        } else if (dp[i-1][j] > dp[i][j-1]) {
            //当两个字符串右元素不相等时，并且其上数组值较大时，往上回溯
            i--;
        } else {
            //当两个字符串右元素不相等时，并且其左数组值较大时，往左回溯
            j--;
        }
    }
    return lcs;
}

int main() {
    string aStr, bStr;
    cout << "输入字符序列aStr：";
    cin >> aStr;
    cout << "输入字符序列bStr：";
    cin >> bStr;

    char* mcs = MaxComSub(aStr, bStr);

    cout << "aStr(" << aStr << ") & " << "bStr(" << bStr << ") => 最长公共子序列为：" << mcs << endl;

    system("pause");
    return 0;
}
```



### 2.5.2. 01背包问题

```c++
#include <iostream>
#include <vector>
#include <map>
#include <algorithm>

using namespace std;

struct Item {
    Item(int weight, int value) {
        this->weight = weight;
        this->value = value;
    }

    Item(Item *pItem) {
    }

    int weight;
    int value;
};

/**
 * 动态规划版
 * @param weights
 * @param values
 * @param capacity
 * @return
 */
int knapsack01(vector<Item> &items, int capacity) {
    int n = items.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1));
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= capacity; j++) {
            if (j >= items[i - 1].weight) {
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - items[i - 1].weight] + items[i - 1].value);
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    return dp[n][capacity];
}

void printMap(map<int, int> &m) {
    for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
        cout << "(" << it->first << ", " << it->second << "%)" << endl;
    }
    cout << endl;
}

int main() {
    int num;
    int capacity;
    vector<Item> items;

    cout << "物品个数:";
    cin >> num;
    cout << "背包容量：";
    cin >> capacity;
    for (int i = 0; i < num; ++i) {
        int weight, value;
        cin >> weight >> value;
        items.emplace_back(weight, value);
    }

    cout << "(动态规划)背包最大价值：" << knapsack01(items, capacity) << endl;
    return 0;
}
```



### 2.5.3. 矩阵连乘

```c++

```



# 三、贪心算法

贪心算法思想：贪心算法是作出在当前看来最好的选择，也就是说贪心算法并不从整体上最优考虑，它所作出的选择只是在某种意义上的局部最优选择



## 3.1. 贪心算法的基本要素

> ***贪心算法的基本要素***
>
> - **贪心选择性质**：所求问题的整体最优解可以通过一系列局部最优的选择，即贪心选择来达到
>
> - 当一个问题的最优解包含其子问题的最优解时，称此问题具有最优子结构性质
>
> - 问题的最优子结构性质是该问题可用动态规划算法或贪心算法求解的关键特征



> **贪心算法的基本思想**：每一步都选择当前情况下的最优解，即局部最优解，而不考虑全局最优解。贪心算法通过贪心选择策略来逐步构建解，每次选择局部最优解，希望最终得到全局最优解。
>
> - 在每一步选择中，选择当前情况下的最优解。
> - 不考虑全局最优解，而是根据局部最优解的选择策略。
> - 适用于问题具有贪心选择性质，即每一步的最优选择不依赖于其他步骤的选择。

## 3.2. 贪心算法的性质

适合采用贪心法的优化问题必须具备最优子结构性质和贪心(Greedy)选择性：

> - 当一个问题的优化解包含了子问题的优化解时，则称该问题具有优化子结构性质
> - 若一个优化问题的全局优化解可以通过局部优化选择得到，则该问题称为具有 Greedy选择性质



## 3.3. 证明贪心算法的准确性

贪心算法正确性证明方法如下：

> - ①证明算法所求解的问题具有优化子结构
> - ②证明算法所求解的问题具有Greedy选择性
> - ③算法是按②中的Greedy选择性进行局部优化选择的



## 3.4. 解题模板

**典型的贪心算法解题模板**

```c++
// 贪心算法解题模板
int greedyAlgorithm(vector<int>& nums) {
    // 根据问题要求，定义变量或数据结构来存储解的信息
    
    // 对输入数据进行排序或预处理（如果需要）
    
    // 根据贪心策略，选择当前局部最优解，并更新解的信息
    
    // 检查解是否满足问题要求，如果满足则返回解，否则继续迭代
    
    // 返回最终解（如果有需要）
}
```

该模板包括以下几个关键步骤：

> 1. 定义变量或数据结构：根据问题的要求，定义变量或数据结构来存储解的信息，例如最终解、当前解、最优解等。
> 2. 排序或预处理：根据问题的特点，对输入数据进行排序或预处理，以便进行贪心选择。
> 3. 贪心策略：根据问题的要求，选择当前局部最优解，并更新解的信息。贪心算法的核心在于选择局部最优解，并相信这些选择最终会导致全局最优解。
> 4. 检查解的有效性：在每次选择局部最优解后，检查解是否满足问题的要求。如果满足，则返回当前解作为最终解；否则，继续迭代选择下一个局部最优解。
> 5. 返回最终解（如果有需要）：根据问题的要求，返回最终得到的解。

需要根据具体问题进行适当的调整和修改，包括选择贪心策略、确定解的数据结构和有效性检查条件。



## 3.5. 例题

### 3.5.1. 活动安排问题

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;
/**
 * 活动安排问题-贪心选择
 * @param start_times
 * @param end_times
 * @return
 */
vector<pair<int, int>> activity_selection(vector<int> &start_times, vector<int> &end_times) {
    int n = start_times.size();
    vector<pair<int, int>> activities;
    activities.reserve(n);
    for (int i = 0; i < n; i++) {
        activities.emplace_back(start_times[i], end_times[i]);
    }

    //结束时间从小到达排序（贪心开始）
    sort(activities.begin(), activities.end(), [](pair<int, int> a, pair<int, int> b) {
        return a.second < b.second;
    });

    vector<pair<int, int>> selected_activities;
    int current_end_time = 0;
    for (auto activity: activities) {
        // 下一活动的起始时间必须比前一活动的结束时间晚 （时间冲突问题）
        if (activity.first >= current_end_time) {
            selected_activities.push_back(activity);
            current_end_time = activity.second;
        }
    }
    return selected_activities;
}

void printActivity(vector<int> start_times, vector<int> end_times, const vector<pair<int, int>>& selected_activities) {
    for (auto activity: selected_activities) {
        int count;
        for (int i = 0; i < start_times.size(); ++i) {
            if (start_times[i] == activity.first && end_times[i] == activity.second) {
                count = i;
            }
        }
        cout << "[" << count << ", " << "(" << activity.first << ", " << activity.second << ")" << "]" << endl;
    }
}

/*
1 2
3 4
0 6
5 7
8 9
5 9
*/
int main() {
    int num;
    vector<int> start_times;
    vector<int> end_times;

    cout << "活动个数：";
    cin >> num;

    cout << "开始时间-结束时间：" << endl;
    for (int i = 0; i < num; ++i) {
        int startTime;
        cin >> startTime;
        start_times.push_back(startTime);

        int endTime;
        cin >> endTime;
        end_times.push_back(endTime);
    }
    vector<pair<int, int>> selected_activities = activity_selection(start_times, end_times);

    printActivity(start_times, end_times, selected_activities);

    system("pause");
    return 0;
}
```



### 3.5.2. 01背包问题

```c++
#include <iostream>
#include <vector>
#include <map>
#include <algorithm>

using namespace std;

struct Item {
    Item(int weight, int value) {
        this->weight = weight;
        this->value = value;
    }

    Item(Item *pItem) {
    }

    int weight;
    int value;
};

bool compareItems(Item a, Item b) {
    return a.value / a.weight > b.value / b.weight;
}

/**
 * 贪心算法版
 * @param items
 * @param capacity
 * @return
 */
int knapsack(vector<Item> &items, int capacity, map<int, int> &ratio) {
    sort(items.begin(), items.end(), compareItems);
    int n = items.size();
    int totalValue = 0;
    for (int i = 0; i < n; i++) {
        if (capacity >= items[i].weight) {
            totalValue += items[i].value;
            capacity -= items[i].weight;
            ratio.insert(pair<int, int>(i, 100));
        } else {
            totalValue += capacity * (items[i].value / items[i].weight);
            ratio.insert(pair<int, int>(i, (double) capacity / items[i].weight * 100));
            break;
        }
    }
    return totalValue;
}

void printMap(map<int, int> &m) {
    for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
        cout << "(" << it->first << ", " << it->second << "%)" << endl;
    }
    cout << endl;
}

int main() {
    int num;
    int capacity;
    vector<Item> items;

    cout << "物品个数:";
    cin >> num;
    cout << "背包容量：";
    cin >> capacity;
    for (int i = 0; i < num; ++i) {
        int weight, value;
        cin >> weight >> value;
        items.emplace_back(weight, value);
    }

   /* items = {
            {10, 60},
            {20, 100},
            {30, 120},
            {40, 160}
    };
    capacity = 50;*/

    map<int, int> ratio;
    cout << "(贪心)总价值: " << knapsack(items, capacity, ratio) << endl;
    printMap(ratio);
    return 0;
}
```



# 四、回溯算法

## 4.1. 基本思想

**回溯算法在包含问题所有解的解空间树中，按照深度优先的策略，从根出发进行搜索**

> - 搜索每到达解空间树的一个结点，总是先判断以该结点为根的子树（或结点）是否肯定不包含问题的解
> - 如果肯定不包含，则跳过对该子树的系统搜索，并一层一层地向它的祖先回溯， 直到遇上一个还有未被搜索过的儿子的结点，才转向该结点的一个未曾搜索过的儿子结点，继续搜索
> - 否则，进入该子树，继续按深度优先的策略进行搜索，直到搜索到叶子节点



> 回溯算法的基本思想：通过尝试所有可能的解，并回溯到之前的状态来寻找问题的解。回溯算法通常使用递归的方式，在搜索过程中使用剪枝策略来避免不必要的搜索，以提高效率。
>
> - 通过尝试所有可能的解，并回溯到之前的状态来寻找问题的解。
> - 在搜索过程中，使用剪枝策略来避免不必要的搜索。
> - 适用于问题的解空间非常大，需要搜索所有可能解的情况。

## 4.2. 回溯算法“解”的本质

> - 通过回溯算法解决问题的多个解称为解向量空间（简称解空间，即解决一个问题的所有可能的决策序列构成该问题的解空间）
> - 问题的解空间应至少包含问题的一个解。 解空间中满足约束条件的决策序列称为可行解。
> - 一般来说，解任何问题都有一个目 标，在约束条件下使目标达到最优的可行解称为该问题的最优解。



## 4.3. 回溯算法的思路

### 4.3.1 问题的解空间树

通常把解空间组织成树型结构称为解空间树或状态空间，树中的每一个结点确定所求解问题的一个问题状态。



### 4.3.2. 搜索解空间树

> 1. 回溯法的基本方法
>    - 确定了解空间的组织结构后，回溯法就从开始结点（根结点）出发，以深度优先的 方式搜索整个解空间。
>    - 首先开始结点（根结点）就成为一个活结点（活结点是指自身已生成但其孩子结点 没有全部生成的结点），同时也成为当前的扩展结点（扩展结点是指正在产生孩子结点 的结点）
>    - 在当前的扩展结点处，搜索向纵深方向移至一个新结点。这个新结点就成为一个新 的活结点，并成为当前的扩展结点。
>    - 如果当前的扩展结点处不能再向纵深方向移动，则当前的扩展结点就成为死结点 （死结点是指由根结点到该结点构成的部分解不满足约束条件，或者其子结点已经搜索 完毕）
>    - 此时应往回移动（回溯）至最近的一个活结点处，并使这个活结点成为当前的扩展结点
>    - 回溯法即以这种工作方式递归地在解空间中搜索，直至找到所要求的解或解空间中 已无活结点时为止
> 2. 回溯法搜索解空间时，通常采用两种策略避免无效搜索，提高回溯的搜索效率:（统称为剪枝函数）
>    - ①用约束函数在扩展结点处剪除不满足约束的子树
>    - ②用限界函数剪去得不到问题解或最优解的子树
> 3. 用回溯法解题的一般步骤
>    - ① 针对所给的问题，定义搜索问题的解向量(x1, x2, … , xn)和每个分量 xi 的取值范 围，确定问题的解空间
>    - ② 确定易于搜索的解空间结构—解空间树
>    - ③ 确定结点的扩展搜索规则
>    - ④ 以深度优先的方式搜索解空间，并在搜索过程中用剪枝函数（约束函数或限界函 数）避免无效搜索



## 4.4. 回溯法算法的分析

### 4.4.1. 回溯算法的空间分析

**回溯法解题的一个显著特征是：问题的解空间是在搜索过程中动态生成的，并不 需要在算法运行时构造一棵真正的树结构，然后再在该解空间树中搜索问题的解。在任何时刻，算法只保存从根结点到当前扩展结点的路径。**

- 若解空间树中从根到叶的最长路 径长度为 h(n)，则回溯法所需的存储空间通常为 O(h(n))
- 如果显式存储整个解空间则需要 O(2h(n))或 O(h(n)!)的空间

### 4.4.2. 回溯算法的时间分析 

**通常以解空间树中的结点数作为回溯算法时间复杂度的分析依据，假设解空间树共有 n 层**

- 第 1 层有 m0 个满足约束条件的结点，每个结点有 m1 个满足约束条件的子结点
- 第 2 层有 m0m1 个满足约束条件的结点，同理，第 3 层有 m0m1m2 个满足约束条件的 结点 
- 第 n 层有 m0m1…mn-1 个满足约束条件的结点

`采用回溯法求所有解的算法的执行时间为` 
$$
m_0+m_0m_1+m_0m_1m_2+...+m_0m_1m_2...m_{n-1}
$$


## 4.5. “子集树”和“排列树”解题模板

### 4.5.1. 子集树

当所给的问题是从 n 个元素的集合 S 中找出满足某种性质的子集时，相应的解空 间树称为子集树。

n 个物品的 0-1 背包问题相应的解空间树是一棵子集树。子集树通常有 2^n 个叶结点其结点总个数为2 ^n+1 -1。遍历子集树的任何算法均需 Ω(2n )的计算时间

回溯法搜索子集树，递归算法的一般形式（框架）可描述如下： 

```c++
int x[n]; 										//存放解向量，全局变量
void Backtrack(int t) {							//子集树的递归回溯框架
    if (t > n) {								//已搜索到叶子结点，输出一个可行解
		Output(x);
    } else {
        for (int i=0; i<=1; i++) { 				//用 i 枚举 t 所有可能的路径
			x[t]=i; 							//生成一个可能的解分量
 			if (Constraint(t) && Bound(t)) {	//满足约束条件和限界函数,继续下一层
				Backtrack(t+1);
            }
        }
 	}
}
```

其中，形式参数 t 表示递归深度，即当前扩展结点在解空间树中的深度。

n 用来控 制递归深度，即解空间树的高度。当 t>n 时，算法已搜索到一个叶结点。此时，由函数 Output(x)对得到的可行解 x 进行记录或输出处理。

函数 Constraint(t)和 Bound(t)表示在当 前扩展结点处的约束函数和限界函数。

函数 Constraint(t)返回的值为 true 则表示在当前 扩展结点处 x[1:t]的取值满足问题的约束条件，否则不满足问题的约束条件，可剪去相 6 应的子树。

函数 Bound(t) 返回的值为 true 则表示在当前扩展结点处 x[1:t]的取值尚未使 目标函数越界，否则，在当前扩展结点处 x[1:t]的取值已使目标函数越界，可剪去相应 的子树。

### 4.5.2. 排列树

当所给的问题是确定 n 个元素的满足某种性质的排列时，相应的解空间树称为排列树。

旅行售货员问题相应的解空间树就是一棵排列树。排列树通常有(n-1)！（或 n!）个叶结点。遍历排列树的任何算法均需 Ω(n!)的计算时间。

用回溯法搜索排列树，递归算法的一般形式（框架）可描述如下：

```c++
int x[n]; 										//x 存放解向量，并初始化为单位排列(1, 2, …, n)
void Backtrack(int t) {							//排列树的递归回溯框架
 	if (t > n) {								//搜索到叶子结点,输出一个可行解
		Output(x);
    } else {
		for (int i=t; i<=n; i++) {				//用 i 枚举 t 所有可能的路径
			Swap(x[t],x[i]); 					//为保证排列中每个元素不同，通过交换来实现
 			if (Constraint(t) && Bound(t)) {	//满足约束条件和限界函数，进入下一层
				Backtrack(t +1);
            }
 			Swap(x[t],x[i]); 					//恢复状态
		}
    }
}
```





## 4.6. 例题

### 4.6.1. 子集和匹配问题

```c++
#include<iostream>

using namespace std;

/**
 * 限定约束函数：当前子集和是否大于”限定数“
 * @param num
 * @param gather
 * @param subset
 * @param subsetSum
 * @return
 */
bool boundSum(int num, const int gather[], int subset[], int subsetSum) {
    int sum = 0;
    for (int i = 0; i < num; ++i) {
        if (subset[i] == 1) {
            sum += gather[i];
            if (sum > subsetSum) {
                return false;
            }
        }
    }
    return true;
}

/**
 * 当前子集和数
 * @param num
 * @param gather
 * @param subset
 * @param subsetSum
 * @return
 */
bool currentSum(int num, const int gather[], int subset[], int subsetSum) {
    int sum = 0;
    for (int i = 0; i < num; ++i) {
        if (subset[i] == 1) {
            sum += gather[i];
        }
    }
    return sum == subsetSum;
}

/**
 * 限定子集和（子集树）
 * @param count
 * @param num
 * @param number
 * @param gather
 * @param subset
 * @param subsetSum
 */
void subsetEquals(int &count, int num, int number, const int gather[], int subset[], int subsetSum) {
    if (num >= number) {
        if (currentSum(num, gather, subset, subsetSum)) {
            count++;
            cout << "子集" << count << "：" << "{";
            for (int i = 0; i < num; i++) {
                if (subset[i] == 1) {
                    cout << gather[i] << ", ";
                }
            }
            cout << "}" << endl;
        }
    } else {
        for (int i = 1; i >= 0; i--) {
            subset[num] = i;
            if (boundSum(num, gather, subset, subsetSum)) {
                subsetEquals(count, num + 1, number, gather, subset, subsetSum);
            }
        }
    }
}

/**
 * 限定子集和（排列树）
 * @param count
 * @param num
 * @param number
 * @param gather
 * @param subset
 * @param subsetSum
 */
void subsetEqual(int &count, int num, int number, const int gather[], int subset[], int subsetSum) {
    if (num > number) {
        if (currentSum(num, gather, subset, subsetSum)) {
            count++;
            cout << "子集" << count << "：" << "{";
            for (int i = 0; i < num; i++) {
                if (subset[i] == 1) {
                    cout << gather[i] << ", ";
                }
            }
            cout << "}" << endl;
        }
    } else {
        for (int i = num; i <= number; i++) {
            subset[num] = i;
            if (boundSum(num, gather, subset, subsetSum)) {
                subsetEquals(count, num + 1, number, gather, subset, subsetSum);
            }
        }
    }
}

int main() {
    int num = 0, number = 0, count = 0;
    cout << "输入集合元素个数：";
    cin >> number;
    cout << "输入集合各元素值：";
    int gather[number];
    int subset[number];
    for (int i = 0; i < number; ++i) {
        cin >> gather[i];
    }
    int subsetSum = 0;
    cout << "输入匹配子集和：";
    cin >> subsetSum;

    subsetEquals(count, num, number, gather, subset, subsetSum);
    if (count == 0) {
        cout << "无解" << endl;
    }
    system("pause");
    return 0;
}
```



### 4.6.2. 皇后问题

```c++
#include<iostream>

using namespace std;

void outPut(int putSum, int layer, int queenSite[]) {
    cout << "方案" << putSum << "：";
    for (int i = 1; i <= layer; i++) {
        if (i != layer) {
            cout << "(" << i << "," << queenSite[i] << ")、";
        } else {
            cout << "(" << i << "," << queenSite[i] << ")" << endl;
        }
    }
}

/**
 * 限定约束函数：当前位置是否可放
 * @param queenSited
 * @param queenSite
 * @return
 */
bool isPlace(int queenSited, int queenSite[]) {
    for (int j = 1; j < queenSited; j++) {
        if ((abs(queenSited - j) == abs(queenSite[queenSited] - queenSite[j])) ||
            (queenSite[queenSited] == queenSite[j])) {
            return false;
        }
    }
    return true;
}

/**
 * N皇后问题
 * @param putLayer
 * @param layer
 * @param queenSite
 * @param putSum
 */
void putQueen(int putLayer, int layer, int queenSite[], int &putSum) {
    if (putLayer > layer) {
        putSum++;
        outPut(putSum, layer, queenSite);
    } else {
        for (int i = 1; i <= layer; i++) {
            queenSite[putLayer] = i;
            if (isPlace(putLayer, queenSite)) { //限定约束函数
                putQueen(putLayer + 1, layer, queenSite, putSum);
            }
        }
    }
}

int main() {
    int queenNum;
    cout << "输入皇后数量：";
    cin >> queenNum;

    int queenSite[queenNum];
    int putSum = 0;
    putQueen(1, queenNum, queenSite, putSum);

    cout << "总方案数为：" << putSum << endl;

    system("pause");
    return 0;
}
```



### 4.6.3. 作业批处理

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int numJobs; // 作业的数量
vector<int> durations; // 每个作业的持续时间
vector<bool> assigned; // 记录作业是否已被分配给某台机器
int minTotalTime = INT_MAX; // 记录最小的总处理时间

// 回溯函数，用于枚举所有可能的分配方案
void backtracking(int currentMachine, int currentTotalTime) {
    // 如果当前总处理时间已经超过了最小总处理时间，则剪枝
    if (currentTotalTime >= minTotalTime) {
        return;
    }

    // 如果所有作业都已经被分配，更新最小总处理时间
    if (currentMachine == numJobs) {
        minTotalTime = currentTotalTime;
        return;
    }

    // 尝试将当前作业分配给每一台机器
    for (int i = 0; i < numJobs; i++) {
        if (!assigned[i]) {
            assigned[i] = true; // 标记当前作业已被分配
            backtracking(currentMachine + 1, max(currentTotalTime, durations[i])); // 递归调用回溯函数
            assigned[i] = false; // 恢复当前作业未被分配状态
        }
    }
}

int main() {
    cout << "Enter the number of jobs: ";
    cin >> numJobs;

    durations.resize(numJobs);
    assigned.resize(numJobs, false);

    cout << "Enter the duration of each job: ";
    for (int i = 0; i < numJobs; i++) {
        cin >> durations[i];
    }

    backtracking(0, 0);

    cout << "Minimum total processing time: " << minTotalTime << endl;

    return 0;
}
```



# 五、分支限界算法

## 5.1. 基本思想

分支限界法的搜索策略是：在扩展结点处，先生成其所有的儿子结点（分支），使其变为活结点，然后再从当前的活结点表中选择下一个扩展结点。为了有效地选择下 一个扩展结点，以加速搜索的进程，在每一活结点处，计算一个函数值（限界），并根据这些已计算出的函数值（作为优先级），从当前活结点表中选择一个最有利的结点作为扩展结点，使搜索朝着解空间树上有最优解的分支推进，以便尽快地找出一个最优解。

> 分支限界算法的基本思想：将问题的解空间划分为一系列子集，通过优先级队列或普通队列管理节点的扩展和选择。分支限界算法使用剪枝策略来减少搜索空间，提高搜索效率。它适用于问题具有可行性和最优性质，并且搜索空间可以有序地划分为子集的情况。
>
> - 将问题的解空间划分为一个个子集，每个子集都对应一个节点。
> - 使用优先级队列或者普通队列来管理节点的扩展和选择。
> - 通过剪枝策略来减少搜索空间，提高搜索效率。
> - 适用于问题具有可行性和最优性质，并且搜索空间可以被有序地划分为子集的情况。

## 5.2. 设计算法

限界函数设计难以找出通用的方法，需根据具体问题来分析。一般地先要确定问题解的特性：

> 1. **目标函数是求最大值**
>
>    设计上界限界函数 ub（根结点的ub值通常大于或等于最优解的ub 值），若si是sj 的双亲结点，应满足 ub(si)≥ub(sj)，当找到一个可行解 ub(sk) 后，将所有小于 ub(sk)的结点剪枝。
>
> 
>
> - **目标函数是求最小值**
>
>   设计下界限界函数 lb（根结点的 lb值一定要小于或等于最优解的 lb 值），若si是sj 的双亲结点，应满足 lb(si)≤lb(sj)，当找到一个可行解 lb(sk) 后，将所有大于 lb(sk)的结点剪枝。



## 5.3. 常见分支限界算法

常见的分支限界法分为： ① 队列式（FIFO）分支限界法 ② 优先队列式（PQ）分支限界法 

### 5.3.1. 队列式分支限界法

队列式分支限界法将活结点表组织成一个队列，并按照队列先进先出（FIFO）原则选取下一个结点为扩展结点。

步骤如下：

>  ① 将根结点加入活结点队列
>
>  ② 从活结点队列中取出队头结点，作为当前扩展结点
>
>  ③ 对当前扩展结点，先从左到右地生成它的所有孩子结点，用约束条件或限界函数检查，把所有满足约束条件或限界函数的孩子结点加入活结点队列
>
> ④ 重复步骤②和③，直到找到一个解或活结点队列为空为止

### 5.3.2. 优先队列式分支限界法

优先队列式分支限界法的主要特点是将活结点表组组成一个优先队列（用堆实现），并选取优先级最高的活结点成为当前扩展结点。

步骤如下：

>  ① 计算起始结点（根结点）的优先级（与特定问题相关的信息的函数值决定优先 级。一般优先级以限界函数值为基础）并加入优先队列。
>
> ② 从优先队列中取出优先级最高的结点作为当前扩展结点，使搜索朝着解空间树上可能有最优解的分支推进，以便尽快地找出一个最优解
>
>  ③ 对当前扩展结点，先从左到右地生成它的所有孩子结点，然后用约束条件或限界函数检查，对所有满足约束条件或限界函数的孩子结点计算优先级并加入优先队列
>
>  ④ 重复步骤②和③，直到找到一个解或优先队列为空为止



## 5.4. 解题步骤及模板

### 5.4.1. 解题步骤

`在队列式分支限界法中，状态以先进先出的方式进行扩展，而在优先队列式分支限界法中，状态根据优先级进行扩展。在优先队列式分支限界法中，需要定义一个合适的优先级函数来确定状态的扩展顺序。`

> **队列式分支限界法的解题步骤：**
>
> 1. 初始化问题的初始状态，并创建一个空队列。
> 2. 将初始状态加入队列。
> 3. 从队列中取出一个状态，进行扩展得到所有可能的子状态。
> 4. 对于每个子状态，判断是否满足问题的限界条件，如果满足则进行进一步处理。
> 5. 对满足限界条件的子状态，将其加入队列。
> 6. 重复步骤3至5，直到队列为空或找到满足问题要求的解。
> 7. 返回找到的解或判断无解。



> **优先队列式分支限界法的解题步骤：**
>
> 1. 初始化问题的初始状态，并创建一个空优先队列。
> 2. 将初始状态加入优先队列。
> 3. 从优先队列中取出一个状态，进行扩展得到所有可能的子状态。
> 4. 对于每个子状态，判断是否满足问题的限界条件，如果满足则进行进一步处理。
> 5. 对满足限界条件的子状态，根据一定的优先级规则将其加入优先队列。
> 6. 重复步骤3至5，直到优先队列为空或找到满足问题要求的解。
> 7. 返回找到的解或判断无解。



### 5.4.1. 解题模板

**队列式分支限界法解题模板：**

```c++
// 定义状态结构体
struct State {
    // 状态的属性
};

// 定义比较函数，用于优先队列的排序
struct CompareState {
    bool operator()(const State& a, const State& b) const {
        // 返回a和b的优先级，true表示a优先级高于b
    }
};

void queueBranchAndBound() {
    // 初始化队列
    queue<State> q;
    // 初始化初始状态
    State initState;
    // 将初始状态加入队列
    q.push(initState);

    while (!q.empty()) {
        // 取出队头状态
        State curState = q.front();
        q.pop();

        // 处理curState的情况
        // ...

        // 扩展curState的子状态
        vector<State> nextStates = getNextStates(curState);

        // 将所有子状态加入队列
        for (State nextState : nextStates) {
            q.push(nextState);
        }
    }
}
```



**优先队列式分支限界法解题模板：**

```c++
// 定义状态结构体
struct State {
    // 状态的属性
};

// 定义比较函数，用于优先队列的排序
struct CompareState {
    bool operator()(const State& a, const State& b) const {
        // 返回a和b的优先级，true表示a优先级高于b
    }
};

void priorityQueueBranchAndBound() {
    // 初始化优先队列
    priority_queue<State, vector<State>, CompareState> pq;
    // 初始化初始状态
    State initState;
    // 将初始状态加入优先队列
    pq.push(initState);

    while (!pq.empty()) {
        // 取出优先级最高的状态
        State curState = pq.top();
        pq.pop();

        // 处理curState的情况
        // ...

        // 扩展curState的子状态
        vector<State> nextStates = getNextStates(curState);

        // 将所有子状态加入优先队列
        for (State nextState : nextStates) {
            pq.push(nextState);
        }
    }
}
```



## 5.5. 例题

### 5.5.1. 单源最短路径问题

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
#include <algorithm>

using namespace std;

const int MAX_N = 100;  // 图的最大节点数

int n;                  // 图中节点数
int c[MAX_N][MAX_N];    // 图的邻接矩阵表示
int dist[MAX_N];        // 存储当前源点到各节点的最短路径长度
int pre[MAX_N];         // 存储当前源点到各节点的最短路径上的前驱节点

// 定义状态结构体
struct State {
    int u;              // 当前处理的节点
    int d;              // 当前路径长度
    State(int _u, int _d) : u(_u), d(_d) {}

    // 定义优先级比较函数
    bool operator<(const State &other) const {
        return d > other.d;
    }
};

/**
 * 优先队列分支限界算法求解单源最短路径问题
 * @param start
 */
void shortest_path(int start) {
    priority_queue<State> q;
    q.push(State(start, 0));
    fill(dist, dist + n, INT_MAX);
    fill(pre, pre + n, -1);
    dist[start] = 0;
    while (!q.empty()) {
        State state = q.top();
        q.pop();
        int u = state.u, d = state.d;
        if (dist[u] < d) {
            continue;
        }
        for (int v = 0; v < n; v++) {
            if (c[u][v] == INT_MAX) {
                continue;
            }
            if (dist[v] > dist[u] + c[u][v]) {
                dist[v] = dist[u] + c[u][v];
                pre[v] = u;
                q.push(State(v, dist[v]));
            }
        }
    }
}

// 输出最短路径
void print_path(int s, int t) {
    vector<int> path;
    for (int u = t; u != -1; u = pre[u]) {
        path.push_back(u);
    }
    reverse(path.begin(), path.end());
    cout << "此路径为：";
    for (int i = 0; i < path.size(); i++) {
        if (i == 0) {
            cout << path[i];
        } else {
            cout << "->" << path[i];
        }
    }
    cout << endl;
}

int main() {
    n = 4;
    fill(c[0], c[0] + MAX_N * MAX_N, INT_MAX);
    c[0][1] = c[1][0] = 30;
    c[0][2] = c[2][0] = 6;
    c[0][3] = c[3][0] = 4;
    c[1][2] = c[2][1] = 5;
    c[1][3] = c[3][1] = 10;
    c[2][3] = c[3][2] = 20;
    int start = 0;
    shortest_path(start);
    for (int i = 0; i < n; i++) {
        cout << "最短距离：" << start << "->" << i << ": " << dist[i] << "\t=> ";
        print_path(0, i);
    }
    return 0;
}
```

