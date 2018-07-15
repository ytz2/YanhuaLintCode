## 943. Range Sum Query - Immutable

Given an integer array nums, find the sum of the elements between indices`i`and`j(i ≤ j)`, inclusive.

### Example

```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -
>
 1
sumRange(2, 5) -
>
 -1
sumRange(0, 5) -
>
 -3
```

[https://www.lintcode.com/problem/range-sum-query-immutable/description](https://www.lintcode.com/problem/range-sum-query-immutable/description)

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



