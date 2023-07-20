# 数组基础题目

## 数组操作题目

### [189. 轮转数组](https://leetcode.cn/problems/rotate-array/)

难度中等

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

C++版本

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k = k % nums.size();
        if (k != 0 || nums.size() != 1) {
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

C++版本

```c++
class Solution {
public:
    int pivotIndex(vector<int> &nums) {
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

C++版本

```
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

```
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

#### [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

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

































































































































































































