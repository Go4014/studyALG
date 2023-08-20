# 数组

## 数组操作

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



## 二维数组

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



## 数组排序

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



## 二分查找

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



### [154. 寻找旋转排序数组中的最小值 II](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array-ii/)

困难

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,4,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,4]`
- 若旋转 `7` 次，则可以得到 `[0,1,4,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个可能存在 **重复** 元素值的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

你必须尽可能减少整个过程的操作步骤。

**示例 1：**

```
输入：nums = [1,3,5]
输出：1
```

C++版本

```c++
// 二分法
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0, right = nums.size()-1;
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] < nums[right]) {
                right = mid;
            } else if(nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right--;
            }
        }
        return nums[left];
    }
};

// 分治法
class Solution {
public:
    int findMin(vector<int>& nums) {
        return getMin(nums, 0, nums.size()-1);
    }

    int getMin(vector<int>& nums, int left, int right) {
        if(left == right) {
            return nums[left];
        }
        int mid = left + (right - left) / 2;
        int x = getMin(nums, left, mid);
        int y = getMin(nums, mid+1, right);
        return x > y ? y : x;
    }
};
```

Java版本

```java
// 二分法
class Solution {
    public int findMin(int[] nums) {
        int low = 0;
        int high = nums.length - 1;
        while (low < high) {
            int pivot = low + (high - low) / 2;
            if (nums[pivot] < nums[high]) {
                high = pivot;
            } else if (nums[pivot] > nums[high]) {
                low = pivot + 1;
            } else {
                high -= 1;
            }
        }
        return nums[low];
    }
}

// 分治法
class Solution {
    public int findMin(int[] nums) {
        return getMin(nums, 0, nums.length-1);
    }

    int getMin(int[] nums, int left, int right) {
        if(left == right) {
            return nums[left];
        }
        int mid = left + (right - left) / 2;
        int x = getMin(nums, left, mid);
        int y = getMin(nums, mid+1, right);
        return x > y ? y : x;
    }
}
```



### [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

中等

整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

C++版本

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = (int)nums.size();
        if (!n) {
            return -1;
        }
        if (n == 1) {
            return nums[0] == target ? 0 : -1;
        }
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (nums[mid] == target) return mid;
            if (nums[0] <= nums[mid]) {
                if (nums[0] <= target && target < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[n - 1]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
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
        int n = nums.length;
        if (n == 0) {
            return -1;
        }
        if (n == 1) {
            return nums[0] == target ? 0 : -1;
        }
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[0] <= nums[mid]) {
                if (nums[0] <= target && target < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[n - 1]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }
        return -1;
    }
}
```



### [81. 搜索旋转排序数组 II](https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/)

中等

已知存在一个按非降序排列的整数数组 `nums` ，数组中的值不必互不相同。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转** ，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,4,4,5,6,6,7]` 在下标 `5` 处经旋转后可能变为 `[4,5,6,6,7,0,1,2,4,4]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，请你编写一个函数来判断给定的目标值是否存在于数组中。如果 `nums` 中存在这个目标值 `target` ，则返回 `true` ，否则返回 `false` 。

你必须尽可能减少整个操作步骤。

**示例 1：**

```
输入：nums = [2,5,6,0,0,1,2], target = 0
输出：true
```

C++版本

```c++
class Solution {
public:
    bool search(vector<int> &nums, int target) {
        int n = nums.size();
        if (n == 0) {
            return false;
        }
        if (n == 1) {
            return nums[0] == target;
        }
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (nums[mid] == target) {
                return true;
            }
            if (nums[l] == nums[mid] && nums[mid] == nums[r]) {
                ++l;
                --r;
            } else if (nums[l] <= nums[mid]) {
                if (nums[l] <= target && target < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[n - 1]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }
        return false;
    }
};
```

Java版本

```java
class Solution {
    public boolean search(int[] nums, int target) {
        int n = nums.length;
        if (n == 0) {
            return false;
        }
        if (n == 1) {
            return nums[0] == target;
        }
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (nums[mid] == target) {
                return true;
            }
            if (nums[l] == nums[mid] && nums[mid] == nums[r]) {
                ++l;
                --r;
            } else if (nums[l] <= nums[mid]) {
                if (nums[l] <= target && target < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[n - 1]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }
        return false;
    }
}
```



### [278. 第一个错误的版本](https://leetcode.cn/problems/first-bad-version/)

简单

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 `n` 个版本 `[1, 2, ..., n]`，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 `bool isBadVersion(version)` 接口来判断版本号 `version` 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

**示例 1：**

```
输入：n = 5, bad = 4
输出：4
解释：
调用 isBadVersion(3) -> false 
调用 isBadVersion(5) -> true 
调用 isBadVersion(4) -> true
所以，4 是第一个错误的版本。
```

C++版本

```c++
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int left = 1, right = n;
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(isBadVersion(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

Java版本

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int left = 1, right = n;
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(isBadVersion(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```



### [162. 寻找峰值](https://leetcode.cn/problems/find-peak-element/)

中等

峰值元素是指其值严格大于左右相邻值的元素。

给你一个整数数组 `nums`，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 **任何一个峰值** 所在位置即可。

你可以假设 `nums[-1] = nums[n] = -∞` 。

你必须实现时间复杂度为 `O(log n)` 的算法来解决此问题。

**示例 1：**

```
输入：nums = [1,2,3,1]
输出：2
解释：3 是峰值元素，你的函数应该返回其索引 2。
```

C++版本

```c++
// 迭代爬坡
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        int idx = rand() % n;

        // 辅助函数，输入下标 i，返回一个二元组 (0/1, nums[i])
        // 方便处理 nums[-1] 以及 nums[n] 的边界情况
        auto get = [&](int i) -> pair<int, int> {
            if (i == -1 || i == n) {
                return {0, 0};
            }
            return {1, nums[i]};
        };

        while (!(get(idx - 1) < get(idx) && get(idx) > get(idx + 1))) {
            if (get(idx) < get(idx + 1)) {
                idx += 1;
            }
            else {
                idx -= 1;
            }
        }
        
        return idx;
    }
};

// 迭代爬坡的二分查找优化
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();

        // 辅助函数，输入下标 i，返回一个二元组 (0/1, nums[i])
        // 方便处理 nums[-1] 以及 nums[n] 的边界情况
        auto get = [&](int i) -> pair<int, int> {
            if (i == -1 || i == n) {
                return {0, 0};
            }
            return {1, nums[i]};
        };

        int left = 0, right = n - 1, ans = -1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (get(mid - 1) < get(mid) && get(mid) > get(mid + 1)) {
                ans = mid;
                break;
            }
            if (get(mid) < get(mid + 1)) {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }
        return ans;
    }
};
```

Java版本

```java
// 迭代爬坡
class Solution {
    public int findPeakElement(int[] nums) {
        int n = nums.length;
        int idx = (int) (Math.random() * n);

        while (!(compare(nums, idx - 1, idx) < 0 && compare(nums, idx, idx + 1) > 0)) {
            if (compare(nums, idx, idx + 1) < 0) {
                idx += 1;
            } else {
                idx -= 1;
            }
        }
        
        return idx;
    }

    // 辅助函数，输入下标 i，返回一个二元组 (0/1, nums[i])
    // 方便处理 nums[-1] 以及 nums[n] 的边界情况
    public int[] get(int[] nums, int idx) {
        if (idx == -1 || idx == nums.length) {
            return new int[]{0, 0};
        }
        return new int[]{1, nums[idx]};
    }

    public int compare(int[] nums, int idx1, int idx2) {
        int[] num1 = get(nums, idx1);
        int[] num2 = get(nums, idx2);
        if (num1[0] != num2[0]) {
            return num1[0] > num2[0] ? 1 : -1;
        }
        if (num1[1] == num2[1]) {
            return 0;
        }
        return num1[1] > num2[1] ? 1 : -1;
    }
}

// 迭代爬坡的二分查找优化
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();

        // 辅助函数，输入下标 i，返回一个二元组 (0/1, nums[i])
        // 方便处理 nums[-1] 以及 nums[n] 的边界情况
        auto get = [&](int i) -> pair<int, int> {
            if (i == -1 || i == n) {
                return {0, 0};
            }
            return {1, nums[i]};
        };

        int left = 0, right = n - 1, ans = -1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (get(mid - 1) < get(mid) && get(mid) > get(mid + 1)) {
                ans = mid;
                break;
            }
            if (get(mid) < get(mid + 1)) {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }
        return ans;
    }
};
```



### [852. 山脉数组的峰顶索引](https://leetcode.cn/problems/peak-index-in-a-mountain-array/)

中等

符合下列属性的数组 `arr` 称为 **山脉数组** ：

- `arr.length >= 3`
- 存在`i`（`0 < i < arr.length - 1`）使得：
  - `arr[0] < arr[1] < ... arr[i-1] < arr[i] `
  - `arr[i] > arr[i+1] > ... > arr[arr.length - 1]`

给你由整数组成的山脉数组 `arr` ，返回满足 `arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1]` 的下标 `i` 。

你必须设计并实现时间复杂度为 `O(log(n))` 的解决方案。

**示例 1：**

```
输入：arr = [0,1,0]
输出：1
```

C++版本

```c++
// 二分查找
// 方法一
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int left = 1, right = arr.size() - 2;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(arr[mid-1] < arr[mid] && arr[mid] > arr[mid+1]) {
                return mid;
            } else if(arr[mid-1] < arr[mid]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
};

// 方法二
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int left = 1, right = arr.size() - 2, res = 0;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(arr[mid] > arr[mid+1]) {
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
// 二分查找
// 方法一
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int left = 1, right = arr.length - 2;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(arr[mid-1] < arr[mid] && arr[mid] > arr[mid+1]) {
                return mid;
            } else if(arr[mid-1] < arr[mid]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}

// 方法二
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int n = arr.length;
        int left = 1, right = n - 2, ans = 0;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (arr[mid] > arr[mid + 1]) {
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



### [1095. 山脉数组中查找目标值](https://leetcode.cn/problems/find-in-mountain-array/)

困难

给你一个 **山脉数组** `mountainArr`，请你返回能够使得 `mountainArr.get(index)` **等于** `target` **最小** 的下标 `index` 值。

如果不存在这样的下标 `index`，就请返回 `-1`。

何为山脉数组？如果数组 `A` 是一个山脉数组的话，那它满足如下条件：

**首先**，`A.length >= 3`

**其次**，在 `0 < i < A.length - 1` 条件下，存在 `i` 使得：

- `A[0] < A[1] < ... A[i-1] < A[i]`
- `A[i] > A[i+1] > ... > A[A.length - 1]`

你将 **不能直接访问该山脉数组**，必须通过 `MountainArray` 接口来获取数据：

- `MountainArray.get(k)` - 会返回数组中索引为`k` 的元素（下标从 0 开始）
- `MountainArray.length()` - 会返回该数组的长度

**示例 1：**

```
输入：array = [1,2,3,4,5,3,1], target = 3
输出：2
解释：3 在数组中出现了两次，下标分别为 2 和 5，我们返回最小的下标 2。
```

C++版本

```c++
/**
 * // This is the MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * class MountainArray {
 *   public:
 *     int get(int index);
 *     int length();
 * };
 */

class Solution {
public:
    int findInMountainArray(int target, MountainArray &mountainArr) {
        int left = 1, right = mountainArr.length() - 2, res = 0;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(mountainArr.get(mid) > mountainArr.get(mid+1)) {
                res = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        if(mountainArr.get(res) == target) {
            return res;
        }
        left = 0;
        right = res - 1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(mountainArr.get(mid) == target) {
                return mid;
            } else if(mountainArr.get(mid) > target){
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        left = res + 1;
        right = mountainArr.length() - 1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(mountainArr.get(mid) == target) {
                return mid;
            } else if(mountainArr.get(mid) > target){
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
/**
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface MountainArray {
 *     public int get(int index) {}
 *     public int length() {}
 * }
 */
 
class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        int left = 1, right = mountainArr.length() - 2, res = 0;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(mountainArr.get(mid) > mountainArr.get(mid+1)) {
                res = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        if(mountainArr.get(res) == target) {
            return res;
        }
        left = 0;
        right = res - 1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(mountainArr.get(mid) == target) {
                return mid;
            } else if(mountainArr.get(mid) > target){
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        left = res + 1;
        right = mountainArr.length() - 1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(mountainArr.get(mid) == target) {
                return mid;
            } else if(mountainArr.get(mid) > target){
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```



### [744. 寻找比目标字母大的最小字母](https://leetcode.cn/problems/find-smallest-letter-greater-than-target/)

简单

给你一个字符数组 `letters`，该数组按**非递减顺序**排序，以及一个字符 `target`。`letters` 里**至少有两个不同**的字符。

返回 `letters` 中大于 `target` 的最小的字符。如果不存在这样的字符，则返回 `letters` 的第一个字符。

**示例 1：**

```
输入: letters = ["c", "f", "j"]，target = "a"
输出: "c"
解释：letters 中字典上比 'a' 大的最小字符是 'c'。
```

C++版本

```c++
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        int left = 0, right = letters.size() - 1;
        if(target < letters[left++] || target >= letters[right++]) {
            return letters[0];
        }
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(target < letters[mid]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return letters[left];
    }
};
```

Java版本

```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int left = 0, right = letters.length - 1;
        if(target < letters[left++] || target >= letters[right++]) {
            return letters[0];
        }
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(target < letters[mid]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return letters[left];
    }
}
```



### [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

困难

给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

算法的时间复杂度应该为 `O(log (m+n))` 。

**示例 1：**

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

C++版本

```c++
// 二分查找
class Solution {
public:
    int getKthElement(const vector<int>& nums1, const vector<int>& nums2, int k) {
        /* 主要思路：要找到第 k (k>1) 小的元素，那么就取 pivot1 = nums1[k/2-1] 和 pivot2 = nums2[k/2-1] 进行比较
         * 这里的 "/" 表示整除
         * nums1 中小于等于 pivot1 的元素有 nums1[0 .. k/2-2] 共计 k/2-1 个
         * nums2 中小于等于 pivot2 的元素有 nums2[0 .. k/2-2] 共计 k/2-1 个
         * 取 pivot = min(pivot1, pivot2)，两个数组中小于等于 pivot 的元素共计不会超过 (k/2-1) + (k/2-1) <= k-2 个
         * 这样 pivot 本身最大也只能是第 k-1 小的元素
         * 如果 pivot = pivot1，那么 nums1[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums1 数组
         * 如果 pivot = pivot2，那么 nums2[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums2 数组
         * 由于我们 "删除" 了一些元素（这些元素都比第 k 小的元素要小），因此需要修改 k 的值，减去删除的数的个数
         */

        int m = nums1.size();
        int n = nums2.size();
        int index1 = 0, index2 = 0;

        while (true) {
            // 边界情况
            if (index1 == m) {
                return nums2[index2 + k - 1];
            }
            if (index2 == n) {
                return nums1[index1 + k - 1];
            }
            if (k == 1) {
                return min(nums1[index1], nums2[index2]);
            }

            // 正常情况
            int newIndex1 = min(index1 + k / 2 - 1, m - 1);
            int newIndex2 = min(index2 + k / 2 - 1, n - 1);
            int pivot1 = nums1[newIndex1];
            int pivot2 = nums2[newIndex2];
            if (pivot1 <= pivot2) {
                k -= newIndex1 - index1 + 1;
                index1 = newIndex1 + 1;
            }
            else {
                k -= newIndex2 - index2 + 1;
                index2 = newIndex2 + 1;
            }
        }
    }

    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int totalLength = nums1.size() + nums2.size();
        if (totalLength % 2 == 1) {
            return getKthElement(nums1, nums2, (totalLength + 1) / 2);
        }
        else {
            return (getKthElement(nums1, nums2, totalLength / 2) + getKthElement(nums1, nums2, totalLength / 2 + 1)) / 2.0;
        }
    }
};

// 划分数组
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size() > nums2.size()) {
            return findMedianSortedArrays(nums2, nums1);
        }
        
        int m = nums1.size();
        int n = nums2.size();
        int left = 0, right = m;
        // median1：前一部分的最大值
        // median2：后一部分的最小值
        int median1 = 0, median2 = 0;

        while (left <= right) {
            // 前一部分包含 nums1[0 .. i-1] 和 nums2[0 .. j-1]
            // 后一部分包含 nums1[i .. m-1] 和 nums2[j .. n-1]
            int i = (left + right) / 2;
            int j = (m + n + 1) / 2 - i;

            // nums_im1, nums_i, nums_jm1, nums_j 分别表示 nums1[i-1], nums1[i], nums2[j-1], nums2[j]
            int nums_im1 = (i == 0 ? INT_MIN : nums1[i - 1]);
            int nums_i = (i == m ? INT_MAX : nums1[i]);
            int nums_jm1 = (j == 0 ? INT_MIN : nums2[j - 1]);
            int nums_j = (j == n ? INT_MAX : nums2[j]);

            if (nums_im1 <= nums_j) {
                median1 = max(nums_im1, nums_jm1);
                median2 = min(nums_i, nums_j);
                left = i + 1;
            } else {
                right = i - 1;
            }
        }

        return (m + n) % 2 == 0 ? (median1 + median2) / 2.0 : median1;
    }
};
```

Java版本

```java
// 二分查找
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int length1 = nums1.length, length2 = nums2.length;
        int totalLength = length1 + length2;
        if (totalLength % 2 == 1) {
            int midIndex = totalLength / 2;
            double median = getKthElement(nums1, nums2, midIndex + 1);
            return median;
        } else {
            int midIndex1 = totalLength / 2 - 1, midIndex2 = totalLength / 2;
            double median = (getKthElement(nums1, nums2, midIndex1 + 1) + getKthElement(nums1, nums2, midIndex2 + 1)) / 2.0;
            return median;
        }
    }

    public int getKthElement(int[] nums1, int[] nums2, int k) {
        /* 主要思路：要找到第 k (k>1) 小的元素，那么就取 pivot1 = nums1[k/2-1] 和 pivot2 = nums2[k/2-1] 进行比较
         * 这里的 "/" 表示整除
         * nums1 中小于等于 pivot1 的元素有 nums1[0 .. k/2-2] 共计 k/2-1 个
         * nums2 中小于等于 pivot2 的元素有 nums2[0 .. k/2-2] 共计 k/2-1 个
         * 取 pivot = min(pivot1, pivot2)，两个数组中小于等于 pivot 的元素共计不会超过 (k/2-1) + (k/2-1) <= k-2 个
         * 这样 pivot 本身最大也只能是第 k-1 小的元素
         * 如果 pivot = pivot1，那么 nums1[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums1 数组
         * 如果 pivot = pivot2，那么 nums2[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums2 数组
         * 由于我们 "删除" 了一些元素（这些元素都比第 k 小的元素要小），因此需要修改 k 的值，减去删除的数的个数
         */

        int length1 = nums1.length, length2 = nums2.length;
        int index1 = 0, index2 = 0;
        int kthElement = 0;

        while (true) {
            // 边界情况
            if (index1 == length1) {
                return nums2[index2 + k - 1];
            }
            if (index2 == length2) {
                return nums1[index1 + k - 1];
            }
            if (k == 1) {
                return Math.min(nums1[index1], nums2[index2]);
            }
            
            // 正常情况
            int half = k / 2;
            int newIndex1 = Math.min(index1 + half, length1) - 1;
            int newIndex2 = Math.min(index2 + half, length2) - 1;
            int pivot1 = nums1[newIndex1], pivot2 = nums2[newIndex2];
            if (pivot1 <= pivot2) {
                k -= (newIndex1 - index1 + 1);
                index1 = newIndex1 + 1;
            } else {
                k -= (newIndex2 - index2 + 1);
                index2 = newIndex2 + 1;
            }
        }
    }
}

// 划分数组
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int m = nums1.length;
        int n = nums2.length;
        int left = 0, right = m;
        // median1：前一部分的最大值
        // median2：后一部分的最小值
        int median1 = 0, median2 = 0;

        while (left <= right) {
            // 前一部分包含 nums1[0 .. i-1] 和 nums2[0 .. j-1]
            // 后一部分包含 nums1[i .. m-1] 和 nums2[j .. n-1]
            int i = (left + right) / 2;
            int j = (m + n + 1) / 2 - i;

            // nums_im1, nums_i, nums_jm1, nums_j 分别表示 nums1[i-1], nums1[i], nums2[j-1], nums2[j]
            int nums_im1 = (i == 0 ? Integer.MIN_VALUE : nums1[i - 1]);
            int nums_i = (i == m ? Integer.MAX_VALUE : nums1[i]);
            int nums_jm1 = (j == 0 ? Integer.MIN_VALUE : nums2[j - 1]);
            int nums_j = (j == n ? Integer.MAX_VALUE : nums2[j]);

            if (nums_im1 <= nums_j) {
                median1 = Math.max(nums_im1, nums_jm1);
                median2 = Math.min(nums_i, nums_j);
                left = i + 1;
            } else {
                right = i - 1;
            }
        }

        return (m + n) % 2 == 0 ? (median1 + median2) / 2.0 : median1;
    }
}
```



### [74. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

中等

给你一个满足下述两条属性的 `m x n` 整数矩阵：

- 每行中的整数从左到右按非递减顺序排列。
- 每行的第一个整数大于前一行的最后一个整数。

给你一个整数 `target` ，如果 `target` 在矩阵中，返回 `true` ；否则，返回 `false` 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
输出：true
```

C++版本

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n = matrix.size(), m = matrix[0].size();
        int left = 0, right = n*m - 1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(matrix[mid/m][mid%m] == target) {
                return true;
            } else if(matrix[mid/m][mid%m] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return false;
    }
};
```

Java版本

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int low = 0, high = m * n - 1;
        while (low <= high) {
            int mid = (high - low) / 2 + low;
            int x = matrix[mid / n][mid % n];
            if (x < target) {
                low = mid + 1;
            } else if (x > target) {
                high = mid - 1;
            } else {
                return true;
            }
        }
        return false;
    }
}
```



### [240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)

中等

编写一个高效的算法来搜索 `m x n` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/25/searchgrid2.jpg)

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true
```

C++版本

```c++
// 按行二分查找
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        for(int i = 0; i < m; i++) {
            int left = 0, right = matrix[i].size() - 1;
            while(left <= right) {
                int mid = left + (right - left) / 2;
                if(matrix[i][mid] == target) {
                    return true;
                } else if(matrix[i][mid] > target) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
        }
        return false;
    }
};
// Z 字形查找
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int x = 0, y = n - 1;
        while (x < m && y >= 0) {
            if (matrix[x][y] == target) {
                return true;
            }
            if (matrix[x][y] > target) {
                --y;
            }
            else {
                ++x;
            }
        }
        return false;
    }
};
```

Java版本

```java
// 按行二分查找
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        for(int i = 0; i < m; i++) {
            int left = 0, right = matrix[i].length - 1;
            while(left <= right) {
                int mid = left + (right - left) / 2;
                if(matrix[i][mid] == target) {
                    return true;
                } else if(matrix[i][mid] > target) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
        }
        return false;
    }
}

// Z 字形查找
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int x = 0, y = n - 1;
        while (x < m && y >= 0) {
            if (matrix[x][y] == target) {
                return true;
            }
            if (matrix[x][y] > target) {
                --y;
            } else {
                ++x;
            }
        }
        return false;
    }
}
```



### [69. x 的平方根](https://leetcode.cn/problems/sqrtx/)

简单

给你一个非负整数 `x` ，计算并返回 `x` 的 **算术平方根** 。

由于返回类型是整数，结果只保留 **整数部分** ，小数部分将被 **舍去 。**

**注意：**不允许使用任何内置指数函数和算符，例如 `pow(x, 0.5)` 或者 `x ** 0.5` 。

**示例 1：**

```
输入：x = 4
输出：2
```

C++版本

```c++
// 二分查找
class Solution {
public:
    int mySqrt(int x) {
        int left = 0, right = x, res = 0;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if((long)mid * mid <= x) {
                res = mid;
                left = mid + 1;;
            } else {
                right = mid - 1;
            }
        }
        return res;
    }
};

// 牛顿迭代
class Solution {
public:
    int mySqrt(int x) {
        if (x == 0) {
            return 0;
        }

        double C = x, x0 = x;
        while (true) {
            double xi = 0.5 * (x0 + C / x0);
            if (fabs(x0 - xi) < 1e-7) {
                break;
            }
            x0 = xi;
        }
        return int(x0);
    }
};
```

Java版本

```java
// 二分查找
class Solution {
    public int mySqrt(int x) {
        int left = 0, right = x, res = 0;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if((long)mid * mid <= x) {
                res = mid;
                left = mid + 1;;
            } else {
                right = mid - 1;
            }
        }
        return res;
    }
}

// 牛顿迭代
class Solution {
    public int mySqrt(int x) {
        if (x == 0) {
            return 0;
        }

        double C = x, x0 = x;
        while (true) {
            double xi = 0.5 * (x0 + C / x0);
            if (Math.abs(x0 - xi) < 1e-7) {
                break;
            }
            x0 = xi;
        }
        return (int) x0;
    }
}
```



### [287. 寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)

中等

给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。

假设 `nums` 只有 **一个重复的整数** ，返回 **这个重复的数** 。

你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。

**示例 1：**

```
输入：nums = [1,3,4,2,2]
输出：2
```

C++版本

```c++
// 方法一：二分查找
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n = nums.size();
        int l = 1, r = n - 1, ans = -1;
        while (l <= r) {
            int mid = (l + r) >> 1;
            int cnt = 0;
            for (int i = 0; i < n; ++i) {
                cnt += nums[i] <= mid;
            }
            if (cnt <= mid) {
                l = mid + 1;
            } else {
                r = mid - 1;
                ans = mid;
            }
        }
        return ans;
    }
};

// 方法二：二进制
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n = nums.size(), ans = 0;
        // 确定二进制下最高位是多少
        int bit_max = 31;
        while (!((n - 1) >> bit_max)) {
            bit_max -= 1;
        }
        for (int bit = 0; bit <= bit_max; ++bit) {
            int x = 0, y = 0;
            for (int i = 0; i < n; ++i) {
                if (nums[i] & (1 << bit)) {
                    x += 1;
                }
                if (i >= 1 && (i & (1 << bit))) {
                    y += 1;
                }
            }
            if (x > y) {
                ans |= 1 << bit;
            }
        }
        return ans;
    }
};

// 方法三：快慢指针
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0, fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
};
```

Java版本

```Java
// 方法一：二分查找
class Solution {
    public int findDuplicate(int[] nums) {
        int n = nums.length;
        int l = 1, r = n - 1, ans = -1;
        while (l <= r) {
            int mid = (l + r) >> 1;
            int cnt = 0;
            for (int i = 0; i < n; ++i) {
                if (nums[i] <= mid) {
                    cnt++;
                }
            }
            if (cnt <= mid) {
                l = mid + 1;
            } else {
                r = mid - 1;
                ans = mid;
            }
        }
        return ans;
    }
}

// 方法二：二进制
class Solution {
    public int findDuplicate(int[] nums) {
        int n = nums.length, ans = 0;
        int bit_max = 31;
        while (((n - 1) >> bit_max) == 0) {
            bit_max -= 1;
        }
        for (int bit = 0; bit <= bit_max; ++bit) {
            int x = 0, y = 0;
            for (int i = 0; i < n; ++i) {
                if ((nums[i] & (1 << bit)) != 0) {
                    x += 1;
                }
                if (i >= 1 && ((i & (1 << bit)) != 0)) {
                    y += 1;
                }
            }
            if (x > y) {
                ans |= 1 << bit;
            }
        }
        return ans;
    }
}

// 方法三：快慢指针
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = 0, fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
```



### [50. Pow(x, n)](https://leetcode.cn/problems/powx-n/)

中等

实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 `x` 的整数 `n` 次幂函数（即，`xn` ）。

**示例 1：**

```
输入：x = 2.00000, n = 10
输出：1024.00000
```

C++版本

```c++
// 方法一：快速幂 + 递归
class Solution {
public:
    double quickMul(double x, long long N) {
        if (N == 0) {
            return 1.0;
        }
        double y = quickMul(x, N / 2);
        return N % 2 == 0 ? y * y : y * y * x;
    }

    double myPow(double x, int n) {
        long long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
};

// 方法二：快速幂 + 迭代
class Solution {
public:
    double quickMul(double x, long long N) {
        double ans = 1.0;
        // 贡献的初始值为 x
        double x_contribute = x;
        // 在对 N 进行二进制拆分的同时计算答案
        while (N > 0) {
            if (N % 2 == 1) {
                // 如果 N 二进制表示的最低位为 1，那么需要计入贡献
                ans *= x_contribute;
            }
            // 将贡献不断地平方
            x_contribute *= x_contribute;
            // 舍弃 N 二进制表示的最低位，这样我们每次只要判断最低位即可
            N /= 2;
        }
        return ans;
    }

    double myPow(double x, int n) {
        long long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
};
```

Java版本

```java
// 方法一：快速幂 + 递归
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }

    public double quickMul(double x, long N) {
        if (N == 0) {
            return 1.0;
        }
        double y = quickMul(x, N / 2);
        return N % 2 == 0 ? y * y : y * y * x;
    }
}

// 方法二：快速幂 + 迭代
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }

    public double quickMul(double x, long N) {
        double ans = 1.0;
        // 贡献的初始值为 x
        double x_contribute = x;
        // 在对 N 进行二进制拆分的同时计算答案
        while (N > 0) {
            if (N % 2 == 1) {
                // 如果 N 二进制表示的最低位为 1，那么需要计入贡献
                ans *= x_contribute;
            }
            // 将贡献不断地平方
            x_contribute *= x_contribute;
            // 舍弃 N 二进制表示的最低位，这样我们每次只要判断最低位即可
            N /= 2;
        }
        return ans;
    }
}
```



### [367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

简单

给你一个正整数 `num` 。如果 `num` 是一个完全平方数，则返回 `true` ，否则返回 `false` 。

**完全平方数** 是一个可以写成某个整数的平方的整数。换句话说，它可以写成某个整数和自身的乘积。

不能使用任何内置的库函数，如 `sqrt` 。

**示例 1：**

```
输入：num = 16
输出：true
解释：返回 true ，因为 4 * 4 = 16 且 4 是一个整数。
```

C++版本

```
class Solution {
public:
    bool isPerfectSquare(int num) {
        int left = 0, right = num;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if((long)mid * mid < num) {
                left = mid + 1;
            } else if((long)mid * mid > num){
                right = mid - 1;
            } else {
                return true;
            }
        }
        return false;
    }
};
```

Java版本

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        int left = 0, right = num;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if((long)mid * mid < num) {
                left = mid + 1;
            } else if((long)mid * mid > num){
                right = mid - 1;
            } else {
                return true;
            }
        }
        return false;
    }
}
```



### [1300. 转变数组后最接近目标值的数组和](https://leetcode.cn/problems/sum-of-mutated-array-closest-to-target/)

中等

给你一个整数数组 `arr` 和一个目标值 `target` ，请你返回一个整数 `value` ，使得将数组中所有大于 `value` 的值变成 `value` 后，数组的和最接近 `target` （最接近表示两者之差的绝对值最小）。

如果有多种使得和最接近 `target` 的方案，请你返回这些整数中的最小值。

请注意，答案不一定是 `arr` 中的数字。

**示例 1：**

```
输入：arr = [4,9,3], target = 10
输出：3
解释：当选择 value 为 3 时，数组会变成 [3, 3, 3]，和为 9 ，这是最接近 target 的方案。
```

C++版本

```c++
// 方法一：枚举 + 二分查找
class Solution {
public:
    int findBestValue(vector<int>& arr, int target) {
        sort(arr.begin(), arr.end());
        int n = arr.size();
        vector<int> prefix(n + 1);
        for (int i = 1; i <= n; ++i) {
            prefix[i] = prefix[i - 1] + arr[i - 1];
        }

        int r = *max_element(arr.begin(), arr.end());
        int ans = 0, diff = target;
        for (int i = 1; i <= r; ++i) {
            auto iter = lower_bound(arr.begin(), arr.end(), i);
            int cur = prefix[iter - arr.begin()] + (arr.end() - iter) * i;
            if (abs(cur - target) < diff) {
                ans = i;
                diff = abs(cur - target);
            }
        }
        return ans;
    }
};

// 方法二：双重二分查找
class Solution {
public:
    int check(const vector<int>& arr, int x) {
        int ret = 0;
        for (const int& num: arr) {
            ret += (num >= x ? x : num);
        }
        return ret;
    }

    int findBestValue(vector<int>& arr, int target) {
        sort(arr.begin(), arr.end());
        int n = arr.size();
        vector<int> prefix(n + 1);
        for (int i = 1; i <= n; ++i) {
            prefix[i] = prefix[i - 1] + arr[i - 1];
        }

        int l = 0, r = *max_element(arr.begin(), arr.end()), ans = -1;
        while (l <= r) {
            int mid = (l + r) / 2;
            auto iter = lower_bound(arr.begin(), arr.end(), mid);
            int cur = prefix[iter - arr.begin()] + (arr.end() - iter) * mid;
            if (cur <= target) {
                ans = mid;
                l = mid + 1;
            }
            else {
                r = mid - 1;
            }
        }
        int choose_small = check(arr, ans);
        int choose_big = check(arr, ans + 1);
        return abs(choose_small - target) <= abs(choose_big - target) ? ans : ans + 1;
    }
};
```

Java版本

```java
// 方法一：枚举 + 二分查找
class Solution {
    public int findBestValue(int[] arr, int target) {
        Arrays.sort(arr);
        int n = arr.length;
        int[] prefix = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            prefix[i] = prefix[i - 1] + arr[i - 1];
        }
        int r = arr[n - 1];
        int ans = 0, diff = target;
        for (int i = 1; i <= r; ++i) {
            int index = Arrays.binarySearch(arr, i);
            if (index < 0) {
                index = -index - 1;
            }
            int cur = prefix[index] + (n - index) * i;
            if (Math.abs(cur - target) < diff) {
                ans = i;
                diff = Math.abs(cur - target);
            }
        }
        return ans;
    }
}

// 方法二：双重二分查找
class Solution {
    public int findBestValue(int[] arr, int target) {
        Arrays.sort(arr);
        int n = arr.length;
        int[] prefix = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            prefix[i] = prefix[i - 1] + arr[i - 1];
        }
        int l = 0, r = arr[n - 1], ans = -1;
        while (l <= r) {
            int mid = (l + r) / 2;
            int index = Arrays.binarySearch(arr, mid);
            if (index < 0) {
                index = -index - 1;
            }
            int cur = prefix[index] + (n - index) * mid;
            if (cur <= target) {
                ans = mid;
                l = mid + 1;
            }
            else {
                r = mid - 1;
            }
        }
        int chooseSmall = check(arr, ans);
        int chooseBig = check(arr, ans + 1);
        return Math.abs(chooseSmall - target) <= Math.abs(chooseBig - target) ? ans : ans + 1;
    }

    public int check(int[] arr, int x) {
        int ret = 0;
        for (int num : arr) {
            ret += Math.min(num, x);
        }
        return ret;
    }
}
```



### [400. 第 N 位数字](https://leetcode.cn/problems/nth-digit/)

中等

给你一个整数 `n` ，请你在无限的整数序列 `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...]` 中找出并返回第 `n` 位上的数字。

**示例 1：**

```
输入：n = 11
输出：0
解释：第 11 位数字在序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... 里是 0 ，它是 10 的一部分。
```

C++版本

```c++
// 方法一：二分查找
class Solution {
public:
    int findNthDigit(int n) {
        int low = 1, high = 9;
        while (low < high) {
            int mid = (high - low) / 2 + low;
            if (totalDigits(mid) < n) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        int d = low;
        int prevDigits = totalDigits(d - 1);
        int index = n - prevDigits - 1;
        int start = (int) pow(10, d - 1);
        int num = start + index / d;
        int digitIndex = index % d;
        int digit = (num / (int) (pow(10, d - digitIndex - 1))) % 10;
        return digit;
    }

    int totalDigits(int length) {
        int digits = 0;
        int curLength = 1, curCount = 9;
        while (curLength <= length) {
            digits += curLength * curCount;
            curLength++;
            curCount *= 10;
        }
        return digits;
    }
};

// 方法二：直接计算
class Solution {
public:
    int findNthDigit(int n) {
        int d = 1, count = 9;
        while (n > (long) d * count) {
            n -= d * count;
            d++;
            count *= 10;
        }
        int index = n - 1;
        int start = (int) pow(10, d - 1);
        int num = start + index / d;
        int digitIndex = index % d;
        int digit = (num / (int) (pow(10, d - digitIndex - 1))) % 10;
        return digit;
    }
};
```

Java版本

```java
// 方法一：二分查找
class Solution {
    public int findNthDigit(int n) {
        int low = 1, high = 9;
        while (low < high) {
            int mid = (high - low) / 2 + low;
            if (totalDigits(mid) < n) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        int d = low;
        int prevDigits = totalDigits(d - 1);
        int index = n - prevDigits - 1;
        int start = (int) Math.pow(10, d - 1);
        int num = start + index / d;
        int digitIndex = index % d;
        int digit = (num / (int) (Math.pow(10, d - digitIndex - 1))) % 10;
        return digit;
    }

    public int totalDigits(int length) {
        int digits = 0;
        int curLength = 1, curCount = 9;
        while (curLength <= length) {
            digits += curLength * curCount;
            curLength++;
            curCount *= 10;
        }
        return digits;
    }
}

// 方法二：直接计算
class Solution {
    public int findNthDigit(int n) {
        int d = 1, count = 9;
        while (n > (long) d * count) {
            n -= d * count;
            d++;
            count *= 10;
        }
        int index = n - 1;
        int start = (int) Math.pow(10, d - 1);
        int num = start + index / d;
        int digitIndex = index % d;
        int digit = (num / (int)(Math.pow(10, d - digitIndex - 1))) % 10;
        return digit;
    }
}
```



### [875. 爱吃香蕉的珂珂](https://leetcode.cn/problems/koko-eating-bananas/)

中等

珂珂喜欢吃香蕉。这里有 `n` 堆香蕉，第 `i` 堆中有 `piles[i]` 根香蕉。警卫已经离开了，将在 `h` 小时后回来。

珂珂可以决定她吃香蕉的速度 `k` （单位：根/小时）。每个小时，她将会选择一堆香蕉，从中吃掉 `k` 根。如果这堆香蕉少于 `k` 根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉。 

珂珂喜欢慢慢吃，但仍然想在警卫回来前吃掉所有的香蕉。

返回她可以在 `h` 小时内吃掉所有香蕉的最小速度 `k`（`k` 为整数）。

**示例 1：**

```
输入：piles = [3,6,7,11], h = 8
输出：4
```

C++版本

```c++
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int low = 1;
        int high = 0;
        for (int pile : piles) {
            high = max(high, pile);
        }
        int k = high;
        while (low < high) {
            int speed = (high - low) / 2 + low;
            long time = getTime(piles, speed);
            if (time <= h) {
                k = speed;
                high = speed;
            } else {
                low = speed + 1;
            }
        }
        return k;
    }

    long getTime(const vector<int>& piles, int speed) {
        long time = 0;
        for (int pile : piles) {
            int curTime = (pile + speed - 1) / speed;
            time += curTime;
        }
        return time;
    }
};
```

Java版本

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int low = 1;
        int high = 0;
        for (int pile : piles) {
            high = Math.max(high, pile);
        }
        int k = high;
        while (low < high) {
            int speed = (high - low) / 2 + low;
            long time = getTime(piles, speed);
            if (time <= h) {
                k = speed;
                high = speed;
            } else {
                low = speed + 1;
            }
        }
        return k;
    }

    public long getTime(int[] piles, int speed) {
        long time = 0;
        for (int pile : piles) {
            int curTime = (pile + speed - 1) / speed;
            time += curTime;
        }
        return time;
    }
}
```



### [410. 分割数组的最大值](https://leetcode.cn/problems/split-array-largest-sum/)

困难

给定一个非负整数数组 `nums` 和一个整数 `m` ，你需要将这个数组分成 `m` 个非空的连续子数组。

设计一个算法使得这 `m` 个子数组各自和的最大值最小。

**示例 1：**

```
输入：nums = [7,2,5,10,8], m = 2
输出：18
解释：
一共有四种方法将 nums 分割为 2 个子数组。 
其中最好的方式是将其分为 [7,2,5] 和 [10,8] 。
因为此时这两个子数组各自的和的最大值为18，在所有情况中最小。
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int splitArray(vector<int>& nums, int m) {
        int n = nums.size();
        vector<vector<long long>> f(n + 1, vector<long long>(m + 1, LLONG_MAX));
        vector<long long> sub(n + 1, 0);
        for (int i = 0; i < n; i++) {
            sub[i + 1] = sub[i] + nums[i];
        }
        f[0][0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= min(i, m); j++) {
                for (int k = 0; k < i; k++) {
                    f[i][j] = min(f[i][j], max(f[k][j - 1], sub[i] - sub[k]));
                }
            }
        }
        return (int)f[n][m];
    }
};

// 方法二：二分查找 + 贪心
class Solution {
public:
    bool check(vector<int>& nums, int x, int m) {
        long long sum = 0;
        int cnt = 1;
        for (int i = 0; i < nums.size(); i++) {
            if (sum + nums[i] > x) {
                cnt++;
                sum = nums[i];
            } else {
                sum += nums[i];
            }
        }
        return cnt <= m;
    }

    int splitArray(vector<int>& nums, int m) {
        long long left = 0, right = 0;
        for (int i = 0; i < nums.size(); i++) {
            right += nums[i];
            if (left < nums[i]) {
                left = nums[i];
            }
        }
        while (left < right) {
            long long mid = (left + right) >> 1;
            if (check(nums, mid, m)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int splitArray(int[] nums, int m) {
        int n = nums.length;
        int[][] f = new int[n + 1][m + 1];
        for (int i = 0; i <= n; i++) {
            Arrays.fill(f[i], Integer.MAX_VALUE);
        }
        int[] sub = new int[n + 1];
        for (int i = 0; i < n; i++) {
            sub[i + 1] = sub[i] + nums[i];
        }
        f[0][0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= Math.min(i, m); j++) {
                for (int k = 0; k < i; k++) {
                    f[i][j] = Math.min(f[i][j], Math.max(f[k][j - 1], sub[i] - sub[k]));
                }
            }
        }
        return f[n][m];
    }
}

// 方法二：二分查找 + 贪心
class Solution {
    public int splitArray(int[] nums, int m) {
        int left = 0, right = 0;
        for (int i = 0; i < nums.length; i++) {
            right += nums[i];
            if (left < nums[i]) {
                left = nums[i];
            }
        }
        while (left < right) {
            int mid = (right - left) / 2 + left;
            if (check(nums, mid, m)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    public boolean check(int[] nums, int x, int m) {
        int sum = 0;
        int cnt = 1;
        for (int i = 0; i < nums.length; i++) {
            if (sum + nums[i] > x) {
                cnt++;
                sum = nums[i];
            } else {
                sum += nums[i];
            }
        }
        return cnt <= m;
    }
}
```



### [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

中等

给定一个含有 `n` 个正整数的数组和一个正整数 `target` **。**

找出该数组中满足其和 `≥ target` 的长度最小的 **连续子数组** `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度**。**如果不存在符合条件的子数组，返回 `0` 。

**示例 1：**

```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

C++版本

```c++
// 暴力
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        int ans = INT_MAX;
        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                if (sum >= s) {
                    ans = min(ans, j - i + 1);
                    break;
                }
            }
        }
        return ans == INT_MAX ? 0 : ans;
    }
};

// 前缀和+二分查找
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        int ans = INT_MAX;
        vector<int> sums(n + 1, 0); 
        // 为了方便计算，令 size = n + 1 
        // sums[0] = 0 意味着前 0 个元素的前缀和为 0
        // sums[1] = A[0] 前 1 个元素的前缀和为 A[0]
        // 以此类推
        for (int i = 1; i <= n; i++) {
            sums[i] = sums[i - 1] + nums[i - 1];
        }
        for (int i = 1; i <= n; i++) {
            int target = s + sums[i - 1];
            auto bound = lower_bound(sums.begin(), sums.end(), target);
            if (bound != sums.end()) {
                ans = min(ans, static_cast<int>((bound - sums.begin()) - (i - 1)));
            }
        }
        return ans == INT_MAX ? 0 : ans;
    }
};

// 滑动窗口
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        int ans = INT_MAX;
        int start = 0, end = 0;
        int sum = 0;
        while (end < n) {
            sum += nums[end];
            while (sum >= s) {
                ans = min(ans, end - start + 1);
                sum -= nums[start];
                start++;
            }
            end++;
        }
        return ans == INT_MAX ? 0 : ans;
    }
};
```

Java版本

```java
// 暴力
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                if (sum >= s) {
                    ans = Math.min(ans, j - i + 1);
                    break;
                }
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}

// 前缀和+二分查找
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int[] sums = new int[n + 1]; 
        // 为了方便计算，令 size = n + 1 
        // sums[0] = 0 意味着前 0 个元素的前缀和为 0
        // sums[1] = A[0] 前 1 个元素的前缀和为 A[0]
        // 以此类推
        for (int i = 1; i <= n; i++) {
            sums[i] = sums[i - 1] + nums[i - 1];
        }
        for (int i = 1; i <= n; i++) {
            int target = s + sums[i - 1];
            int bound = Arrays.binarySearch(sums, target);
            if (bound < 0) {
                bound = -bound - 1;
            }
            if (bound <= n) {
                ans = Math.min(ans, bound - (i - 1));
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}

// 滑动窗口
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int start = 0, end = 0;
        int sum = 0;
        while (end < n) {
            sum += nums[end];
            while (sum >= s) {
                ans = Math.min(ans, end - start + 1);
                sum -= nums[start];
                start++;
            }
            end++;
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```



### [658. 找到 K 个最接近的元素](https://leetcode.cn/problems/find-k-closest-elements/)

中等

给定一个 **排序好** 的数组 `arr` ，两个整数 `k` 和 `x` ，从数组中找到最靠近 `x`（两数之差最小）的 `k` 个数。返回的结果必须要是按升序排好的。

整数 `a` 比整数 `b` 更接近 `x` 需要满足：

- `|a - x| < |b - x|` 或者
- `|a - x| == |b - x|` 且 `a < b`

**示例 1：**

```
输入：arr = [1,2,3,4,5], k = 4, x = 3
输出：[1,2,3,4]
```

C++版本

```c++
// 排序
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        sort(arr.begin(), arr.end(), [x](int a, int b) -> bool {
            return abs(a - x) < abs(b - x) || abs(a - x) == abs(b - x) && a < b;
        });
        sort(arr.begin(), arr.begin() + k);
        return vector<int>(arr.begin(), arr.begin() + k);
    }
};

// 二分查找 + 双指针
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        int right = lower_bound(arr.begin(), arr.end(), x) - arr.begin();
        int left = right - 1;
        while (k--) {
            if (left < 0) {
                right++;
            } else if (right >= arr.size()) {
                left--;
            } else if (x - arr[left] <= arr[right] - x) {
                left--;
            } else {
                right++;
            }
        }
        return vector<int>(arr.begin() + left + 1, arr.begin() + right);
    }
};
```

Java版本

```java
// 二分查找 + 双指针
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        List<Integer> list = new ArrayList<Integer>();
        for (int num : arr) {
            list.add(num);
        }
        Collections.sort(list, (a, b) -> {
            if (Math.abs(a - x) != Math.abs(b - x)) {
                return Math.abs(a - x) - Math.abs(b - x);
            } else {
                return a - b;
            }
        });
        List<Integer> ans = list.subList(0, k);
        Collections.sort(ans);
        return ans;
    }
}

// 二分查找 + 双指针
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int right = binarySearch(arr, x);
        int left = right - 1;
        while (k-- > 0) {
            if (left < 0) {
                right++;
            } else if (right >= arr.length) {
                left--;
            } else if (x - arr[left] <= arr[right] - x) {
                left--;
            } else {
                right++;
            }
        }
        List<Integer> ans = new ArrayList<Integer>();
        for (int i = left + 1; i < right; i++) {
            ans.add(arr[i]);
        }
        return ans;
    }

    public int binarySearch(int[] arr, int x) {
        int low = 0, high = arr.length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (arr[mid] >= x) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return low;
    }
}
```



### [719. 找出第 K 小的数对距离](https://leetcode.cn/problems/find-k-th-smallest-pair-distance/)

困难

数对 `(a,b)` 由整数 `a` 和 `b` 组成，其数对距离定义为 `a` 和 `b` 的绝对差值。

给你一个整数数组 `nums` 和一个整数 `k` ，数对由 `nums[i]` 和 `nums[j]` 组成且满足 `0 <= i < j < nums.length` 。返回 **所有数对距离中** 第 `k` 小的数对距离。

**示例 1：**

```
输入：nums = [1,3,1], k = 1
输出：0
解释：数对和对应的距离如下：
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
距离第 1 小的数对是 (1,1) ，距离为 0 。
```

C++版本

```c++
// 排序 + 二分查找
class Solution {
public:
    int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size(), left = 0, right = nums.back() - nums.front();
        while (left <= right) {
            int mid = (left + right) / 2;
            int cnt = 0;
            for (int j = 0; j < n; j++) {
                int i = lower_bound(nums.begin(), nums.begin() + j, nums[j] - mid) - nums.begin();
                cnt += j - i;
            }
            if (cnt >= k) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};

// 排序 + 二分查找 + 双指针
class Solution {
public:
    int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size(), left = 0, right = nums.back() - nums.front();
        while (left <= right) {
            int mid = (left + right) / 2;
            int cnt = 0;
            for (int i = 0, j = 0; j < n; j++) {
                while (nums[j] - nums[i] > mid) {
                    i++;
                }
                cnt += j - i;
            }
            if (cnt >= k) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

Java版本

```java
// 排序 + 二分查找
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length, left = 0, right = nums[n - 1] - nums[0];
        while (left <= right) {
            int mid = (left + right) / 2;
            int cnt = 0;
            for (int j = 0; j < n; j++) {
                int i = binarySearch(nums, j, nums[j] - mid);
                cnt += j - i;
            }
            if (cnt >= k) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    public int binarySearch(int[] nums, int end, int target) {
        int left = 0, right = end;
        while (left < right) {
            int mid = (left + right) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }
}
// 排序 + 二分查找 + 双指针
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length, left = 0, right = nums[n - 1] - nums[0];
        while (left <= right) {
            int mid = (left + right) / 2;
            int cnt = 0;
            for (int i = 0, j = 0; j < n; j++) {
                while (nums[j] - nums[i] > mid) {
                    i++;
                }
                cnt += j - i;
            }
            if (cnt >= k) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```



### [1011. 在 D 天内送达包裹的能力](https://leetcode.cn/problems/capacity-to-ship-packages-within-d-days/)

中等

传送带上的包裹必须在 `days` 天内从一个港口运送到另一个港口。

传送带上的第 `i` 个包裹的重量为 `weights[i]`。每一天，我们都会按给出重量（`weights`）的顺序往传送带上装载包裹。我们装载的重量不会超过船的最大运载重量。

返回能在 `days` 天内将传送带上的所有包裹送达的船的最低运载能力。

**示例 1：**

```
输入：weights = [1,2,3,4,5,6,7,8,9,10], days = 5
输出：15
解释：
船舶最低载重 15 就能够在 5 天内送达所有包裹，如下所示：
第 1 天：1, 2, 3, 4, 5
第 2 天：6, 7
第 3 天：8
第 4 天：9
第 5 天：10

请注意，货物必须按照给定的顺序装运，因此使用载重能力为 14 的船舶并将包装分成 (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) 是不允许的。 
```

C++版本

```c++
// 二分查找转化为判定问题
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int days) {
        // 确定二分查找左右边界
        int left = *max_element(weights.begin(), weights.end()), right = accumulate(weights.begin(), weights.end(), 0);
        while (left < right) {
            int mid = (left + right) / 2;
            // need 为需要运送的天数
            // cur 为当前这一天已经运送的包裹重量之和
            int need = 1, cur = 0;
            for (int weight: weights) {
                if (cur + weight > mid) {
                    ++need;
                    cur = 0;
                }
                cur += weight;
            }
            if (need <= days) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

Java版本

```java
// 二分查找转化为判定问题
class Solution {
    public int shipWithinDays(int[] weights, int days) {
        // 确定二分查找左右边界
        int left = Arrays.stream(weights).max().getAsInt(), right = Arrays.stream(weights).sum();
        while (left < right) {
            int mid = (left + right) / 2;
            // need 为需要运送的天数
            // cur 为当前这一天已经运送的包裹重量之和
            int need = 1, cur = 0;
            for (int weight : weights) {
                if (cur + weight > mid) {
                    ++need;
                    cur = 0;
                }
                cur += weight;
            }
            if (need <= days) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```



### [1482. 制作 m 束花所需的最少天数](https://leetcode.cn/problems/minimum-number-of-days-to-make-m-bouquets/)

中等

给你一个整数数组 `bloomDay`，以及两个整数 `m` 和 `k` 。

现需要制作 `m` 束花。制作花束时，需要使用花园中 **相邻的 `k` 朵花** 。

花园中有 `n` 朵花，第 `i` 朵花会在 `bloomDay[i]` 时盛开，**恰好** 可以用于 **一束** 花中。

请你返回从花园中摘 `m` 束花需要等待的最少的天数。如果不能摘到 `m` 束花则返回 **-1** 。

**示例 1：**

```
输入：bloomDay = [1,10,3,10,2], m = 3, k = 1
输出：3
解释：让我们一起观察这三天的花开过程，x 表示花开，而 _ 表示花还未开。
现在需要制作 3 束花，每束只需要 1 朵。
1 天后：[x, _, _, _, _]   // 只能制作 1 束花
2 天后：[x, _, _, _, x]   // 只能制作 2 束花
3 天后：[x, _, x, _, x]   // 可以制作 3 束花，答案为 3
```

C++版本

```c++
class Solution {
public:
    int minDays(vector<int>& bloomDay, int m, int k) {
        if (m > bloomDay.size() / k) {
            return -1;
        }
        int low = INT_MAX, high = 0;
        int length = bloomDay.size();
        for (int i = 0; i < length; i++) {
            low = min(low, bloomDay[i]);
            high = max(high, bloomDay[i]);
        }
        while (low < high) {
            int days = (high - low) / 2 + low;
            if (canMake(bloomDay, days, m, k)) {
                high = days;
            } else {
                low = days + 1;
            }
        }
        return low;
    }

    bool canMake(vector<int>& bloomDay, int days, int m, int k) {
        int bouquets = 0;
        int flowers = 0;
        int length = bloomDay.size();
        for (int i = 0; i < length && bouquets < m; i++) {
            if (bloomDay[i] <= days) {
                flowers++;
                if (flowers == k) {
                    bouquets++;
                    flowers = 0;
                }
            } else {
                flowers = 0;
            }
        }
        return bouquets >= m;
    }
};
```

Java版本

```java
class Solution {
    public int minDays(int[] bloomDay, int m, int k) {
        if (m > bloomDay.length / k) {
            return -1;
        }
        int low = Integer.MAX_VALUE, high = 0;
        int length = bloomDay.length;
        for (int i = 0; i < length; i++) {
            low = Math.min(low, bloomDay[i]);
            high = Math.max(high, bloomDay[i]);
        }
        while (low < high) {
            int days = (high - low) / 2 + low;
            if (canMake(bloomDay, days, m, k)) {
                high = days;
            } else {
                low = days + 1;
            }
        }
        return low;
    }

    public boolean canMake(int[] bloomDay, int days, int m, int k) {
        int bouquets = 0;
        int flowers = 0;
        int length = bloomDay.length;
        for (int i = 0; i < length && bouquets < m; i++) {
            if (bloomDay[i] <= days) {
                flowers++;
                if (flowers == k) {
                    bouquets++;
                    flowers = 0;
                }
            } else {
                flowers = 0;
            }
        }
        return bouquets >= m;
    }
}
```



## 数组双指针

> 1. 对撞指针适用范围
>
>    对撞指针一般用来解决有序数组或者字符串问题：
>
>    - 查找有序数组中满足某些约束条件的一组元素问题：比如二分查找、数字之和等问题。
>    - 字符串反转问题：反转字符串、回文数、颠倒二进制等问题。
>
> 2. 快慢指针适用范围
>
>    - 快慢指针一般用于处理数组中的移动、删除元素问题，或者链表中的判断是否有环、长度问题
>
> 3. 分离双指针使用范围 
>
>    - 分离双指针一般用于处理有序数组合并，求交集、并集问题。



### [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)

简单

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 **回文串** 。

字母和数字都属于字母数字字符。

给你一个字符串 `s`，如果它是 **回文串** ，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入: s = "A man, a plan, a canal: Panama"
输出：true
解释："amanaplanacanalpanama" 是回文串。
```

C++版本

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        int left = 0, right = s.size()-1;
        while(left < right) {
            if(!isalnum(s[left])) {
                left++;
                continue;
            }
            if(!isalnum(s[right])) {
                right--;
                continue;
            }
            if(tolower(s[left]) == tolower(s[right])) {
                left++;
                right--;
            } else {
                return false;
            }
        }
        return true;
    }
};
```

Java版本

```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0, right = s.length() - 1;
        while(left < right) {
            if(!Character.isLetterOrDigit(s.charAt(left))) {
                left++;
                continue;
            }
            if(!Character.isLetterOrDigit(s.charAt(right))) {
                right--;
                continue;
            }
            if(Character.toLowerCase(s.charAt(left)) == Character.toLowerCase(s.charAt(right))) {
                left++;
                right--;
            } else {
                return false;
            }
        }
        return true;
    }
}
```



### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

中等

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

C++版本

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0, right = height.size()-1;
        int maxArea = 0;
        while(left < right) {
            maxArea = max(min(height[left], height[right]) * (right - left), maxArea);

            if(height[left] > height[right]) {
                right--;
            } else {
                left++;
            }
        }
        return maxArea;
    }
};
```

Java版本

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length-1;
        int maxArea = 0;
        while(left < right) {
            maxArea = Math.max(Math.min(height[left], height[right]) * (right - left), maxArea);

            if(height[left] > height[right]) {
                right--;
            } else {
                left++;
            }
        }
        return maxArea;
    }
}
```



### [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

简单

给你一个 **升序排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

C++版本

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int len = nums.size(), slow = 0, fast = 1;
        while(fast < len) {
            if(nums[fast] != nums[slow]) {
                slow++;
                nums[slow] = nums[fast];
            }
            fast++;
        }
        return slow+1;
    }
};
```

Java版本

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int len = nums.length, slow = 0, fast = 1;
        while(fast < len) {
            if(nums[fast] != nums[slow]) {
                slow++;
                nums[slow] = nums[fast];
            }
            fast++;
        }
        return slow+1;
    }
}
```



### [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

简单

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

C++版本

```c++
// 两个集合
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set1, set2;
        for (auto& num : nums1) {
            set1.insert(num);
        }
        for (auto& num : nums2) {
            set2.insert(num);
        }
        return getIntersection(set1, set2);
    }

    vector<int> getIntersection(unordered_set<int>& set1, unordered_set<int>& set2) {
        if (set1.size() > set2.size()) {
            return getIntersection(set2, set1);
        }
        vector<int> intersection;
        for (auto& num : set1) {
            if (set2.count(num)) {
                intersection.push_back(num);
            }
        }
        return intersection;
    }
};

// 排序 + 双指针
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        int length1 = nums1.size(), length2 = nums2.size();
        int index1 = 0, index2 = 0;
        vector<int> intersection;
        while (index1 < length1 && index2 < length2) {
            int num1 = nums1[index1], num2 = nums2[index2];
            if (num1 == num2) {
                // 保证加入元素的唯一性
                if (!intersection.size() || num1 != intersection.back()) {
                    intersection.push_back(num1);
                }
                index1++;
                index2++;
            } else if (num1 < num2) {
                index1++;
            } else {
                index2++;
            }
        }
        return intersection;
    }
};
```

Java版本

```java
// 两个集合
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<Integer>();
        Set<Integer> set2 = new HashSet<Integer>();
        for (int num : nums1) {
            set1.add(num);
        }
        for (int num : nums2) {
            set2.add(num);
        }
        return getIntersection(set1, set2);
    }

    public int[] getIntersection(Set<Integer> set1, Set<Integer> set2) {
        if (set1.size() > set2.size()) {
            return getIntersection(set2, set1);
        }
        Set<Integer> intersectionSet = new HashSet<Integer>();
        for (int num : set1) {
            if (set2.contains(num)) {
                intersectionSet.add(num);
            }
        }
        int[] intersection = new int[intersectionSet.size()];
        int index = 0;
        for (int num : intersectionSet) {
            intersection[index++] = num;
        }
        return intersection;
    }
}

// 排序 + 双指针
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int length1 = nums1.length, length2 = nums2.length;
        int[] intersection = new int[length1 + length2];
        int index = 0, index1 = 0, index2 = 0;
        while (index1 < length1 && index2 < length2) {
            int num1 = nums1[index1], num2 = nums2[index2];
            if (num1 == num2) {
                // 保证加入元素的唯一性
                if (index == 0 || num1 != intersection[index - 1]) {
                    intersection[index++] = num1;
                }
                index1++;
                index2++;
            } else if (num1 < num2) {
                index1++;
            } else {
                index2++;
            }
        }
        return Arrays.copyOfRange(intersection, 0, index);
    }
}
```



### [350. 两个数组的交集 II](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)

简单

给你两个整数数组 `nums1` 和 `nums2` ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
```

C++版本

```c++
// 哈希表
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size() > nums2.size()) {
            return intersect(nums2, nums1);
        }
        unordered_map <int, int> m;
        for (int num : nums1) {
            ++m[num];
        }
        vector<int> intersection;
        for (int num : nums2) {
            if (m.count(num)) {
                intersection.push_back(num);
                --m[num];
                if (m[num] == 0) {
                    m.erase(num);
                }
            }
        }
        return intersection;
    }
};

// 排序 + 双指针
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        int length1 = nums1.size(), length2 = nums2.size();
        vector<int> intersection;
        int index1 = 0, index2 = 0;
        while (index1 < length1 && index2 < length2) {
            if (nums1[index1] < nums2[index2]) {
                index1++;
            } else if (nums1[index1] > nums2[index2]) {
                index2++;
            } else {
                intersection.push_back(nums1[index1]);
                index1++;
                index2++;
            }
        }
        return intersection;
    }
};
```

Java版本

```java
// 哈希表
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return intersect(nums2, nums1);
        }
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int num : nums1) {
            int count = map.getOrDefault(num, 0) + 1;
            map.put(num, count);
        }
        int[] intersection = new int[nums1.length];
        int index = 0;
        for (int num : nums2) {
            int count = map.getOrDefault(num, 0);
            if (count > 0) {
                intersection[index++] = num;
                count--;
                if (count > 0) {
                    map.put(num, count);
                } else {
                    map.remove(num);
                }
            }
        }
        return Arrays.copyOfRange(intersection, 0, index);
    }
}

// 排序 + 双指针
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int length1 = nums1.length, length2 = nums2.length;
        int[] intersection = new int[Math.min(length1, length2)];
        int index1 = 0, index2 = 0, index = 0;
        while (index1 < length1 && index2 < length2) {
            if (nums1[index1] < nums2[index2]) {
                index1++;
            } else if (nums1[index1] > nums2[index2]) {
                index2++;
            } else {
                intersection[index] = nums1[index1];
                index1++;
                index2++;
                index++;
            }
        }
        return Arrays.copyOfRange(intersection, 0, index);
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



### [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

简单

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。

不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。

**示例 1：**

```
输入：s = ["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

C++版本

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int left = 0, right = s.size() - 1; left < right; ++left, --right) {
            swap(s[left], s[right]);
        }
    }
};
```

Java版本

```java
class Solution {
    public void reverseString(char[] s) {
        int n = s.length;
        for (int left = 0, right = n - 1; left < right; ++left, --right) {
            char tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;
        }
    }
}
```



### [345. 反转字符串中的元音字母](https://leetcode.cn/problems/reverse-vowels-of-a-string/)

简单

给你一个字符串 `s` ，仅反转字符串中的所有元音字母，并返回结果字符串。

元音字母包括 `'a'`、`'e'`、`'i'`、`'o'`、`'u'`，且可能以大小写两种形式出现不止一次。

**示例 1：**

```
输入：s = "hello"
输出："holle"
```

C++版本

```c++
class Solution {
public:
    string reverseVowels(string s) {
        auto isVowel = [vowels = "aeiouAEIOU"s](char ch) {
            return vowels.find(ch) != string::npos;
        };

        int left = 0, right = s.size() - 1;
        while(left < right) {
            while(left < right && !isVowel(s[left])) {
                left++;
            }
            while(left < right && !isVowel(s[right])) {
                right--;
            }
            if(left > right) {
                break;
            }
            swap(s[left], s[right]);
            left++;
            right--;
        }
        return s;
    }
};
```

Java版本

```java
class Solution {
    public String reverseVowels(String s) {
        int n = s.length();
        char[] arr = s.toCharArray();
        int i = 0, j = n - 1;
        while (i < j) {
            while (i < n && !isVowel(arr[i])) {
                ++i;
            }
            while (j > 0 && !isVowel(arr[j])) {
                --j;
            }
            if (i < j) {
                swap(arr, i, j);
                ++i;
                --j;
            }
        }
        return new String(arr);
    }

    public boolean isVowel(char ch) {
        return "aeiouAEIOU".indexOf(ch) >= 0;
    }

    public void swap(char[] arr, int i, int j) {
        char temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```



### [611. 有效三角形的个数](https://leetcode.cn/problems/valid-triangle-number/)

中等

给定一个包含非负整数的数组 `nums` ，返回其中可以组成三角形三条边的三元组个数。

**示例 1:**

```
输入: nums = [2,2,3,4]
输出: 3
解释:有效的组合是: 
2,3,4 (使用第一个 2)
2,3,4 (使用第二个 2)
2,2,3
```

C++版本

```c++
// 排序 + 二分查找
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int left = j + 1, right = n - 1, k = j;
                while (left <= right) {
                    int mid = (left + right) / 2;
                    if (nums[mid] < nums[i] + nums[j]) {
                        k = mid;
                        left = mid + 1;
                    }
                    else {
                        right = mid - 1;
                    }
                }
                ans += k - j;
            }
        }
        return ans;
    }
};

// 排序 + 双指针
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int k = i;
            for (int j = i + 1; j < n; ++j) {
                while (k + 1 < n && nums[k + 1] < nums[i] + nums[j]) {
                    ++k;
                }
                ans += max(k - j, 0);
            }
        }
        return ans;
    }
};
```

Java版本

```java
// 排序 + 二分查找
class Solution {
    public int triangleNumber(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int left = j + 1, right = n - 1, k = j;
                while (left <= right) {
                    int mid = (left + right) / 2;
                    if (nums[mid] < nums[i] + nums[j]) {
                        k = mid;
                        left = mid + 1;
                    } else {
                        right = mid - 1;
                    }
                }
                ans += k - j;
            }
        }
        return ans;
    }
}

// 排序 + 双指针
class Solution {
    public int triangleNumber(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int k = i;
            for (int j = i + 1; j < n; ++j) {
                while (k + 1 < n && nums[k + 1] < nums[i] + nums[j]) {
                    ++k;
                }
                ans += Math.max(k - j, 0);
            }
        }
        return ans;
    }
}
```



### [15. 三数之和](https://leetcode.cn/problems/3sum/)

中等

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

C++版本

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        // 枚举 a
        for (int first = 0; first < n; ++first) {
            // 需要和上一次枚举的数不相同
            if (first > 0 && nums[first] == nums[first - 1]) {
                continue;
            }
            // c 对应的指针初始指向数组的最右端
            int third = n - 1;
            int target = -nums[first];
            // 枚举 b
            for (int second = first + 1; second < n; ++second) {
                // 需要和上一次枚举的数不相同
                if (second > first + 1 && nums[second] == nums[second - 1]) {
                    continue;
                }
                // 需要保证 b 的指针在 c 的指针的左侧
                while (second < third && nums[second] + nums[third] > target) {
                    --third;
                }
                // 如果指针重合，随着 b 后续的增加
                // 就不会有满足 a+b+c=0 并且 b<c 的 c 了，可以退出循环
                if (second == third) {
                    break;
                }
                if (nums[second] + nums[third] == target) {
                    ans.push_back({nums[first], nums[second], nums[third]});
                }
            }
        }
        return ans;
    }
};
```

Java 版本

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        int length = nums.length;
        if(length < 3) {
            return ans;
        }
        Arrays.sort(nums);
        for(int i = 0; i < length; i++) {
            // 排除重复三元组
            if(i > 0 && nums[i] == nums[i-1]) {
                continue;
            }
            int k = length - 1;
            for(int j = i + 1; j < length; j++) {
                // 排除重复三元组
                if(j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                while(j < k && nums[i] + nums[j] + nums[k] == 0) {
                    k--;
                }
                if(j == k) {
                    break;
                }
                if(nums[i] + nums[j] + nums[k] == 0) {
                    List<Integer> list = new ArrayList<>();
                    list.add(nums[i]);
                    list.add(nums[j]);
                    list.add(nums[k]);
                    ans.add(list);
                }
            }
        }
        return ans;
    }
}
```



### [16. 最接近的三数之和](https://leetcode.cn/problems/3sum-closest/)

中等

给你一个长度为 `n` 的整数数组 `nums` 和 一个目标值 `target`。请你从 `nums` 中选出三个整数，使它们的和与 `target` 最接近。

返回这三个数的和。

假定每组输入只存在恰好一个解。

**示例 1：**

```
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```

C++版本

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int len = nums.size();
        sort(nums.begin(), nums.end());
        int ans = nums[0] + nums[1] + nums[2];
        for(int i= 0; i < len; i++) {
            if(i > 0 && nums[i] == nums[i-1]) {
                continue;
            }
            int left = i+1, right = len-1;
            while(left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if(sum == target) {
                    return target;
                } else {
                    if(abs(sum-target) < abs(ans-target)) {
                        ans = sum;
                    }
                    if(sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }
        return ans;
    }
};
```

Java版本

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int length = nums.length;
        Arrays.sort(nums);
        int ans = nums[0] + nums[1] + nums[2];
        for(int i = 0; i < length; i++) {
            if(i > 0 && nums[i] == nums[i-1]) {
                continue;
            }
            int j = i+1, k = length-1;
            while(j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                if(sum == target) {
                    return target;
                } else {
                    if(Math.abs(sum-target) < Math.abs(ans-target)) {
                        ans = sum;
                    }
                    if(sum > target) {
                        k--;
                    } else {
                        j++;
                    }
                }
            }
        }
        return ans;
    }
}
```



### [18. 四数之和](https://leetcode.cn/problems/4sum/)

中等

给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且**不重复**的四元组 `[nums[a], nums[b], nums[c], nums[d]]` （若两个四元组元素一一对应，则认为两个四元组重复）：

- `0 <= a, b, c, d < n`
- `a`、`b`、`c` 和 `d` **互不相同**
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

你可以按 **任意顺序** 返回答案 。

**示例 1：**

```
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

C++版本

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> quadruplets;
        if (nums.size() < 4) {
            return quadruplets;
        }
        sort(nums.begin(), nums.end());
        int length = nums.size();
        for (int i = 0; i < length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            if ((long) nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3] > target) {
                break;
            }
            if ((long) nums[i] + nums[length - 3] + nums[length - 2] + nums[length - 1] < target) {
                continue;
            }
            for (int j = i + 1; j < length - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                if ((long) nums[i] + nums[j] + nums[j + 1] + nums[j + 2] > target) {
                    break;
                }
                if ((long) nums[i] + nums[j] + nums[length - 2] + nums[length - 1] < target) {
                    continue;
                }
                int left = j + 1, right = length - 1;
                while (left < right) {
                    long sum = (long) nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum == target) {
                        quadruplets.push_back({nums[i], nums[j], nums[left], nums[right]});
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        left++;
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }
        return quadruplets;
    }
};
```

Java版本

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (nums == null || nums.length < 4) {
            return res;
        }
        Arrays.sort(nums);
        int length = nums.length;
        for (int i = 0; i < length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            if ((long) nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3] > target) {
                break;
            }
            if ((long) nums[i] + nums[length - 3] + nums[length - 2] + nums[length - 1] < target) {
                continue;
            }
            for (int j = i + 1; j < length - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                if ((long) nums[i] + nums[j] + nums[j + 1] + nums[j + 2] > target) {
                    break;
                }
                if ((long) nums[i] + nums[j] + nums[length - 2] + nums[length - 1] < target) {
                    continue;
                }
                int left = j + 1, right = length - 1;
                while (left < right) {
                    long sum = (long) nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum == target) {
                        res.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        left++;
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }
        return res;
    }
}
```



### [658. 找到 K 个最接近的元素](https://leetcode.cn/problems/find-k-closest-elements/)

中等

给定一个 **排序好** 的数组 `arr` ，两个整数 `k` 和 `x` ，从数组中找到最靠近 `x`（两数之差最小）的 `k` 个数。返回的结果必须要是按升序排好的。

整数 `a` 比整数 `b` 更接近 `x` 需要满足：

- `|a - x| < |b - x|` 或者
- `|a - x| == |b - x|` 且 `a < b`

**示例 1：**

```
输入：arr = [1,2,3,4,5], k = 4, x = 3
输出：[1,2,3,4]
```

C++版本

```c++
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        int right = lower_bound(arr.begin(), arr.end(), x) - arr.begin();
        int left = right - 1;
        while (k--) {
            if (left < 0) {
                right++;
            } else if (right >= arr.size()) {
                left--;
            } else if (x - arr[left] <= arr[right] - x) {
                left--;
            } else {
                right++;
            }
        }
        return vector<int>(arr.begin() + left + 1, arr.begin() + right);
    }
};
```

Java版本

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int right = binarySearch(arr, x);
        int left = right - 1;
        while (k-- > 0) {
            if (left < 0) {
                right++;
            } else if (right >= arr.length) {
                left--;
            } else if (x - arr[left] <= arr[right] - x) {
                left--;
            } else {
                right++;
            }
        }
        List<Integer> ans = new ArrayList<Integer>();
        for (int i = left + 1; i < right; i++) {
            ans.add(arr[i]);
        }
        return ans;
    }

    public int binarySearch(int[] arr, int x) {
        int low = 0, high = arr.length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (arr[mid] >= x) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return low;
    }
}
```



### [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

中等

给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色

必须在不使用库内置的 sort 函数的情况下解决这个问题。

**示例 1：**

```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

C++版本

```c++
// 方法一：单指针
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n = nums.size();
        int ptr = 0;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 0) {
                swap(nums[i], nums[ptr]);
                ++ptr;
            }
        }
        for (int i = ptr; i < n; ++i) {
            if (nums[i] == 1) {
                swap(nums[i], nums[ptr]);
                ++ptr;
            }
        }
    }
};

// 方法二：双指针
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n = nums.size();
        int p0 = 0, p2 = n - 1;
        for (int i = 0; i <= p2; ++i) {
            while (i <= p2 && nums[i] == 2) {
                swap(nums[i], nums[p2]);
                --p2;
            }
            if (nums[i] == 0) {
                swap(nums[i], nums[p0]);
                ++p0;
            }
        }
    }
};
```

Java版本

```Java
// 方法一：单指针
class Solution {
    public void sortColors(int[] nums) {
        int n = nums.length;
        int ptr = 0;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 0) {
                int temp = nums[i];
                nums[i] = nums[ptr];
                nums[ptr] = temp;
                ++ptr;
            }
        }
        for (int i = ptr; i < n; ++i) {
            if (nums[i] == 1) {
                int temp = nums[i];
                nums[i] = nums[ptr];
                nums[ptr] = temp;
                ++ptr;
            }
        }
    }
}

// 方法二：双指针
class Solution {
    public void sortColors(int[] nums) {
        int n = nums.length;
        int p0 = 0, p1 = 0;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 1) {
                int temp = nums[i];
                nums[i] = nums[p1];
                nums[p1] = temp;
                ++p1;
            } else if (nums[i] == 0) {
                int temp = nums[i];
                nums[i] = nums[p0];
                nums[p0] = temp;
                if (p0 < p1) {
                    temp = nums[i];
                    nums[i] = nums[p1];
                    nums[p1] = temp;
                }
                ++p0;
                ++p1;
            }
        }
    }
}
```



### [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

简单

给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。

**示例 1：**

```
输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]
```

C++版本

```c++
// 方法二：双指针
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int n = nums.size();
        int negative = -1;
        for (int i = 0; i < n; ++i) {
            if (nums[i] < 0) {
                negative = i;
            } else {
                break;
            }
        }

        vector<int> ans;
        int i = negative, j = negative + 1;
        while (i >= 0 || j < n) {
            if (i < 0) {
                ans.push_back(nums[j] * nums[j]);
                ++j;
            }
            else if (j == n) {
                ans.push_back(nums[i] * nums[i]);
                --i;
            }
            else if (nums[i] * nums[i] < nums[j] * nums[j]) {
                ans.push_back(nums[i] * nums[i]);
                --i;
            }
            else {
                ans.push_back(nums[j] * nums[j]);
                ++j;
            }
        }

        return ans;
    }
};

// 方法二：双指针
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0, j = n - 1, pos = n - 1; i <= j;) {
            if (nums[i] * nums[i] > nums[j] * nums[j]) {
                ans[pos] = nums[i] * nums[i];
                ++i;
            }
            else {
                ans[pos] = nums[j] * nums[j];
                --j;
            }
            --pos;
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：双指针
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int negative = -1;
        for (int i = 0; i < n; ++i) {
            if (nums[i] < 0) {
                negative = i;
            } else {
                break;
            }
        }

        int[] ans = new int[n];
        int index = 0, i = negative, j = negative + 1;
        while (i >= 0 || j < n) {
            if (i < 0) {
                ans[index] = nums[j] * nums[j];
                ++j;
            } else if (j == n) {
                ans[index] = nums[i] * nums[i];
                --i;
            } else if (nums[i] * nums[i] < nums[j] * nums[j]) {
                ans[index] = nums[i] * nums[i];
                --i;
            } else {
                ans[index] = nums[j] * nums[j];
                ++j;
            }
            ++index;
        }

        return ans;
    }
}

// 方法二：双指针
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0, j = n - 1, pos = n - 1; i <= j;) {
            if (nums[i] * nums[i] > nums[j] * nums[j]) {
                ans[pos] = nums[i] * nums[i];
                ++i;
            } else {
                ans[pos] = nums[j] * nums[j];
                --j;
            }
            --pos;
        }
        return ans;
    }
}
```



### [881. 救生艇](https://leetcode.cn/problems/boats-to-save-people/)

中等

给定数组 `people` 。`people[i]`表示第 `i` 个人的体重 ，**船的数量不限**，每艘船可以承载的最大重量为 `limit`。

每艘船最多可同时载两人，但条件是这些人的重量之和最多为 `limit`。

返回 *承载所有人所需的最小船数* 。

**示例 1：**

```
输入：people = [1,2], limit = 3
输出：1
解释：1 艘船载 (1, 2)
```

C++版本

```c++
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        sort(people.begin(), people.end());
        int count = 0;
        int left = 0, right = people.size() - 1; 
        while(left <= right) {
            if(people[left] + people[right] <= limit) {
                left++;
            }
            count++;
            right--;
        }
        return count;
    }
};
```

Java版本

```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int count = 0;
        int left = 0, right = people.length - 1; 
        while(left <= right) {
            if(people[left] + people[right] <= limit) {
                left++;
            }
            count++;
            right--;
        }
        return count;
    }
}
```



### [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

困难

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

C++版本

```c++
// 方法一：动态规划
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if (n == 0) {
            return 0;
        }
        vector<int> leftMax(n);
        leftMax[0] = height[0];
        for (int i = 1; i < n; ++i) {
            leftMax[i] = max(leftMax[i - 1], height[i]);
        }

        vector<int> rightMax(n);
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            rightMax[i] = max(rightMax[i + 1], height[i]);
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += min(leftMax[i], rightMax[i]) - height[i];
        }
        return ans;
    }
};

// 方法二：单调栈
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        stack<int> stk;
        int n = height.size();
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && height[i] > height[stk.top()]) {
                int top = stk.top();
                stk.pop();
                if (stk.empty()) {
                    break;
                }
                int left = stk.top();
                int currWidth = i - left - 1;
                int currHeight = min(height[left], height[i]) - height[top];
                ans += currWidth * currHeight;
            }
            stk.push(i);
        }
        return ans;
    }
};

// 方法三：双指针
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        int left = 0, right = height.size() - 1;
        int leftMax = 0, rightMax = 0;
        while (left < right) {
            leftMax = max(leftMax, height[left]);
            rightMax = max(rightMax, height[right]);
            if (height[left] < height[right]) {
                ans += leftMax - height[left];
                ++left;
            } else {
                ans += rightMax - height[right];
                --right;
            }
        }
        return ans;
    }
};
```

Java版本

```java
// 方法一：动态规划
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        if (n == 0) {
            return 0;
        }

        int[] leftMax = new int[n];
        leftMax[0] = height[0];
        for (int i = 1; i < n; ++i) {
            leftMax[i] = Math.max(leftMax[i - 1], height[i]);
        }

        int[] rightMax = new int[n];
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            rightMax[i] = Math.max(rightMax[i + 1], height[i]);
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += Math.min(leftMax[i], rightMax[i]) - height[i];
        }
        return ans;
    }
}

// 方法二：单调栈
class Solution {
    public int trap(int[] height) {
        int ans = 0;
        Deque<Integer> stack = new LinkedList<Integer>();
        int n = height.length;
        for (int i = 0; i < n; ++i) {
            while (!stack.isEmpty() && height[i] > height[stack.peek()]) {
                int top = stack.pop();
                if (stack.isEmpty()) {
                    break;
                }
                int left = stack.peek();
                int currWidth = i - left - 1;
                int currHeight = Math.min(height[left], height[i]) - height[top];
                ans += currWidth * currHeight;
            }
            stack.push(i);
        }
        return ans;
    }
}

// 方法三：双指针
class Solution {
    public int trap(int[] height) {
        int ans = 0;
        int left = 0, right = height.length - 1;
        int leftMax = 0, rightMax = 0;
        while (left < right) {
            leftMax = Math.max(leftMax, height[left]);
            rightMax = Math.max(rightMax, height[right]);
            if (height[left] < height[right]) {
                ans += leftMax - height[left];
                ++left;
            } else {
                ans += rightMax - height[right];
                --right;
            }
        }
        return ans;
    }
}
```



### [443. 压缩字符串](https://leetcode.cn/problems/string-compression/)

中等

给你一个字符数组 `chars` ，请使用下述算法压缩：

从一个空字符串 `s` 开始。对于 `chars` 中的每组 **连续重复字符** ：

- 如果这一组长度为 `1` ，则将字符追加到 `s` 中。
- 否则，需要向 `s` 追加字符，后跟这一组的长度。

压缩后得到的字符串 `s` **不应该直接返回** ，需要转储到字符数组 `chars` 中。需要注意的是，如果组长度为 `10` 或 `10` 以上，则在 `chars` 数组中会被拆分为多个字符。

请在 **修改完输入数组后** ，返回该数组的新长度。

你必须设计并实现一个只使用常量额外空间的算法来解决此问题。

**示例 1：**

```
输入：chars = ["a","a","b","b","c","c","c"]
输出：返回 6 ，输入数组的前 6 个字符应该是：["a","2","b","2","c","3"]
解释："aa" 被 "a2" 替代。"bb" 被 "b2" 替代。"ccc" 被 "c3" 替代。
```

C++版本

```c++
class Solution {
public:
    int compress(vector<char>& chars) {
        int n = chars.size();
        int write = 0, left = 0;
        for (int read = 0; read < n; read++) {
            if (read == n - 1 || chars[read] != chars[read + 1]) {
                chars[write++] = chars[read];
                int num = read - left + 1;
                if (num > 1) {
                    int anchor = write;
                    while (num > 0) {
                        chars[write++] = num % 10 + '0';
                        num /= 10;
                    }
                    reverse(&chars[anchor], &chars[write]);
                }
                left = read + 1;
            }
        }
        return write;
    }
};
```

Java版本

```java
class Solution {
    public int compress(char[] chars) {
        int n = chars.length;
        int write = 0, left = 0;
        for (int read = 0; read < n; read++) {
            if (read == n - 1 || chars[read] != chars[read + 1]) {
                chars[write++] = chars[read];
                int num = read - left + 1;
                if (num > 1) {
                    int anchor = write;
                    while (num > 0) {
                        chars[write++] = (char) (num % 10 + '0');
                        num /= 10;
                    }
                    reverse(chars, anchor, write - 1);
                }
                left = read + 1;
            }
        }
        return write;
    }

    public void reverse(char[] chars, int left, int right) {
        while (left < right) {
            char temp = chars[left];
            chars[left] = chars[right];
            chars[right] = temp;
            left++;
            right--;
        }
    }
}
```



### [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

简单

给你一个 **升序排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

**示例 1：**

```
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
```

C++版本

```c++
// 快慢指针
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int len = nums.size(), slow = 0, fast = 1;
        while(fast < len) {
            if(nums[fast] != nums[slow]) {
                slow++;
                nums[slow] = nums[fast];
            }
            fast++;
        }
        return slow+1;
    }
};
```

Java版本

```java
// 快慢指针
class Solution {
    public int removeDuplicates(int[] nums) {
        int len = nums.length, slow = 0, fast = 1;
        while(fast < len) {
            if(nums[fast] != nums[slow]) {
                slow++;
                nums[slow] = nums[fast];
            }
            fast++;
        }
        return slow+1;
    }
}
```



### [80. 删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)

中等

给你一个有序数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

**说明：**

为什么返回数值是整数，但输出的答案是数组呢？

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**示例 1：**

```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。 不需要考虑数组中超出新长度后面的元素。
```

C++版本

```c++
// 方法一
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int len = nums.size(), slow = 2, fast = 2;
        if(len < 2) {
            return len;
        }
        while(fast < len) {
            if (nums[slow - 2] != nums[fast]) {
                nums[slow] = nums[fast];
                ++slow;
            }
            ++fast;
        }
        return slow;
    }
};

// 方法二
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int len = nums.size(), slow = 0, fast = 1;
        bool flag = true;
        while(fast < len) {
            if(nums[fast] == nums[slow]) {
                if(flag) {
                    flag = false;
                    slow++;
                    nums[slow] = nums[fast];
                }
            } else {
                flag = true;
                slow++;
                nums[slow] = nums[fast];
            }
            fast++;
        }
        return slow+1;
    }
};

// 方法三
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() < 2) {
            return nums.size();
        }
        int slow = 2;
        for(int fast = 2; fast < nums.size(); fast++) {
            if(nums[fast] != nums[slow-2]) {
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }
};
```

Java版本

```java
// 方法一
class Solution {
    public int removeDuplicates(int[] nums) {
        int len = nums.length, slow = 2, fast = 2;
        if(len < 2) {
            return len;
        }
        while(fast < len) {
            if (nums[slow - 2] != nums[fast]) {
                nums[slow] = nums[fast];
                ++slow;
            }
            ++fast;
        }
        return slow;
    }
}

// 方法二
class Solution {
    public int removeDuplicates(int[] nums) {
        int len = nums.length, slow = 0, fast = 1;
        boolean flag = true;
        while(fast < len) {
            if(nums[fast] == nums[slow]) {
                if(flag) {
                    flag = false;
                    slow++;
                    nums[slow] = nums[fast];
                }
            } else {
                flag = true;
                slow++;
                nums[slow] = nums[fast];
            }
            fast++;
        }
        return slow+1;
    }
}

// 方法三
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length < 2) {
            return nums.length;
        }
        int slow = 2;
        for(int fast = 2; fast < nums.length; fast++) {
            if(nums[fast] != nums[slow-2]) {
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }
}
```



### [27. 移除元素](https://leetcode.cn/problems/remove-element/)

简单

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组**。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

C++版本

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int len = nums.size(), slow = 0, fast = 0;
        while(fast < len) {
            if(nums[fast] != val) {
                nums[slow] = nums[fast];
                slow++;
            }
            fast++;
        }
        return slow;
    }
};

// 优化
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int left = 0, right = nums.size();
        while (left < right) {
            if (nums[left] == val) {
                nums[left] = nums[right - 1];
                right--;
            } else {
                left++;
            }
        }
        return left;
    }
};
```

Java版本

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int len = nums.length, slow = 0, fast = 0;
        while(fast < len) {
            if(nums[fast] != val) {
                nums[slow] = nums[fast];
                slow++;
            }
            fast++;
        }
        return slow;
    }
}

// 优化
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        int right = nums.length;
        while (left < right) {
            if (nums[left] == val) {
                nums[left] = nums[right - 1];
                right--;
            } else {
                left++;
            }
        }
        return left;
    }
}
```



### [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

简单

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

C++版本

```c++
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



### [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

简单

给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

C++版本

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int index = (m--) + (n--) - 1;
        while(m >= 0 && n >= 0) {
            if(nums1[m] >= nums2[n]) {
                nums1[index--] = nums1[m--];
            } else {
                nums1[index--] = nums2[n--];
            }
        }
        while(m >= 0) {
            nums1[index--] = nums1[m--];
        }
        while(n >= 0) {
            nums1[index--] = nums2[n--];
        }
    }
};
```

Java版本

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int index = (m--) + (n--) - 1;
        while(m >= 0 && n >= 0) {
            if(nums1[m] >= nums2[n]) {
                nums1[index--] = nums1[m--];
            } else {
                nums1[index--] = nums2[n--];
            }
        }
        while(m >= 0) {
            nums1[index--] = nums1[m--];
        }
        while(n >= 0) {
            nums1[index--] = nums2[n--];
        }
    }
}
```



### [845. 数组中的最长山脉](https://leetcode.cn/problems/longest-mountain-in-array/)

中等

把符合下列属性的数组 `arr` 称为 **山脉数组** ：

- `arr.length >= 3`
- 存在下标 `i`（`0 < i < arr.length - 1`），满足
  - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
  - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

给出一个整数数组 `arr`，返回最长山脉子数组的长度。如果不存在山脉子数组，返回 `0` 。

**示例 1：**

```
输入：arr = [2,1,4,7,3,2,5]
输出：5
解释：最长的山脉子数组是 [1,4,7,3,2]，长度为 5。
```

C++版本

```c++
// 枚举山顶
class Solution {
public:
    int longestMountain(vector<int>& arr) {
        int n = arr.size();
        if (!n) {
            return 0;
        }
        vector<int> left(n);
        for (int i = 1; i < n; ++i) {
            left[i] = (arr[i - 1] < arr[i] ? left[i - 1] + 1 : 0);
        }
        vector<int> right(n);
        for (int i = n - 2; i >= 0; --i) {
            right[i] = (arr[i + 1] < arr[i] ? right[i + 1] + 1 : 0);
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 0 && right[i] > 0) {
                ans = max(ans, left[i] + right[i] + 1);
            }
        }
        return ans;
    }
};

// 枚举山脚
class Solution {
public:
    int longestMountain(vector<int>& arr) {
        int n = arr.size();
        int ans = 0;
        int left = 0;
        while (left + 2 < n) {
            int right = left + 1;
            if (arr[left] < arr[left + 1]) {
                while (right + 1 < n && arr[right] < arr[right + 1]) {
                    ++right;
                }
                if (right < n - 1 && arr[right] > arr[right + 1]) {
                    while (right + 1 < n && arr[right] > arr[right + 1]) {
                        ++right;
                    }
                    ans = max(ans, right - left + 1);
                }
                else {
                    ++right;
                }
            }
            left = right;
        }
        return ans;
    }
};
```

Java版本

```java
// 枚举山顶
class Solution {
    public int longestMountain(int[] arr) {
        int n = arr.length;
        if (n == 0) {
            return 0;
        }
        int[] left = new int[n];
        for (int i = 1; i < n; ++i) {
            left[i] = arr[i - 1] < arr[i] ? left[i - 1] + 1 : 0;
        }
        int[] right = new int[n];
        for (int i = n - 2; i >= 0; --i) {
            right[i] = arr[i + 1] < arr[i] ? right[i + 1] + 1 : 0;
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 0 && right[i] > 0) {
                ans = Math.max(ans, left[i] + right[i] + 1);
            }
        }
        return ans;
    }
}

// 枚举山脚
class Solution {
    public int longestMountain(int[] arr) {
        int n = arr.length;
        int ans = 0;
        int left = 0;
        while (left + 2 < n) {
            int right = left + 1;
            if (arr[left] < arr[left + 1]) {
                while (right + 1 < n && arr[right] < arr[right + 1]) {
                    ++right;
                }
                if (right < n - 1 && arr[right] > arr[right + 1]) {
                    while (right + 1 < n && arr[right] > arr[right + 1]) {
                        ++right;
                    }
                    ans = Math.max(ans, right - left + 1);
                } else {
                    ++right;
                }
            }
            left = right;
        }
        return ans;
    }
}
```



### [719. 找出第 K 小的数对距离](https://leetcode.cn/problems/find-k-th-smallest-pair-distance/)

困难

数对 `(a,b)` 由整数 `a` 和 `b` 组成，其数对距离定义为 `a` 和 `b` 的绝对差值。

给你一个整数数组 `nums` 和一个整数 `k` ，数对由 `nums[i]` 和 `nums[j]` 组成且满足 `0 <= i < j < nums.length` 。返回 **所有数对距离中** 第 `k` 小的数对距离。

**示例 1：**

```
输入：nums = [1,3,1], k = 1
输出：0
解释：数对和对应的距离如下：
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
距离第 1 小的数对是 (1,1) ，距离为 0 。
```

C++版本

```c++
class Solution {
public:
    int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size(), left = 0, right = nums.back() - nums.front();
        while (left <= right) {
            int mid = (left + right) / 2;
            int cnt = 0;
            for (int i = 0, j = 0; j < n; j++) {
                while (nums[j] - nums[i] > mid) {
                    i++;
                }
                cnt += j - i;
            }
            if (cnt >= k) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

Java版本

```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length, left = 0, right = nums[n - 1] - nums[0];
        while (left <= right) {
            int mid = (left + right) / 2;
            int cnt = 0;
            for (int i = 0, j = 0; j < n; j++) {
                while (nums[j] - nums[i] > mid) {
                    i++;
                }
                cnt += j - i;
            }
            if (cnt >= k) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```



### [334. 递增的三元子序列](https://leetcode.cn/problems/increasing-triplet-subsequence/)

中等

给你一个整数数组 `nums` ，判断这个数组中是否存在长度为 `3` 的递增子序列。

如果存在这样的三元组下标 `(i, j, k)` 且满足 `i < j < k` ，使得 `nums[i] < nums[j] < nums[k]` ，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：nums = [1,2,3,4,5]
输出：true
解释：任何 i < j < k 的三元组都满足题意
```

C++版本

```c++
// 双向遍历
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int n = nums.size();
        if (n < 3) {
            return false;
        }
        vector<int> leftMin(n);
        leftMin[0] = nums[0];
        for (int i = 1; i < n; i++) {
            leftMin[i] = min(leftMin[i - 1], nums[i]);
        }
        vector<int> rightMax(n);
        rightMax[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rightMax[i] = max(rightMax[i + 1], nums[i]);
        }
        for (int i = 1; i < n - 1; i++) {
            if (nums[i] > leftMin[i - 1] && nums[i] < rightMax[i + 1]) {
                return true;
            }
        }
        return false;
    }
};

// 贪心
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int n = nums.size();
        if (n < 3) {
            return false;
        }
        int first = nums[0], second = INT_MAX;
        for (int i = 1; i < n; i++) {
            int num = nums[i];
            if (num > second) {
                return true;
            } else if (num > first) {
                second = num;
            } else {
                first = num;
            }
        }
        return false;
    }
};
```

Java版本

```java
// 双向遍历
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int n = nums.length;
        if (n < 3) {
            return false;
        }
        int[] leftMin = new int[n];
        leftMin[0] = nums[0];
        for (int i = 1; i < n; i++) {
            leftMin[i] = Math.min(leftMin[i - 1], nums[i]);
        }
        int[] rightMax = new int[n];
        rightMax[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rightMax[i] = Math.max(rightMax[i + 1], nums[i]);
        }
        for (int i = 1; i < n - 1; i++) {
            if (nums[i] > leftMin[i - 1] && nums[i] < rightMax[i + 1]) {
                return true;
            }
        }
        return false;
    }
}

// 贪心
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int n = nums.length;
        if (n < 3) {
            return false;
        }
        int first = nums[0], second = Integer.MAX_VALUE;
        for (int i = 1; i < n; i++) {
            int num = nums[i];
            if (num > second) {
                return true;
            } else if (num > first) {
                second = num;
            } else {
                first = num;
            }
        }
        return false;
    }
}
```



### [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode.cn/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

简单

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数在数组的前半部分，所有偶数在数组的后半部分。

**示例：**

```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

C++版本

```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        for(int slow = 0, fast = 0; fast < nums.size(); fast++) {
            if(nums[fast] % 2 == 1) {
                int temp = nums[fast];
                nums[fast] = nums[slow];
                nums[slow] = temp;
                slow++;
            }
        }
        return nums;
    }
};
```

Java版本

```java
class Solution {
    public int[] exchange(int[] nums) {
        for(int slow = 0, fast = 0; fast < nums.length; fast++) {
            if(nums[fast] % 2 == 1) {
                int temp = nums[fast];
                nums[fast] = nums[slow];
                nums[slow] = temp;
                slow++;
            }
        }
        return nums;
    }
}
```



### [978. 最长湍流子数组](https://leetcode.cn/problems/longest-turbulent-subarray/)

中等

给定一个整数数组 `arr` ，返回 `arr` 的 *最大湍流子数组的**长度*** 。

如果比较符号在子数组中的每个相邻元素对之间翻转，则该子数组是 **湍流子数组** 。

更正式地来说，当 `arr` 的子数组 `A[i], A[i+1], ..., A[j]` 满足仅满足下列条件时，我们称其为*湍流子数组*：

- 若 `i <= k < j` ：
  - 当 `k` 为奇数时， `A[k] > A[k+1]`，且
  - 当 `k` 为偶数时，`A[k] < A[k+1]`；
- **或** 若 `i <= k < j` 
  - 当 `k` 为偶数时，`A[k] > A[k+1]` ，且
  - 当 `k` 为奇数时， `A[k] < A[k+1]`。

**示例 1：**

```
输入：arr = [9,4,2,10,7,8,8,1,9]
输出：5
解释：arr[1] > arr[2] < arr[3] > arr[4] < arr[5]
```

C++版本

```c++
// 方法一：滑动窗口
class Solution {
public:
    int maxTurbulenceSize(vector<int>& arr) {
        int n = arr.size();
        int ret = 1;
        int left = 0, right = 0;

        while (right < n - 1) {
            if (left == right) {
                if (arr[left] == arr[left + 1]) {
                    left++;
                }
                right++;
            } else {
                if (arr[right - 1] < arr[right] && arr[right] > arr[right + 1]) {
                    right++;
                } else if (arr[right - 1] > arr[right] && arr[right] < arr[right + 1]) {
                    right++;
                } else {
                    left = right;
                }
            }
            ret = max(ret, right - left + 1);
        }
        return ret;
    }
};

// 方法二：动态规划
class Solution {
public:
    int maxTurbulenceSize(vector<int>& arr) {
        int n = arr.size();
        vector<vector<int>> dp(n, vector<int>(2, 1));
        dp[0][0] = dp[0][1] = 1;
        for (int i = 1; i < n; i++) {
            if (arr[i - 1] > arr[i]) {
                dp[i][0] = dp[i - 1][1] + 1;
            } else if (arr[i - 1] < arr[i]) {
                dp[i][1] = dp[i - 1][0] + 1;
            }
        }

        int ret = 1;
        for (int i = 0; i < n; i++) {
            ret = max(ret, dp[i][0]);
            ret = max(ret, dp[i][1]);
        }
        return ret;
    }
};

// 方法三：双指针
class Solution {
public:
    int maxTurbulenceSize(vector<int>& arr) {
        int len = arr.size();
        if (len < 2) {
            return len;
        }

        int left = 0;
        int right = 1;
		// 为 true 表示 arr[i - 1] < arr[i]
        bool pre = false;
        int res = 1;
        while (right < len) {
            bool current = arr[right - 1] < arr[right];
            if (current == pre) {
                left = right - 1;
            }
            if (arr[right - 1] == arr[right]) {
                left = right;
            }
            right++;
            res = max(res, right - left);
            pre = current;
        }
        return res;
    }
};
```

Java版本

```java
// 方法一：滑动窗口
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        int n = arr.length;
        int ret = 1;
        int left = 0, right = 0;

        while (right < n - 1) {
            if (left == right) {
                if (arr[left] == arr[left + 1]) {
                    left++;
                }
                right++;
            } else {
                if (arr[right - 1] < arr[right] && arr[right] > arr[right + 1]) {
                    right++;
                } else if (arr[right - 1] > arr[right] && arr[right] < arr[right + 1]) {
                    right++;
                } else {
                    left = right;
                }
            }
            ret = Math.max(ret, right - left + 1);
        }
        return ret;
    }
}

// 方法二：动态规划
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        int n = arr.length;
        int[][] dp = new int[n][2];
        dp[0][0] = dp[0][1] = 1;
        for (int i = 1; i < n; i++) {
            dp[i][0] = dp[i][1] = 1;
            if (arr[i - 1] > arr[i]) {
                dp[i][0] = dp[i - 1][1] + 1;
            } else if (arr[i - 1] < arr[i]) {
                dp[i][1] = dp[i - 1][0] + 1;
            }
        }

        int ret = 1;
        for (int i = 0; i < n; i++) {
            ret = Math.max(ret, dp[i][0]);
            ret = Math.max(ret, dp[i][1]);
        }
        return ret;
    }
}

// 方法三：双指针
public class Solution {

    public int maxTurbulenceSize(int[] arr) {
        int len = arr.length;
        if (len < 2) {
            return len;
        }

        int left = 0;
        int right = 1;
		// 为 true 表示 arr[i - 1] < arr[i]
        boolean pre = false;
        int res = 1;
        while (right < len) {
            boolean current = arr[right - 1] < arr[right];
            if (current == pre) {
                left = right - 1;
            }
            if (arr[right - 1] == arr[right]) {
                left = right;
            }
            right++;
            res = Math.max(res, right - left);
            pre = current;
        }
        return res;
    }
}
```



### [925. 长按键入](https://leetcode.cn/problems/long-pressed-name/)

简单

你的朋友正在使用键盘输入他的名字 `name`。偶尔，在键入字符 `c` 时，按键可能会被*长按*，而字符可能被输入 1 次或多次。

你将会检查键盘输入的字符 `typed`。如果它对应的可能是你的朋友的名字（其中一些字符可能被长按），那么就返回 `True`。

**示例 1：**

```
输入：name = "alex", typed = "aaleex"
输出：true
解释：'alex' 中的 'a' 和 'e' 被长按。
```

C++版本

```c++
class Solution {
public:
    bool isLongPressedName(string name, string typed) {
        int index1 = 0, index2 = 0;
        while(index2 < typed.size()) {
            if(index1 < name.size() && name[index1] == typed[index2]) {
                index1++;
                index2++;
            } else if(index2 > 0 && typed[index2-1] == typed[index2]) {
                index2++;
            } else {
                return false;
            }
        }
        return index1 == name.size();
    }
};
```

Java版本

```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {
        int index1 = 0, index2 = 0;
        while(index2 < typed.length()) {
            if(index1 < name.length() && name.charAt(index1) == typed.charAt(index2)) {
                index1++;
                index2++;
            } else if(index2 > 0 && typed.charAt(index2-1) == typed.charAt(index2)) {
                index2++;
            } else {
                return false;
            }
        }
        return index1 == name.length();
    }
}
```



### [844. 比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)

简单

给定 `s` 和 `t` 两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 `true` 。`#` 代表退格字符。

**注意：**如果对空文本输入退格字符，文本继续为空。

**示例 1：**

```
输入：s = "ab#c", t = "ad#c"
输出：true
解释：s 和 t 都会变成 "ac"。
```

C++版本

```c++
// 方法一：重构字符串
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        return build(S) == build(T);
    }

    string build(string str) {
        string ret;
        for (char ch : str) {
            if (ch != '#') {
                ret.push_back(ch);
            } else if (!ret.empty()) {
                ret.pop_back();
            }
        }
        return ret;
    }
};

// 方法二：双指针
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        int i = S.length() - 1, j = T.length() - 1;
        int skipS = 0, skipT = 0;

        while (i >= 0 || j >= 0) {
            while (i >= 0) {
                if (S[i] == '#') {
                    skipS++, i--;
                } else if (skipS > 0) {
                    skipS--, i--;
                } else {
                    break;
                }
            }
            while (j >= 0) {
                if (T[j] == '#') {
                    skipT++, j--;
                } else if (skipT > 0) {
                    skipT--, j--;
                } else {
                    break;
                }
            }
            if (i >= 0 && j >= 0) {
                if (S[i] != T[j]) {
                    return false;
                }
            } else {
                if (i >= 0 || j >= 0) {
                    return false;
                }
            }
            i--, j--;
        }
        return true;
    }
};
```

Java版本

```java
// 方法一：重构字符串 
class Solution {
    public boolean backspaceCompare(String S, String T) {
        return build(S).equals(build(T));
    }

    public String build(String str) {
        StringBuffer ret = new StringBuffer();
        int length = str.length();
        for (int i = 0; i < length; ++i) {
            char ch = str.charAt(i);
            if (ch != '#') {
                ret.append(ch);
            } else {
                if (ret.length() > 0) {
                    ret.deleteCharAt(ret.length() - 1);
                }
            }
        }
        return ret.toString();
    }
}

// 方法二：双指针
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int i = S.length() - 1, j = T.length() - 1;
        int skipS = 0, skipT = 0;

        while (i >= 0 || j >= 0) {
            while (i >= 0) {
                if (S.charAt(i) == '#') {
                    skipS++;
                    i--;
                } else if (skipS > 0) {
                    skipS--;
                    i--;
                } else {
                    break;
                }
            }
            while (j >= 0) {
                if (T.charAt(j) == '#') {
                    skipT++;
                    j--;
                } else if (skipT > 0) {
                    skipT--;
                    j--;
                } else {
                    break;
                }
            }
            if (i >= 0 && j >= 0) {
                if (S.charAt(i) != T.charAt(j)) {
                    return false;
                }
            } else {
                if (i >= 0 || j >= 0) {
                    return false;
                }
            }
            i--;
            j--;
        }
        return true;
    }
}
```



### [415. 字符串相加](https://leetcode.cn/problems/add-strings/)

简单

给定两个字符串形式的非负整数 `num1` 和`num2` ，计算它们的和并同样以字符串形式返回。

你不能使用任何內建的用于处理大整数的库（比如 `BigInteger`）， 也不能直接将输入的字符串转换为整数形式。

**示例 1：**

```
输入：num1 = "11", num2 = "123"
输出："134"
```

C++版本

```c++
class Solution {
public:
    string addStrings(string num1, string num2) {
        int i = num1.length() - 1, j = num2.length() - 1, add = 0;
        string ans = "";
        while (i >= 0 || j >= 0 || add != 0) {
            int x = i >= 0 ? num1[i] - '0' : 0;
            int y = j >= 0 ? num2[j] - '0' : 0;
            int result = x + y + add;
            ans.push_back('0' + result % 10);
            add = result / 10;
            i -= 1;
            j -= 1;
        }
        // 计算完以后的答案需要翻转过来
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

Java版本

```java
class Solution {
    public String addStrings(String num1, String num2) {
        int i = num1.length() - 1, j = num2.length() - 1, add = 0;
        StringBuffer ans = new StringBuffer();
        while (i >= 0 || j >= 0 || add != 0) {
            int x = i >= 0 ? num1.charAt(i) - '0' : 0;
            int y = j >= 0 ? num2.charAt(j) - '0' : 0;
            int result = x + y + add;
            ans.append(result % 10);
            add = result / 10;
            i--;
            j--;
        }
        // 计算完以后的答案需要翻转过来
        ans.reverse();
        return ans.toString();
    }
}
```



## 数组滑动窗口

### [1343. 大小为 K 且平均值大于等于阈值的子数组数目](https://leetcode.cn/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/)

中等

给你一个整数数组 `arr` 和两个整数 `k` 和 `threshold` 。

请你返回长度为 `k` 且平均值大于等于 `threshold` 的子数组数目。

**示例 1：**

```
输入：arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4
输出：3
解释：子数组 [2,5,5],[5,5,5] 和 [5,5,8] 的平均值分别为 4，5 和 6 。其他长度为 3 的子数组的平均值都小于 4 （threshold 的值)。
```

C++版本

```c++
class Solution {
public:
    int numOfSubarrays(vector<int>& arr, int k, int threshold) {
        int left = 0, right = 0, window_sum = 0, res = 0;
        int len = arr.size();
        while(right < len) {
            window_sum += arr[right];
            if(right - left + 1 >= k) {
               if(window_sum/k >= threshold){
                   res++;
               }
               window_sum -= arr[left];
               left++;
            }
            right++;
        }
        return res;
    }
};
```

Java版本

```java
class Solution {
    public int numOfSubarrays(int[] arr, int k, int threshold) {
        int left = 0, right = 0, window_sum = 0, res = 0;
        int len = arr.length;
        while(right < len) {
            window_sum += arr[right];
            if(right - left + 1 >= k) {
               if(window_sum/k >= threshold){
                   res++;
               }
               window_sum -= arr[left];
               left++;
            }
            right++;
        }
        return res;
    }
}
```



### [643. 子数组最大平均数 I](https://leetcode.cn/problems/maximum-average-subarray-i/)

简单

给你一个由 `n` 个元素组成的整数数组 `nums` 和一个整数 `k` 。

请你找出平均数最大且 **长度为 `k`** 的连续子数组，并输出该最大平均数。

任何误差小于 `10-5` 的答案都将被视为正确答案。

**示例 1：**

```
输入：nums = [1,12,-5,-6,50,3], k = 4
输出：12.75
解释：最大平均数 (12-5-6+50)/4 = 51/4 = 12.75
```

C++版本

```c++
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        int left = 0, right = 0, len = nums.size();
        double res = -10000, window_sum = 0;
        while(right < len) {
            window_sum += nums[right];
            if(right - left + 1 >= k) {
                res = max(window_sum/k, res);
                window_sum -= nums[left];
                left++;
            }
            right++;
        }
        return res;
    }
};
```

Java版本

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int left = 0, right = 0, len = nums.length;
        double res = -10000, window_sum = 0;
        while(right < len) {
            window_sum += nums[right];
            if(right - left + 1 >= k) {
                res = Math.max(window_sum/k, res);
                window_sum -= nums[left];
                left++;
            }
            right++;
        }
        return res;
    }
}
```



### [1052. 爱生气的书店老板](https://leetcode.cn/problems/grumpy-bookstore-owner/)

中等

有一个书店老板，他的书店开了 `n` 分钟。每分钟都有一些顾客进入这家商店。给定一个长度为 `n` 的整数数组 `customers` ，其中 `customers[i]` 是在第 `i` 分钟开始时进入商店的顾客数量，所有这些顾客在第 `i` 分钟结束后离开。

在某些时候，书店老板会生气。 如果书店老板在第 `i` 分钟生气，那么 `grumpy[i] = 1`，否则 `grumpy[i] = 0`。

当书店老板生气时，那一分钟的顾客就会不满意，若老板不生气则顾客是满意的。

书店老板知道一个秘密技巧，能抑制自己的情绪，可以让自己连续 `minutes` 分钟不生气，但却只能使用一次。

请你返回 *这一天营业下来，最多有多少客户能够感到满意* 。

**示例 1：**

```
输入：customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], minutes = 3
输出：16
解释：书店老板在最后 3 分钟保持冷静。
感到满意的最大客户数量 = 1 + 1 + 1 + 1 + 7 + 5 = 16.
```

C++版本

```c++
class Solution {
public:
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int minutes) {
        int right = 0, window_sum = 0, sum = 0;
        while(right < customers.size()) {
            window_sum += customers[right] * grumpy[right];
            if(right + 1 >= minutes) {
                if(window_sum > sum) {
                    sum  = window_sum;
                }
                window_sum -= customers[right - minutes + 1] * grumpy[right - minutes + 1];
            }
            right++;
        }
        for(int i = 0; i < customers.size(); i++) {
            if(grumpy[i] == 0) {
                sum += customers[i];
            }
        }
        return sum;
    }
};
```

Java版本

```java
class Solution {
    public int maxSatisfied(int[] customers, int[] grumpy, int minutes) {
        int right = 0, window_sum = 0, sum = 0;
        while(right < customers.length) {
            window_sum += customers[right] * grumpy[right];
            if(right + 1 >= minutes) {
                if(window_sum > sum) {
                    sum  = window_sum;
                }
                window_sum -= customers[right - minutes + 1] * grumpy[right - minutes + 1];
            }
            right++;
        }
        for(int i = 0; i < customers.length; i++) {
            if(grumpy[i] == 0) {
                sum += customers[i];
            }
        }
        return sum;
    }
}
```



### [1423. 可获得的最大点数](https://leetcode.cn/problems/maximum-points-you-can-obtain-from-cards/)

中等

几张卡牌 **排成一行**，每张卡牌都有一个对应的点数。点数由整数数组 `cardPoints` 给出。

每次行动，你可以从行的开头或者末尾拿一张卡牌，最终你必须正好拿 `k` 张卡牌。

你的点数就是你拿到手中的所有卡牌的点数之和。

给你一个整数数组 `cardPoints` 和整数 `k`，请你返回可以获得的最大点数。

**示例 1：**

```
输入：cardPoints = [1,2,3,4,5,6,1], k = 3
输出：12
解释：第一次行动，不管拿哪张牌，你的点数总是 1 。但是，先拿最右边的卡牌将会最大化你的可获得点数。最优策略是拿右边的三张牌，最终点数为 1 + 6 + 5 = 12 。
```

C++版本

```c++
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int left = 0, sum = 0;
        while(left < k) {
            sum += cardPoints[left++];
        }
        int maxSum = sum;
        while(left > 0) {
            left--;
            sum += cardPoints[cardPoints.size() - (k - left)] - cardPoints[left];
            maxSum = max(sum, maxSum);
        }
        return maxSum;
    }
};
```

Java版本

```java
class Solution {
    public int maxScore(int[] cardPoints, int k) {
        int left = 0, sum = 0;
        while(left < k) {
            sum += cardPoints[left++];
        }
        int maxSum = sum;
        while(left > 0) {
            left--;
            sum += cardPoints[cardPoints.length - (k - left)] - cardPoints[left];
            maxSum = Math.max(sum, maxSum);
        }
        return maxSum;
    }
}
```



### [1456. 定长子串中元音的最大数目](https://leetcode.cn/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

中等

给你字符串 `s` 和整数 `k` 。

请返回字符串 `s` 中长度为 `k` 的单个子字符串中可能包含的最大元音字母数。

英文中的 **元音字母** 为（`a`, `e`, `i`, `o`, `u`）。

**示例 1：**

```
输入：s = "abciiidef", k = 3
输出：3
解释：子字符串 "iii" 包含 3 个元音字母。
```

C++版本

```c++
class Solution {
public:
    int maxVowels(string s, int k) {
        int right = 0, len = s.size();
        int maxVowel = 0, window_sum = 0;
        while(right < len) {
            if(isVowel(s[right])) {
                window_sum++;
            }
            if(right + 1 >= k) {
                maxVowel = max(maxVowel, window_sum);
                if(isVowel(s[right - k + 1])) {
                    window_sum--;
                }
            }
            right++;
        }
        return maxVowel;
    }

    bool isVowel(char vowel) {
        return vowel == 'a' || vowel == 'e' || vowel == 'i' || vowel == 'o' || vowel == 'u';
    }
};
```

Java版本

```java
class Solution {
    public int maxVowels(String s, int k) {
        int right = 0, len = s.length();
        int maxVowel = 0, window_sum = 0;
        while(right < len) {
            if(isVowel(s.charAt(right))) {
                window_sum++;
            }
            if(right + 1 >= k) {
                maxVowel = Math.max(maxVowel, window_sum);
                if(isVowel(s.charAt(right - k + 1))) {
                    window_sum--;
                }
            }
            right++;
        }
        return maxVowel;
    }

    public boolean isVowel(char vowel) {
        return vowel == 'a' || vowel == 'e' || vowel == 'i' || vowel == 'o' || vowel == 'u';
    }
}
```



### [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)

中等

给你两个字符串 `s1` 和 `s2` ，写一个函数来判断 `s2` 是否包含 `s1` 的排列。如果是，返回 `true` ；否则，返回 `false` 。

换句话说，`s1` 的排列之一是 `s2` 的 **子串** 。

**示例 1：**

```
输入：s1 = "ab" s2 = "eidbaooo"
输出：true
解释：s2 包含 s1 的排列之一 ("ba").
```

C++版本

```c++
// 方法一：滑动窗口
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        vector<int> cnt1(26), cnt2(26);
        for (int i = 0; i < n; ++i) {
            ++cnt1[s1[i] - 'a'];
            ++cnt2[s2[i] - 'a'];
        }
        if (cnt1 == cnt2) {
            return true;
        }
        for (int i = n; i < m; ++i) {
            ++cnt2[s2[i] - 'a'];
            --cnt2[s2[i - n] - 'a'];
            if (cnt1 == cnt2) {
                return true;
            }
        }
        return false;
    }
};
// 优化
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        vector<int> cnt(26);
        for (int i = 0; i < n; ++i) {
            --cnt[s1[i] - 'a'];
            ++cnt[s2[i] - 'a'];
        }
        int diff = 0;
        for (int c: cnt) {
            if (c != 0) {
                ++diff;
            }
        }
        if (diff == 0) {
            return true;
        }
        for (int i = n; i < m; ++i) {
            int x = s2[i] - 'a', y = s2[i - n] - 'a';
            if (x == y) {
                continue;
            }
            if (cnt[x] == 0) {
                ++diff;
            }
            ++cnt[x];
            if (cnt[x] == 0) {
                --diff;
            }
            if (cnt[y] == 0) {
                ++diff;
            }
            --cnt[y];
            if (cnt[y] == 0) {
                --diff;
            }
            if (diff == 0) {
                return true;
            }
        }
        return false;
    }
};

// 方法二：双指针
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        vector<int> cnt(26);
        for (int i = 0; i < n; ++i) {
            --cnt[s1[i] - 'a'];
        }
        int left = 0;
        for (int right = 0; right < m; ++right) {
            int x = s2[right] - 'a';
            ++cnt[x];
            while (cnt[x] > 0) {
                --cnt[s2[left] - 'a'];
                ++left;
            }
            if (right - left + 1 == n) {
                return true;
            }
        }
        return false;
    }
};
```

Java版本

```java
// 方法一：滑动窗口
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        int[] cnt1 = new int[26];
        int[] cnt2 = new int[26];
        for (int i = 0; i < n; ++i) {
            ++cnt1[s1.charAt(i) - 'a'];
            ++cnt2[s2.charAt(i) - 'a'];
        }
        if (Arrays.equals(cnt1, cnt2)) {
            return true;
        }
        for (int i = n; i < m; ++i) {
            ++cnt2[s2.charAt(i) - 'a'];
            --cnt2[s2.charAt(i - n) - 'a'];
            if (Arrays.equals(cnt1, cnt2)) {
                return true;
            }
        }
        return false;
    }
}
// 优化
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        int[] cnt = new int[26];
        for (int i = 0; i < n; ++i) {
            --cnt[s1.charAt(i) - 'a'];
            ++cnt[s2.charAt(i) - 'a'];
        }
        int diff = 0;
        for (int c : cnt) {
            if (c != 0) {
                ++diff;
            }
        }
        if (diff == 0) {
            return true;
        }
        for (int i = n; i < m; ++i) {
            int x = s2.charAt(i) - 'a', y = s2.charAt(i - n) - 'a';
            if (x == y) {
                continue;
            }
            if (cnt[x] == 0) {
                ++diff;
            }
            ++cnt[x];
            if (cnt[x] == 0) {
                --diff;
            }
            if (cnt[y] == 0) {
                ++diff;
            }
            --cnt[y];
            if (cnt[y] == 0) {
                --diff;
            }
            if (diff == 0) {
                return true;
            }
        }
        return false;
    }
}

// 方法二：双指针
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        int[] cnt = new int[26];
        for (int i = 0; i < n; ++i) {
            --cnt[s1.charAt(i) - 'a'];
        }
        int left = 0;
        for (int right = 0; right < m; ++right) {
            int x = s2.charAt(right) - 'a';
            ++cnt[x];
            while (cnt[x] > 0) {
                --cnt[s2.charAt(left) - 'a'];
                ++left;
            }
            if (right - left + 1 == n) {
                return true;
            }
        }
        return false;
    }
}
```





































