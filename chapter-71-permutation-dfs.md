## 15. Permutations

Given a list of numbers, return all possible permutations.

##### Notice

You can assume that there is no duplicate numbers in the list.

Have you met this question in a real interview?

Yes

**Example**

For nums =`[1,2,3]`, the permutations are:

```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

https://www.lintcode.com/en/old/problem/permutations/\#

### 解题分析:

模板套路类题目

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    vector<vector<int>> permute(vector<int> &nums) {
        // write your code here
        vector<vector<int>> results;
        vector<int> subset;
        vector<bool> visited(nums.size(), false);
        dfs(nums, subset, visited, results);
        return results;
    }
    
    void  dfs(const vector<int>& nums, vector<int>& subset, vector<bool>& visited, vector<vector<int>>& results)
    {
        if (subset.size() == nums.size())
        {
            results.push_back(subset);
            return;
        }
        for (int i = 0; i < nums.size(); i++)
        {
            if (visited[i])
                continue;
            visited[i] = true;
            subset.push_back(nums[i]);
            dfs(nums, subset, visited, results);
            subset.pop_back();
            visited[i] = false;
        }
    }
};
```

### 

## 16. Permutations II

Given a list of numbers with duplicate number in it. Find all**unique**permutations.

Have you met this question in a real interview?

Yes

**Example**

For numbers`[1,2,2]`the unique permutations are:

```
[
  [1,2,2],
  [2,1,2],
  [2,2,1]
]
```

https://www.lintcode.com/en/old/problem/permutations-ii/\#

### 解题分析:

模板套路类题目, 这里有一个去重的问题 

叙述： 当重复出现的时候nums\[i\] == nums\[i-1\]， 且前面已经被visited，才可以继续

### 代码：

```cpp
class Solution {
public:
    /*
     * @param :  A list of integers
     * @return: A list of unique permutations
     */
    vector<vector<int>> permuteUnique(vector<int> &nums) {
        // write your code here
        vector<bool> visited(nums.size(), false);
        vector<int> subset;
        vector<vector<int>> results;
        sort(nums.begin(), nums.end());
        dfs(nums, subset, visited, results);
        return results;
    }
    
    void dfs(const vector<int>& nums, vector<int>& subset, vector<bool>& visited, vector<vector<int>>& results)
    {
        if (subset.size() == nums.size())
        {
            results.push_back(subset);
            return;
        }
        
        for (int i = 0; i < nums.size(); i++)
        {
            if (visited[i])
                continue;
            if (i != 0 && nums[i] == nums[i-1] && !visited[i-1])
                continue;
            visited[i] = true;
            subset.push_back(nums[i]);
            dfs(nums, subset, visited, results);
            subset.pop_back();
            visited[i] = false;
        }
    }
};
```

### 



