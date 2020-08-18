## 17. Subsets

Given a set of distinct integers, return all possible subsets.

##### Notice

* Elements in a subset must be in
  _non-descending_
  order.
* The solution set must not contain duplicate subsets.

Have you met this question in a real interview?

Yes

**Example**

If S =`[1,2,3]`, a solution is:

```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

[https://www.lintcode.com/en/old/problem/subsets/\#](https://www.lintcode.com/en/old/problem/subsets/#)

[https://leetcode.com/problems/subsets/](https://leetcode.com/problems/subsets/)

### 解题分析:

全子集问题，组合问题，permutation问题，基本的套路就是一个，回溯，放上去，往下搜索，再拿下来

大概有这么几个套路：

排序

放一个空子集进去

回溯法： push上去，向下搜索，pop

去重： 在two sum两根指针里用过了, i!= beg && nums\[i\] == nums\[i-1\] continue

![](/assets/backtrack.png)

### 代码：

```
class Solution {
public:
    /**
     * @param nums: A set of numbers
     * @return: A list of lists
     */
    vector<vector<int>> subsets(vector<int> &nums) {
        // write your code here
        vector<vector<int>> res; 
        if (nums.empty())
        {
            res.push_back(vector<int>());
            return res;
        }    

        sort(nums.begin(), nums.end());
        vector<int> solution;
        helper(nums, 0 , solution, res);
        return res;
    }

    void helper(vector<int>& nums, int ind, vector<int>& solution, vector<vector<int>>& res)
    {
        res.push_back(solution);
        for (int i = ind; i < nums.size(); i++)
        {
            if (i != ind && nums[i] == nums[i-1])
                continue;
            solution.push_back(nums[i]);
            helper(nums, i+1, solution, res);
            solution.pop_back();
        }
    }
};
```

### 复杂度分析:

o\(2^n\)

2020/08/18 golang如果要改变array， 就要传指针

```go
func subsets(nums []int) [][]int {
    res := [][]int{[]int{}}
    solution := []int{}
    helper(0, nums, solution, &res)
    return res
}

func helper(cur int, nums, solution []int, res *[][]int) {    
    if cur == len(nums){
        return
    }
    for i := cur; i < len(nums); i++ {
        solution = append(solution, nums[i])
        tmp := make([]int, len(solution))
        copy(tmp, solution)
        *res = append(*res, tmp)
        helper(i+1, nums, solution, res)
        solution = solution[:len(solution) - 1]
    }
}
```

## 680. Split String

Give a string, you can choose to split the string after one character or two adjacent characters, and make the string to be composed of only one character or two characters. Output all possible results.

Have you met this question in a real interview?

Yes

**Example**

Given the string`"123"`  
return`[["1","2","3"],["12","3"],["1","23"]]`

[https://www.lintcode.com/en/old/problem/split-string/](https://www.lintcode.com/en/old/problem/split-string/)

### 解题分析:

求所有方案问题， 套路又不算套路。 写一个没那么套路的， 再写一个套路一点的。

### 代码：

```
class Solution {
public:
    /*
     * @param : a string to be split
     * @return: all possible split string array
     */
    vector<vector<string>> splitString(string& s) {
        // write your code here
        vector<vector<string>> res;
        if (s.empty()) {{}};
        vector<string> solution;
        helper(res, solution, s, 0);
        return res;
    }

    void helper(vector<vector<string>>& res, vector<string>& solution, string& s, int cur) {
        if (cur >= s.size()) {
            res.push_back(solution);
            return;
        }
        solution.push_back(s.substr(cur,  1));
        helper(res, solution, s, cur + 1);
        solution.pop_back();
        if (cur == s.size() - 1) return;
        solution.push_back(s.substr(cur,  2));
        helper(res, solution, s, cur + 2);
        solution.pop_back();
    }
};
```

### 复杂度分析:

o\(2^n\)

## 570. Find the Missing Number II

Giving a string with number from 1-`n`in random order, but miss`1`number.Find that number.

##### Notice

n &lt;= 30

Have you met this question in a real interview?

Yes

**Example**

Given n =`20`, str =`19201234567891011121314151618`

return`17`

[https://www.lintcode.com/en/old/problem/find-the-missing-number-ii/](https://www.lintcode.com/en/old/problem/find-the-missing-number-ii/)

### 解题分析:

题目给的是也是一个string, 那么涉及到对string如何理解的问题比如 12 可以理解为一个数字 12，或者1， 2

那么枚举以下情况

1 如果后面为0 那么必须为10

1如果后面为1-9 怎可以分开理解

后面如何剪枝？ visited, 为什么要剪枝？  11， 已经理解为1了，则不需要第二个1， 应该第二个为1x

那么最后应该是有一个解使得这个字符串一直到最后一位可以被理解，那么这个也应该是递归的出口。

递归过程中，应该有其他的出口: 搜索失败

### 代码:

### 复杂度分析:

o\(2^n\)

2020/08/18重新写这一版

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @param str: a string with number from 1-n in random order and miss one number
     * @return: An integer
     */
    int findMissing2(int n, string &str) {
        // write your code here
        if (str.empty()) return n;
        vector<bool> collect(n+1, false);
        return helper(n, str, collect, 0);
    }

    int helper(int n, string& str, vector<bool>& collect, int cur) {
        if (cur >= str.size()){
            int count = 0, num = 0;
            for (int i = 1; i <= n; i++) {
                if (!collect[i]) {
                    count++;
                    num = i;
                }
            }
            if (count == 1) return num;
            return 0;
        }
        if (str[cur] == '0') return 0;
        for (int len = 1; len < 3; len++) {
            if (len == 2 && ( n < 10 || cur == str.size() -1 )) continue;
            int num = stoi(str.substr(cur, len));
            if (num <= n && !collect[num] ) {
                collect[num] = true;
                auto r = helper(n, str, collect, cur + len);
                if (r) return r;
                collect[num] = false;
            }            
        }
        return 0;
    }
};
```

## 153. Combination Sum II

Given a collection of candidate numbers \(_C_\) and a target number \(_T_\), find all unique combinations in_C\_where the candidate numbers sums to\_T_.

Each number in\_C\_may only be used once in the combination.

##### Notice

* All numbers \(including target\) will be positive integers.
* Elements in a combination \(a1, a2, … , ak\) must be in non-descending order. \(ie, a1 ≤ a2 ≤ … ≤ ak\).
* The solution set must not contain duplicate combinations.

Have you met this question in a real interview?

Yes

**Example**

Given candidate set`[10,1,6,7,2,1,5]`and target`8`,

A solution set is:

```
[
  [1,7],
  [1,2,5],
  [2,6],
  [1,1,6]
]
```

[https://www.lintcode.com/en/old/problem/combination-sum-ii/\#](https://www.lintcode.com/en/old/problem/combination-sum-ii/#)

[https://leetcode.com/problems/combination-sum-ii/](https://leetcode.com/problems/combination-sum-ii/)

### 解题分析:

其实可以理解成subsets里面找和为target的，就可以直接无脑上套路了

记得剪枝

if \(total + num\[i\] &gt; target\) break;

### 代码：

```
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<vector<int>> res;
        vector<int> combo;
        helper(candidates, res, combo, target, 0);
        return res;
    }

    void helper(const vector<int>& candidates,vector<vector<int>>& res, vector<int>& comb, int target, int cur) {
        if (target == 0) {
            res.push_back(comb);
            return;
        }
        if (target < 0 || cur >= candidates.size()) return;
        for (int i = cur; i < candidates.size(); i++) {
            if (i != cur && candidates[i] == candidates[i-1]) continue;
            comb.push_back(candidates[i]);
            helper(candidates, res, comb, target - candidates[i], i + 1);
            comb.pop_back();
        }
    }
};
```

### 复杂度分析:

o\(2^n\)

## 135. Combination Sum

Given a set of candidate numbers \(_**C**_\) and a target number \(_**T**_\), find all unique combinations in_**C**\_where the candidate numbers sums to_**T**\_.

**Example**

Given candidate set`[2,3,6,7]`and target`7`, a solution set is:

```
[7]
[2, 2, 3]
```

[https://www.lintcode.com/en/old/problem/combination-sum/\#](https://www.lintcode.com/en/old/problem/combination-sum/#)

[https://leetcode.com/problems/combination-sum/](https://leetcode.com/problems/combination-sum/)

### 解题分析:

方案取还是不取，无脑上DFS

### 代码：

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> comb;
        sort(candidates.begin(), candidates.end());
        helper(res, comb, candidates, target, 0);
        return res;
    }
    
    void helper(vector<vector<int>>& res, vector<int>& comb, vector<int>& candidates, int target, int cur) {
        if (target == 0) {
            res.push_back(comb);
            return;
        }
        if (target < 0 || cur >= candidates.size()) return;
        
        for (int i = cur; i < candidates.size(); i++) {
            comb.push_back(candidates[i]);
            helper(res, comb, candidates, target - candidates[i], i);
            comb.pop_back();
        }
    }
};
```

### 复杂度分析:

o\(2^n\)

## 152. Combinations

Given two integers_n\_and\_k_, return all possible combinations of_k\_numbers out of 1 ...\_n_.

##### Notice

You don't need to care the order of combinations, but you should make sure the numbers in a combination are sorted.

Have you met this question in a real interview?

Yes

**Example**

Given`n = 4`and`k = 2`, a solution is:

```
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4]
]
```

[https://www.lintcode.com/en/old/problem/combinations/\#](https://www.lintcode.com/en/old/problem/combinations/#)

### 解题分析:

记得剪枝

### 代码：

```
class Solution {
public:
    /**
     * @param n: Given the range of numbers
     * @param k: Given the numbers of combinations
     * @return: All the combinations of k numbers out of 1..n
     */
    vector<vector<int>> combine(int n, int k) {
        // write your code here
        vector<vector<int>> result;
        if ( n == 0 || k == 0)
            return result;
        vector<int> subset;
        dfs(n, k, 1, subset, result);
        return result;
    }

    void dfs(int n, int k, int startIndex, vector<int>& subset, vector<vector<int>>& result)
    {

        if (subset.size() == k)
        {
            result.push_back(subset);
            return;
        }
        if (startIndex > n)
            return;
        if (n - startIndex + 1 < k - subset.size())
            return;
        for (int i = startIndex; i <=n; i++)
        {
            subset.push_back(i);
            dfs(n, k, i+1, subset, result);
            subset.pop_back();
        }
    }

};
```

### 复杂度分析:

o\(2^n\)

## 18. Subsets II

Given a list of numbers that may has duplicate numbers, return all possible subsets

##### Notice

* Each element in a subset must be in
  _non-descending_
  order.
* The ordering between two subsets is free.
* The solution set must not contain duplicate subsets.

Have you met this question in a real interview?

Yes

**Example**

If S =`[1,2,2]`, a solution is:

```
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

[https://www.lintcode.com/en/old/problem/subsets-ii/\#](https://www.lintcode.com/en/old/problem/subsets-ii/#)

### 解题分析:

DFS去重

### 代码：

```
class Solution {
public:
    /**
     * @param nums: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    vector<vector<int>> subsetsWithDup(vector<int> &nums) {
        // write your code here
        vector<vector<int>> results;
        vector<int> subset;
        if (nums.empty())
        {
            results.push_back(subset);
            return results;
        }
        sort(nums.begin(), nums.end());
        dfs(nums, subset, 0, results);
        return results;
    }

    void dfs(vector<int>& nums, vector<int>& subset, int startIndex, vector<vector<int>>& results)
    {
        results.push_back(subset);
        for (int i = startIndex; i < nums.size(); i++)
        {
            if (nums[i] == nums[i-1] && i != startIndex)
                continue;
            subset.push_back(nums[i]);
            dfs(nums, subset, i+1, results);
            subset.pop_back();
        }
    }
};
```

### 复杂度分析:

o\(2^n\)

## 780. Remove Invalid Parentheses

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

##### Notice

The input string may contain letters other than the parentheses`(`and`)`.

Have you met this question in a real interview?

Yes

**Example**

```
"()())()" -
>
 ["()()()", "(())()"]
"(a)())()" -
>
 ["(a)()()", "(a())()"]
")(" -
>
 [""]
```

[https://www.lintcode.com/en/old/problem/remove-invalid-parentheses/\#](https://www.lintcode.com/en/old/problem/remove-invalid-parentheses/#)

### 解题分析:

这个完全是凭经验做第一次分析了。。。第一道坎就是判断是不是valid，经典的做法是放一个stack,放狗一搜满足以下条件即可

1. 在到达最后一个之前 num of \) &lt;= num of \(
2. 在到达最后一个之后 num of \( = num of \)

所以要做就是从左到右扫，移除左右括号，放到里面扫

BFS把最小edit转化为最大长度问题，因为是层级遍历，所以维护一个最大长度就好了。 或者干脆放个层级遍历也行。 都能干着活。

### 代码：

```
class Solution {
public:
    /**
     * @param s: The input string
     * @return: Return all possible results
     */
    vector<string> removeInvalidParentheses(string &s) {
        // Write your code here

        unordered_set<string> visited;
        queue<string> q;
        q.push(s);
        visited.insert(s);
        vector<string> candidates;
        int maxLen = 0;
        while(!q.empty())
        {
            auto str = q.front();
            q.pop();

            if (str.size() < maxLen)
                continue;

            if (isValid(str))
            {
                maxLen = max(maxLen, (int)str.size());
                candidates.push_back(str);
                continue;
            }
            for (int i = 0; i < str.size(); i++)
            {
                if (s[i] != '(' && s[i] != ')')
                {
                    continue;
                }
                auto newStr = str.substr(0, i)+str.substr(i+1);
                if (visited.find(newStr) != visited.end())
                    continue;
                visited.insert(newStr);
                q.push(newStr);
            }
        }

        return candidates;
    }



    // 0... n-1 
    // l = num of ( r = num of )
    // i !=n-1 l>=r and i==n-1 && l==r ==>valid
    bool isValid(const string& s)
    {
        int l = 0, r = 0;

        for (int i = 0; i< s.size(); i++)
        {
            if (s[i] == '(') 
                l++;
            else if (s[i] == ')')
                r++;
            if (i != s.size()-1 && r > l)
                return false;
            else if (i == s.size()-1 && r!=l)
                return false;
        }
        return true;
    }


};
```

### 复杂度分析:

o\(n CnK\) k为最后的长度

### NOTE:

看了别人DFS的解法，先数出左右括号数，然后找出要移除的左右数目，之后先移除右括号，然后再移除左括号最后的结果就是了

我觉得不如BFS好理解。

另外还有一个更快的 [http://www.cnblogs.com/grandyang/p/4944875.html](http://www.cnblogs.com/grandyang/p/4944875.html)

两边sweep， 也是不太好理解

## 582. Word Break II

Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

Have you met this question in a real interview?

Yes

**Example**

Gieve s =`lintcode`,  
dict =`["de", "ding", "co", "code", "lint"]`.

A solution is`["lint code", "lint co de"]`.

[https://www.lintcode.com/en/old/problem/word-break-ii/\#](https://www.lintcode.com/en/old/problem/word-break-ii/#)

### 解题分析:

套路题，BFS也可以做，但是BFS做剪枝有个套路， 就是从i开始往后如不可以break,都没有必要搜索，所以可以加个memorized search这样速度稍微快一些。字符串向下搜索常用技巧。。。。。

```
if (canBreak.find(startIndex) != canBreak.end() && !canBreak[startIndex])
    return;
for (int i = startIndex; i < s.size(); i++)
{
  ....
  int n = results.size();
  helper(s, wordDict, subset, results, i+1, canBreak);
  canBreak[i+1] = results.size() != n;
  ...
}
```

### 代码：

```
class Solution {
public:
    /*
     * @param s: A string
     * @param wordDict: A set of words.
     * @return: All possible sentences.
     */
    vector<string> wordBreak(string &s, unordered_set<string> &wordDict) {
        // write your code here
        vector<string> results;
        if (s.empty() || wordDict.empty())
            return results;
        vector<string> subset;
        unordered_map<int, bool> canBreak;

        helper(s, wordDict, subset, results, 0, canBreak );
    }

    void helper(const string& s, const unordered_set<string>& wordDict, vector<string>& subset, vector<string>& results, int startIndex, unordered_map<int, bool>& canBreak)
    {
        if (startIndex >= s.size())
        {
            pushResult(results, subset);
            return;
        }
        if (canBreak.find(startIndex) != canBreak.end() && !canBreak[startIndex])
            return;
        for (int i = startIndex; i < s.size(); i++)
        {

            auto str = s.substr(startIndex, i-startIndex+1);
            if (wordDict.find(str) == wordDict.end())
                continue;
            subset.push_back(str);
            int n = results.size();
            helper(s, wordDict, subset, results, i+1, canBreak);
            canBreak[i+1] = results.size() != n;
            subset.pop_back();
        }

    }

    void pushResult(vector<string>& results, const vector<string>& subset)
    {
        string res;

        for(const auto& each : subset)
            res += each +" ";
        res.pop_back();
        results.push_back(res);
    }
};
```

### 复杂度分析:

o\(n CnK\) k为最后的长度

