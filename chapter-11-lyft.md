## 44. Wildcard Matching

Given an input string \(`s`\) and a pattern \(`p`\), implement wildcard pattern matching with support for`'?'`and`'*'`.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```

The matching should cover the**entire**input string \(not partial\).

**Note:**

* `s`
   could be empty and contains only lowercase letters
  `a-z`
  .
* `p`
  could be empty and contains only lowercase letters
  `a-z`
  , and characters like
  `?`
   or 
  `*`
  .

**Example 1:**

```
Input:

s = "aa"
p = "a"

Output:
 false

Explanation:
 "a" does not match the entire string "aa".
```

**Example 2:**

```
Input:

s = "aa"
p = "*"

Output:
 true

Explanation:
 '*' matches any sequence.
```

**Example 3:**

```
Input:

s = "cb"
p = "?a"

Output:
 false

Explanation:
 '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```

**Example 4:**

```
Input:

s = "adceb"
p = "*a*b"

Output:
 true

Explanation:
 The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```

**Example 5:**

```
Input:

s = "acdcb"
p = "a*c?b"

Output:
 false
```

[https://leetcode.com/problems/wildcard-matching/description/](https://leetcode.com/problems/wildcard-matching/description/)

### 解题分析:

字符类的动态规划：

dp\[i\]\[j\] : 从 0-i 个和0-j个是否符合描述

dp的初始条件

dp\[0\]\[0\] true

dp\[0\]\[1-j\]  = dp\[0\]\[i-1\] && p\[i\] == '\*'  除非 都是\*\*\*\*\*\*\*否则中间出现一个就完了， 因为source为空

dp\[1-i\]\[0\] , match不上全为false

dp的定义：

dp\[i\]\[j\]:

a dp\[i-1\]\[j-1\] = true,且， i, j match, 即 '?' 或者相等

b j = \* 那么两种情况： 吃掉第i个，或者不算 dp\[i\]\[j-1\] or dp\[i-1\]\[j\]

c否则肯定不match

答案： m, n

### 代码：

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(); int n = p.size();
        // 1 state,  1 - i for s matches 1-j for p
        vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
        // 2. init
        // both empty
        dp[0][0]  = true;
        // s empty, p is not
        // only if *****
        for (int i = 0; i < p.size(); i++)
            dp[0][i+1] = dp[0][i] && p[i] == '*';
        // s is full, p is empty
        for (int j = 0; j < s.size(); j++)
            dp[j+1][0] = false;
        // 3 function
        // a) dp[i-1][j-1] && s[i] == p[j] || p[j] == '?'
        // b) p[j] == '*' , dp[i][j] = dp[i][j-1] || dp[i-1][j]
        for (int i = 0; i < s.size(); i++)
        {
            for (int j = 0; j<p.size(); j++)
            {
                if (dp[i][j] && (s[i] == p[j] || p[j] == '?'))
                    dp[i+1][j+1] = true;
                else if (p[j] == '*')
                {
                    dp[i+1][j+1] = dp[i][j+1] || dp[i+1][j];
                }
                else
                    dp[i+1][j+1] = false;
            }
        }
        return dp.back().back();
    }
};
```

## 10. Regular Expression Matching

Given an input string \(`s`\) and a pattern \(`p`\), implement regular expression matching with support for`'.'`and`'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the**entire**input string \(not partial\).

**Note:**

* `s`
   could be empty and contains only lowercase letters
  `a-z`
  .
* `p`
  could be empty and contains only lowercase letters
  `a-z`
  , and characters like 
  `.`
   or 
  `*`
  .

**Example 1:**

```
Input:

s = "aa"
p = "a"

Output:
 false

Explanation:
 "a" does not match the entire string "aa".
```

**Example 2:**

```
Input:

s = "aa"
p = "a*"

Output:
 true

Explanation:
 '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input:

s = "ab"
p = ".*"

Output:
 true

Explanation:
 ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**

```
Input:

s = "aab"
p = "c*a*b"

Output:
 true

Explanation:
 c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
```

**Example 5:**

```
Input:

s = "mississippi"
p = "mis*is*p*."

Output:
 false
```

[https://leetcode.com/problems/regular-expression-matching/description/](https://leetcode.com/problems/regular-expression-matching/description/)

### 解题分析:

**字符类的动态规划：**

这个题感觉好难， 只能这样想了：

如果是\* ， 那么要么完全不重复， 取 \[i\]\[j-2\]的结果， 要么就是 \*前面的字符和当前的字符match,且 这个\*和i的前面一个也match

### 代码：

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();

        vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));

        dp[0][0] = true;
        // if s is empty
        for (int i = 1; i < p.size(); i+=2)
            dp[0][i+1] = dp[0][i-1] && p[i] == '*'; // a*b*c*
        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j< n; j++)
            {
                if (dp[i][j] && (s[i] == p[j] || p[j] == '.'))
                    dp[i+1][j+1] = true;
                else if (p[j] == '*')
                {
                    dp[i+1][j+1] = dp[i+1][j-1] || (p[j-1] == '.' || p[j-1] == s[i]) && dp[i][j+1];
                }
            }
        }
        return dp.back().back();
    }
};
```

## 11. Container With Most Water

Givennnon-negative integersa1,a2, ...,an, where each represents a point at coordinate \(i,ai\).nvertical lines are drawn such that the two endpoints of lineiis at \(i,ai\) and \(i, 0\). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container andnis at least 2.

[https://leetcode.com/problems/container-with-most-water/description/](https://leetcode.com/problems/container-with-most-water/description/)

### 解题分析:

贪婪

### 代码：

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int beg = 0, end = height.size()-1;
        int res = 0;
        while(beg < end)
        {
            int area = min(height[beg], height[end]) * (end-beg);
            res = max(res, area);
            if (height[beg] < height[end])
                beg++;
            else
                end--;
        }
        return res; 
    }
};
```

## 442. Find All Duplicates in an Array

Given an array of integers, 1 ≤ a\[i\] ≤n\(n= size of array\), some elements appear**twice**and others appear**once**.

Find all the elements that appear**twice**in this array.

Could you do it without extra space and in O\(n\) runtime?

**Example:**

```
Input:

[4,3,2,7,8,2,3,1]


Output:

[2,3]
```

### 把value当做index， 然后用翻成负数来标记已经访问过了

### 解题分析:

val to index

### 代码：

```cpp
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        /*
        unordered_map<int, int> dict;
        for (auto each : nums)
            dict[each]++;
        vector<int> res;
        for (auto p : dict)
        {
            if (p.second > 1)
                res.push_back(p.first);
        }
        return res;
        */
        // flip board game
        vector<int> res;
        for (auto each : nums)
        {
            int absv = abs(each)-1;
            if (nums[absv] > 0)
                nums[absv] = - nums[absv];
            else
                res.push_back(absv+1);
        }
        return res;
    }



};
```

## 753. Asteroid Collision

We are given an array`asteroids`of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction \(positive meaning right, negative meaning left\). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

**Example 1:**

```
Input:

asteroids = [5, 10, -5]

Output:
 [5, 10]

Explanation:

The 10 and -5 collide resulting in 10.  The 5 and 10 never collide.
```

**Example 2:**

```
Input:

asteroids = [8, -8]

Output:
 []

Explanation:

The 8 and -8 collide exploding each other.
```

**Example 3:**

```
Input:

asteroids = [10, 2, -5]

Output:
 [10]

Explanation:

The 2 and -5 collide resulting in -5.  The 10 and -5 collide resulting in 10.
```

**Example 4:**

```
Input:

asteroids = [-2, -1, 1, 2]

Output:
 [-2, -1, 1, 2]

Explanation:

The -2 and -1 are moving left, while the 1 and 2 are moving right.
Asteroids moving the same direction never meet, so no asteroids will meet each other.
```

**Note:**

The length of

`asteroids`

will be at most

`10000`

.

Each asteroid will be a non-zero integer in the range

`[-1000, 1000].`

.

东风压倒西风的题目，就是stack， 要点就是记住pop后的四个状态

空

同向

相等

把反向的炸掉

### 代码：

```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        if (asteroids.empty())
            return asteroids;
        stack<int> stk;
        for (auto each : asteroids)
        {
            if (stk.empty())
                stk.push(each);
            else if (stk.top() < 0  || stk.top() > 0 && each > 0 )
                stk.push(each);
            else if (stk.top() + each == 0 )// top > 0, each < 0
                stk.pop();
            else if (stk.top() + each > 0)
                continue;
            else // stk.top() + each < 0// top will be killed
            {
                while(!stk.empty() && stk.top() >0 && stk.top() + each < 0)
                    stk.pop();
                if (stk.empty() || stk.top() < 0)
                    stk.push(each);
                else if (stk.top() + each == 0)
                    stk.pop();
                else if (stk.top() + each > 0)
                    continue;
            }
        }
        vector<int> result;
        while(!stk.empty())
        {
            result.push_back(stk.top());
            stk.pop();
        }   
        reverse(result.begin(), result.end());
        return result; 
    }
};
```

## 238. Product of Array Except Self

Given an array`nums`of_n_integers where_n_&gt; 1,  return an array`output`such that`output[i]`is equal to the product of all the elements of`nums`except`nums[i]`.

**Example:**

```
Input:
[1,2,3,4]
Output:
[24,12,8,6]
```

**Note:**Please solve it**without division**and in O\(_n_\).

**Follow up:**  
Could you solve it with constant space complexity? \(The output array**does not**count as extra space for the purpose of space complexity analysis.\)

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        
        vector<int> res;
        if (nums.empty())
            return res;
        int n = nums.size();
        if (n == 1)
            return {1};

        vector<int> left(n, 1), right(n,1);
        for (int i = 1; i<n; i++)
            left[i] = left[i-1]*nums[i-1];

        right[n-1] = 1;
        for (int i = n-2; i>=0; i--)
            right[i] = right[i+1]* nums[i+1];
        for (int i = 0; i < n; i++)
            res.push_back(left[i]*right[i]);
        return res;
    }
};

class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        
        vector<int> res;
        if (nums.empty())
            return res;
        int n = nums.size();
        res = vector<int>(n, 1);
        for (int i = n-2; i>=0; i--)
            res[i] = res[i+1]* nums[i+1];
        int num = 1;
        for (int i = 0; i < n; i++)
        {
            res[i] = num * res[i];
            num *= nums[i];
        }
        return res;
    }
};

```

result

