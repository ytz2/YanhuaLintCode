## 943. Range Sum Query - Immutable

Given an integer array nums, find the sum of the elements between indices`i`and`j(i ≤ j)`, inclusive.

### Example

```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -
>
 1
sumRange(2, 5) -
>
 -1
sumRange(0, 5) -
>
 -3
```

https://www.lintcode.com/problem/range-sum-query-immutable/description

### 解题分析:

感觉没啥技巧，硬上就好了

### 代码：

```cpp
class NumArray {
public:

    NumArray(vector<int> nums) {
        prefixsum.push_back(0);
        for (int i = 0; i < nums.size(); i++)
            prefixsum.push_back(nums[i]+prefixsum[i]);

    }
    
    int sumRange(int i, int j) {
        if (i < 0) i = 0;
        if (j+1 > prefixsum.size()-1) j = prefixsum.size()-2;
        return prefixsum[j+1] - prefixsum[i];
    }
    vector<int> prefixsum;    
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```

## 548. Intersection of Two Arrays II

Given two arrays, write a function to compute their intersection.

### Example

Given_nums1_=`[1, 2, 2, 1]`,_nums2_=`[2, 2]`, return`[2, 2]`.

### Challenge

* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to num2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

https://www.lintcode.com/problem/intersection-of-two-arrays-ii/description

### 解题分析:

感觉没啥技巧，硬上就好了

### 代码：

```cpp
class Solution {
public:
    
    /*
     * @param nums1: an integer array
     * @param nums2: an integer array
     * @return: an integer array
     */
    vector<int> intersection(vector<int> nums1, vector<int> nums2) {
        // write your code here
        vector<int> res;
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        auto it1 = nums1.begin();
        auto it2 = nums2.begin();
        while(it1!= nums1.end() && it2!=nums2.end())
        {
            if (*it1 < *it2)
                it1++;
            else if (*it1 > *it2)
                it2++;
            else
            {
                res.push_back(*it1);
                it1++;it2++;
            }
        }
        return res;
    }
};

class Solution {
public:
    
    /*
     * @param nums1: an integer array
     * @param nums2: an integer array
     * @return: an integer array
     */
    vector<int> intersection(vector<int> nums1, vector<int> nums2) {
        // write your code here
        vector<int> res;
        if (nums1.size() < nums2.size())
            swap(nums1, nums2);
        unordered_map<int, int> counter;
        for (auto each : nums1)
            counter[each]++;
        for (auto each : nums2)
        {
            if (counter.count(each) )
            {
                res.push_back(each);
                if (--counter[each] == 0)
                    counter.erase(each);
            }
        }
        return res;
    }
};
```



