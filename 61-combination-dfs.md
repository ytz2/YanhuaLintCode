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



## 680. Split String

Give a string, you can choose to split the string after one character or two adjacent characters, and make the string to be composed of only one character or two characters. Output all possible results.

Have you met this question in a real interview?

Yes

**Example**

Given the string`"123"`  
return`[["1","2","3"],["12","3"],["1","23"]]`

https://www.lintcode.com/en/old/problem/split-string/

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
        if (s.empty())
        {
            res.push_back(vector<string>());
            return res;
        }
        vector<string> subset;
        dfs(s, 0, subset, res);
        return res;
    }
    
    void dfs(const string& s, int startIndex, vector<string>& subset, vector<vector<string>>& result)
    {
        // exit
        if (startIndex >= s.size())
        {
            result.push_back(subset);
            return;
        }
        subset.push_back(s.substr(startIndex, 1));
        dfs(s, startIndex+1, subset, result);
        subset.pop_back();
        if (startIndex == s.size() - 1)
            return;
        subset.push_back(s.substr(startIndex, 2));
        dfs(s, startIndex+2, subset, result);
        subset.pop_back();
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

https://www.lintcode.com/en/old/problem/find-the-missing-number-ii/

### 解题分析:

题目给的是也是一个string, 那么涉及到对string如何理解的问题比如 12 可以理解为一个数字 12，或者1， 2

那么枚举以下情况

1 如果后面为0 那么必须为10

1如果后面为1-9 怎可以分开理解

后面如何剪枝？ visited, 为什么要剪枝？  11， 已经理解为1了，则不需要第二个1， 应该第二个为1x



那么最后应该是有一个解使得这个字符串一直到最后一位可以被理解，那么这个也应该是递归的出口。 

递归过程中，应该有其他的出口: 搜索失败



### 代码：

```
class Solution {
public:
    /**
     * @param n: An integer
     * @param str: a string with number from 1-n in random order and miss one number
     * @return: An integer
     */
    int findMissing2(int n, string &str) {
        // write your code here
        if (n == 0 || str.empty())
            return -1;
        unordered_set<int> visited;
        int result = -1;
        dfs(n, str, 0, visited, result);
        return result;
    }
    
    void dfs(int n, const string& str, int startIndex, unordered_set<int>& visited, int& result)
    {
        if (startIndex >= str.size() )
        {
            if (visited.size() == n-1)
                result = getMissed(visited, n);
            return;
        }
        int oneNum = stoi(str.substr(startIndex, 1));
        if (oneNum  == 0 || oneNum > n)
            return;
        
        if (visited.find(oneNum) == visited.end())
        {
            visited.insert(oneNum);
            dfs(n, str, startIndex+1, visited, result);
            visited.erase(oneNum);
        }
        
        if (startIndex == str.size()-1)
            return;
        int twoNum = stoi(str.substr(startIndex,2));
        if (twoNum < 10 || visited.find(twoNum) != visited.end() || twoNum > n)
            return;
        visited.insert(twoNum);
        dfs(n, str, startIndex+2, visited, result);
        visited.erase(twoNum);
    }
    
    int getMissed(unordered_set<int>& dict, int n)
    {
        // look for the missted one
        for (int i = 1; i<= n; i++)
        {
            if (dict.find(i) == dict.end())
            {
                return i; 
            }
        }
        return -1;
    }
};
```

### 复杂度分析:

o\(2^n\)



