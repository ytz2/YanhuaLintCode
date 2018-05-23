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

[https://www.lintcode.com/en/old/problem/permutations/\#](https://www.lintcode.com/en/old/problem/permutations/#)

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

[https://www.lintcode.com/en/old/problem/permutations-ii/\#](https://www.lintcode.com/en/old/problem/permutations-ii/#)

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

## 52. Next Permutation

Given a list of integers, which denote a permutation.

Find the next permutation in ascending order.

##### Notice

The list may contains duplicate integers.

Have you met this question in a real interview?

Yes

**Example**

For`[1,3,2,3]`, the next permutation is`[1,3,3,2]`

For`[4,3,2,1]`, the next permutation is`[1,2,3,4]`time since it is smaller than the input time numerically.

https://www.lintcode.com/en/old/problem/next-permutation/

### 解题分析:

![](/assets/demo2.png)

![](/assets/demo.png)



这个题目要明白是属于字典顺序

1。 从后面往前找，找到a\[i\] &lt; a\[i+1\]的数字 

2.   从后面往前找找到第一个a\[j\] &gt; a\[i\]

3. swap\(a\[i\], a\[j\] 

4. reverse\(i+1, end\)



原理：

1。如果是递减，则是最后的字典顺序，无下一个

2。 那么往前找不是递减的， 因为是找字典顺序， 所以下一个当前点必然比现在的要大 （逆字典）

3。大了之后还不够，因为现在是升序

如果上来想不出方法，可以试着找找规律，我们关注的重点应是原数组末尾。

从末尾往左走，如果一直递增，例如`...9,7,5`，那么下一个排列一定会牵扯到左边更多的数，直到一个非递增数为止，例如`...6,9,7,5`。对于原数组的变化就只到`6`这里，和左侧其他数再无关系。`6`这个位置会变成`6`**右侧所有数中比`6`大的最小的数**，而`6`会进入最后3个数中，且后3个数必是**升序数组**。



**Q：i为0怎么办？**  
A：i为0说明整个数组是降序的，直接翻转整个数组即可。

**Q：有重复元素怎么办？**  
A：在遍历时只要严格满足`nums[i] > nums[i - 1]`和`nums[j] > nums[i - 1]`就不会有问题。

**Q：元素过少是否要单独考虑？**  
A：当元素个数小于等于1个时，可以直接返回。

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: A list of integers
     */
    vector<int> nextPermutation(vector<int> &nums) {
        // write your code here
        if (nums.size() <= 1)
            return nums;
        int i = nums.size() - 2;
        //step 1
        while(i >=0 && nums[i] >= nums[i+1])
            i--;
        // step 2
        if (i >= 0)
        {
            //step 3
            int j = nums.size()-1;
            while(j >= 0 && nums[j] <= nums[i])
                j--;
            swap(nums[i], nums[j]);
        }
        //step 4
        reverse(nums.begin()+i+1, nums.end());
        return nums;
    }
};
```

### 



