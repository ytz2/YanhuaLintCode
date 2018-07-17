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
 '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

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

https://leetcode.com/problems/regular-expression-matching/description/

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



