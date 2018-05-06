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
        res.push_back(vector<int>());
        if (nums.empty())
            return res;

        sort(nums.begin(), nums.end());
        vector<int> solution;
        helper(nums, 0 , solution, res);
        return res;
    }

    void helper(vector<int>& nums, int ind, vector<int>& solution, vector<vector<int>>& res)
    {
        for (int i = ind; i < nums.size(); i++)
        {
            if (i != ind && nums[i] == nums[i-1])
                continue;
            solution.push_back(nums[i]);
            res.push_back(solution);
            helper(nums, i+1, solution, res);
            solution.pop_back();
        }
    }
};
```

### 复杂度分析:

o\(2^n\)

