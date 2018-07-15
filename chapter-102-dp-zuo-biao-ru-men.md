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

** x **

**n**

grid filled with non-negative numbers, find a path from top left to bottom right which

**minimizes**

the sum of all numbers along its path.

[https://www.lintcode.com/problem/minimum-path-sum/description](https://www.lintcode.com/problem/minimum-path-sum/description)

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

## 111. Climbing Stairs

You are climbing a stair case. It takes **n** steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

### Example

Given an example n=3 , 1+1+1=2+1=1+2=3

return 3

[https://www.lintcode.com/problem/climbing-stairs/description](https://www.lintcode.com/problem/climbing-stairs/description)

### 解题分析:

递推公式都告诉你了。。。直接写吧

### 代码：

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: An integer
     */
    int climbStairs(int n) {
        // write your code here
        if (n <= 2)
            return n;
        vector<int> dp(n+1, 0);
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i<=n; i++ )
            dp[i] = dp[i-1] + dp[i-2];
        return dp[n];
    }
};
```

## 114. Unique Paths

A robot is located at the top-left corner of a\_m\_x\_n\_grid.

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid.

How many possible unique paths are there?

### Example

Given m =`3`and n =`3`, return`6`.  
Given m =`4`and n =`5`, return`35`.

[https://www.lintcode.com/problem/unique-paths/description](https://www.lintcode.com/problem/unique-paths/description)

### 解题分析:

判断： 求个数， 2^n的暴力解

递归的定义：

dp\[i\]\[j\] 从 0， 0 到i,j的个数

递归的初始化

行列为1，这是因为我们的状态方程无效

状态方程：

dp\[i\]\[j\] = dp\[i-1\]\[j\] + dp\[i\]\[j-1\]

递归的结果：

dp\[n\]\[n\]

### 代码：

```cpp
class Solution {
public:
    /**
     * @param m: positive integer (1 <= m <= 100)
     * @param n: positive integer (1 <= n <= 100)
     * @return: An integer
     */
    int uniquePaths(int m, int n) {
        // write your code here
        //state
        if (m == 0 || n == 0)
            return 0;
        vector<vector<int>> dp(m, vector<int>(n, 0));
        // init
        fill(dp[0].begin(), dp[0].end(),1);
        for (int i = 0; i< m; i++)
            dp[i][0] = 1;
        for (int i = 1; i<m; i++)
            for (int j = 1; j<n;j++)
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
        return dp[m-1][n-1];
    }
};
```



## 115. Unique Paths II

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as`1`and`0`respectively in the grid.

### Example

For example,  


There is one obstacle in the middle of a 3x3 grid as illustrated below.

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]

```

The total number of unique paths is`2`.

https://www.lintcode.com/problem/unique-paths-ii/description

### 解题分析:

同上， 有obstacle则为0

### 代码：

```cpp
class Solution {
public:
    /**
     * @param obstacleGrid: A list of lists of integers
     * @return: An integer
     */
    int uniquePathsWithObstacles(vector<vector<int>> &obstacleGrid) {
        int m = obstacleGrid.size(); 
        int n = obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        dp[0][0] = obstacleGrid[0][0] == 0;
        for (int j=1; j<n; j++)
            dp[0][j] = dp[0][j-1] && obstacleGrid[0][j]==0; 
        for (int i = 1; i< m; i++)
            dp[i][0] = dp[i-1][0] && obstacleGrid[i][0]==0;
            
        for (int i = 1; i<m; i++)
            for (int j = 1; j<n;j++)
                dp[i][j] = obstacleGrid[i][j]? 0 : dp[i-1][j] + dp[i][j-1];
        return dp[m-1][n-1];
    }
};
```



