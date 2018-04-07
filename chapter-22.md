## 457 Classical Binary Search

Find any position of a target number in a sorted array. Return -1 if target does not exist.

Have you met this question in a real interview?

Yes

**Example**

Given`[1, 2, 2, 4, 5, 5]`.

For target =`2`, return 1 or 2.

For target =`5`, return 4 or 5.

For target =`6`, return -1.

[http://www.lintcode.com/en/problem/classical-binary-search/](http://www.lintcode.com/en/problem/classical-binary-search/)

### 解题分析:

不用分析了

### 代码：

```cpp
class Solution {
public:
    /*
     * @param nums: An integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */
    int findPosition(vector<int> &nums, int target) {
        // write your code here
        int n = nums.size();
        if (!n)
            return -1;
        int beg = 0, end = n - 1;

        while(beg+1 < end)
        {
            int mid = beg + (end - beg) / 2;
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                beg = mid;
            else 
                end = mid;
        }

        if (nums[beg] == target)
            return beg;
        if (nums[end] == target)
            return end;
        return -1;
    }
};
```

### 复杂度分析:

log\(n\)





