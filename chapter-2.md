# Binary Search and log\(n\) problems

## 458 Last Position of Target

Find the last position of a target number in a sorted array. Return -1 if target does not exist.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 2, 2, 4, 5, 5]`.

For target =`2`, return 2.

For target =`5`, return 5.

For target =`6`, return -1.

[http://www.lintcode.com/en/problem/longest-palindrome/](http://www.lintcode.com/en/problem/last-position-of-target/#)

### 解题分析:

关键词：sorted array, last position, -1

典型的二分查找加线性搜索， 直接写代码就行了



### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: An integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */
    int lastPosition(vector<int> &nums, int target) {
        // write your code here
        if (nums.empty())
            return -1;
        size_t beg = 0, end = nums.size() - 1, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg) / 2;
            if (nums[mid] >= target)
                end = mid;
            else
                beg = mid;
        }
        int ind = -1;
        if (nums[end] == target)
            ind = end;
        else if (nums[beg] == target)
            ind = beg;
        else 
            return -1;
        while(ind + 1 < nums.size() && nums[ind] == nums[ind+1])
            ind++;
        return ind;
    }
};
```

### 复杂度分析:

O\(log\(n\)\), n = nums.size\(\), 最坏的情况是o\(n\)



## 585 Maximum Number in Mountain Sequence

Given a mountain sequence of`n`integers which increase firstly and then decrease, find the mountain top.

Have you met this question in a real interview?

Yes

**Example**

Given`nums`=`[1, 2, 4, 8, 6, 3]`return`8`  
Given`nums`=`[10, 9, 8, 7]`, return`10`

http://www.lintcode.com/en/problem/maximum-number-in-mountain-sequence/\#

### 解题分析:

二分法的第二种应用OOOXXX， 找分界点问题。

任何一类问题，当左边是O， 右边是X的时候找最后一个O，或者第一个X的时候可以用二分法的思想来解决。 

找第一个X伪码如下：

for \( beg : end\)

   mid = beg + \(end - beg\) /2 

   if  isX\(mid\):

       beg = mid

   else

       end = mid

if \(isX\(end\)\)

      return end

return beg

### 代码：

```cpp
class Solution {
public:
    /**
     * @param nums: a mountain sequence which increase firstly and then decrease
     * @return: then mountain top
     */
    int mountainSequence(vector<int> &nums) {
        // write your code here
        int n = nums.size();
        if ( 0 == n)
            return INT_MIN;
        if ( 1 == n)
            return nums[0];
    
        int beg = 0, end = n-1, mid = 0;
        while( beg + 1 < end)
        {
            mid = beg + (end-beg)/2;
            if (nums[mid] > nums[mid+1]) // decreasing
                end = mid;
            else // increasing
                beg = mid;
        }
        if (nums[beg] > nums[beg+1])
            return nums[beg];
        return nums[end];
    }
};
```

### 复杂度分析:

O\(log\(n\)\)





