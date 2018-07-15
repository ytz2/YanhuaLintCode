## 109. Triangle

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

### Example

Given the following triangle:

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

The minimum path sum from top to bottom is 11 \(i.e., 2 + 3 + 5 + 1 = 11\).

[https://www.lintcode.com/problem/triangle/description](https://www.lintcode.com/problem/triangle/description)

### 解题分析:

判断：

1. 求最小值
2. 暴力解需要2^n的解法

有可能是DP

a 递归的状态

dp\[i\]\[j\] : 到 i, j的最小和

b 递归的初始条件

row, col = 0,这个比较特殊， 对角线初始化就好了， 因为只能从一个方向来

c 递归的定义

dp\[i\]\[j\] = min\(dp\[i-1\]\[j-1\], dp\[i-1\]\[j\]\)+matrix\[i\]\[j\]

d递归的结果

扫描最后一个array找出最小的一个

### 代码：

```cpp
class Solution {
public:
    /**
     * @param triangle: a list of lists of integers
     * @return: An integer, minimum path sum
     */
    int minimumTotal(vector<vector<int>> &triangle) {
        // write your code here
        // state dp[i][j] = min sum to i, j
        // dp state
        auto dp = triangle;
        // init state
        for (int i = 1; i < triangle.size(); i++)
        {
            dp[i][0] = dp[i-1][0]+triangle[i][0];
            dp[i][i] = dp[i-1][i-1] + triangle[i][i];
        }

        /*  i-1,j-1     i-1, j
        *               i, j
        */ 

        for (int i = 1; i < triangle.size(); i++)
        {
            for (int j =1; j <triangle[i].size()-1; j++)
            {
                dp[i][j] = min(dp[i-1][j-1], dp[i-1][j])+triangle[i][j];
            }
        }
        int res = INT_MAX;
        for (auto each : dp.back())
            res = min(res, each);
        return res;
    }
};
```



## 110. Minimum Path Sum

Given a 

**m**

** x **

**n**

 grid filled with non-negative numbers, find a path from top left to bottom right which 

**minimizes**

 the sum of all numbers along its path.

https://www.lintcode.com/problem/minimum-path-sum/description

### 解题分析:

同上

### 代码：

```cpp
class Solution {
public:
    /**
     * @param grid: a list of lists of integers
     * @return: An integer, minimizes the sum of all numbers along its path
     */
    int minPathSum(vector<vector<int>> &grid) {
        // write your code here
        // like the triangle but much easier
        if (grid.empty() || grid[0].empty())
            return 0;
        auto dp = grid;
        // init
        for (int i = 1; i< grid.size(); i++)
            dp[i][0] = dp[i-1][0] + grid[i][0];
        for (int j = 1; j< grid[0].size(); j++)
            dp[0][j] = dp[0][j-1] + grid[0][j];
        // function
        
        for (int i = 1; i < grid.size(); i++)
        {
            for (int j = 1; j<grid[0].size(); j++)
            {
                dp[i][j] = min(dp[i][j-1], dp[i-1][j]) + grid[i][j];
            }
        }
        return dp.back().back();
    }
};
```



