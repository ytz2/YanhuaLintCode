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

[https://leetcode.com/problems/triangle/](https://leetcode.com/problems/triangle/)

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
    int minimumTotal(vector<vector<int>>& triangle) {
        if (triangle.empty()) return 0;
        int n =  triangle.back().size();
        vector<vector<int>> dp(2, vector<int>(n, 0));
        for (int i = 0; i < n; i++) {
            dp[0][i] = triangle.back()[i];
        }
        for (int i = 1; i < n; i++ ) {
            for (int j = 0; j < n - i; j++) {
                dp[i%2][j] = min(dp[(i+1)%2][j], dp[(i+1)%2][j+1]) + triangle[n - i -1][j];
            }
        }
        return dp[(n+1) %2][0];
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

[https://leetcode.com/problems/minimum-path-sum/](https://leetcode.com/problems/minimum-path-sum/)

### 解题分析:

同上

### 代码：

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if (grid.empty() || grid[0].empty())
            return 0;
        vector<vector<int>> dp(grid.size(), vector<int>(grid[0].size(), 0));
        int s = 0;
        for (int i = 0; i < grid[0].size(); i++) {
            s += grid[0][i];
            dp[0][i] = s;
        }
        s = 0;
        for (int i = 0; i < grid.size(); i++) {
            s += grid[i][0];
            dp[i][0] = s;
        }
        for (int i = 1; i < grid.size(); i++) {
            for (int j = 1; j < grid[0].size(); j++) {
                dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
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

[https://leetcode.com/problems/climbing-stairs/](https://leetcode.com/problems/climbing-stairs/)

### 解题分析:

递推公式都告诉你了。。。直接写吧

### 代码：

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2) return n;
        vector<int> dp{1, 2};
        for (int i = 2; i < n; i++){
            dp[i%2] = dp[(i-1)%2] + dp[(i-2)%2];
        }
        return dp[(n-1)%2];
    }
};
```

## 272. Climbing Stairs II

A child is running up a staircase with`n`steps, and can hop either 1 step, 2 steps, or 3 steps at a time. Implement a method to count how many possible ways the child can run up the stairs.

### Example

n=`3`  
1+1+1=2+1=1+2=3=3

return`4`

[https://www.lintcode.com/problem/climbing-stairs-ii/description](https://www.lintcode.com/problem/climbing-stairs-ii/description)

### 解题分析:

递推公式都告诉你了。。。直接写吧

### 代码：

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @return: An Integer
     */
    int climbStairs2(int n) {
        // write your code here
        if (n==0)
            return 1;
        if (n <= 2)
            return n;
        if (n == 3)
            return 4;
        vector<int> dp(n, 0);
        dp[0] = 1;dp[1]=2;dp[2]=4;
        for (int i = 3; i<n;i++)
            dp[i] = dp[i-3] + dp[i-2]+dp[i-1];
        return dp.back();
    }
};
```

## 

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

[https://www.lintcode.com/problem/unique-paths-ii/description](https://www.lintcode.com/problem/unique-paths-ii/description)

[https://leetcode.com/problems/unique-paths-ii/](https://leetcode.com/problems/unique-paths-ii/)

### 解题分析:

同上， 有obstacle则为0

### 代码：

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        auto& o = obstacleGrid;
        if (o.empty() || o[0].empty() || o[0][0] || o.back().back()) return 0;
        vector<vector<int>> dp(o.size(), vector<int>(o[0].size(), 0));
        dp[0][0] = 1;
        for (int i = 1; i < o[0].size(); i++) 
            dp[0][i] = o[0][i] == 1 ? 0 : dp[0][i-1];
        for (int i = 1; i < o.size(); i++) 
            dp[i][0] = o[i][0] == 1 ? 0 : dp[i - 1][0];
        for (int i = 1; i < dp.size(); i++) {
            for (int j = 1; j < dp[0].size(); j++) {
                if (o[i][j] == 1) dp[i][j] = 0;
                else dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp.back().back();
    }
};
```

## 116. Jump Game

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

### Example

A =`[2,3,1,1,4]`, return`true`.

A =`[3,2,1,0,4]`, return`false`.

[https://www.lintcode.com/problem/jump-game/description](https://www.lintcode.com/problem/jump-game/description)

[https://leetcode.com/problems/jump-game/](https://leetcode.com/problems/jump-game/)

### 解题分析:

接龙形的DP,这种DP的特点是当前的位置由i以前的位置决定，但是没有明显的递推关系， 需要往回寻找符合跳往i的位置j是否存在且j以前的状态需要叠加。

接龙的形式： j , state j -&gt; i, state i -&gt; last answer

dp的定义

dp\[i\]， 能否到达

dp的初始条件：

dp\[0\] = jump\[0\]!=0;

dp的状态方程：

dp\[i\] = any of dp\[j\]且j+jump\[j\]&gt;=i

dp的答案：

dp.back\(\)

### 代码：

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) return true;
        vector<bool> dp(n, false);
        dp[0] = true;
        for (int i = 1; i < n; i++) {
            for (int j = i - 1; j >=0; j--) {
                if (dp[j] && i - j <= nums[j]) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp.back();
    }
};
```

## 117. Jump Game II

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

### Example

Given array A =`[2,3,1,1,4]`

The minimum number of jumps to reach the last index is`2`. \(Jump`1`step from index 0 to 1, then`3`steps to the last index.\)

[https://www.lintcode.com/problem/jump-game-ii/description](https://www.lintcode.com/problem/jump-game-ii/description)

### 代码：

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        vector<int> dp(nums.size(), INT_MAX);
        dp[0] = 0;
        for (int i = 1; i < nums.size(); i++) {
            for (int j =0; j < i; j++) {
                if (dp[j] != INT_MAX && i - j <= nums[j]) 
                {
                     dp[i] = min(dp[i], dp[j] + 1);
                }   
            }
        }
        return dp.back();
    }
};
```

## 

## 76. Longest Increasing Subsequence

Given a sequence of integers, find the longest increasing subsequence \(LIS\).

You code should return the length of the LIS.

### Example

For`[5, 4, 1, 2, 3]`, the LIS is`[1, 2, 3]`, return`3`  
For`[4, 2, 4, 5, 3, 7]`, the LIS is`[2, 4, 5, 7]`, return`4`

### Challenge

Time complexity O\(n^2\) or O\(nlogn\)

[https://www.lintcode.com/problem/longest-increasing-subsequence/description](https://www.lintcode.com/problem/longest-increasing-subsequence/description)

[https://leetcode.com/problems/longest-increasing-subsequence/solution/](https://leetcode.com/problems/longest-increasing-subsequence/solution/)

### 解题分析:

有一道不需要管顺序的，是放一个hash set,然后上下shoot求边界。这个题不是：

为什么？

1. 求最长
2. 给的是一个sequence
3. 暴力的话是o\(n^2\) 

接龙dp优化不了太多，只是减少重复计算而已。 还是o\(n^2\),平均复杂度稍微好点

dp的定义

dp\[i\]， 以i为结尾的最长subseq

dp的初始条件：

dp\[\*\] = 1

dp的状态方程：

dp\[i\] = any of max\(a\[j\] &lt; a\[i\]\)+1

dp的答案：

max of dp

另外这个题放了一个prev在里面来track从哪里来的， prev初始为各自的index,这样就可以找到最优的其中之一的解

### 代码：

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.empty()) return 0;
        vector<int> dp(nums.size(), 1);
        dp[0] = 1;
        int res = 1;
        for (int i = 1; i < nums.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j])
                {
                    dp[i] = max(dp[i], dp[j]+1);
                    res = max(dp[i], res);
                }   
            }
        }
        return res;
    }
};
```

## 603. Largest Divisible Subset

Given a set of`distinct positive`integers, find the largest subset such that every pair`(Si, Sj)`of elements in this subset satisfies:`Si % Sj = 0`or`Sj % Si = 0`.

### Example

Given nums =`[1,2,3]`, return`[1,2]`or`[1,3]`

Given nums =`[1,2,4,8]`, return`[1,2,4,8]`

[https://www.lintcode.com/problem/largest-divisible-subset/description](https://www.lintcode.com/problem/largest-divisible-subset/description)

题目描述的不准确，其实是找倍数关系的递增数组

给的不是sequence，而是给的是collection,看上去不是DP， 但实际上最后求得是一组数， 那么他们是顺序数出，先sort,然后就可以接龙了， 接龙的思路和longest increase subsequence是一样的，略去

### 代码：

```cpp
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        if (nums.empty()) return {};
        vector<int> counter(nums.size(), 1);
        vector<int> from(nums.size(), 0);
        int maxInd = 0;
        int maxCount = 1;
        for (int i = 0; i < nums.size(); i++) from[i] = i;
        for (int i = 1; i < nums.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] % nums[j]) continue;
                if (counter[j] + 1 > counter[i]) {
                    counter[i] = counter[j] + 1;
                    from[i] = j;
                    if (counter[i] > maxCount) {
                        maxCount = counter[i];
                        maxInd = i;
                    }
                }
            }
        }
        vector<int> res;
        int i = maxInd;
        res.push_back(nums[i]);
        while( from[i] != i) {
            res.push_back(nums[from[i]]);
            i = from[i];
        }
        return res;
    }
};
```

## 513. Perfect Squares

Given a positive integer`n`, find the least number of perfect square numbers \(for example,`1, 4, 9, 16, ...`\) which sum to n.

### Example

Given n =`12`, return`3`because`12 = 4 + 4 + 4`  
Given n =`13`, return`2`because`13 = 4 + 9`

[https://www.lintcode.com/problem/perfect-squares/description](https://www.lintcode.com/problem/perfect-squares/description)

### 解题分析:

dp的定义

dp\[i\] 第i个数可以表示的个数

dp的初始条件：

dp\[0\] = 0

dp的状态方程：

dp\[i\] = any of min\(a\[i - j^2\)+1\)

dp的答案：

dp.back\(\)

### 代码：

```cpp
class Solution {
public:
    /**
     * @param n: a positive integer
     * @return: An integer
     */
    int numSquares(int n) {
        // write your code here
        vector<int> dp(n+1, INT_MAX);
        dp[0] = 0;
        for (int i = 1; i<=n; i++)
        {
            for (int j = 1; i-j*j >=0; j++)
            {
                dp[i] = min(dp[i-j*j]+1, dp[i]);
            }
        }
        return dp.back();
    }
};
```

## 630. Knight Shortest Path II

Given a knight in a chessboard`n * m`\(a binary matrix with 0 as empty and 1 as barrier\). the knight initialze position is`(0, 0)`and he wants to reach position`(n - 1, m - 1)`, Knight can only be from left to right. Find the shortest path to the destination position, return the length of the route. Return`-1`if knight can not reached.

### Example

```
[[0,0,0,0],
 [0,0,0,0],
 [0,0,0,0]]

Return 3

[[0,0,0,0],
 [0,0,0,0],
 [0,1,0,0]]

Return -1
```

[https://www.lintcode.com/problem/knight-shortest-path-ii/description](https://www.lintcode.com/problem/knight-shortest-path-ii/description)

### 解题分析:

从左往右走，画下格子，就好了

### 代码：

```cpp
class Solution {
public:
    /**
     * @param grid: a chessboard included 0 and 1
     * @return: the shortest path
     */
    int shortestPath2(vector<vector<bool>> &grid) {
        // write your code here
        int m = grid.size();
        if (m == 0)
            return -1;
        int n = grid[0].size();
        if (n == 0)
            return -1;

        vector<int> dx{-1, 1, -2, 2};
        vector<int> dy{-2, -2, -1,-1};
        vector<vector<int>> dp(m, vector<int>(n, INT_MAX));
        dp[0][0] = 0;
        for (int j=1; j < n; j++)
        {
            for (int i = 0; i < m; i++)
            {
                if (grid[i][j])
                    continue;
                for (int k = 0; k < 4; k++)
                {
                    int ox = i + dx[k];
                    int oy = j + dy[k];
                    if (ox >= 0 && ox < m && oy >= 0 && oy < n && dp[ox][oy] != INT_MAX)
                    {
                        dp[i][j] = min(dp[i][j], dp[ox][oy]+1);
                    }
                }
            }
        }
        if (dp[m-1][n-1] == INT_MAX)
            return -1;
        return dp[m-1][n-1];
    }
```



