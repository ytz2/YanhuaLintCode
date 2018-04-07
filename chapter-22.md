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



## 141 Sqrt\(x\)

Implement int`sqrt(int x)`.

Compute and return the square root of_x_.



**Example**

sqrt\(3\) = 1

sqrt\(4\) = 2

sqrt\(5\) = 2

sqrt\(10\) = 3

http://www.lintcode.com/en/problem/sqrtx/

### 解题分析:



求解sqrt就是找一个OOXX的位置，左面的平方小于x,右面的平方大于x，中间要注意的是大数的溢出。再就是注意等于号

### 代码：

```cpp
class Solution {
public:
    /**
     * @param x: An integer
     * @return: The sqrt of x
     */
    int sqrt(int x) {
        // write your code here
        if (x<0)
            return INT_MAX;
        if (x==0)
            return 0;
        long beg = 1, end = x, mid = 0;
        while(beg + 1 < end)
        {
            mid = beg + (end - beg)/2;
            if (mid*mid <= x)
                beg = mid;
            else
                end = mid;
        }
        if (end*end <= x)
            return end;
        return beg;
    }
};
```

### 复杂度分析:

log\(x\)



## 586 Sqrt\(x\) II

Implement`double sqrt(double x)`and`x >= 0`.

Compute and return the square root of x.

##### Notice

You do not care about the accuracy of the result, we will help you to output results.



**Example**

Given`n`=`2`return`1.41421356`

http://www.lintcode.com/en/problem/sqrtx-ii/

### 解题分析:

求解sqrt就是找一个OOXX的位置, 但是稍微有点无聊的就是delta，这个是靠试出来的， 否则过不了，题目应该加个精度。 ooxx的精髓就是保证两边在分歧位置的两侧， 所以mid\*mid &gt; x 

### 代码：

```cpp
class Solution {
public:
    /*
     * @param x: a double
     * @return: the square root of x
     */
    double sqrt(double x) {
        // write your code here
        static double delta = 1e-12;
        if (x <= 0 )
            return 0;

        double beg = 0, end = x < 1 ? 1: x;
        while( end - beg > delta )
        {
            double mid = (beg + end)/2;
            if ( mid*mid > x)
                end = mid;
            else 
                beg = mid;
        }
        if (end*end <= x)
            return end;
        return beg;
    }
};
```

### 复杂度分析:

log\(x\)



