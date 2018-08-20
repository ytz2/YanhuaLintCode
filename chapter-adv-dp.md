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

![](/assets/区间DP.png)

## 312 Burst Balloons

Given`n`balloons, indexed from`0`to`n-1`. Each balloon is painted with a number on it represented by array`nums`. You are asked to burst all the balloons. If the you burst balloon`i`you will get`nums[left] * nums[i] * nums[right]`coins. Here`left`and`right`are adjacent indices of`i`. After the burst, the`left`and`right`then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

**Note:**

* You may imagine
  `nums[-1] = nums[n] = 1`
  . They are not real therefore you can not burst them.
* 0 ≤
  `n`
  ≤ 500, 0 ≤
  `nums[i]`
  ≤ 100

**Example:**

```
Input:
[3,1,5,8]
Output:
167 

Explanation: 
nums = [3,1,5,8] --
>
 [3,5,8] --
>
   [3,8]   --
>
  [8]  --
>
 []
             coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167



这道题很容易想的是用递归的解法

选任意一个，打爆，一直打到最后没有，求出每个解的最小值
这样做的坏处是n!的复杂度

那么由于是n!,且求最大，一般对应DP的解。 套用 区间类模板的话， 这类题目是从大化小之后最值问题，那么可以反过来看

假设最后我打爆的是第k个， 那么我最后的解就是 【0， k-1] + k, [k+1,n] 的和的最大

那么再延伸一下 我在【i,j]的区间呢， 【i, k-1], k, [k+1, j]的最大解
最后的解就是【0,n] 

那么为什么是DP呢，因为从大化小的时候会产生很多中间区间，从而可以做记忆化搜索

另外这道题有一个trick,左右各push 1， 这样可以保证边界问题
```

```cpp
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        nums.insert(nums.begin(), 1);
        nums.push_back(1);
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        vector<vector<bool>> visited(n, vector<bool>(n, false));
        for (int i = 1; i <=n-2; i++)
        {
            visited[i][i] = true;
            dp[i][i] = nums[i-1]*nums[i]*nums[i+1];
        }
        return search(nums, dp, visited, 1, n-2);
        //return dp[1][n-2];
    }

    int search(vector<int>& nums, vector<vector<int>>& dp, vector<vector<bool>>& visited, int i, int j)
    {
        if (visited[i][j])
            return dp[i][j];

        for (int k = i; k <= j; k++) //这里要相等， 因为越界没关系，0，但是不越界会miss左边一个右面一堆的问题
        {
            int l = search(nums, dp, visited, i, k-1);
            int r = search(nums, dp, visited, k+1, j);
            int v = nums[k]*nums[i-1]*nums[j+1];
            dp[i][j] = max(dp[i][j], l+r+v);
        }
        visited[i][j] = true;
        return dp[i][j];
    }
};
```

这道题还可以用类似panlidrome dp的技巧来解，其实p那个就是区间类DP， 问你区间内i到j是不是dp的问题

```
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        nums.insert(nums.begin(), 1);
        nums.push_back(1);
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        vector<vector<bool>> visited(n, vector<bool>(n, false));
        for (int i = 1; i <=n-2; i++)
        {
            visited[i][i] = true;
            dp[i][i] = nums[i-1]*nums[i]*nums[i+1];
        }

        for (int l = 2; l <= n-2; l++)
        {
            for (int i = 1; i+l-1 <=n-2; i++)
            {
                int j = i+l-1;
                for (int k = i; k<=j; k++)
                    dp[i][j] = max(dp[i][j], nums[k]*nums[i-1]*nums[j+1] + dp[i][k-1]+dp[k+1][j]);
            }
        }
        return dp[1][n-2];
    }

};
```

![](/assets/pipeidp.png)

## 72 Edit Distance

iven two words_word1\_and\_word2_, find the minimum number of operations required to convert_word1\_to\_word2_.

You have the following 3 operations permitted on a word:

1. Insert a character
2. Delete a character
3. Replace a character

**Example 1:**

```
Input:
 word1 = "horse", word2 = "ros"

Output:
 3

Explanation:

horse -
>
 rorse (replace 'h' with 'r')
rorse -
>
 rose (remove 'r')
rose -
>
 ros (remove 'e')
```

**Example 2:**

```
Input:
 word1 = "intention", word2 = "execution"

Output:
 5

Explanation:

intention -
>
 inention (remove 't')
inention -
>
 enention (replace 'i' with 'e')
enention -
>
 exention (replace 'n' with 'x')
exention -
>
 exection (replace 'n' with 'c')
exection -
>
 execution (insert 'u')
```

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        for (int i = 0; i <= n; i++)
            dp[0][i] = i;
        for (int i = 0; i <= m; i++)
            dp[i][0] = i;
        for (int i = 1; i <= m; i++)
        {
            for (int j = 1; j <= n; j++)
            {
                dp[i][j] = min(min(dp[i][j-1], dp[i-1][j])+1, dp[i-1][j-1] + ((word1[i-1] == word2[j-1])?  0 : 1)); 
            }
        }
        return dp.back().back();
    }
};
```

## 97 Interleaving String

iGiven_s1_,_s2_,_s3_, find whether_s3\_is formed by the interleaving of\_s1\_and\_s2_.

**Example 1:**

```
Input:
 s1 = "aabcc", s2 = "dbbca", 
s3
 = "aadbbcbcac"

Output:
 true
```

**Example 2:**

```
Input:
 s1 = "aabcc", s2 = "dbbca", 
s3
 = "aadbbbaccc"

Output:
 false
```

```cpp
Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

Example 1:

Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
Example 2:

Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
```

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int m = s1.size(), n = s2.size();
        if (m+n != s3.size())
            return false;
        vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
        dp[0][0] = true;
        for (int i = 1; i<=m; i++)
            dp[i][0] = dp[i-1][0] && s1[i-1] == s3[i-1];
        for (int i = 1; i <= n; i++)
            dp[0][i] = dp[0][i-1] && s2[i-1] == s3[i-1];

        for (int i = 1; i <=m; i++ )
        {
            for (int j = 1; j <=n; j++)
                dp[i][j] = dp[i-1][j] && s1[i-1] == s3[i+j-1] || dp[i][j-1] && s2[j-1] == s3[i+j-1];
        }
        return dp.back().back();
    }
};
```

![](/assets/beibao.png)

## 92 Backpack

Given\_n\_items with size Ai, an integer\_m\_denotes the size of a backpack. How full you can fill this backpack?

### Example

If we have`4`items with size`[2, 3, 5, 7]`, the backpack size is 11, we can select`[2, 3, 5]`, so that the max size we can fill this backpack is`10`. If the backpack size is`12`. we can select`[2, 3, 7]`so that we can fulfill the backpack.

You function should return the max size we can fill in the given backpack.

### Challenge

O\(n x m\) time and O\(m\) memory.

O\(n x m\) memory is also acceptable if you do not know how to optimize memory.

### Notice

You can not divide any item into small pieces.

```cpp
class Solution {
public:
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    int backPack(int m, vector<int> &A) {
        // write your code here

        vector<vector<bool>> dp(A.size()+1, vector<bool>(m+1, false));
        dp[0][0] = true;
        for (int i = 1; i <= A.size(); i++)
            dp[i][0] = true;
        for (int i = 1; i <= A.size(); i++)
        {
            for (int j = 0; j <=m; j++)
            {
                dp[i][j] = dp[i-1][j];
                if (j >= A[i-1] && dp[i-1][j-A[i-1]])
                    dp[i][j] = true;
            }
        }
        int res = 0;
        for (int i = 0; i <= m; i++)
            if (dp.back()[i]) res = i;
        return res;
    }
};
```

## 669 Coin Change

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return`-1`.

### Example

Given coins =`[1, 2, 5]`, amount =`11`  
return`3`\(11 = 5 + 5 + 1\)

Given coins =`[2]`, amount =`3`  
return`-1`.

### Notice

You may assume that you have an infinite number of each kind of coin.

### 接龙问题

```cpp
class Solution {
public:
    /**
     * @param coins: a list of integer
     * @param amount: a total amount of money amount
     * @return: the fewest number of coins that you need to make up
     */
    int coinChange(vector<int> &coins, int amount) {
        // write your code here

        vector<int> dp(amount+1, 0);

        int n = coins.size();
        for (int i = 1; i <= amount; i++)
        {
            dp[i] = -1;
            for (int j = 0; j < n; j++)
            {
                if (i >= coins[j] && dp[i-coins[j]] != -1)
                {
                    if (dp[i] == -1 || dp[i-coins[j]] + 1 < dp[i])
                        dp[i] = dp[i-coins[j]]+1;
                }
            }
        }

        return dp.back();
    }
};
```



