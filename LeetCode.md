# 数组基础题目

## 数组操作题目

### [189. 轮转数组](https://leetcode.cn/problems/rotate-array/)

难度中等

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

**示例 1:**

```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
```

C++版本

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k = k % nums.size();
        if (k != 0 && nums.size() != 1) {
            reverse(nums, 0, nums.size() - 1);
            reverse(nums, 0, k - 1);
            reverse(nums, k, nums.size() - 1);
        }
    }

    void reverse(vector<int>& nums, int start, int end) {
        while(start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
};
```

Java版本

```java
class Solution {
    private void reverse(int[] nums, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            int temp = nums[j];
            nums[j] = nums[i];
            nums[i] = temp;
        }
    }
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k %= n;
        reverse(nums, 0, n - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, n - 1);
    }
}
```



### [66. 加一](https://leetcode.cn/problems/plus-one/)

难度简单

给定一个由 **整数** 组成的 **非空** 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

**示例 1：**

```
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
```

C++版本

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        for(int i = digits.size() - 1; i >= 0; i--) {
            if(digits[i] != 9) {
                digits[i] += 1;
                return digits;
            } else {
                digits[i] = 0;
            }
        }
        digits.insert(digits.begin(), 1);
        return digits;
    }
};
```

Java版本

```java
public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i--) {
            if (digits[i] == 9) {
                digits[i] = 0;
            } else {
                digits[i] += 1;
                return digits;
            }
        }
        //如果所有位都是进位，则长度+1
        digits= new int[digits.length + 1];
        digits[0] = 1;
        return digits;
}
```



### [724. 寻找数组的中心下标](https://leetcode.cn/problems/find-pivot-index/)

难度简单

给你一个整数数组 `nums` ，请计算数组的 **中心下标** 。

数组 **中心下标** 是数组的一个下标，其左侧所有元素相加的和等于右侧所有元素相加的和。

如果中心下标位于数组最左端，那么左侧数之和视为 `0` ，因为在下标的左侧不存在元素。这一点对于中心下标位于数组最右端同样适用。

如果数组有多个中心下标，应该返回 **最靠近左边** 的那一个。如果数组不存在中心下标，返回 `-1` 。

**示例 1：**

```
输入：nums = [1, 7, 3, 6, 5, 6]
输出：3
解释：
中心下标是 3 。
左侧数之和 sum = nums[0] + nums[1] + nums[2] = 1 + 7 + 3 = 11 ，
右侧数之和 sum = nums[4] + nums[5] = 5 + 6 = 11 ，二者相等。
```

C++版本

```c++
class Solution {
public:
    int pivotIndex(vector<int> &nums) {
        // 求出数组总和
        int total = accumulate(nums.begin(), nums.end(), 0);
        int sum = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if (2 * sum + nums[i] == total) {
                return i;
            }
            sum += nums[i];
        }
        return -1;
    }
};
```

Java版本

```java
class Solution {
    public int pivotIndex(int[] nums) {
        // 求出数组总和
        int total = Arrays.stream(nums).sum();
        int sum = 0;
        for (int i = 0; i < nums.length; ++i) {
            if (2 * sum + nums[i] == total) {
                return i;
            }
            sum += nums[i];
        }
        return -1;
    }
}
```



### [485. 最大连续 1 的个数](https://leetcode.cn/problems/max-consecutive-ones/)

难度简单

给定一个二进制数组 `nums` ， 计算其中最大连续 `1` 的个数。

**示例 1：**

```
输入：nums = [1,1,0,1,1,1]
输出：3
解释：开头的两位和最后的三位都是连续 1 ，所以最大连续 1 的个数是 3.
```

C++版本

```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int total = 0,current = 0;
        for(int i = 0; i < nums.size(); i++) {
            if(nums[i] == 1) {
                current++;
            } else {
                total = total < current ? current : total;
                current = 0;
            }
        }
        total = total < current ? current : total;
        return total;
    }
};
```

Java版本

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int maxCount = 0, count = 0;
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 1) {
                count++;
            } else {
                maxCount = Math.max(maxCount, count);
                count = 0;
            }
        }
        maxCount = Math.max(maxCount, count);
        return maxCount;
    }
}
```



### [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

难度中等

给你一个整数数组 `nums`，返回 *数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积* 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内。

请**不要使用除法，**且在 `O(*n*)` 时间复杂度内完成此题。

**示例 1:**

```
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```

C++版本

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> newNums(nums.size(), 1);
        int left = 1;
        for(int i = 0; i < nums.size(); i++) {
            newNums[i] *= left;
            left *= nums[i];
        }

        int right = 1;
        for(int i = nums.size() - 1; i >= 0; i--) {
            newNums[i] *= right;
            right *= nums[i];
        }
        return newNums;
    }
};
```

Java版本

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] res = new int[nums.length];
        int left = 1, right = 1;
        for (int i = 0; i < nums.length; i++) {
            res[i] = left;
            left *= nums[i];
        }
        
        for (int i = nums.length - 1; i > 0 ; i--) {
            right *= nums[i];
            res[i - 1] *= right;
        }
        return res;
    }
}
```



## 二维数组题目

### [498. 对角线遍历](https://leetcode.cn/problems/diagonal-traverse/)

难度中等

给你一个大小为 `m x n` 的矩阵 `mat` ，请以对角线遍历的顺序，用一个数组返回这个矩阵中的所有元素。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/04/10/diag1-grid.jpg)

```
输入：mat = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,4,7,5,3,6,8,9]
```

C++版本

```c++
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& mat) {
        int rows = mat.size();
        int cols = mat[0].size();
        vector<int> res(rows * cols);
        int x = 0, y = 0;
        for(int i = 0; i < rows * cols; i++) {
            res[i] = mat[x][y];
            if((x + y) % 2 == 0) {
                if(y == cols - 1) {
                    x += 1;
                } else if (x == 0) {
                    y += 1;
                } else {
                    x -= 1;
                    y += 1;
                }
            } else {
                if(x == rows - 1) {
                    y += 1;
                } else if (y == 0) {
                    x += 1;
                } else {
                    x += 1;
                    y -= 1;
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
    public int[] findDiagonalOrder(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int[] res = new int[m * n];
        int pos = 0;
        for (int i = 0; i < m + n - 1; i++) {
            if (i % 2 == 1) {
                int x = i < n ? 0 : i - n + 1;
                int y = i < n ? i : n - 1;
                while (x < m && y >= 0) {
                    res[pos] = mat[x][y];
                    pos++;
                    x++;
                    y--;
                }
            } else {
                int x = i < m ? i : m - 1;
                int y = i < m ? 0 : i - m + 1;
                while (x >= 0 && y < n) {
                    res[pos] = mat[x][y];
                    pos++;
                    x--;
                    y++;
                }
            }
        }
        return res;
    }
}
```



### [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)

难度中等

给定一个 *n* × *n* 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在**[ 原地](https://baike.baidu.com/item/原地算法)** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

C++版本

```c++
// 方法一：辅助数组
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        auto arr = matrix;
        for(int i = 0; i < matrix.size(); i++) {
            for(int j = 0; j < matrix.size(); j++) {
                arr[j][matrix.size() - 1 - i] = matrix[i][j];
            }
        }
        matrix = arr;
    }
};

// 方法二：原地旋转
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for(int i = 0; i < n / 2; i++) {
            for(int j = 0; j < (n + 1) / 2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n-1-j][i];
                matrix[n-1-j][i] = matrix[n-1-i][n-1-j];
                matrix[n-1-i][n-1-j] = matrix[j][n-1-i];
                matrix[j][n-1-i] = temp;
            }
        }
    }
};

// 方法三：原地翻转
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        // 横轴翻转
        for(int i  = 0; i < n / 2; i++) {
            for(int j = 0; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n-1-i][j];
                matrix[n-1-i][j] = temp;
            }
        }

        // 对角线翻转
        for(int i = 1; i < n; i++) {
            for(int j = 0; j < i; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
    }
};
```



### [73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

难度中等

给定一个 `*m* x *n*` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **[原地](http://baike.baidu.com/item/原地算法)** 算法**。**

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

```
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
```

C++版本

```c++
// 数组充当标识
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int rows = matrix.size();
        int cols = matrix[0].size();
        vector<bool> row(rows, false), col(cols, false);
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(!matrix[i][j]) {
                    row[i] = col[j] = true;
                }
            }
        }

        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(row[i] || col[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
};

// 第一行、第一列当标识
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        bool flag1 = false;//表示第一行是否有0
        bool flag2 = false;//表示第一列是否有0
        int rows = matrix.size();
        int cols = matrix[0].size();
        for(int i=0;i<rows;i++){
            if(matrix[i][0]==0)
                flag2 = true;
        }
        for(int i=0;i<cols;i++){
            if(matrix[0][i]==0)
                flag1 = true;
        }
        for(int i=1;i<rows;i++){//以第一行和第一列为标志位，标志是否这一行/列置0
            for(int j=1;j<cols;j++){
                if(matrix[i][j]==0){
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        //把标志位所在列置0
        for(int i=1;i<cols;i++){
            if(matrix[0][i]==0){
                for(int j=1;j<rows;j++){
                    matrix[j][i] = 0;
                }
            }
        }
        //把标志位所在行置0
        for(int i=1;i<rows;i++){
            if(matrix[i][0]==0){
                for(int j=1;j<cols;j++)
                    matrix[i][j] = 0;
            }
        }
        //如果第一行有0存在则将第一行置0
        if(flag1==true){
            for(int i=0;i<cols;i++)
                matrix[0][i] = 0;
        }
        //如果第一列有0存在则将第一列置0
        if(flag2==true){
            for(int i=0;i<rows;i++)
                matrix[i][0] = 0;
        }
        return;
    }
};
```

Java版本

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean[] row = new boolean[m];
        boolean[] col = new boolean[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    row[i] = col[j] = true;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (row[i] || col[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}

class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean flagCol0 = false, flagRow0 = false;
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                flagCol0 = true;
            }
        }
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                flagRow0 = true;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (flagCol0) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
        if (flagRow0) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
    }
}
```



### [54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)

难度中等

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

C++版本

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int rows  = matrix.size();
        int cols = matrix[0].size();
        vector<int> res;
        if(matrix.empty()) {
            return res;
        }
        // 初始化边界
        int s = 0;
        int x = rows-1;
        int z = 0;
        int y = cols-1;
        while(true) {
            // 左往右
            for(int i = z; i <= y; i++) {
                res.push_back(matrix[s][i]);
            }
            if(++s > x) break;
            // 上往下
            for(int i = s; i <= x; i++) {
                res.push_back(matrix[i][y]);
            }
            if(--y < z) break;
            // 右往左
            for(int i = y; i >= z; i--) {
                res.push_back(matrix[x][i]);
            }
            if(--x < s) break;
            // 下往上
            for(int i = x; i >= s; i--) {
                res.push_back(matrix[i][z]);
            }
            if(++z > y) break;
        }
        return res;
    }
};
```

Java版本

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new LinkedList<>();
        if (matrix.length == 0) {
            return res;
        }
        int up = 0, down = matrix.length - 1, left = 0, right = matrix[0].length - 1;
        while (true) {
            for (int col = left; col <= right; ++col) {
                res.add(matrix[up][col]);
            }
            if (++up > down) {
                break;
            }
            for (int row = up; row <= down; ++row) {
                res.add(matrix[row][right]);
            }
            if (--right < left) {
                break;
            }
            for (int col = right; col >= left; --col) {
                res.add(matrix[down][col]);
            }
            if (--down < up) {
                break;
            }
            for (int row = down; row >= up; --row) {
                res.add(matrix[row][left]);
            }
            if (++left > right) {
                break;
            }
        }
        return res;
    }
}
```



### [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

难度中等

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

C++版本

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        int num = 1;
        int left = 0, top = 0, right = n - 1, bottom = n - 1;
        //初始化数组
        vector<vector<int>> res(n,vector<int>(n));
        while (num <= n*n ) {
            //left to right
            for (int i = left; i <= right; ++i) res[top][i] = num++;
            ++top;
            //top to bottom
            for (int i = top; i <= bottom; ++i) res[i][right] = num++;
            --right;
            //right to left
            for (int i = right; i >= left; --i) res[bottom][i] = num++;
            --bottom;
            //bottom to top
            for (int i = bottom; i >= top; --i) res[i][left] = num++;
            ++left;
        }
        return res;
    }
};
```

Java版本

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int l = 0, r = n - 1, t = 0, b = n - 1;
        int[][] mat = new int[n][n];
        int num = 1, tar = n * n;
        while(num <= tar){
            for(int i = l; i <= r; i++) mat[t][i] = num++; // left to right.
            t++;
            for(int i = t; i <= b; i++) mat[i][r] = num++; // top to bottom.
            r--;
            for(int i = r; i >= l; i--) mat[b][i] = num++; // right to left.
            b--;
            for(int i = b; i >= t; i--) mat[i][l] = num++; // bottom to top.
            l++;
        }
        return mat;
    }
}
```



### [289. 生命游戏](https://leetcode.cn/problems/game-of-life/)

难度中等

根据 [百度百科](https://baike.baidu.com/item/生命游戏/2926434?fr=aladdin) ， **生命游戏** ，简称为 **生命** ，是英国数学家约翰·何顿·康威在 1970 年发明的细胞自动机。

给定一个包含 `m × n` 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态： `1` 即为 **活细胞** （live），或 `0` 即为 **死细胞** （dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

1. 如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
2. 如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
3. 如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
4. 如果死细胞周围正好有三个活细胞，则该位置死细胞复活；

下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。给你 `m x n` 网格面板 `board` 的当前状态，返回下一个状态。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/26/grid1.jpg)

```
输入：board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
输出：[[0,0,0],[1,0,1],[0,1,1],[0,1,0]]
```

C++版本

```c++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int neighbors[3] = {0, 1, -1};

        int rows = board.size();
        int cols = board[0].size();

        // 遍历面板每一个格子里的细胞
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {

                // 对于每一个细胞统计其八个相邻位置里的活细胞数量
                int liveNeighbors = 0;

                for (int i = 0; i < 3; i++) {
                    for (int j = 0; j < 3; j++) {

                        if (!(neighbors[i] == 0 && neighbors[j] == 0)) {
                            // 相邻位置的坐标
                            int r = (row + neighbors[i]);
                            int c = (col + neighbors[j]);

                            // 查看相邻的细胞是否是活细胞
                            if ((r < rows && r >= 0) && (c < cols && c >= 0) && (abs(board[r][c]) == 1)) {
                                liveNeighbors += 1;
                            }
                        }
                    }
                }

                // 规则 1 或规则 3 
                if ((board[row][col] == 1) && (liveNeighbors < 2 || liveNeighbors > 3)) {
                    // -1 代表这个细胞过去是活的现在死了
                    board[row][col] = -1;
                }
                // 规则 4
                if (board[row][col] == 0 && liveNeighbors == 3) {
                    // 2 代表这个细胞过去是死的现在活了
                    board[row][col] = 2;
                }
            }
        }

        // 遍历 board 得到一次更新后的状态
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                if (board[row][col] > 0) {
                    board[row][col] = 1;
                } else {
                    board[row][col] = 0;
                }
            }
        }
    }
};
```

Java版本

```java
class Solution {
    public void gameOfLife(int[][] board) {
        int[] neighbors = {0, 1, -1};

        int rows = board.length;
        int cols = board[0].length;

        // 遍历面板每一个格子里的细胞
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {

                // 对于每一个细胞统计其八个相邻位置里的活细胞数量
                int liveNeighbors = 0;

                for (int i = 0; i < 3; i++) {
                    for (int j = 0; j < 3; j++) {

                        if (!(neighbors[i] == 0 && neighbors[j] == 0)) {
                            // 相邻位置的坐标
                            int r = (row + neighbors[i]);
                            int c = (col + neighbors[j]);

                            // 查看相邻的细胞是否是活细胞
                            if ((r < rows && r >= 0) && (c < cols && c >= 0) && (Math.abs(board[r][c]) == 1)) {
                                liveNeighbors += 1;
                            }
                        }
                    }
                }

                // 规则 1 或规则 3 
                if ((board[row][col] == 1) && (liveNeighbors < 2 || liveNeighbors > 3)) {
                    // -1 代表这个细胞过去是活的现在死了
                    board[row][col] = -1;
                }
                // 规则 4
                if (board[row][col] == 0 && liveNeighbors == 3) {
                    // 2 代表这个细胞过去是死的现在活了
                    board[row][col] = 2;
                }
            }
        }

        // 遍历 board 得到一次更新后的状态
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                if (board[row][col] > 0) {
                    board[row][col] = 1;
                } else {
                    board[row][col] = 0;
                }
            }
        }
    }
}
```



## 数组排序题目

### [剑指 Offer 45. 把数组排成最小的数](https://leetcode.cn/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

难度中等

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

解题思路：
此题求拼接起来的最小数字，本质上是一个排序问题。设数组 nums 中任意两数字的字符串为 *x* 和 *y* ，则规定 **排序判断规则** 为：

- 若拼接字符串 **x+y** > **y+x** ，则 x “大于” y ；
- 反之，若 **x+y** < **y+x** ，则 x “小于” y ；

**示例 1:**

```
输入: [10,2]
输出: "102"
```

C++版本

```c++
// 个人版
class Solution {
public:
    string minNumber(vector<int>& nums) {
        vector<string> strVec;
        int len = nums.size();
        for(int i = 0; i < len; i++) {
            strVec.push_back(to_string(nums[i]));
        }
        quickSort(strVec, 0, len - 1);
        string res;
        for(string s : strVec) {
            res.append(s);
        }
        return res;
    }

    void quickSort(vector<string>& strVec, int left, int right){
        if(left < right) {
            int pi = partition(strVec, left, right);
            quickSort(strVec, left, pi - 1);
            quickSort(strVec, pi + 1, right);
        }
    }

    int partition(vector<string>& strVec, int left, int right){
        string pivot = strVec[left];
        int i = left + 1;
        string temp;
        for(int j = i; j <= right; j++) {
            if(strVec[j] + pivot < pivot + strVec[j]) {
                temp = strVec[i];
                strVec[i] = strVec[j];
                strVec[j] = temp;
                i++;
            }
        }
        temp = strVec[i - 1];
        strVec[i -1] = strVec[left];
        strVec[left] = temp;
        return i - 1;
    }
};

// 快速排序
class Solution {
public:
    string minNumber(vector<int>& nums) {
        vector<string> strs;
        for(int i = 0; i < nums.size(); i++)
            strs.push_back(to_string(nums[i]));
        quickSort(strs, 0, strs.size() - 1);
        string res;
        for(string s : strs)
            res.append(s);
        return res;
    }
private:
    void quickSort(vector<string>& strs, int l, int r) {
        if(l >= r) return;
        int i = l, j = r;
        while(i < j) {
            while(strs[j] + strs[l] >= strs[l] + strs[j] && i < j) j--;
            while(strs[i] + strs[l] <= strs[l] + strs[i] && i < j) i++;
            swap(strs[i], strs[j]);
        }
        swap(strs[i], strs[l]);
        quickSort(strs, l, i - 1);
        quickSort(strs, i + 1, r);
    }
};

// 内置排序
class Solution {
public:
    string minNumber(vector<int>& nums) {
        vector<string> strs;
        string res;
        for(int i = 0; i < nums.size(); i++)
            strs.push_back(to_string(nums[i]));
        sort(strs.begin(), strs.end(), [](string& x, string& y){ return x + y < y + x; });
        for(int i = 0; i < strs.size(); i++)
            res.append(strs[i]);
        return res;
    }
};
```

Java版本

```java
// 快速排序
class Solution {
    public String minNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++)
            strs[i] = String.valueOf(nums[i]);
        quickSort(strs, 0, strs.length - 1);
        StringBuilder res = new StringBuilder();
        for(String s : strs)
            res.append(s);
        return res.toString();
    }
    
    void quickSort(String[] strs, int l, int r) {
        if(l >= r) return;
        int i = l, j = r;
        String tmp = strs[i];
        while(i < j) {
            while((strs[j] + strs[l]).compareTo(strs[l] + strs[j]) >= 0 && i < j) j--;
            while((strs[i] + strs[l]).compareTo(strs[l] + strs[i]) <= 0 && i < j) i++;
            tmp = strs[i];
            strs[i] = strs[j];
            strs[j] = tmp;
        }
        strs[i] = strs[l];
        strs[l] = tmp;
        quickSort(strs, l, i - 1);
        quickSort(strs, i + 1, r);
    }
}

// 内置排序
class Solution {
    public String minNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++)
            strs[i] = String.valueOf(nums[i]);
        Arrays.sort(strs, (x, y) -> (x + y).compareTo(y + x));
        StringBuilder res = new StringBuilder();
        for(String s : strs)
            res.append(s);
        return res.toString();
    }
}
```



### [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

难度简单

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

C++版本

```c++
// 快慢指针
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size(), left = 0, right = 0;
        while (right < n) {
            if (nums[right]) {
                swap(nums[left], nums[right]);
                left++;
            }
            right++;
        }
    }
};
```

Java版本

```java
// 快慢指针
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length, left = 0, right = 0;
        while (right < n) {
            if (nums[right] != 0) {
                swap(nums, left, right);
                left++;
            }
            right++;
        }
    }

    public void swap(int[] nums, int left, int right) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;
    }
}
```



### [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

难度中等

给定整数数组 `nums` 和整数 `k`，请返回数组中第 `k` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例 1:**

```
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

C++版本

```c++
// 快速排序
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        return quickSort(nums, 0, nums.size() - 1, k);
    }

    int quickSort(vector<int>& nums, int left, int right,int k) {
        if(left < right) {
            int point = partion(nums,left,right);
            if(nums.size() - k == point) {
                return nums[nums.size() - k];
            } else if(nums.size() - k < point) {
                return quickSort(nums, left, point-1, k);
            } else {
                return quickSort(nums, point+1, right, k);
            }
        }
        return nums[nums.size() - k];
    }

    int partion(vector<int>& nums, int left, int right) {
        int temp, key = nums[left];
        int index = left + 1;
        for(int i = index; i <= right; i++) {
            if(nums[i] < key) {
                temp = nums[i];
                nums[i] = nums[index];
                nums[index] = temp;
                index++;
            }
        }
        temp = nums[index - 1];
        nums[index - 1] = nums[left];
        nums[left] = temp;
        return index - 1;
    }
};

// 堆排序
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        buildMaxHeap(nums);
        int len = nums.size();
        for(int i = 0; i < len; i++) {
            swap(nums[0], nums[len - i - 1]);
            headify(nums, 0, len - i - 2);
        }
        return nums[len - k];
    }

    void buildMaxHeap(vector<int>& nums) {
        int len = nums.size();
        for(int i = (len - 2) / 2; i >= 0 ; i--) {
            headify(nums, i, len - 1);
        }
    }

    void headify(vector<int>& nums, int index, int end) {
        int left = index * 2 + 1;
        int right = left + 1;
        while(left <= end) {
            int max_index = index;
            if(nums[left] > nums[max_index]) {
                max_index = left;
            }
            if(right <= end && nums[right] > nums[max_index]) {
                max_index = right;
            }
            if(index == max_index){
                break;
            }
            swap(nums[index], nums[max_index]);
            index = max_index;
            left = index * 2 + 1;
            right = left + 1;
        }
    }
};
```

Java版本

```java
// 快速排序
class Solution {
    Random random = new Random();

    public int findKthLargest(int[] nums, int k) {
        return quickSelect(nums, 0, nums.length - 1, nums.length - k);
    }

    public int quickSelect(int[] a, int l, int r, int index) {
        int q = randomPartition(a, l, r);
        if (q == index) {
            return a[q];
        } else {
            return q < index ? quickSelect(a, q + 1, r, index) : quickSelect(a, l, q - 1, index);
        }
    }

    public int randomPartition(int[] a, int l, int r) {
        int i = random.nextInt(r - l + 1) + l;
        swap(a, i, r);
        return partition(a, l, r);
    }

    public int partition(int[] a, int l, int r) {
        int x = a[r], i = l - 1;
        for (int j = l; j < r; ++j) {
            if (a[j] <= x) {
                swap(a, ++i, j);
            }
        }
        swap(a, i + 1, r);
        return i + 1;
    }

    public void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}

// 堆排序
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int heapSize = nums.length;
        buildMaxHeap(nums, heapSize);
        for (int i = nums.length - 1; i >= nums.length - k + 1; --i) {
            swap(nums, 0, i);
            --heapSize;
            maxHeapify(nums, 0, heapSize);
        }
        return nums[0];
    }

    public void buildMaxHeap(int[] a, int heapSize) {
        for (int i = heapSize / 2; i >= 0; --i) {
            maxHeapify(a, i, heapSize);
        } 
    }

    public void maxHeapify(int[] a, int i, int heapSize) {
        int l = i * 2 + 1, r = i * 2 + 2, largest = i;
        if (l < heapSize && a[l] > a[largest]) {
            largest = l;
        } 
        if (r < heapSize && a[r] > a[largest]) {
            largest = r;
        }
        if (largest != i) {
            swap(a, i, largest);
            maxHeapify(a, largest, heapSize);
        }
    }

    public void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```



### [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

难度中等

给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。

必须在不使用库内置的 sort 函数的情况下解决这个问题。

**示例 1：**

```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

C++版本

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int left = 0, index = 0, right = nums.size() - 1;
        while(index <= right) {
            if(index < left) {
                index++;
            } else if(nums[index] == 0) {
                swap(nums[index], nums[left++]);
            } else if(nums[index] == 2) {
                swap(nums[index],nums[right--]);
            } else {
                index++;
            }
        }
    }
};
```

Java版本

```java
class Solution {
    public void sortColors(int[] nums) {
        int left = 0, index = 0, right = nums.length - 1;
        while(index <= right) {
            if(index < left) {
                index++;
            } else if(nums[index] == 0) {
                int temp = nums[index];
                nums[index] = nums[left];
                nums[left] = temp;
                left++;
            } else if(nums[index] == 2) {
                int temp = nums[index];
                nums[index] = nums[right];
                nums[right] = temp;
                right--;
            } else {
                index++;
            }
        }
    }
}
```



### [912. 排序数组](https://leetcode.cn/problems/sort-an-array/)

难度中等

给你一个整数数组 `nums`，请你将该数组升序排列。

**示例 1：**

```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```

C++版本

```c++
// 希尔排序
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        int gap = nums.size() / 2;
        while(gap > 0) {
            for(int i = gap; i < nums.size(); i++) {
                int temp = nums[i];
                int j = i;
                while(j >= gap && nums[j - gap] > temp) {
                    nums[j] = nums[j - gap];
                    j -= gap;
                }
                nums[j] = temp;
            }
            gap /= 2;
        }
        return nums;
    }
};

// 快速排序（超时）
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        quickSort(nums, 0, nums.size() - 1);
        return nums;
    }

    void quickSort(vector<int>& nums, int left, int right) {
        if(left < right) {
            int point = partion(nums,left,right);
            quickSort(nums, left, point-1);
            quickSort(nums, point+1, right);
        }
    }

    int partion(vector<int>& nums, int left, int right) {
        int index = rand() % (right - left + 1) + left;
        swap(nums[left], nums[index]);
        int key = nums[left];
        while(left < right) {
            while(left < right && nums[right] >= key) right--;
            if(left < right) nums[left] = nums[right];
            while(left < right && nums[left] <= key) left++;
            if(left < right) nums[right] = nums[left];
        }
        nums[left] = key;
        return left;
    }
};

// 堆排序
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        int len = nums.size();
        buildMaxHeap(nums);
        for(int i = 0; i < len; i++) {
            swap(nums[0], nums[len - i - 1]);
            heapify(nums, 0, len - i - 2);
        }
        return nums;
    }

    void buildMaxHeap(vector<int>& nums) {
        int len = nums.size();
        for(int i = (len - 2) / 2; i >= 0; i--) {
            heapify(nums, i, len - 1);
        }
    }

    void heapify(vector<int>& nums, int index, int end) {
        int left = index * 2 + 1;
        int right = left + 1;
        while(left <= end) {
            int maxIndex = index;
            if(nums[left] > nums[maxIndex]) {
                maxIndex = left;
            }
            if(right <= end && nums[right] > nums[maxIndex]) {
                maxIndex = right;
            }
            if(maxIndex == index) {
                break;
            }
            swap(nums[index], nums[maxIndex]);
            index = maxIndex;
            left = index * 2 + 1;
            right = left + 1;
        }
    }
};

// 归并排序
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        mergeSort(nums, 0, nums.size() - 1);
        return nums;
    }

    void mergeSort(vector<int>& nums, int left, int right) {
        if(left >= right) {
            return;
        }
        int mid = (left + right) / 2;
        mergeSort(nums, left, mid);
        mergeSort(nums, mid+1, right);
        merge(nums, left, right);
    }

    void merge(vector<int>& nums, int left, int right) {
        int mid = (left + right) / 2;
        int start = left, end = mid + 1, cnt = 0;
        vector<int> tmp(right - left + 1);
        while (start <= mid && end <= right) {
            if (nums[start] <= nums[end]) {
                tmp[cnt++] = nums[start++];
            } else {
                tmp[cnt++] = nums[end++];
            }
        }
        while (start <= mid) {
            tmp[cnt++] = nums[start++];
        }
        while (end <= right) {
            tmp[cnt++] = nums[end++];
        }
        for (int i = 0; i < right - left + 1; ++i) {
            nums[i + left] = tmp[i];
        }
    }
};
```

Java版本

```java
// 快速排序
class Solution {
    public int[] sortArray(int[] nums) {
        randomizedQuicksort(nums, 0, nums.length - 1);
        return nums;
    }

    public void randomizedQuicksort(int[] nums, int l, int r) {
        if (l < r) {
            int pos = randomizedPartition(nums, l, r);
            randomizedQuicksort(nums, l, pos - 1);
            randomizedQuicksort(nums, pos + 1, r);
        }
    }

    public int randomizedPartition(int[] nums, int l, int r) {
        int i = new Random().nextInt(r - l + 1) + l; // 随机选一个作为我们的主元
        swap(nums, r, i);
        return partition(nums, l, r);
    }

    public int partition(int[] nums, int l, int r) {
        int pivot = nums[r];
        int i = l - 1;
        for (int j = l; j <= r - 1; ++j) {
            if (nums[j] <= pivot) {
                i = i + 1;
                swap(nums, i, j);
            }
        }
        swap(nums, i + 1, r);
        return i + 1;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}

// 堆排序
class Solution {
    public int[] sortArray(int[] nums) {
        heapSort(nums);
        return nums;
    }

    public void heapSort(int[] nums) {
        int len = nums.length - 1;
        buildMaxHeap(nums, len);
        for (int i = len; i >= 1; --i) {
            swap(nums, i, 0);
            len -= 1;
            maxHeapify(nums, 0, len);
        }
    }

    public void buildMaxHeap(int[] nums, int len) {
        for (int i = len / 2; i >= 0; --i) {
            maxHeapify(nums, i, len);
        }
    }

    public void maxHeapify(int[] nums, int i, int len) {
        for (; (i << 1) + 1 <= len;) {
            int lson = (i << 1) + 1;
            int rson = (i << 1) + 2;
            int large;
            if (lson <= len && nums[lson] > nums[i]) {
                large = lson;
            } else {
                large = i;
            }
            if (rson <= len && nums[rson] > nums[large]) {
                large = rson;
            }
            if (large != i) {
                swap(nums, i, large);
                i = large;
            } else {
                break;
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}

// 归并排序
class Solution {
    int[] tmp;

    public int[] sortArray(int[] nums) {
        tmp = new int[nums.length];
        mergeSort(nums, 0, nums.length - 1);
        return nums;
    }

    public void mergeSort(int[] nums, int l, int r) {
        if (l >= r) {
            return;
        }
        int mid = (l + r) >> 1;
        mergeSort(nums, l, mid);
        mergeSort(nums, mid + 1, r);
        int i = l, j = mid + 1;
        int cnt = 0;
        while (i <= mid && j <= r) {
            if (nums[i] <= nums[j]) {
                tmp[cnt++] = nums[i++];
            } else {
                tmp[cnt++] = nums[j++];
            }
        }
        while (i <= mid) {
            tmp[cnt++] = nums[i++];
        }
        while (j <= r) {
            tmp[cnt++] = nums[j++];
        }
        for (int k = 0; k < r - l + 1; ++k) {
            nums[k + l] = tmp[k];
        }
    }
}
```

### [506. 相对名次](https://leetcode.cn/problems/relative-ranks/)

难度简单

给你一个长度为 `n` 的整数数组 `score` ，其中 `score[i]` 是第 `i` 位运动员在比赛中的得分。所有得分都 **互不相同** 。

运动员将根据得分 **决定名次** ，其中名次第 `1` 的运动员得分最高，名次第 `2` 的运动员得分第 `2` 高，依此类推。运动员的名次决定了他们的获奖情况：

- 名次第 `1` 的运动员获金牌 `"Gold Medal"` 。
- 名次第 `2` 的运动员获银牌 `"Silver Medal"` 。
- 名次第 `3` 的运动员获铜牌 `"Bronze Medal"` 。
- 从名次第 `4` 到第 `n` 的运动员，只能获得他们的名次编号（即，名次第 `x` 的运动员获得编号 `"x"`）。

使用长度为 `n` 的数组 `answer` 返回获奖，其中 `answer[i]` 是第 `i` 位运动员的获奖情况。

**示例 1：**

```
输入：score = [5,4,3,2,1]
输出：["Gold Medal","Silver Medal","Bronze Medal","4","5"]
解释：名次为 [1st, 2nd, 3rd, 4th, 5th] 。
```

**提示：**

- `n == score.length`
- `1 <= n <= 104`
- `0 <= score[i] <= 106`
- `score` 中的所有值 **互不相同**

C++版本

```c++
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& score) {
        int n = score.size();
        string desc[3] = {"Gold Medal", "Silver Medal", "Bronze Medal"};
        vector<pair<int, int>> arr;

        for (int i = 0; i < n; ++i) {
            arr.emplace_back(make_pair(-score[i], i));
        }
        sort(arr.begin(), arr.end());
        vector<string> ans(n);
        for (int i = 0; i < n; ++i) {
            if (i >= 3) {
                ans[arr[i].second] = to_string(i + 1);
            } else {
                ans[arr[i].second] = desc[i];
            }
        }
        return ans;
    }
};
```

Java版本

```java
class Solution {
    public String[] findRelativeRanks(int[] score) {
        int n = score.length;
        String[] desc = {"Gold Medal", "Silver Medal", "Bronze Medal"};
        int[][] arr = new int[n][2];

        for (int i = 0; i < n; ++i) {
            arr[i][0] = score[i];
            arr[i][1] = i;
        }
        Arrays.sort(arr, (a, b) -> b[0] - a[0]);
        String[] ans = new String[n];
        for (int i = 0; i < n; ++i) {
            if (i >= 3) {
                ans[arr[i][1]] = Integer.toString(i + 1);
            } else {
                ans[arr[i][1]] = desc[i];
            }
        }
        return ans;
    }
}
```



### [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

难度简单

给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组`nums1`中。为了应对这种情况，`nums1` 的初始长度为`m + n`，其中前`m`个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为`n`。

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

C++版本

```c++
// 归并思想
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int num = (m--) + (n--) -1;
        while(n >= 0 && m >= 0) {
            nums1[num--] = nums1[m] > nums2[n] ? nums1[m--] : nums2[n--];
        }
        while(n >= 0) {
            nums1[num--] = nums2[n--];
        }
    }
};
```

Java版本

```java
// 归并思想
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int p = m-- + n-- - 1;
        while (m >= 0 && n >= 0) {
            nums1[p--] = nums1[m] > nums2[n] ? nums1[m--] : nums2[n--];
        }
        
        while (n >= 0) {
            nums1[p--] = nums2[n--];
        }
    }
}
```



### [剑指 Offer 51. 数组中的逆序对](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

难度困难

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

**示例 1:**

```
输入: [7,5,6,4]
输出: 5
```

C++版本

```c++
// 归并思想
class Solution {
public:
    int mergeSort(vector<int>& nums, vector<int>& tmp, int left, int right) {
        if (left >= right) {
            return 0;
        }

        int mid = (left + right) / 2;
        int inv_count = mergeSort(nums, tmp, left, mid) + mergeSort(nums, tmp, mid + 1, right);
        int i = left, j = mid + 1, pos = left;
        while (i <= mid && j <= right) {
            if (nums[i] <= nums[j]) {
                tmp[pos] = nums[i];
                ++i;
                inv_count += (j - (mid + 1));
            }
            else {
                tmp[pos] = nums[j];
                ++j;
            }
            ++pos;
        }
        for (int k = i; k <= mid; ++k) {
            tmp[pos++] = nums[k];
            inv_count += (j - (mid + 1));
        }
        for (int k = j; k <= right; ++k) {
            tmp[pos++] = nums[k];
        }
        copy(tmp.begin() + left, tmp.begin() + right + 1, nums.begin() + left);
        return inv_count;
    }

    int reversePairs(vector<int>& nums) {
        int len = nums.size();
        vector<int> tmp(len);
        return mergeSort(nums, tmp, 0, len - 1);
    }
};

// 离散化树状数组（难）
class BIT {
private:
    vector<int> tree;
    int n;

public:
    BIT(int _n): n(_n), tree(_n + 1) {}

    static int lowbit(int x) {
        return x & (-x);
    }

    int query(int x) {
        int ret = 0;
        while (x) {
            ret += tree[x];
            x -= lowbit(x);
        }
        return ret;
    }

    void update(int x) {
        while (x <= n) {
            ++tree[x];
            x += lowbit(x);
        }
    }
};

class Solution {
public:
    int reversePairs(vector<int>& nums) {
        int n = nums.size();
        vector<int> tmp = nums;
        // 离散化
        sort(tmp.begin(), tmp.end());
        for (int& num: nums) {
            num = lower_bound(tmp.begin(), tmp.end(), num) - tmp.begin() + 1;
        }
        // 树状数组统计逆序对
        BIT bit(n);
        int ans = 0;
        for (int i = n - 1; i >= 0; --i) {
            ans += bit.query(nums[i] - 1);
            bit.update(nums[i]);
        }
        return ans;
    }
};
```

Java版本

```java
public class Solution {
    public int reversePairs(int[] nums) {
        int len = nums.length;

        if (len < 2) {
            return 0;
        }

        int[] copy = new int[len];
        for (int i = 0; i < len; i++) {
            copy[i] = nums[i];
        }

        int[] temp = new int[len];
        return reversePairs(copy, 0, len - 1, temp);
    }

    private int reversePairs(int[] nums, int left, int right, int[] temp) {
        if (left == right) {
            return 0;
        }

        int mid = left + (right - left) / 2;
        int leftPairs = reversePairs(nums, left, mid, temp);
        int rightPairs = reversePairs(nums, mid + 1, right, temp);

        if (nums[mid] <= nums[mid + 1]) {
            return leftPairs + rightPairs;
        }

        int crossPairs = mergeAndCount(nums, left, mid, right, temp);
        return leftPairs + rightPairs + crossPairs;
    }

    private int mergeAndCount(int[] nums, int left, int mid, int right, int[] temp) {
        for (int i = left; i <= right; i++) {
            temp[i] = nums[i];
        }

        int i = left;
        int j = mid + 1;

        int count = 0;
        for (int k = left; k <= right; k++) {
            if (i == mid + 1) {
                nums[k] = temp[j];
                j++;
            } else if (j == right + 1) {
                nums[k] = temp[i];
                i++;
            } else if (temp[i] <= temp[j]) {
                nums[k] = temp[i];
                i++;
            } else {
                nums[k] = temp[j];
                j++;
                count += (mid - i + 1);
            }
        }
        return count;
    }
}

// 离散化树状数组（难）
class Solution {
    public int reversePairs(int[] nums) {
        int n = nums.length;
        int[] tmp = new int[n];
        System.arraycopy(nums, 0, tmp, 0, n);
        // 离散化
        Arrays.sort(tmp);
        for (int i = 0; i < n; ++i) {
            nums[i] = Arrays.binarySearch(tmp, nums[i]) + 1;
        }
        // 树状数组统计逆序对
        BIT bit = new BIT(n);
        int ans = 0;
        for (int i = n - 1; i >= 0; --i) {
            ans += bit.query(nums[i] - 1);
            bit.update(nums[i]);
        }
        return ans;
    }
}

class BIT {
    private int[] tree;
    private int n;

    public BIT(int n) {
        this.n = n;
        this.tree = new int[n + 1];
    }

    public static int lowbit(int x) {
        return x & (-x);
    }

    public int query(int x) {
        int ret = 0;
        while (x != 0) {
            ret += tree[x];
            x -= lowbit(x);
        }
        return ret;
    }

    public void update(int x) {
        while (x <= n) {
            ++tree[x];
            x += lowbit(x);
        }
    }
}
```



### [315. 计算右侧小于当前元素的个数](https://leetcode.cn/problems/count-of-smaller-numbers-after-self/)

难度困难

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
// 归并思想
class Solution {
private:
    vector<int> index;
    vector<int> temp;
    vector<int> tempIndex;
    vector<int> ans;

public:
    vector<int> countSmaller(vector<int>& nums) {
        int n = nums.size();
        index.resize(n);
        temp.resize(n);
        tempIndex.resize(n);
        ans.resize(n);

        for (int i = 0; i < n; ++i) {
            index[i] = i;
        }

        int l = 0, r = n - 1;
        mergeSort(nums, l, r);

        return ans;
    }

    void mergeSort(std::vector<int>& a, int l, int r) {
        if (l >= r) {
            return;
        }

        int mid = (l + r) >> 1;
        mergeSort(a, l, mid);
        mergeSort(a, mid + 1, r);
        merge(a, l, mid, r);
    }

    void merge(std::vector<int>& a, int l, int mid, int r) {
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
};

// 离散化树状数组（难）
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
```

Java版本

```java
// 归并思想
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
// 离散化树状数组（难）
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
```



### [169. 多数元素](https://leetcode.cn/problems/majority-element/)

难度简单

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1：**

```
输入：nums = [3,2,3]
输出：3
```

C++版本

```c++
// 方法一：分治
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int len = nums.size();
        return getMajorityElement(nums, 0, len-1);
    }

    int getMajorityElement(vector<int>& nums, int left, int right) {
        if(left == right) {
            return nums[left];
        }

        int mid = left + (right - left) / 2;
        int left_mode = getMajorityElement(nums, left, mid);
        int right_mode = getMajorityElement(nums, mid+1, right);

        if(left_mode == right_mode) {
            return left_mode;
        }

        int left_mode_cnt = 0, right_mode_cnt = 0;
        for(int i = left; i <= right; i++) {
            if(nums[i] == left_mode) {
                left_mode_cnt++;
            }
            if(nums[i] == right_mode) {
                right_mode_cnt++;
            }
        }
        if(left_mode_cnt > right_mode_cnt) {
            return left_mode;
        }
        return right_mode;
    }
};

// 方法二:投票算法
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        //boyer-moore投票方法：如果我们把众数记为+1，把其他数记为−1，全部加起来，显然和大于0（众数个数大于n/2）
        int cand = 0, count = 0;
        for(auto num: nums){
            if(count == 0){
                cand = num;
            }
            if(cand == num){
                count++;
            }
            else    count--;
        }
        return cand;
    }
};

// 方法三：哈希表
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> counts;
        int majority = 0, cnt = 0;
        for (int num: nums) {
            ++counts[num];
            if (counts[num] > cnt) {
                majority = num;
                cnt = counts[num];
            }
        }
        return majority;
    }
};
```

Java版本

```java
// 方法一：分治
class Solution {
    public int majorityElement(int[] nums) {
        return getMajorityElement(nums, 0, nums.length-1);
    }

    public int getMajorityElement(int[] nums, int left, int right) {
        if(left == right) {
            return nums[left];
        }

        int mid = left + (right - left) / 2;
        int left_mode = getMajorityElement(nums, left, mid);
        int right_mode = getMajorityElement(nums, mid+1, right);

        if(left_mode == right_mode) {
            return left_mode;
        }

        int left_mode_cnt = 0, right_mode_cnt = 0;
        for(int i = left; i <= right; i++) {
            if(nums[i] == left_mode) {
                left_mode_cnt++;
            }
            if(nums[i] == right_mode) {
                right_mode_cnt++;
            }
        }
        if(left_mode_cnt > right_mode_cnt) {
            return left_mode;
        }
        return right_mode;
    }
}

// 方法二：投票算法
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        Integer candidate = null;

        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            count += (num == candidate) ? 1 : -1;
        }
        return candidate;
    }
}

// 方法三：哈希表
class Solution {
    private Map<Integer, Integer> countNums(int[] nums) {
        Map<Integer, Integer> counts = new HashMap<Integer, Integer>();
        for (int num : nums) {
            if (!counts.containsKey(num)) {
                counts.put(num, 1);
            } else {
                counts.put(num, counts.get(num) + 1);
            }
        }
        return counts;
    }

    public int majorityElement(int[] nums) {
        Map<Integer, Integer> counts = countNums(nums);

        Map.Entry<Integer, Integer> majorityEntry = null;
        for (Map.Entry<Integer, Integer> entry : counts.entrySet()) {
            if (majorityEntry == null || entry.getValue() > majorityEntry.getValue()) {
                majorityEntry = entry;
            }
        }

        return majorityEntry.getKey();
    }
}
```



### [剑指 Offer 40. 最小的k个数](https://leetcode.cn/problems/zui-xiao-de-kge-shu-lcof/)

简单

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

**示例 1：**

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

C++版本

```c++
// 大根堆
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        buildMaxHeap(arr);
        int len = arr.size();
        vector<int> nums(k);
        for(int i = 0; i < len; i++) {
            swap(arr[0], arr[len-i-1]);
            heapify(arr, 0, len-i-2);
        }
        for(int i = 0; i < k; i++) {
            nums[i] = arr[i];
        }
        return nums;
    }

    void buildMaxHeap(vector<int>& arr) {
        int len = arr.size();
        for(int i = (len - 2) / 2; i >= 0; i--) {
            heapify(arr, i, len-1);
        }
    }

    void heapify(vector<int>& arr, int index, int end) {
        int left = index * 2 + 1;
        int right = left + 1;
        while(left <= end) {
            int maxIndex = index;
            if(arr[left] > arr[maxIndex]) {
                maxIndex = left;
            }
            if(right <= end && arr[right] > arr[maxIndex]) {
                maxIndex = right;
            }
            if(maxIndex == index) {
                break;
            }
            swap(arr[maxIndex], arr[index]);
            index = maxIndex;
            left = index * 2 + 1;
            right = left + 1;
        }
    }
};

// 快排
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        quickSort(arr, 0, arr.size() - 1);
        vector<int> res(k);
        for(int i = 0; i < k; i++) {
            res[i] = arr[i];
        }
        return res;
    }
private:
    int partion(vector<int>& arr, int left, int right) {
        int key = arr[left];
        while(left < right) {
            while(left < right && arr[right] >= key) right--;
            arr[left] = arr[right];
            while(left < right && arr[left] <= key) left++;
            arr[right] = arr[left];
        }
        arr[left] = key;
        return left;
    }

    void quickSort(vector<int>& arr, int left, int right) {
        if(left >= right) {
            return;
        }
        int point = partion(arr, left, right);
        // 递归左（右）子数组执行哨兵划分
        quickSort(arr, left, point - 1);
        quickSort(arr, point + 1, right);
    }
};
```

Java版本

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        int[] vec = new int[k];
        if (k == 0) { // 排除 0 的情况
            return vec;
        }
        int len = arr.length;
        buildMaxHeap(arr);
        for(int i = 0; i < len; i++) {
            int temp = arr[len-i-1];
            arr[len-i-1]= arr[0];
            arr[0] = temp;
            heapify(arr, 0, len-i-2);
        }
        
        for(int i = 0; i < k; i++) {
            vec[i] = arr[i];
        }
        return vec;
    }
    
    public void buildMaxHeap(int[] arr) {
        int len = arr.length;
        for(int i = (len - 2) / 2; i >= 0; i--) {
            heapify(arr, i, len-1);
        }
    }

    public void heapify(int[] arr, int index, int end) {
        int left = index * 2 + 1;
        int right = left + 1;
        while(left <= end) {
            int maxIndex = index;
            if(arr[left] > arr[maxIndex]) {
                maxIndex = left;
            }
            if(right <= end && arr[right] > arr[maxIndex]) {
                maxIndex = right;
            }
            if(maxIndex == index) {
                break;
            }
            int temp = arr[maxIndex];
            arr[maxIndex]= arr[index];
            arr[index] = temp;
            index = maxIndex;
            left = index * 2 + 1;
            right = left + 1;
        }
    }
}

// 快排
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        int[] vec = new int[k];
        if (k == 0) { // 排除 0 的情况
            return vec;
        }
        int len = arr.length;
        quickSort(arr, 0, len-1);
        for(int i = 0; i < k; i++) {
            vec[i] = arr[i];
        }
        return vec;
    }

    public int partion(int[] arr, int left, int right) {
        int key = arr[left];
        while(left < right) {
            while(left < right && arr[right] >= key) right--;
            arr[left] = arr[right];
            while(left < right && arr[left] <= key) left++;
            arr[right] = arr[left];
        }
        arr[left] = key;
        return left;
    }

    public void quickSort(int[] arr, int left, int right) {
        if(left >= right) {
            return;
        }
        int point = partion(arr, left, right);
        // 递归左（右）子数组执行哨兵划分
        quickSort(arr, left, point - 1);
        quickSort(arr, point + 1, right);
    }
};
```



### [1122. 数组的相对排序](https://leetcode.cn/problems/relative-sort-array/)

简单

相关企业

给你两个数组，`arr1` 和 `arr2`，`arr2` 中的元素各不相同，`arr2` 中的每个元素都出现在 `arr1` 中。

对 `arr1` 中的元素进行排序，使 `arr1` 中项的相对顺序和 `arr2` 中的相对顺序相同。未在 `arr2` 中出现过的元素需要按照升序放在 `arr1` 的末尾。

**示例 1：**

```
输入：arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
输出：[2,2,2,1,4,3,3,9,6,7,19]
```

**提示：**

- `1 <= arr1.length, arr2.length <= 1000`
- `0 <= arr1[i], arr2[i] <= 1000`

C++版本

```c++
// 计数排序
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        int max = 0, min = 1000;
        for(int num : arr1) {
            if(num >= max) {
                max = num;
            }
            if(num <= min) {
                min = num;
            }
        }
        int size = max - min + 1;
        vector<int> counts(size, 0);
        for(int num : arr1) {
            counts[num - min] += 1;
        }

        vector<int> res;
        for(int num : arr2) {
            while(counts[num - min]-- > 0) {
                res.push_back(num);
            }
        }

        for(int q = 0; q < size; q++) {
            while(counts[q]-- > 0) {
                res.push_back(q + min);
            }
        }
        return res;
    }
};
```

Java版本

```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        int max = 0, min = 1000;
        for(int num : arr1) {
            if(num >= max) {
                max = num;
            }
            if(num <= min) {
                min = num;
            }
        }
        int size = max - min + 1;
        int[] counts = new int[size];
        for(int num : arr1) {
            counts[num - min] += 1;
        }

        int[] res = new int[arr1.length];
        int index = 0;
        for(int num : arr2) {
            while(counts[num - min]-- > 0) {
                res[index++] = num;
            }
        }

        for(int q = 0; q < size; q++) {
            while(counts[q]-- > 0) {
                res[index++] = q + min;
            }
        }
        return res;
    }
}
```



### [220. 存在重复元素 III](https://leetcode.cn/problems/contains-duplicate-iii/)

困难

给你一个整数数组 `nums` 和两个整数 `indexDiff` 和 `valueDiff` 。

找出满足下述条件的下标对 `(i, j)`：

- `i != j`,
- `abs(i - j) <= indexDiff`
- `abs(nums[i] - nums[j]) <= valueDiff`

如果存在，返回 `true` *；*否则，返回 `false` 。

**示例 1：**

```
输入：nums = [1,2,3,1], indexDiff = 3, valueDiff = 0
输出：true
解释：可以找出 (i, j) = (0, 3) 。
满足下述 3 个条件：
i != j --> 0 != 3
abs(i - j) <= indexDiff --> abs(0 - 3) <= 3
abs(nums[i] - nums[j]) <= valueDiff --> abs(1 - 1) <= 0
```

C++版本

```c++
// 桶排序
class Solution {
public:
    int getID(int num, long valueDiff) {
        return num < 0 ? (num + 1ll) / valueDiff - 1 : num / valueDiff;
    }

    bool containsNearbyAlmostDuplicate(vector<int>& nums, int indexDiff, int valueDiff) {
        unordered_map<int, int> mp;
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            long x = nums[i];
            int id = getID(x, valueDiff + 1ll);
            if (mp.count(id)) {
                return true;
            }
            if (mp.count(id - 1) && abs(x - mp[id - 1]) <= valueDiff) {
                return true;
            }
            if (mp.count(id + 1) && abs(x - mp[id + 1]) <= valueDiff) {
                return true;
            }
            mp[id] = x;
            if (i >= indexDiff) {
                mp.erase(getID(nums[i - indexDiff], valueDiff + 1ll));
            }
        }
        return false;
    }
};

// 滑动窗口 + 有序集合
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        int n = nums.size();
        set<int> rec;
        for (int i = 0; i < n; i++) {
            auto iter = rec.lower_bound(max(nums[i], INT_MIN + t) - t);
            if (iter != rec.end() && *iter <= min(nums[i], INT_MAX - t) + t) {
                return true;
            }
            rec.insert(nums[i]);
            if (i >= k) {
                rec.erase(nums[i - k]);
            }
        }
        return false;
    }
};
```

Java版本

```java
// 桶排序
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        int n = nums.length;
        Map<Long, Long> map = new HashMap<Long, Long>();
        long w = (long) t + 1;
        for (int i = 0; i < n; i++) {
            long id = getID(nums[i], w);
            if (map.containsKey(id)) {
                return true;
            }
            if (map.containsKey(id - 1) && Math.abs(nums[i] - map.get(id - 1)) < w) {
                return true;
            }
            if (map.containsKey(id + 1) && Math.abs(nums[i] - map.get(id + 1)) < w) {
                return true;
            }
            map.put(id, (long) nums[i]);
            if (i >= k) {
                map.remove(getID(nums[i - k], w));
            }
        }
        return false;
    }

    public long getID(long x, long w) {
        if (x >= 0) {
            return x / w;
        }
        return (x + 1) / w - 1;
    }
}

// 滑动窗口 + 有序集合
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        int n = nums.length;
        TreeSet<Long> set = new TreeSet<Long>();
        for (int i = 0; i < n; i++) {
            Long ceiling = set.ceiling((long) nums[i] - (long) t);
            if (ceiling != null && ceiling <= (long) nums[i] + (long) t) {
                return true;
            }
            set.add((long) nums[i]);
            if (i >= k) {
                set.remove((long) nums[i - k]);
            }
        }
        return false;
    }
}
```



### [164. 最大间距](https://leetcode.cn/problems/maximum-gap/)

困难

给定一个无序的数组 `nums`，返回 *数组在排序之后，相邻元素之间最大的差值* 。如果数组元素个数小于 2，则返回 `0` 。

您必须编写一个在「线性时间」内运行并使用「线性额外空间」的算法。

**示例 1:**

```
输入: nums = [3,6,9,1]
输出: 3
解释: 排序后的数组是 [1,3,6,9], 其中相邻元素 (3,6) 和 (6,9) 之间都存在最大差值 3。
```

C++版本

```c++
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        int len = nums.size();
        if(len < 2) {
            return 0;
        }
        radixSort(nums);
        int maximumGap = 0;
        for(int i = 1; i < len; i++) {
            maximumGap = maximumGap > nums[i] - nums[i-1] ? maximumGap : nums[i] - nums[i-1];
        }
        return maximumGap;
    }

    // 获取num的第digit位数字
    int getDigit(int num, int digit) {
        return (num / static_cast<int>(pow(10, digit - 1))) % 10;
    }

    // 基数排序
    void radixSort(vector<int>& arr) {
        int maxVal = *max_element(arr.begin(), arr.end());
        int digits = 0;
        while (maxVal > 0) {
            maxVal /= 10;
            digits++;
    	}

    	vector<int> count(10, 0);
    	vector<int> output(arr.size());

    	for (int i = 1; i <= digits; ++i) {
        	fill(count.begin(), count.end(), 0);

        	// 统计每个位的数字出现次数
        	for (int j = 0; j < arr.size(); ++j) {
            	int digit = getDigit(arr[j], i);
            	count[digit]++;
        	}

        	// 将count[i]更新为包含小于等于i的元素个数
        	for (int j = 1; j < count.size(); ++j) {
            	count[j] += count[j - 1];
        	}

        	// 根据当前位的数字重新排序元素
        	for (int j = arr.size() - 1; j >= 0; --j) {
            	int digit = getDigit(arr[j], i);
            	output[count[digit] - 1] = arr[j];
            	count[digit]--;
        	}
        	// 将输出结果复制回原数组，准备下一轮排序
        	copy(output.begin(), output.end(), arr.begin());
    	}
	}
};
```

Java版本

```java
class Solution {
    public int maximumGap(int[] nums) {
        int n = nums.length;
        if (n < 2) {
            return 0;
        }
        long exp = 1;
        int[] buf = new int[n];
        int maxVal = Arrays.stream(nums).max().getAsInt();

        while (maxVal >= exp) {
            int[] cnt = new int[10];
            for (int i = 0; i < n; i++) {
                int digit = (nums[i] / (int) exp) % 10;
                cnt[digit]++;
            }
            for (int i = 1; i < 10; i++) {
                cnt[i] += cnt[i - 1];
            }
            for (int i = n - 1; i >= 0; i--) {
                int digit = (nums[i] / (int) exp) % 10;
                buf[cnt[digit] - 1] = nums[i];
                cnt[digit]--;
            }
            System.arraycopy(buf, 0, nums, 0, n);
            exp *= 10;
        }

        int ret = 0;
        for (int i = 1; i < n; i++) {
            ret = Math.max(ret, nums[i] - nums[i - 1]);
        }
        return ret;
    }
}
```



### [217. 存在重复元素](https://leetcode.cn/problems/contains-duplicate/)

简单

给你一个整数数组 `nums` 。如果任一值在数组中出现 **至少两次** ，返回 `true` ；如果数组中每个元素互不相同，返回 `false` 。

**示例 1：**

```
输入：nums = [1,2,3,1]
输出：true
```

C++版本

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> s;
        for (int x: nums) {
            if (s.find(x) != s.end()) {
                return true;
            }
            s.insert(x);
        }
        return false;
    }
};
```

Java版本

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        for (int x : nums) {
            if (!set.add(x)) {
                return true;
            }
        }
        return false;
    }
}
```



### [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

简单

给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

**示例 1 ：**

```
输入：nums = [2,2,1]
输出：1
```

C++版本

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        for(int i = 1; i < nums.size(); i++) {
            nums[0] ^= nums[i];
        }
        return nums[0];
    }
};
```

Java版本

```java
class Solution {
    public int singleNumber(int[] nums) {
        for(int i = 1; i < nums.length; i++) {
            nums[0] ^= nums[i];
        }
        return nums[0];
    }
}
```



### [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

中等

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

**示例 1：**

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

C++版本

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        int len = intervals.size();
        vector<vector<int>> merge;
        int left = 0, right;
        while(left < len) {
            right = left + 1;
            while(right < len && intervals[right][0] <= intervals[right-1][1]) {
                if(intervals[right][1] < intervals[right-1][1]) {
                    intervals[right][1] = intervals[right-1][1];
                }
                right++;
            }
            merge.push_back({intervals[left][0], intervals[right-1][1]});
            left = right;
        }
        return merge;
    }
};
```

Java版本

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (x, y) -> x[0] - y[0]);
        ArrayList<int []>  merge = new ArrayList<>();
        int len = intervals.length;
        int left = 0, right;
        while(left < len) {
            right = left + 1;
            while(right < len && intervals[right][0] <= intervals[right-1][1]) {
                if(intervals[right][1] < intervals[right-1][1]) {
                    intervals[right][1] = intervals[right-1][1];
                }
                right++;
            }
            merge.add(new int[]{intervals[left][0], intervals[right-1][1]});
            left = right;
        }
        return merge.toArray(new int[merge.size()][]);
    }
}
```



### [179. 最大数](https://leetcode.cn/problems/largest-number/)

中等

给定一组非负整数 `nums`，重新排列每个数的顺序（每个数不可拆分）使之组成一个最大的整数。

**注意：**输出结果可能非常大，所以你需要返回一个字符串而不是整数。

**示例 1：**

```
输入：nums = [10,2]
输出："210"
```

C++版本

```c++
// 自定义快速排序
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        int len = nums.size();
        vector<string> strs;
        for(int i = 0; i < len; i++) {
            strs.push_back(to_string(nums[i]));
        }
        quickSort(strs, 0, len-1);
        if(strs[0] == "0") {
            return "0";
        }
        string res;
        for(int i = 0; i < len; i++) {
            res.append(strs[i]);
        }
        return res;
    }

    void quickSort(vector<string>& strs, int left, int right) {
        if(left >= right) {
            return;
        }
        int pivot = partion(strs, left, right);
        quickSort(strs, left, pivot-1);
        quickSort(strs, pivot+1, right);
    }

    int partion(vector<string>& strs, int left, int right) {
        int index = rand() % (right - left + 1) + left;
        swap(strs[left], strs[index]);
        string key = strs[left];
        while(left < right) {
            while(left < right && strs[right] + key <= key + strs[right]) right--;
            strs[left] = strs[right];
            while(left < right && strs[left] + key >= key + strs[left]) left++;
            strs[right] = strs[left];
        }
        strs[left] = key;
        return left;
    }
};

//调用库函数自定义排序
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        vector<string> cpy;
        for(auto x: nums) {
            cpy.push_back(to_string(x));
        }
        sort(cpy.begin(), cpy.end(), [](const string& x, const string& y){ 
            return x + y > y + x; 
        });
        if(cpy[0] == "0") {
            return "0";
        }
        string ans;
        for(auto& x: cpy) {
            ans += x;
        }
        return ans;
    }
};
```

Java版本

```java
class Solution {
    public String largestNumber(int[] nums) {
        /*
        自定义排序规则
         */
        int n = nums.length;
        String[] ss = new String[n];
        for (int i = 0; i < n; i++) {
            ss[i] = String.valueOf(nums[i]);
        }
        // 排序 显然当b>a时，(b + a).compareTo(a + b)返回1触发交换，因此目标是最大数
        Arrays.sort(ss, (a, b) -> (b + a).compareTo(a + b));
        // 利用""拼接
        String res = String.join("", ss);
        // 要注意排除"000"的情况
        return res.charAt(0) == '0' ? "0" : res;
    }
}
```



### [384. 打乱数组](https://leetcode.cn/problems/shuffle-an-array/)

中等

给你一个整数数组 `nums` ，设计算法来打乱一个没有重复元素的数组。打乱后，数组的所有排列应该是 **等可能** 的。

实现 `Solution` class:

- `Solution(int[] nums)` 使用整数数组 `nums` 初始化对象
- `int[] reset()` 重设数组到它的初始状态并返回
- `int[] shuffle()` 返回数组随机打乱后的结果

**示例 1：**

```
输入
["Solution", "shuffle", "reset", "shuffle"]
[[[1, 2, 3]], [], [], []]
输出
[null, [3, 1, 2], [1, 2, 3], [1, 3, 2]]

解释
Solution solution = new Solution([1, 2, 3]);
solution.shuffle();    // 打乱数组 [1,2,3] 并返回结果。任何 [1,2,3]的排列返回的概率应该相同。例如，返回 [3, 1, 2]
solution.reset();      // 重设数组到它的初始状态 [1, 2, 3] 。返回 [1, 2, 3]
solution.shuffle();    // 随机返回数组 [1, 2, 3] 打乱后的结果。例如，返回 [1, 3, 2]
```

C++版本

```c++
class Solution {
public:
    Solution(vector<int>& nums) {
        this->nums = nums;
        this->original.resize(nums.size());
        copy(nums.begin(), nums.end(), original.begin());
    }
    
    vector<int> reset() {
        copy(original.begin(), original.end(), nums.begin());
        return nums;
    }
    
    vector<int> shuffle() {
        for (int i = 0; i < nums.size(); ++i) {
            int j = i + rand() % (nums.size() - i);
            swap(nums[i], nums[j]);
        }
        return nums;
    }
private:
    vector<int> nums;
    vector<int> original;
};
```

Java版本

```java
class Solution {
    int[] nums;
    int[] original;

    public Solution(int[] nums) {
        this.nums = nums;
        this.original = new int[nums.length];
        System.arraycopy(nums, 0, original, 0, nums.length);
    }
    
    public int[] reset() {
        System.arraycopy(original, 0, nums, 0, nums.length);
        return nums;
    }
    
    public int[] shuffle() {
        Random random = new Random();
        for (int i = 0; i < nums.length; ++i) {
            int j = i + random.nextInt(nums.length - i);
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
        return nums;
    }
}
```



## 二分查找题目

### [704. 二分查找](https://leetcode.cn/problems/binary-search/)

简单

给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target` ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回 `-1`。
**示例 1:**

```
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
```

C++版本

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size()-1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] == target) {
                return mid;
            } else if(nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
};
```

Java版本

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length-1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] == target) {
                return mid;
            } else if(nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```



### [374. 猜数字大小](https://leetcode.cn/problems/guess-number-higher-or-lower/)

简单

猜数字游戏的规则如下：

- 每轮游戏，我都会从 **1** 到 ***n*** 随机选择一个数字。 请你猜选出的是哪个数字。
- 如果你猜错了，我会告诉你，你猜测的数字比我选出的数字是大了还是小了。

你可以通过调用一个预先定义好的接口 `int guess(int num)` 来获取猜测结果，返回值一共有 3 种可能的情况（`-1`，`1` 或 `0`）：

- -1：我选出的数字比你猜的数字小 `pick < num`
- 1：我选出的数字比你猜的数字大 `pick > num`
- 0：我选出的数字和你猜的数字一样。恭喜！你猜对了！`pick == num`

返回我选出的数字。

**示例 1：**

```
输入：n = 10, pick = 6
输出：6
```

C++版本

```c++
/** 
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0
 * int guess(int num);
 */

class Solution {
public:
    int guessNumber(int n) {
        int m = 1;
        while(m <= n) {
            int mid = m + (n - m) / 2;
            if(guess(mid) == 0) {
                return mid;
            } else if(guess(mid) == -1) {
                n = mid - 1;
            } else {
                m = mid + 1;
            }
        }
        return 0;
    }
};
```

Java版本

```java
/** 
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0
 * int guess(int num);
 */

public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int m = 1;
        while(m <= n) {
            int mid = m + (n - m) / 2;
            if(guess(mid) == 0) {
                return mid;
            } else if(guess(mid) == -1) {
                n = mid - 1;
            } else {
                m = mid + 1;
            }
        }
        return 0;
    }
}
```



### [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

简单

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

**示例 1:**

```
输入: nums = [1,3,5,6], target = 5
输出: 2
```

C++版本

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1, res = nums.size();
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(target <= nums[mid]) {
                res = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return res;
    }
};
```

Java版本

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int n = nums.length;
        int left = 0, right = n - 1, ans = n;
        while (left <= right) {
            int mid = ((right - left) >> 1) + left;
            if (target <= nums[mid]) {
                ans = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }
}
```



### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

中等

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

C++版本

```c++
class Solution { 
public:
    int binarySearch(vector<int>& nums, int target, bool lower) {
        int left = 0, right = (int)nums.size() - 1, ans = (int)nums.size();
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
                ans = mid;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        int leftIdx = binarySearch(nums, target, true);
        int rightIdx = binarySearch(nums, target, false) - 1;
        if (leftIdx <= rightIdx && rightIdx < nums.size() && nums[leftIdx] == target && nums[rightIdx] == target) {
            return vector<int>{leftIdx, rightIdx};
        } 
        return vector<int>{-1, -1};
    }
};
```

Java版本

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int leftIdx = binarySearch(nums, target, true);
        int rightIdx = binarySearch(nums, target, false) - 1;
        if (leftIdx <= rightIdx && rightIdx < nums.length && nums[leftIdx] == target && nums[rightIdx] == target) {
            return new int[]{leftIdx, rightIdx};
        } 
        return new int[]{-1, -1};
    }

    public int binarySearch(int[] nums, int target, boolean lower) {
        int left = 0, right = nums.length - 1, ans = nums.length;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
                ans = mid;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }
}
```



### [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

中等

给你一个下标从 **1** 开始的整数数组 `numbers` ，该数组已按 **非递减顺序排列** ，请你从数组中找出满足相加之和等于目标数 `target` 的两个数。如果设这两个数分别是 `numbers[index1]` 和 `numbers[index2]` ，则 `1 <= index1 < index2 <= numbers.length` 。

以长度为 2 的整数数组 `[index1, index2]` 的形式返回这两个整数的下标 `index1` 和 `index2`。

你可以假设每个输入 **只对应唯一的答案** ，而且你 **不可以** 重复使用相同的元素。

你所设计的解决方案必须只使用常量级的额外空间。

**示例 1：**

```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

C++版本

```c++
// 二分查找
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int len  = numbers.size();
        for(int i = 0; i < len; i++) {
            int left = i+1, right = len-1, res = i;
            while(left <= right) {
                int mid = left + (right - left) / 2;
                if(target - numbers[i] <= numbers[mid]) {
                    res = mid;
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            if(numbers[res] == target - numbers[i]) {
                return vector<int>{i+1, res+1};
            }
        }
        return vector<int>{-1, -1};
    }
};

// 双指针
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int len  = numbers.size();
        int left = 0, right = len-1;
        while(left < right) {
            if(numbers[left] + numbers[right] == target) {
                return vector<int>{left+1, right+1};
            } else if(numbers[left] + numbers[right] < target) {
                left++;
            } else {
                right--;
            }
        }
        return vector<int>{-1, -1};
    }
};
```

Java版本

```java
// 二分查找
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        for (int i = 0; i < numbers.length; ++i) {
            int low = i + 1, high = numbers.length - 1;
            while (low <= high) {
                int mid = (high - low) / 2 + low;
                if (numbers[mid] == target - numbers[i]) {
                    return new int[]{i + 1, mid + 1};
                } else if (numbers[mid] > target - numbers[i]) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            }
        }
        return new int[]{-1, -1};
    }
}

// 双指针
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int low = 0, high = numbers.length - 1;
        while (low < high) {
            int sum = numbers[low] + numbers[high];
            if (sum == target) {
                return new int[]{low + 1, high + 1};
            } else if (sum < target) {
                ++low;
            } else {
                --high;
            }
        }
        return new int[]{-1, -1};
    }
}
```



### [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

中等

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,2]`
- 若旋转 `7` 次，则可以得到 `[0,1,2,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个元素值 **互不相同** 的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。
```

C++版本

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] < nums[right]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return nums[left];
    }
};
```

Java版本

```java
class Solution {
    public int findMin(int[] nums) {
        int low = 0;
        int high = nums.length - 1;
        while (low < high) {
            int pivot = low + (high - low) / 2;
            if (nums[pivot] < nums[high]) {
                high = pivot;
            } else {
                low = pivot + 1;
            }
        }
        return nums[low];
    }
}
```





























































































































































































































































































































































































































