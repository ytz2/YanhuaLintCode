# 1 滚动数组

## 198 House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and**it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight**without alerting the police**.

**Example 1:**

```
Input:
 [1,2,3,1]

Output:
 4

Explanation:
 Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input:
 [2,7,9,3,1]

Output:
 12

Explanation:
 Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        // a[i] = max(nums[i-1] + a[i-2], a[i-1]);
        int n = nums.size();
        if (n== 0)
            return 0;
        if (n == 1)
            return nums[0];
        vector<int> val(2, 0);
        val[1] = nums[0];
        for (int i = 2; i <=n; i++)
            val[i%2] = max(nums[i-1] + val[(i-2)%2], val[(i-1)%2]);
        return val[n%2];
    }
};
```

## 221 Maximal Square

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

**Example:**

```
Input: 


1 0 1 0 0
1 0 
1
1
 1
1 1 
1
1
 1
1 0 0 1 0


Output: 
4
```

```cpp
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int m = matrix.size();
        if ( 0 == m )
            return 0;
        int n = matrix[0].size();
        if ( 0 == n )
            return 0;
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        // init
        int res = 0;
        for (int i = 0;  i < n; i++)
        {
            dp[0][i] = (matrix[0][i] == '1');
            res = max(res, dp[0][i]);
        }    
        for (int i = 0; i < m; i++)
        {
            dp[i][0] = (matrix[i][0] == '1');
            res = max(res, dp[i][0]);
        }    
        
        for (int i = 1; i < m; i++)
        {
            for (int j = 1; j < n; j++)
            {
                if (matrix[i][j] == '1')
                {
                    dp[i][j] = min(dp[i-1][j-1],min(dp[i-1][j], dp[i][j-1]))+1;
                }    
                res = max(res, dp[i][j]);
            }
        }
        return res*res;
    }
};
```



