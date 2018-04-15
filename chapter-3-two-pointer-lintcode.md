## 607 Two Sum - Data structure design

Design and implement a TwoSum class. It should support the following operations:`add`and`find`.

`add`- Add the number to an internal data structure.  
`find`- Find if there exists any pair of numbers which sum is equal to the value.

Have you met this question in a real interview?

Yes

**Example**

```
add(1); add(3); add(5);
find(4) // return true
find(7) // return false
```

[http://www.lintcode.com/en/problem/two-sum-data-structure-design/](http://www.lintcode.com/en/problem/two-sum-data-structure-design/)

### 解题分析:

挺好玩的一个题目， two sum算是经典题了，但这道题是让选数据结构。 什么样的数据结构插入简单搜索也简单？

如果从two sum那道题过来，要求storage是排好的， 且允许重复出现。（multiset\) . 而且搜索的最优定为 o\(n\) 两根指针

### 代码：

```cpp
class TwoSum {
public:
    /*
     * @param number: An integer
     * @return: nothing
     */
    void add(int number) {
        // write your code here
        store.insert(number);
    }

    /*
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    bool find(int value) {
        // write your code here
        auto beg = store.begin();
        auto end = --store.end();
        while(beg != end)
        {
            int sum = *beg+*end;
            if (sum == value)
                return true;
            else if (sum > value)
                --end;
            else
                ++beg;
        }
        return false;
    }

    multiset<int> store;
};
```

### 复杂度分析:

add: o\(log\(n\)\) find o\(n\)

或者不指望排序，但是用unordered\_map&lt;int,int&gt; key是数字，value是count，这样就可以存多个数字了，insert是O（1）, find O\(n\)

```
class TwoSum {
public:
    /*
     * @param number: An integer
     * @return: nothing
     */
    void add(int number) {
        // write your code here
        store[number]++;
    }

    /*
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    bool find(int value) {
        // write your code here
        for (auto each : store)
        {
            auto expected = value - each.first;
            if (expected == each.first)
            {
                if (each.second > 1)
                    return true;
                continue;
            }

            if (store.find(expected) !=store.end())
                return true;
        }
        return false;
    }

    unordered_map<int, int> store;
};
```

## 539 Move Zeroes

Given an array`nums`, write a function to move all`0`'s to the end of it while maintaining the relative order of the non-zero elements.

##### Notice

1. You must do this
   **in-place**
   without making a copy of the array.
2. Minimize the total number of operations.

Have you met this question in a real interview?

Yes

**Example**

Given`nums = [0, 1, 0, 3, 12]`, after calling your function,`nums`should be`[1, 3, 12, 0, 0]`.

[http://www.lintcode.com/en/problem/move-zeroes/\#](http://www.lintcode.com/en/problem/move-zeroes/#)

### 解题分析:

快慢指针

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an integer array
     * @return: nothing
     */
    void moveZeroes(vector<int> &nums) {
        // write your code here
        if (nums.empty())
            return;
        int slow = 0, fast =0;
        while(fast < nums.size())
        {
            if (nums[fast] ==0 )
            {
                fast++;
                continue;
            }

            while(slow<fast && nums[slow]!=0)
                slow++;
            swap(nums[slow],nums[fast]);
            fast++;
        }
    }
};
```

### 复杂度分析:

o\(n\)

#### 608. Two Sum - Input array is sorted

Given an array of integers that is already_sorted in ascending order_, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers \(both index1 and index2\) are not zero-based.

##### Notice

You may assume that each input would have exactly one solution.

Have you met this question in a real interview?

Yes

**Example**

Given nums =`[2, 7, 11, 15]`, target =`9`  
return`[1, 2]`

[http://www.lintcode.com/en/problem/two-sum-input-array-is-sorted/](http://www.lintcode.com/en/problem/two-sum-input-array-is-sorted/)

### 解题分析:

相向指针

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: an array of Integer
     * @param target: target = nums[index1] + nums[index2]
     * @return: [index1 + 1, index2 + 1] (index1 < index2)
     */
    vector<int> twoSum(vector<int> &nums, int target) {
        // write your code here
        vector<int> res(2,-1);
        if (nums.empty())
            return res;
        int beg =0, end = nums.size()-1;
        while(beg != end)
        {
            int sum = nums[beg] + nums[end];
            if (sum == target)
            {
                res[0] = beg+1;
                res[1] = end+1;
                return res;
            }
            else if (sum < target)
                beg++;
            else
                end--;
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n\)

#### 57. 3Sum

Given an array_S\_of n integers, are there elements\_a_,_b_,\_c\_in\_S\_such that`a + b + c = 0`? Find all unique triplets in the array which gives the sum of zero.

##### Notice

Elements in a triplet \(a,b,c\) must be in non-descending order. \(ie, a ≤ b ≤ c\)

The solution set must not contain duplicate triplets.

Have you met this question in a real interview?

Yes

**Example**

For example, given array S =`{-1 0 1 2 -1 -4}`, A solution set is:

```
(-1, 0, 1)
(-1, -1, 2)
```

[http://www.lintcode.com/en/problem/3sum/\#](http://www.lintcode.com/en/problem/3sum/#)

### 解题分析:

去重的金句，只允许第一次出现被搜索

```cpp
                if (beg > i+1 && numbers[beg-1] == numbers[beg])
                {
                    beg++; continue;
                }
```

### 代码：

```cpp
class Solution {
public:
    /**
     * @param numbers: Give an array numbers of n integer
     * @return: Find all unique triplets in the array which gives the sum of zero.
     */
    vector<vector<int>> threeSum(vector<int> &numbers) {
        // write your code here
        vector<vector<int>> res;
        if (numbers.size() < 3)
            return res;
        sort(numbers.begin(), numbers.end());
        int i = 0;
        while( i < numbers.size() - 2)
        {
            if (i>0 && numbers[i] == numbers[i-1])
            {
                i++;
                continue;
            }
            int beg = i+1, end =numbers.size()-1;
            int target = - numbers[i];
            while(beg < end)
            {
                if (beg > i+1 && numbers[beg-1] == numbers[beg])
                {
                    beg++; continue;
                }
                int sum = numbers[beg] + numbers[end];
                if (sum == target)
                {
                    res.push_back(vector<int>{numbers[i], numbers[beg], numbers[end]});
                    beg++; end--;
                }
                else if (sum < target)
                    beg++;
                else
                    end--;
            }
            while(i+1 <numbers.size()-2 && numbers[i] == numbers[i+1])
                i++;
            i++;
        }
        return res;
    }
};
```

### 复杂度分析:

o\(n^2\)

