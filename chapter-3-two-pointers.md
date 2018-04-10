# 相向双指针



## 数组去重问题

## 521 Remove Duplicate Numbers in Array

Given an array of integers, remove the duplicate numbers in it.

You should:  
1. Do it in place in the array.  
2. Move the unique numbers to the front of the array.  
3. Return the total number of the unique numbers.

**Example**

Given_nums_=`[1,3,1,4,4,2]`, you should:

1. Move duplicate integers to the tail of
   _nums_=`[1,3,4,2,?,?]`
2. Return the number of unique integers in
   _nums_&gt;`4`

Actually we don't care about what you place in`?`, we only care about the part which has no duplicate integers.

[http://www.lintcode.com/en/problem/remove-duplicate-numbers-in-array/](http://www.lintcode.com/en/problem/remove-duplicate-numbers-in-array/)

### 解题分析:

空间换时间的话就是弄一个hash set往里面丢， o\(n\)算是最优了， 要么就sort一下快慢指针来做

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: an array of integers
     * @return: the number of unique integers
     */
    int deduplication(vector<int> &nums) {
        // write your code here
        if (nums.empty())
            return 0;
        sort(nums.begin(), nums.end());
        int beg = 0,end = 0;
        while( end < nums.size())
        {
            if (nums[end] == nums[beg])
                end++;
            else
                nums[++beg] = nums[end++];
        }

        return beg+1;
    }
};
```

### 复杂度分析:

nlog\(n\)



## 滑动窗口问题

## 604 Window Sum

Given an array of n integer, and a moving window\(size k\), move the window at each iteration from the start of the array, find the

`sum`

of the element inside the window at each moving.

**Example**

For array`[1,2,7,8,5]`, moving window size k =`3`.  
1 + 2 + 7 = 10  
2 + 7 + 8 = 17  
7 + 8 + 5 = 20  
return`[10,17,20]`



http://www.lintcode.com/en/problem/window-sum/



### 解题分析:

直接写好了, sliding window average....数字信号处理的一个基本算法

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: a list of integers.
     * @param k: length of window.
     * @return: the sum of the element inside the window at each moving.
     */
    vector<int> winSum(vector<int> &nums, int k) {
        // write your code here
        vector<int> result;
        if (nums.empty())
            return result;
        int sum = 0;
        int beg = 0;
        for(;beg < k && beg <nums.size(); beg++)
            sum+=nums[beg];
        result.push_back(sum);
        while(beg < nums.size())
        {
            sum = sum + nums[beg]  - nums[beg-k];
            result.push_back(sum);
            beg++;
        }
        return result;
    }
};
```

### 复杂度分析:

o\(n\)



